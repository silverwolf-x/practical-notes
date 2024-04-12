# vscode中使用rmarkdown

重要！！！一文通

[rmarkdown :: Cheatsheet (rstudio.github.io)](https://rstudio.github.io/cheatsheets/html/rmarkdown.html)

# 1. yaml导出设置

相关学习[第 2 章 R Markdown 的基础知识 | R Markdown 指南 (cosname.github.io)](https://cosname.github.io/rmarkdown-guide/rmarkdown-base.html#yaml)

[R沟通｜Rmarkdown(5)一些常用技巧 - 知乎](file:///C:/Users/Administrator/Desktop/R沟通｜Rmarkdown(5)一些常用技巧 - 知乎.mhtml)

## 基本开头

```yaml
title:  研究型学习1
subtitle: 精算模型
author:
  - 何雨轩2021201710
time:  '`r format(Sys.Date(), "%B %d, %Y")`'
```

author下的affiliation（联系）在html能显示，但在pdf输出失效

## pdf导出设置

需要相关支持，详见后面的下载配置章节

```yaml
documentclass: ctexart
geometry: "left=2cm,right=2cm,top=2cm,bottom=2cm" # 更改全文页边距
output:
    pdf_document:
        fig_caption: true  # 表示图上的注释保留
        keep_tex: true   # 保存中间.tex文档
        df_print: kable  # 数据框输出表格样式
        latex_engine: xelatex # xelatex能编译utf8显示中文
        toc: true
        toc_depth: 2
        # number_sections: true  # 表示每一节的数字保留
```

不过latex编译代码块不会自动换行，这点需要注意

## html导出设置第2版

**原生就是最好的**

一波详细设置，所有参数见[Convert to an HTML document — html_document • rmarkdown (rstudio.com)](https://rmarkdown.rstudio.com/docs/reference/html_document.html)

问题：渲染时引用问题太大[Font size in blockquotes is too large · Issue #3901 · rstudio/rstudio (github.com)](https://github.com/rstudio/rstudio/issues/3901)

**math_method**

​	"default" for `mathjax`

​	[Mathjax和katex的功能比较（VSCode+MPE） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/381263375)，mathjax最通用，且能打`\tag{1}\label{eq}`在正文中引用`\eqref{}`

**highlight**

​	目前`tango`最好看，其它样例见[Garrick Aden-Buie - Pandoc Syntax Highlighting Examples (garrickadenbuie.com)](https://www.garrickadenbuie.com/blog/pandoc-syntax-highlighting-examples/)

**theme**

​	见[Bootswatch：Bootstrap 的免费主题](https://bootswatch.com/)

>  'arg' should be one of "default", "bootstrap", "cerulean", "cosmo", "darkly", "flatly", "journal", "lumen", "paper", "readable", "sandstone", "simplex", "spacelab", "united", "yeti"

提示：上述原生模板对code_folding有原生支持，如果使用`bslib::bootswatch_themes()`的，即

```
theme:
	bootswatch: materia
```

这些支持不太好，有div窗口

**df_print**: paged

​	这个可以翻页，高级！

**code_folding**

​	全部代码最初折叠`hide`，全部最初展开`show`。默认`none`没有这个功能键

​	[第 3 章 html-code-folding| R Markdown 指南 (cosname.github.io)](https://cosname.github.io/rmarkdown-guide/rmarkdown-document.html#html-code-folding)

​	如果想让部分代码块在一开始就显示，则可以在代码块选项中使用 `class.source = 'fold-show'`。反之， `class.source = 'fold-hide'`

[第 3 章 使用 R Markdown 创建常用文档 | R Markdown 指南 (cosname.github.io)](https://cosname.github.io/rmarkdown-guide/rmarkdown-document.html#html-code-folding)

## html导出设置(rmdformats)

所有样式[juba/rmdformats: HTML output formats for RMarkdown documents (github.com)](https://github.com/juba/rmdformats?tab=readme-ov-file#formats-gallery)

模板体验[lockdown template example • rmdformats (juba.github.io)](https://juba.github.io/rmdformats/articles/examples/lockdown.html)

缺点：编译文件较大，1-2MB

rmdformats的特殊参数

- `thumbnails` : if TRUE, display content images as thumbnails
- `lightbox` : if TRUE, add lightbox effect to content images
- `gallery` : if TRUE, add navigation between images when displayed in lightbox
- `use_bookdown` : if TRUE, will use `bookdown` instead of `rmarkdown` for HTML rendering, thus providing section numbering and [cross references](https://bookdown.org/yihui/bookdown/cross-references.html).
- `embed_fonts` : if `TRUE` (default), use local files for fonts used in the template instead of links to Google Web fonts. This leads to bigger files but ensures that the fonts are available

```
output: 
  rmdformats::robobook:
    # use_bookdown: true
    lightbox: true
    gallery: true
    # code_download: true
    code_folding: hide
    highlight: arrow
    df_print: paged
    math_method: mathjax
```

robobook是仿bookdown的，比较简洁

# 2. knit代码输出控制

更多可看[1.5w字的Rmarkdown入门教程汇总 - 大咖驾到 - 博客园 (cnblogs.com)](https://www.cnblogs.com/purple5252/p/14699033.html)

代码输出控制：https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html#rmd-rcode-outformat

rmd一图流简介https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown-2.0.pdf

- 全局设置

~~~R
```{r,include=FALSE}
# install.packages()
# 让knitr编译时可以编译重复的代码块名，否则容易报错
options(knitr.duplicate.label = "allow")
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE, error = FALSE)
knitr::opts_chunk$set(fig.path = "Figs/",out.width="45%",fig.align='center')
```
~~~

- `knit::kable()`用于显示数据框。
    knitr::kable(d1)

- 载入包时不显示载入信息
    {r,message=FALSE}

- 图片设置

    {r,out.width="45%",fig.align='center'}

- 更优雅的输出一块代码的多个print

    {r,collapse = TRUE}

- eval=FALSE代码仅显示而不实际运行

- include=FALSE代码段仅运行， 但是代码和结果都不写入到生成的文档中

- echo=TRUE代码块显示

- results=选择文本型结果的类型。 取值有：

​	markup, 这是缺省选项， 会把文本型结果变成HTML的原样文本格式。
​	hide, 运行了代码后不显示运行结果。
​	hold, 一个代码块所有的代码都显示完， 才显示所有的结果。
​	asis, 文本型输出直接进入到HTML文件中， 这需要R代码直接生成HTML标签， knitr包的`kable()`函数可以把数据框转换为HTML代码的表格。

## R常用命令 

查看默认路径

```
getwd()
```


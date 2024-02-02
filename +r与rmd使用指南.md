# vscode中使用rmarkdown

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

## html导出设置

```yaml
output:
  html_document:
  	keep_md: false
  	df_print: kable
    toc: true
    toc_float:
      collapsed: false
      smooth_scroll: false
```

- （可选）使用`rmdformats`包

```yaml
output:
  rmdformats::readthedown:
    self_contained: true
    thumbnails: true
    lightbox: true
    gallery: false
    highlight: tango
    df_print: kable
```

![img](https://img2020.cnblogs.com/blog/1373034/202104/1373034-20210425091256095-56360833.png)

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


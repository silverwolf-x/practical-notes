# vscode中使用rmarkdown

设置清华源

```
options("repos" = c(CRAN = "https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
```

```bash
options()$repos  ## 查看使用install.packages安装时的默认镜像
```

# 1. yaml导出设置

## 基本开头

```yaml
title:  研究型学习1
subtitle: 精算模型
author: 何雨轩
time:  '`r format(Sys.Date(), "%B %d, %Y")`'
```

## pdf导出设置

需要相关支持，详见后面的下载配置章节

- （可选）R语言需要安装`rticles`包，这个包中有Ctex相关功能，所以可以实现中文的输出

```
install.packages("rticles")
```

>  使用rticles::ctex:时不能在文件名出现如下字符
>
> **Error: The name of the input file cannot contain the special shell characters: [ <>()|\:&;#?*']** 



```yaml
documentclass: ctexart
geometry: "left=2cm,right=2cm,top=2cm,bottom=2cm" # 更改全文页边距
output:
    pdf_document:
    # rticles::ctex:  # 其实和pdf_document没什么区别
        fig_caption: yes  # 表示图上的注释保留
        keep_tex: yes   
        latex_engine: xelatex # xelatex能编译utf8
        # toc: true
        toc_depth: 2
        # number_sections: true  # 表示每一节的数字保留
```

## html导出设置

```
output:
  html_document:
    toc: true
    toc_float:
      collapsed: false
      smooth_scroll: false
```

- （可选）使用`rmdformats`包

```
output:
  rmdformats::readthedown:
    self_contained: true
    thumbnails: true
    lightbox: true
    gallery: false
    highlight: tango
```

![img](https://img2020.cnblogs.com/blog/1373034/202104/1373034-20210425091256095-56360833.png)

# 2. knit代码输出控制

更多可看[1.5w字的Rmarkdown入门教程汇总 - 大咖驾到 - 博客园 (cnblogs.com)](https://www.cnblogs.com/purple5252/p/14699033.html)

代码输出控制：https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html#rmd-rcode-outformat

rmd一图流简介https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown-2.0.pdf

- 全局设置

````{r,include=FALSE}
```{r,include=FALSE}
# include=FALSE：rmd编译时会运行但不显示代码和结果
# 让knitr编译时可以编译重复的代码块名，否则容易报错
options(knitr.duplicate.label = "allow")
knitr::opts_chunk$set(fig.path = "Figs/", echo = TRUE, warning = FALSE, message = FALSE, error = FALSE)
```
````

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

# 3. 下载及初始配置

参考[如何在 VSCODE 中高效使用 R 语言 （图文详解）_vscode运行r语言-CSDN博客](https://blog.csdn.net/u011262253/article/details/113837720)

## 1. vscode配置

- 直接下载vscode的R扩展

- python安装`radian`

    ```
    pip install radian
    ```

- 在系统powershell中寻找`radian`路径

    ```
    where radian
    ```

"ctrl"+", " 进入配置，打开配置的 json 文件

```
    "r.lsp.debug": true,
    "r.rterm.windows": "D:\\Program Files\\Python311\\Scripts\\radian.exe",# python radian包位置
    "r.bracketedPaste": true,
    "r.rterm.option": [
        // "--no-save",
        // "--no-restore",
        "--no-site-file"
    ],
```
- (可选)r.sessionWatcher

    可以实现绘图IDE，查看dataframe。如果想用原生绘图，取消勾选即可

## 2. R配置

- 安装所需r包
  
  ```
  install.packages(c("rmarkdown","knitr","tinytex")) 
  ```
  

> tinytex是rmd使用knit生成pdf必须的包

- 安装Pandoc软件

  ```
  rmarkdown::pandoc_available() # rmarkdown能识别pandoc时返回True
  ```

### 2.1编译为pdf

### latex编译器选择1：tinytex

优点：安装包极小，约123MB

缺点：难以（或者无法）在其它软件编译latex时（如typora，jupyter）调用

```R
tinytex::install_tinytex()
tinytex::is_tinytex()
```

安装好tinytex编译程序后，`tinytex::is_tinytex()`返回True。如果没有tinytex编译器（例如你使用MikTex），则False

详见https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html#rmd-latex-tinytex

**不推荐既安装tinytex编译器又安装MikTex编译器，防止两个Tex编译冲突**

### latex编译器选择2：MikTex

优点：官方推荐之一，能被tex编译软件调用；相较于全量的Texlive，MikTex只安装所需要的包，之后可以按照需求添加；管理页面好看

缺点：约800MB

安装地址：https://miktex.org/download

具体问题（中文显示等）可见本人GitHub	[vscode-jupyter使用.md](https://github.com/Silverwolf-x/practical-notes/blob/master/vscode-jupyter使用.md)



## 4.常见报错

- vscode加载web 视图，报错:“Error: Could not register serviceworkers: InvalidstateError: Failed to regist“
  关闭vscode，按WIN + R，输入cmd，打开终端，然后输入命令code --no-sandbox
-  使用MikTex时，`xletex.log`报错**FATAL xelatex.core - GUI framework cannot be initialized.**

运行MikTex Console：

![MikTex Console修改后界面](/assets/image-20231223220301528.png)
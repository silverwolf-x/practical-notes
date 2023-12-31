# 实用笔记存档

一些弄电脑的笔记存到github上，作为博客来使用

有空就同步到百度网盘上，没空就算了

github的优势在于多次修改

本地可以用typora去编辑md

20231225注：经过一番折腾，决定使用typora编辑md。理由如下：

- 编辑目标是让本地渲染与github渲染一致。



|      |Github      | Markdown Preview Enhanced | Typora |
| ---- | ---- | ---- | ---- |
| |[Working with advanced formatting - GitHub Docs](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting)中文版本[创建和突显代码块 - GitHub 文档](https://docs.github.com/zh/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks) | [简介 (shd101wyy.github.io)](https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/) |  |
|数学渲染 |Mathjax | 可选Katex或Mathjax | Katex |
| 数学格式inline | dollar symbols (`$`), or start the expression with `$` and end it with ``$ | `$...$` 或者` \(...\)`                                       | `$...$` |
| 数学格式display | two dollar symbols `$$`, or use the ````math` code block syntax | `$$...$$` 或者` \[...\]` 或者 ```math` | `$$...$$` |
| 代码块嵌套 | ````或~~~ | ~~~ | ````或~~~ |
|      |      |      |      |



## GitHub格式

高级语法查阅：[Working with advanced formatting - GitHub Docs](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting)

- GitHub使用mathjax渲染数学公式。[Writing mathematical expressions - GitHub Docs](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)支持格式如下：

> 行间inline： dollar symbols (`$`), or start the expression with `$` and end it with ``$`
>
> 块内display： two dollar symbols `$$`, or use the ````math` code block syntax 

- GitHub支持代码块引用

`````
````
```
Look! You can see my backticks.
```
````
`````

和

````
~~~
```
Look! You can see my backticks.
```
~~~
````

## Markdown Preview Enhanced (MPE)格式

- 可选Katex或Mathjax渲染。支持现在主流的latex语法`\(...\) `和`\[...\]`

主要争议在数学公式的支持上，github和typora只用Katex渲染，只支持`$`和`$$`的使用。

可选1：`Markdown Preview Enhanced (MPE)`
> 参考[浙江大学计算机学院「实用技能拾遗」](https://github.com/TonyCrane/PracticalSkillsTutorial)的`lec3：Markdown 语法及应用`以及网上推荐的vscode插件

`Markdown Preview Enhanced (MPE)`确实很强大，可以改预览样式以及

```markdown
• 行内 (inline) 公式，用 $^^.$ 或 \(^^.\) 包裹
• 行间 (display) 公式
    • 单行公式用 \[^^.\] 包裹
    • 多行公式用 equation / align / gather 等环境
    • 不要用 $$^^.$$：TEX 原始语法，会产生很多问题
    • 想输入正常的文本？\text{^^.}
```
但是，它的有一些渲染有问题，例如嵌套代码块

`````
~~~markdown
```{r,include=FALSE}
# include=FALSE：rmd编译时会运行但不显示代码和结果
# 让knitr编译时可以编译重复的代码块名，否则容易报错
options(knitr.duplicate.label = "allow")
knitr::opts_chunk$set(fig.path = "Figs/", echo = TRUE, warning = FALSE, message = FALSE, error = FALSE)
```
~~~
`````

在MPE中会显示问题






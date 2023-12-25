# 实用笔记存档

一些弄电脑的笔记存到github上，作为博客来使用

有空就同步到百度网盘上，没空就算了

github的优势在于多次修改

本地可以用typora去编辑md

20231225注：经过一番折腾，决定使用typora编辑md。理由如下：

编辑目标是让本地渲染与github渲染一致。

主要争议在数学公式的支持上，github和typora只用Katex渲染，只支持`$`和`$$`的使用。

可选1：`Markdown Preview Enhanced (MPE)`
> 参考[浙江大学计算机学院「实用技能拾遗」](https://github.com/TonyCrane/PracticalSkillsTutorial)的`lec3：Markdown 语法及应用`以及网上推荐的vscode插件

`Markdown Preview Enhanced (MPE)`确实很强大，可以改预览样式以及可选Katex或Mathjax渲染。其中Mathjax渲染支持现在主流的latex语法`\(...\) `和`\[...\]`

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

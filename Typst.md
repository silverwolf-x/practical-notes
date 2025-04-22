## 安装
1. [Release Version 0.11.1 (May 17, 2024) · typst/typst (github.com)](https://github.com/typst/typst/releases)解压最新版本，并加入系统变量
2.  在 VS Code 中安装 [Tinymist Typst](https://marketplace.visualstudio.com/items?itemName=myriad-dreamin.tinymist) 和 [Typst Preview](https://marketplace.visualstudio.com/items?itemName=mgt19937.typst-preview) 插件。前者负责语法高亮和错误检查等功能，后者负责预览。
直接开始！

## 快速学习
[Typst.pdf (casea1998.cn)](https://www.casea1998.cn/pdf/pdf/Typst.pdf)
[Typst Documentation](https://typst.app/docs)
模板参考
[typst-cn/awesome-typst-cn: Awesome Typst 列表中文版 (github.com)](https://github.com/typst-cn/awesome-typst-cn)

## 快速语法

latex反义字符由`#`替代

- 标题设置
[Heading Function – Typst Documentation](https://typst.app/docs/reference/model/heading/)
```
#outline() 
#heading[Normal] 
This is a normal heading. 
#heading(outlined: false)[Hidden] 
This heading does not appear in the outline.
```
- 用md语法输入table
[OrangeX4/typst-tablem: Write markdown-like tables easily. (github.com)](https://github.com/OrangeX4/typst-tablem)
table插入标题caption
[Table guide – Typst Documentation](https://typst.app/docs/guides/table-guide/)

注意，使用tablem
```
#import "@preview/tablem:0.1.0": tablem

#let three-line-table = tablem.with(
  render: (columns: auto, ..args) => {
    table(
      columns: columns,
      stroke: none,
      align: center + horizon,
      table.hline(y: 0),
      table.hline(y: 1, stroke: .5pt),
      ..args,
      table.hline(),
    )
  }
)

#three-line-table[
  | *Name* | *Location* | *Height* | *Score* |
  | ------ | ---------- | -------- | ------- |
  | John   | Second St. | 180 cm   |  5      |
  | Wally  | Third Av.  | 160 cm   |  10     |
]
```
时，空白的东西不要打空格，会报错，即

> [!NOTE]
>
> 错误的
>
> ```
> #three-line-table[
> |     | *Location* | *Height* | *Score* |
> | ------ | ---------- | -------- | ------- |
> | John   | Second St. | 180 cm   |  5      |
> | Wally  | Third Av.  | 160 cm   |  10     |
> ]
> 
> ```
> 

>  [!NOTE]
> 
>  正确的
>
> ```
> #three-line-table[
>   || *Location* | *Height* | *Score* |
>   | ------ | ---------- | -------- | ------- |
>   | John   | Second St. | 180 cm   |  5      |
>   | Wally  | Third Av.  | 160 cm   |  10     |
> ]
> ```
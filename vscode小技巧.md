# vscode小技巧

监视nvidia-smi刷新

安装`nvitop`包，输入

```powershell
! pip install nvitop
nvitop -m full
```

> 展示的模式有三种：
>
> 1. auto (默认)
> 2. compact
> 3. full

## 一键更新python所有包

安装`pip-review`包

```
pip-review # 看能更新的包详情
pip-review --auto -C # 自动更新并跳过安装失败的包
```

> options:
>   -h, --help            show this help message and exit
>   --verbose, -v         Show more output
>   --raw, -r             Print raw lines (suitable for passing to pip install)
>   --interactive, -i     Ask interactively to install updates
>   --auto, -a            Automatically install every update found
>   --continue-on-fail, -C
>                         Continue with other installs when one fails
>   --freeze-outdated-packages
>                         Freeze all outdated packages to "requirements.txt" before upgrading them

​	注意pytorch系列的更新要自己到[PyTorch](https://pytorch.org/)找cuda版本的链接，否则pip-review只会更新到cpu版本

```
pip3 uninstall torch torchvision torchaudio
pip3 install ... # 官网自己找
```

## vscode自定义代码片段

Code-首选项-用户代码片段-选择代码块作用域的文件类型

![Snipaste_2020-01-09_15-31-47.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNjI2NTE2Mi0yNzRiZTI5ZWQxNGY3NWU4LnBuZw?x-oss-process=image/format,png)

- 类型一：`全局作用域`
    创建在vscode软件内部。跟随这当前安装的vscode这个软件的
- 类型二：`文件夹作用域`
    创建在某个文件下.vscode这个隐藏文件夹中的，这个代码块只适用于当前文件夹
- 类型三：`特定文件类型作用域`
    创建在vscode软件内部，适用于指定的文件类型。如：创建的是JavaScript类型，那这个代码块就不能再vue文件中使用。

特点：`$`符号表示tab时待定输入项。多个 tab 停留，使用相同序号即可；

例子：

```json
"yaml基本信息，剩余代码片段在yaml环境下": {
    "prefix": "yaml_info",
    "body": [
      "---"
      "title: $1",
      "subtitle: $2",
      "author: ",
      "  - 何雨轩2021201710",
      "date:  '`r format(Sys.Date(), \"%B %d, %Y\")`'"
      "---"
    ],
    "description": "yaml基本信息，按tab选择切换"
  },
```

如果需要自定义代码块含有\$，则需要`\\$`

辅助工具：[自动生成代码块工具网站](https://snippet-generator.app/)

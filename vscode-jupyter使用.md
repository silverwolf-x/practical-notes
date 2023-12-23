# vscode-jupyter使用

## python需要安装的包

编译jupyter需要`ipykernel`，导出html和py需要`notebook`

```powershell
pip install ipykernel notebook
```

## 导出pdf前置准备[^1]

### 1. 安装`Pandoc`

Pandoc官网下载地址: https://pandoc.org/installing.html

这个其实也是typora编译需要的

### 2. 安装`MiKTex`

如果导出pdf通过LaTex，还需要下载`MiKTex`: https://miktex.org/download

> Texlive包比较全但占用空间大，Miktex占用空间小（基础约800MB），需要的包可在线下载，且管理界面美观

首次在导出pdf过程中，会自动安装一些依赖包，根据提示点击安装即可

### 3. 中文显示问题

修改python安装路径下的一个文件

```
D:\Program Files\miniconda3\share\jupyter\nbconvert\templates\latex\index.tex.j2
```

- **基本解决思路**

把 `block doclass`下的 `article` 改成 `ctexart`：

```jinja2
((*- block docclass -*))
\documentclass[11pt]{ctexart}
((*- endblock docclass -*))
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/651704c696924e70b4ffbdee402e1d14.png)

这样H1标题会居中，H2及之后会靠左

![在这里插入图片描述](https://img-blog.csdnimg.cn/8324e4139e1b44a4b2578d8a57faa687.png)

- **H1标题居中调整**

在`index.tex.j2`文件中增加如下命令

```
\CTEXsetup[format={\Large\bfseries}]{section}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2fcdc224ab9d4c90980a082210548ff2.png)





[^1]: 参考[【python】jupyter notebook导出pdf和pdf不显示中文问题-CSDN博客](https://blog.csdn.net/sinat_32872729/article/details/132445344)
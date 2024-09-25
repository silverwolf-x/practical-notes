
# vscode 环境配置

只介绍配置，不详细介绍具体使用习惯与技巧。

关注命令的相关配置，即使用`python --help`、 `pandoc --help`

# python：`miniconda`

## 安装与切换默认环境不是base

https://blog.csdn.net/Inochigohan/article/details/120400990

> 配置文件为C:\Users\Administrator\.condarc
>
> 查看设置
>
> ```
> conda config --show
> ```

- 安装在`Program Files`。虽然程序推荐安装在`ProgramData`目录下，但是这样配置完成后powershell加载conda环境时会弹出黑框运行shell文件。而安装在`Program Files`下时则不会弹出，静默运行
- 配置系统环境

高级系统变量--Path

```
D:\Program Files\miniconda3    
D:\Program Files\miniconda3\Scripts   
D:\Program Files\miniconda3\Library\bin
```
- 打开任意python文件，选择编译器运行
- 终端提示输入`conda init`

- 创建虚拟环境，并切换为默认环境，以方便删除与管理

```
conda create -n envbase python=3.11 -y
```

- vscode的python插件设置中，设置默认python path为`D:\Program Files\miniconda3\envs\envbase\python.exe`

## 重置虚拟环境

即删除所有安装过的包

```
pip freeze > all.txt 
pip uninstall -r all.txt -y
```

## conda命令

切换清华大学源

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

(先把所有工具包进行升级)

```
conda upgrade --all
```

创建虚拟环境

```
conda create -n dev python=3.10 -y # 默认3.10最新版本，并默认yes
conda create -n dev python==3.10# 安装3.10.0版本
conda create -p ./myenv  python=3.10 # 在指定目录下执行以下命令
```

切换环境

```
conda activate dev
conda deactivate
```

重命名

```
conda create -n tf --clone rcnn #把环境 rcnn 重命名成 tf
conda remove -n rcnn --all 
#conda 其实没有重命名指令，实现重命名是通过 clone 完成的，分两步：1.先 clone 一份 new name 的环境。2.删除 old name 的环境。
```

清除环境

```
conda remove -n dev --all # 彻底删除要到文件夹下删除envs/dev
```

清理缓存

```
conda clean -y --all 
```

查看所有虚拟环境

```
conda env list
```

**清除pip下载包缓存**

```
pip cache purge
```

### base环境相关

```
conda config --set auto_activate_base false	# 默认不进入base环境
conda config --set auto_activate_base true	# 默认进入base环境
```

清除base安装的所有包

[python - conda: remove all installed packages from base/root environment - Stack Overflow](https://stackoverflow.com/questions/52830307/conda-remove-all-installed-packages-from-base-root-environment)

```
conda list --revisions
```
## pip设置多个源
在C:\Users\用户名\AppData\Roaming\pip目录下新建配置文件pip.ini
内容如下：
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
extra-index-url =
    https://mirrors.aliyun.com/pypi/simple/
    https://pypi.douban.com/simple/
    https://pypi.mirrors.ustc.edu.cn/simple/
    https://pypi.org/simple

[install]
trusted-host =
    pypi.tuna.tsinghua.edu.cn
    mirrors.aliyun.com
    pypi.douban.com
    pypi.mirrors.ustc.edu.cn
    pypi.org

```
以后执行pip命令，会自动依次查找每个源，如果在某个源中找到了需要安装的包，就会下载并安装。如果在所有源中都找不到包，就会提示找不到匹配的包。

注：首选清华源，速度最快最稳定！

## jupyter常用包安装

编译jupyter需要`ipykernel`，导出html和py需要`notebook`

```powershell
pip install ipykernel notebook ipywidgets
```

```
# torch在pytorch官网中下载cuda版本
pip install tqdm numpy pandas matplotlib scikit-learn seaborn 
pip install transformers
```

## jupyter的tqdm条

[IPyWidget Support in VS Code Python · microsoft/vscode-jupyter Wiki (github.com)](https://github.com/microsoft/vscode-jupyter/wiki/IPyWidget-Support-in-VS-Code-Python)

在setting.json中设置

```
"jupyter.widgetScriptSources": ["jsdelivr.com", "unpkg.com"],
```

然后记得更新vscode版本

## 使用习惯

- 一般安装照常用pip install

- 右键`用code打开`文件夹作为工作区，点击任意python文件，将vscode右下角切换为conda的python虚拟环境。这样使用任意终端时，vscode会自动使用这个python环境


> powershell 脚本中，需要将`python3`命令全部替换为`python`

### 代码格式化

[Formatting Python in Visual Studio Code](https://code.visualstudio.com/docs/python/formatting)

现在已经弃用原本的方法[VSCode最新格式化Python文件的方法_vscode python 格式化-CSDN博客](https://blog.csdn.net/StephanieXYM/article/details/133767499)，需要插件形式格式化

各格式化对比：[python格式化代码只懂autopep8？这里有更好的 - 简书 (jianshu.com)](https://www.jianshu.com/p/dc9d66d357bb)

# `Pandoc`

- `rmd`和`python-jupyter`编译html时需要pandoc转换，`Typora`编译也需要

# latex：`MiKTex`

- 导出pdf需要通过LaTex

优点：官方推荐之一，能被tex编译软件调用；相较于全量的Texlive，MikTex只安装所需要的包，之后可以按照需求添加；管理页面好看

缺点：约800MB

安装地址：https://miktex.org/download

> Texlive包比较全但占用空间大，Miktex占用空间小（基础约800MB），需要的包可在线下载，且管理界面美观

首次在导出pdf过程中，会自动安装一些依赖包，根据提示点击安装即可

## vscode-latexworkshop

安装perl：[配置 VSCode 下 LaTex 环境 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/379264127)

配置json：vscode配置latexmk[在 VSCode 的 LaTeXworkshop 插件中使用 LaTeXmk | 始终 (liam.page)](https://liam.page/2020/04/24/using-LaTeXmk-with-LaTeXworkshop-with-VSCode/)

编译后自动清理多余文件：[vscode 配置 Latex 编译后自动清理多余文件(.log .out等文件)_latex运行一次就自动日志文件-CSDN博客](https://blog.csdn.net/weixin_35757704/article/details/90597405)

## tex使用

精华文档[lec4.pdf (tonycrane.cc)](https://slides.tonycrane.cc/PracticalSkillsTutorial/2023-fall-ckc/lec4/lec4.pdf)

**推荐使用 latexmk，自动选择编译指令**



## 中文显示问题

修改python安装路径下的一个文件

```
D:\Program Files\miniconda3\share\jupyter\nbconvert\templates\latex\index.tex.j2
```

- **基本解决思路**

把 `block doclass`下的 `article` 改成 `ctexart`：

> Miktex基本包自带ctexart，无需额外安装

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

##  使用MikTex时，`xletex.log`报错**FATAL xelatex.core - GUI framework cannot be initialized.**

运行MikTex Console：[MikTex Console修改后界面](<assets/rmd使用指南-MikTex Console修改后界面.png>)

# R

参考[如何在 VSCODE 中高效使用 R 语言 （图文详解）_vscode运行r语言-CSDN博客](https://blog.csdn.net/u011262253/article/details/113837720)

## vscode及python前置设置

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

## 基本配置

设置清华源

```
options("repos" = c(CRAN = "https://mirrors.tuna.tsinghua.edu.cn/CRAN/"))
options()$repos  ## 查看使用install.packages安装时的默认镜像
```

基本安装

```R
install.packages(c("languageserver","ggplot2")) # 一般的包
install.packages(c("rmarkdown","knitr","tinytex")) # rmd相关
# 可选
install.packages("rmdformats") # 可选rmdformat有好看的html
```

## Pandoc与Latex

- 环境配置见上文

> tinytex是rmd使用knit生成pdf必须的包

- 验证Pandoc能被R调用

    ```
    rmarkdown::pandoc_available() # rmarkdown能识别pandoc时返回True
    ```

- tinytex相关

> 优点：安装包极小，约123MB
>
> 缺点：难以（或者无法）在其它软件编译latex时（如typora，jupyter）调用
>
> ```R
> tinytex::install_tinytex()
> tinytex::is_tinytex()
> ```
>
> 安装好tinytex编译程序后，`tinytex::is_tinytex()`返回True。如果没有tinytex编译器（例如使用MikTex），则False
>
> 详见https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/rmarkdown.html#rmd-latex-tinytex

**不推荐既安装tinytex编译器又安装MikTex编译器，防止两个Tex编译冲突**

## 常见报错

- vscode加载web 视图，报错:“Error: Could not register serviceworkers: InvalidstateError: Failed to regist“
    关闭vscode，按WIN + R，输入cmd，打开终端，然后输入命令code --no-sandbox

## shell

>  借助git的bash，见shell.exe放入环境变量
>
> ![](https://img-blog.csdnimg.cn/20190406155849491.png)

右键点击桌面上的“此电脑”，“属性”，“高级系统设置”，“高级”，“环境变量“，在“系统变量”中的Path增加bin的文件夹的路径，如“D:\Git\bin”即可。之后重启vscode确保环境变量刷新

- powershell测试

    ```powershell
    sh --help
    ```

# wget

[wget 的安装与使用（Windows）_windows wget-CSDN博客](https://blog.csdn.net/m0_45447650/article/details/125786723)

# CUDA + cuDNN + TensorRT

## 驱动
首先要安装nvidia驱动（个人喜欢studio版本，更加稳定），此时cmd有如下命令
```cmd
nvidia-smi
```
显示driver version为555.99，**可安装的最高CUDA版本**为12.5

一般而言，如果想要调用cuda要自己电脑上安装，或者在conda环境中安装。
[【环境搭建：onnx模型部署】onnxruntime-gpu安装与测试（python）-CSDN博客](https://blog.csdn.net/qq_40541102/article/details/130086491)


## 特例：Torch
> [!NOTE] torch
> 由于torch的超级懒人包会自带cuda和cudnn的编译，因此直接安装cuda版本的torch和nvidia驱动即可畅通无阻的使用！
> tensorrt可以按照官方指示安装[安装 — Torch-TensorRT文档](https://pytorch.org/TensorRT/getting_started/installation.html)
> ```python
> import torch
> print(f'{torch.__version__=} {torch.version.cuda=} {torch.backends.cudnn.version()=}')
> ```
> >torch.__version__='2.4.0+cu124' torch.version.cuda='12.4' torch.backends.cudnn.version()=90100
> 表明torch版本是2.4.0+cu124，**当前torch内置编译的CUDA版本**为12.4，内置的cudnn版本为9.1.0

## CUDA
[CUDA Toolkit Archive | NVIDIA Developer](https://developer.nvidia.com/cuda-toolkit-archive)
cuda和驱动的版本对应：
[CUDA  Release Notes](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html#id5)
自定义安装--只选择CUDA。
如果没有安装visual studio，还要把CUDA里面的 Visual Studio Integration取消勾选，否则会安装失败
```cmd
nvcc -V
```
> Copyright (c) 2005-2024 NVIDIA Corporation
Built on Thu_Mar_28_02:30:10_Pacific_Daylight_Time_2024
Cuda compilation tools, release 12.4, V12.4.131
Build cuda_12.4.r12.4/compiler.34097967_0

显示系统内安装的是cuda12.4

还有其它命令可以尝试：
在C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4\extras\demo_suite文件夹
```cmd
deviceQuery
```
## cuDNN
[cuDNN Archive | NVIDIA Developer](https://developer.nvidia.com/cudnn-archive)
选择Tarball

解压缩cudnn-windows-x86_64-8.4.0.27_cuda11.6-archive.zip 直接解压缩，完成后点击去你能看到如下三个文件夹（bin、include、lib）。把这三个文件夹的文件分别拷贝到CUDA安装目录对应的（bin、include、lib）文件夹中即可。CUDA的lib目录有x64 、Win32、cmake三个文件夹，拷到其中的x64这个文件夹中即可。

- 查看版本
`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4\include\cudnn_version.h`

```c
#define CUDNN_MAJOR 9
#define CUDNN_MINOR 3
#define CUDNN_PATCHLEVEL 0

#define CUDNN_VERSION (CUDNN_MAJOR * 10000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)
```
表示版本为9.3.0，显示为90300

## TensorRT
[TensorRT 10.x Download | NVIDIA Developer](https://developer.nvidia.com/tensorrt/download/10x)
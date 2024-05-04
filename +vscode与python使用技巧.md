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

## opencv配置

背景：

- 需要`import cv2`的python脚本

- `requirements.txt`中，需要安装opencv-python。但类似pytorch，这是cpu版本。cuda版本按照官方说法需要自行编译

解决：[在windows环境下安装支持CUDA的opencv-python_使用 cuda 重新安装 opencv-CSDN博客](https://blog.csdn.net/shinuone/article/details/131435093)

参考[opencv-python 库使用 cuda 加速 | Triority's blog](https://www.triority.cn/2023/opencv-python-cuda/)

0. 先不急着开设conda虚拟环境

1. 下载网上大佬预编译好的文件。[OpenCV Windows shared libraries](https://github.com/cudawarped/opencv_contrib/releases)中的最新即可。

    > `opencv_contrib_cuda_4.9.0_win_amd64.7z`已经存档到百度网盘中

    - 解压到合适的位置，将路径 `install\x64\vc16\bin`设置为windows系统环境变量

    - 打开`python3\Debug`，将这里的所有3个文件复制到python包文件夹中。以虚拟环境opencv为例，地址为`D:\Program Files\miniconda3\envs\opencv\Lib\site-packages`。直接复制到这下面即可。

2. 观察到所支持的python版本，如`cv2.cp39-win_amd64.pyd`表示支持python3.9。**此时才建python虚拟环境**。如果之前已经建好，可以参照下文更换python版本及重装所有包。

    ```
    conda install python=3.9 -y
    pip freeze | ForEach-Object { ($_ -split '=')[0] } > all.txt
    pip uninstall -r all.txt -y
    pip install -r all.txt
    ```

3. 在conda中安装cuda-toolkit和cudnn环境，使用nvidia渠道保证最新版本（记得要看显卡驱动是否支持这个版本CUDA）

    ```
    conda install cuda-toolkit cudnn -c nvidia
    conda uninstall cuda-toolkit cudnn -y
    ```

    > 否则使用时会报错`ImportError: DLL load failed while importing cv2: 找不到指定的模块。 `

4. 卸载之前安装的cpu版本opencv，以免调用冲突

    ```
    pip uninstall opencv-python uninstall
    ```

    

## 粘性滚动

```
editor.experimental.stickyScroll.enabled
```

![img](https://img-blog.csdnimg.cn/1ba30bfa12374d809b6785d4b928298d.png)

## pipreqs导出包

[bndr/pipreqs: pipreqs - Generate pip requirements.txt file based on imports of any project. Looking for maintainers to move this project forward. (github.com)](https://github.com/bndr/pipreqs#usage)

可以只导出工程的包

可以不要版本

```
pipreqs  --mode no-pin
```

如果要导出pip的所有包

```
pip freeze | findstr /V "==" > requirements.txt
```

## 下载cp311的包

有时最新版本的oython有很多包每跟上，兼容报错

## ydata_profiling中文

这是一个数据探索EDA自动化包，见[招商银行FinTech训练营数据赛道竞赛平台使用说明_牛客网 (nowcoder.com)](https://www.nowcoder.com/discuss/480746781626621952)

词云图没有中文：`~\ydata_profiling\visualisation\plot.py`中，添加font_path

```python
wordcloud = WordCloud(
            background_color="white", random_state=123, width=300, height=200, scale=2,     font_path=r'C:\Users\Administrator\AppData\Local\Microsoft\Windows\Fonts\SimHei.ttf'
        ).generate_from_frequencies(word_dict)
```

这里黑体由[百分百解决你的matplotlib画图中文乱码问题 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/566430362)中http://129.204.205.246/downloads/SimHei.ttf提供

从而后面都能用

```python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']# 黑体
plt.rcParams['axes.unicode_minus'] = False# 解决中文显示问题
```

显示了

## lightgbm gpu编译

[Silverwolf-x/LightGBM-Nvidia-Win10: 编译nvidia gpu成功存档，提供GPU编译后的wheel包，可以直接安装运行 (github.com)](https://github.com/Silverwolf-x/LightGBM-Nvidia-Win10)

## Anaconda环境迁移：直接将之前搭建好的环境从一个机子迁移到另一个机子

[【保姆级教程】Anaconda环境迁移：直接将之前搭建好的环境从一个机子迁移到另一个机子-CSDN博客](https://blog.csdn.net/qq_40968179/article/details/128990022)

1. 复制整个env到新电脑（建议不压缩打包后复制，加快专属传输效率）

2. 新电脑：

    ```shell
    conda config --append envs_dirs D:\ml
    ```

    

移植使用autogluon的多模态模型要移植这个

C:\Users\Administrator\.cache\huggingface\hub\models--google--electra-base-discriminator

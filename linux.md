---
date: 2024-03-05
---

[TOC]

## WSL

### 使用模拟器冻结虚拟化后，要启动后WSL的操作
```
bcdedit /set hypervisorlaunchtype auto
```
之前使用模拟器 提示让我禁用 Hyper-V ， 之后我进行了 Hyper-V 的重启 试过上面的所以的方法都没有效果，在最后执行了 如上指令 在重启电脑 得到了解决 希望可以帮助到你
[Error code: Wsl/Service/CreateInstance/CreateVm/HCS/HCS_E_HYPERV_NOT_INSTALLED · Issue #10332 · microsoft/WSL (github.com)](https://github.com/microsoft/WSL/issues/10332)
### WSL2转化

开启windows的wsl功能，用WSL2还要开启hyper-V

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-version Ubuntu-22.04 2
wsl -l -v
```

### WSL的linux内核更新

powershell中

```
wsl --version # 看linux内核版本
```

> WSL 版本： 2.1.5.0
> 内核版本： 5.15.146.1-2
> WSLg 版本： 1.0.60
> MSRDC 版本： 1.2.5105
> Direct3D 版本： 1.611.1-81528511
> DXCore 版本： 10.0.25131.1002-220531-1700.rs-onecore-base2-hyp
> Windows 版本： 10.0.19045.3324

表示linux内核版本是 5.15.146.1-2

```
wsl --update --web-download # 更新wsl和对应内核（在WSL2下）
```

不要用**wsl --update**命令，它会调用windows更新来查找wsl更新

如果是使用WSL 1的话，内核应该还是在4.4

这样安装了一个windows app软件`适用于 Linux 的 Windows 子系统`

### WSL使用

```
wsl --set-default-version 2 # 设置使用wsl2
wsl --shutdown #停止
wsl -l -v #查看安装情况，version指WSL版本，理想情况是2
```

## Linux发行版安装

### Ubuntu

[Windows10/11 三步安装wsl2 Ubuntu22.04（任意盘） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/466001838)

- 开启WSL

[旧版 WSL 的手动安装步骤 | Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual#step-2---check-requirements-for-running-wsl-2)

- 安装ubuntu

按照ubuntu官方教程下载安装[Install Ubuntu on WSL2 - Ubuntu WSL documentation (readthedocs-hosted.com)](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/guides/install-ubuntu-wsl2/)，不要用wsl --install，从而**绕过windows更新检查**

其中，这里用https://store.rg-adguard.net/下载ubuntu应用安装包。安装即可

也可以[Manual installation steps for older versions of WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions)，但不是最新版

然后

```
sudo apt update
sudo apt full-upgrade -y
sudo apt update
```

### Debian

[WSL2安装Debian(Ubuntu)并配置国内apt源 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/99938831)

### 更改安装位置

[关于WSL2迁移系统、配置默认系统&用户的补充 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/643184142)

```
wsl --export Ubuntu-22.04 e:\wsl-ubuntu22.04.tar
wsl --unregister Ubuntu-22.04
wsl --import Ubuntu-22.04 e:\wsl-ubuntu22.04 e:\wsl-ubuntu22.04.tar --version 2
ubuntu2004 config --default-user sakura
del e:\wsl-ubuntu22.04.tar
```

### 换国内源

修改Linux apt下载地址

```
sudo chmod 777 /etc/apt/sources.list # 提权，方便直接使用vscode编辑文件
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bk # 存档
code /etc/apt/sources.list
```

复制

[ubuntu | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

[debian | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)

```
sudo apt update
```

### 一键配置zsh终端

- 首先安装zsh，安装完毕后会自动进入zsh

```
#!/bin/bash
# 如果是在docker容器内，可以去除sudo
sudo apt update
sudo apt install zsh wget git -y
wget https://gitee.com/matthew1757405148/my_scripts/raw/master/install_zsh/install.sh
chmod +x install.sh
./install.sh
```

- 接着下载并配置插件和主题

```
#!/bin/bash
export ZSH_CUSTOM=$ZSH_CUSTOM
git clone https://gitee.com/hailin_cool/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://gitee.com/Annihilater/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
# 修改zsh配置
sed -i 's/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/g' ~/.zshrc
sed -i 's/ZSH_THEME="[^"]*"/ZSH_THEME="ys"/g' ~/.zshrc
# 下面这一主题需要提前安装
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
sed -i 's/ZSH_THEME="[^"]*"/ZSH_THEME="powerlevel10k\/powerlevel10k"/g' ~/.zshrc
source ~/.zshrc
```

[Ubuntu定制之换源 + VScode + JetBrains-Mono + zsh + powerlevel10k - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/537054661)

- 重新配置p10k

如果你想再次重启配置向导，运行以下程序。你可以随心所欲地做，次数不限。

```text
p10k configure
```

### Ubuntu清理

[ubuntu清理空间技巧 包含【系统日志、缓存、无用包、内核、VScode、conda、snap、pip】_sudo apt autoremove --purge snapd-CSDN博客](https://blog.csdn.net/m0_50181189/article/details/119855107)

## linux命令

[Linux 常用操作命令大全（最后更新时间：2024年1月）_linux常用命令-CSDN博客](https://blog.csdn.net/m0_46422300/article/details/104645072)

```sh
uname -r # 
cd ~       # 返回主目录
rm -r * #删除文件夹下所有文件
```
更新
```sh
sudo apt update
sudo apt full-upgrade -y
sudo apt update
```
清除
```sh
sudo apt-get autoclean                # 删除旧版本软件缓存
sudo apt-get clean                    # 删除系统内所有软件缓存
sudo apt-get autoremove             # 删除系统不再使用的孤立软件
```
## 软件安装

### fastfetch

看linux发行版信息
```sh
wget https://github.com/fastfetch-cli/fastfetch/releases/download/2.11.5/fastfetch-linux-amd64.deb
```

### 编译相关

```
sudo apt install build-essential
```



## 安装miniconda

```
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

- 

> This will activate conda on startup and change the command prompt when activated.
> If you'd prefer that conda's base environment not be activated on startup,
>    run the following command when conda is activated:
>
> conda config --set auto_activate_base false
>
> You can undo this by running `conda init --reverse $SHELL`? [yes|no]

## 开启Hyper-V衍生问题

1. 启动米家游戏时，每次都要重新登陆

[为什么我每次启动这个游戏都需要登录？ 《原神》中没有发生这样的事：r/HonkaiStarRail --- Why I have to login every time launch this game? Doesn't happened in Genshin Impact : r/HonkaiStarRail (reddit.com)](https://www.reddit.com/r/HonkaiStarRail/comments/13ai3vq/why_i_have_to_login_every_time_launch_this_game/?rdt=58711)

解决方案：清理注册表重新登录

[原神PC端如何多个账号自由切换，无需再次输入账号密码 - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv11004659/)

2. 启动mumu需要管理员权限，有时会空转CPU到20%

终极解决方法：使用WSL1，关闭Hyper-V

如果是使用WSL 1的话，内核应该还是在4.4

**解决：**

考察nvitop得到 --startvm

## 移除snap

ubuntun最为诟病的一点是夹带私货snap，我们决定卸载它。[如何在Ubuntu中完全移除Snap-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2168090)

## CUDA-toolkit

WSL2中，可以直接使用cuda驱动，因此可以直接推理，如直接使用torch的cuda加速

```sh
python -c "import torch;print(torch.cuda.is_available())"
```

但是，如果需要cuda编译opencv，llama.cpp，就需要下载编译包CUDA-toolkit

默认linux下CUDA-toolkit后就会顺带安装驱动，但是WSL只需安装CUDA-toolkit即可

**教程[WSL——卸载、安装CUDA_wsl卸载-CSDN博客](https://blog.csdn.net/weixin_45100742/article/details/134499492)选择使用runfile**，后面要加环境变量

```
export PATH="$PATH:/usr/local/cuda/bin"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64/"
export LIBRARY_PATH="$LIBRARY_PATH:/usr/local/cuda/lib64"
```

> 会自动创建/usr/local/cuda，软映射至本次安装的驱动/usr/local/cuda-12.4

```
source ~/.bashrc # 更新系统环境
```

官方提示[CUDA on WSL (nvidia.com)](https://docs.nvidia.com/cuda/wsl-user-guide/index.html#getting-started-with-cuda-on-wsl-2)

卸载：

找到uninstall_cuda

- 找不到？

[环境配置之cuda的卸载（ubuntu） - 代码天地 (codetd.com)](https://www.codetd.com/article/14697335)

[ubuntu 批量卸载软件 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/424094055)

```
sudo dpkg -l |grep cuda # 查看剩余的残留
sudo dpkg -l |grep cuda|awk '{print $2}' | sudo xargs dpkg -P # 批量卸载cuda开头的软件包
sudo apt autoremove 
```

阅读资料：

[(5 封私信 / 1 条消息) NVIDIA-SMI 显示的cuda version 是指当前版本还是最大可以支持的 cuda 版本？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/622711856)

## 实战

### cmake交叉编译llama.cpp

需求：编译cuda mmq版本的llama.cpp以减小sakura模型显存使用，通过linux编译，并最后在windows运行

前提：cuda-toolkit（见上）, cmake, ccache

```
sudo apt install cmake # c语言编译通用软件
sudo apt install ccache # 加速编译
```

编译：

官网[ggerganov/llama.cpp: LLM inference in C/C++ (github.com)](https://github.com/ggerganov/llama.cpp?tab=readme-ov-file#build)

```
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
```

在llama.cpp文件夹下，

```
mkdir build
cd build
cmake .. -DLLAMA_CUDA=ON -DLLAMA_CUDA_FORCE_DMMV=TRUE -DLLAMA_CUDA_DMMV_X=64 -DLLAMA_CUDA_MMV_Y=4 -DLLAMA_CUDA_F16=TRUE -DLLAMA_CUDA_FORCE_MMQ=ON -DBUILD_SHARED_LIBS=TRUE
cmake --build . --config Release -- -j12
```

这里第三行是编译命令关键，来源与sakura交流群

> ```
> > cmake .. -DLLAMA_CUDA=ON -DLLAMA_CUDA_FORCE_DMMV=TRUE -DLLAMA_CUDA_DMMV_X=64 -DLLAMA_CUDA_MMV_Y=4 -DLLAMA_CUDA_F16=TRUE -DLLAMA_CUDA_FORCE_MMQ=ON -DBUILD_SHARED_LIBS=TRUE
> -- The C compiler identification is GNU 12.2.0
> -- The CXX compiler identification is GNU 12.2.0
> -- Detecting C compiler ABI info
> -- Detecting C compiler ABI info - done
> -- Check for working C compiler: /usr/bin/cc - skipped
> -- Detecting C compile features
> -- Detecting C compile features - done
> -- Detecting CXX compiler ABI info
> -- Detecting CXX compiler ABI info - done
> -- Check for working CXX compiler: /usr/bin/c++ - skipped
> -- Detecting CXX compile features
> -- Detecting CXX compile features - done
> -- Found Git: /usr/bin/git (found version "2.39.2") 
> -- Performing Test CMAKE_HAVE_LIBC_PTHREAD
> -- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
> -- Found Threads: TRUE  
> -- Found CUDAToolkit: /usr/local/cuda-12.4/include (found version "12.4.131") 
> -- CUDA found
> -- The CUDA compiler identification is NVIDIA 12.4.131
> -- Detecting CUDA compiler ABI info
> -- Detecting CUDA compiler ABI info - done
> -- Check for working CUDA compiler: /usr/local/cuda-12.4/bin/nvcc - skipped
> -- Detecting CUDA compile features
> -- Detecting CUDA compile features - done
> -- Using CUDA architectures: 60;61;70
> -- CUDA host compiler is GNU 12.2.0
> 
> -- ccache found, compilation results will be cached. Disable with LLAMA_CCACHE=OFF.
> -- CMAKE_SYSTEM_PROCESSOR: x86_64
> -- x86 detected
> -- Configuring done
> -- Generating done
> -- Build files have been written to: /home/sakura/llama.cpp/build
> ```

第四行编译为exe，ccache 加速下需要约5分钟，多线程 -j12需要约90s

这样就编译完linux下的文件了

但是：我们要在windows下运行，即

### linux交叉编译Windows程序

[Ubuntu 使用 ming-w64 交叉编译 Windows 可执行文件 | 区长 (fucknmb.com)](https://fucknmb.com/2017/12/18/Ubuntu使用ming-w64交叉编译Windows可执行文件/)

```
mkdir build2
cd build2
cmake .. \
-D CUDA_CUDART=/usr/local/cuda \
-DCMAKE_TOOLCHAIN_FILE=~/windows.toolchain.cmake \
-DLLAMA_CUDA=ON -DLLAMA_CUDA_FORCE_DMMV=TRUE -DLLAMA_CUDA_DMMV_X=64 -DLLAMA_CUDA_MMV_Y=4 -DLLAMA_CUDA_F16=TRUE -DLLAMA_CUDA_FORCE_MMQ=ON -DBUILD_SHARED_LIBS=TRUE \
-DCMAKE_BUILD_TYPE=MinSizeRel


cmake --build . --config Release  -- -j12
```

重点是，如何

问题是有些gcc便编译失败

>     gcc: error: unrecognized command-line option ‘-Zi’; did you mean ‘-Z’?
>     gcc: error: unrecognized command-line option ‘-MDd’; did you mean ‘-MD’?

vscode全局搜索MDd，发现该程序命令在

```
cmake-3.29.1-linux-x86_64/share/cmake-3.29/Modules/Platform/Windows-NVIDIA-CUDA.cmake
```

这些是Microsoft Visual Studio 专用命令，linux没有

# Docker
dockerwindows使用linux
优点：可以使用WSL，不用开启hyper-v
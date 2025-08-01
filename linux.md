---
0date: 2024-03-05
---

[TOC]

## WSL

### 使用模拟器冻结虚拟化后，要启动后WSL的操作
```cmd
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

下载最新的https://github.com/microsoft/WSL/releases 安装即可更新WSL，内核和WSLg

> [!note]
>
> ```
> wsl --update --web-download # 更新wsl和对应内核（在WSL2下）
> ```
>
> 不要用**wsl --update**命令，它会调用windows更新来查找wsl更新
>
> 如果是使用WSL 1的话，内核应该还是在4.4
>
> 这样安装了一个windows app软件`适用于 Linux 的 Windows 子系统`

### WSL的linux内核自定义更新

垃圾WSL官方推送一坨屎，久久不推送 https://zhuanlan.zhihu.com/p/355606922 
懒得自己编译，使用第三方编译好的库更新

1. https://github.com/Nevuly/WSL2-Linux-Kernel-Rolling 
或者LTS版本(6.12.x) https://github.com/Nevuly/WSL2-Linux-Kernel-Rolling-LTS
> 各版本的重大更新看 https://en.wikipedia.org/wiki/Linux_kernel_version_history
2. 下载bzImage，然后用WSL2自带的wslsetting软件指定内核地址即可，可以在linux中用fastfetch看到

> [!note]
>
> 可能有如下情况：更新自定义内核后，使用基于WSL2后端的windows docker打开时无端占用大量CPU，达到50%。猜测原因是自定义内核选择的内核附加组件缺失。linux中通过下列命令查看
>
> ```
> lsmod
> ```
>
> 暂时的解决方法是使用回原来WSL自带的老旧内核，这是官方编译全的：wslsetting软件中，内核地址留空

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

https://blog.willxup.top/archives/wsl-install

### 更改安装位置

[关于WSL2迁移系统、配置默认系统&用户的补充 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/643184142)

```
wsl --export Ubuntu-22.04 e:\wsl-ubuntu22.04.tar
wsl --unregister Ubuntu-22.04
wsl --import Ubuntu-22.04 e:\wsl-ubuntu22.04 e:\wsl-ubuntu22.04.tar --version 2
wsl --set-default Ubuntu-22.04 

del e:\wsl-ubuntu22.04.tar
```

### 换国内源

修改Linux apt下载地址

```
cp /etc/apt/sources.list ~/sources.list # 复制出来根目录修改
code ~/sources.list # 用 VS Code 编辑
sudo cp ~/sources.list /etc/apt/sources.list # 编辑完后覆盖回去
```

复制

[ubuntu | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

[debian | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)

```
sudo apt update && sudo apt full-upgrade --autoremove -y
```

### root账户与用户提权

一直用root操作会有系统损坏风险，最好设立账户然后提权
-  可以无限设置密码
```powershell
wsl -d Arch -u root
```
- 切换账户
```
su root 
cp /etc/sudoers ~/sudoers
sudo cp ~/sudoers /etc/sudoers
```


> [!note]
>
> 这是 `/etc/sudoers` 文件的内容，它控制用户和用户组使用 `sudo` 命令时的权限。以下是内容的中文解释：
1. **Root 用户权限:**
   
   ```
   root ALL=(ALL:ALL) ALL
   ```
   这行表示 `root` 用户拥有全系统的最高权限，可以以任何用户或用户组身份在任何主机上执行任何命令。
   
2. **Wheel 组:**
   ```
   # %wheel ALL=(ALL:ALL) ALL
   ```
   `wheel` 组通常用于控制具有管理权限的用户。取消注释这行后，`wheel` 组的所有成员将可以通过 `sudo` 执行任何命令。

3. **Wheel 组免密码:**
   ```
   # %wheel ALL=(ALL:ALL) NOPASSWD: ALL
   ```
   取消注释这行后，`wheel` 组的用户可以在使用 `sudo` 时不输入密码，直接执行命令。
   

**一般注释Wheel 就可以让所有用户都有sudo权限**

### debian配置zsh终端

- 首先安装zsh

```
sudo apt install zsh
zsh --version
chsh -s $(which zsh) # 将现在登陆的用户设置为默认shell
# 注销，重新登陆后查看是否切换为zsh
echo $SHELL 
```

- 接着使用oh-my-zsh管理插件

  ```
  sh -c "$(wget -O- https://install.ohmyz.sh)"
  ```

  下载并配置插件和主题

  powerlevel10k,zsh -autosuggestions,zsh-syntax-highlighting

  ```
  git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```

[Ubuntu定制之换源 + VScode + JetBrains-Mono + zsh + powerlevel10k - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/537054661)

- zsh 设置

  一些插件需要设置才能启动

  ```
  nano .zshrc
  ```

  进入`.zshrc`，用如下命令启用插件

  ```
  # >>> Oh My Zsh 设置 <<<
  export ZSH="$HOME/.oh-my-zsh"
  source $ZSH/oh-my-zsh.sh
  # OMZ 会自动从插件目录加载，否则需要手动 source 它们
  ZSH_THEME="powerlevel10k/powerlevel10k"
  plugins=(
    git
    zsh-autosuggestions
    zsh-syntax-highlighting
  )
  
  # 自动建议插件的配置（放在 plugins= 之后）
  ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=cyan'
  ZSH_AUTOSUGGEST_STRATEGY=(match_prev_cmd completion)
  ZSH_AUTOSUGGEST_BUFFER_MAX_SIZE=20
  ```

  然后刷新，并重启终端

  ```
  source ~/.zshrc
  ```

- 配置powerlevel10k主题。如果你想再次重启配置向导，运行以下程序。你可以随心所欲地做，次数不限。

  ```
  p10k configure
  ```

- zsh更新

```
omz update
```

## Arch Linux

作为Linux发行版原神，以折腾为名，虽然老大仍然是debian系，声量在社区中不断增长，archwiki也很完善
https://zhuanlan.zhihu.com/p/613738433

### 联网测试

```
ping -c 4 1.1.1.1
```

### 安装appx

https://github.com/yuk7/ArchWSL

**20250501起arch官方支持了archlinux**

https://wiki.archlinux.org/title/Install_Arch_Linux_on_WSL

#### 创建账户

开始默认是root账户，wsl的名字是archlinux。创建sakura到wheel用户组

```
useradd -m -G wheel -s /bin/bash sakura
passwd sakura
```

安装sudo，配置权限

（安装rust版本的sudohttps://github.com/trifectatechfoundation/sudo-rs)

```
pacman -S sudo
sudo pacman -S nano
```

然后

```
nano visudo
```

找到这行并取消注释，保存退出

```
# %wheel ALL=(ALL:ALL) ALL
```

测试是否成功

```
su - sakura
sudo ls /
```

最后更换wsl默认账户

```
wsl --manage archlinux --set-default-user sakura
```

#### 设置UTF8

`nano` 和其他终端程序依赖正确的 locale 来处理字符宽度（尤其是中文是双字节）

运行

```
locale
```

出现`locale: Cannot set LC_XXX to default locale: No such file or directory`表示 `en_US.UTF-8` 的 locale **没有被生成**，需要你手动启用并生成。

编辑 `/etc/locale.gen`，取消对 `en_US.UTF-8` 的注释（删除前面的 `#`）

然后，执行生成 locale

```
sudo locale-gen
```

验证设置

```
locale
```

没有报错

#### 更换位置

```
wsl --export archlinux e:\archlinux.tar
wsl --unregister archlinux
wsl --import archlinux e:\archlinux e:\archlinux.tar --version 2
wsl --set-default archlinux 
wsl --manage archlinux --set-default-user sakura
del e:\archlinux.tar
```



### 清华源（**也要tun模式才能连网络**）

### nano 编辑完成后，ctrl s ，然后enter确定保存

方法1：sudo提权为root操作

```
sudo nano /etc/pacman.d/mirrorlist
# 编辑完保存，然后enter确认
```

方法2：

```
# 复制到主目录
cp /etc/pacman.d/mirrorlist ~/mirrorlist

# 用 VS Code 编辑（无 sudo）
code ~/mirrorlist

# 编辑完成后替换回原文件
sudo cp ~/mirrorlist /etc/pacman.d/mirrorlist
```

https://mirrors.tuna.tsinghua.edu.cn/help/archlinux/

#### archlinuxcn

Arch Linux 中文社区仓库 是由 Arch Linux 中文社区驱动的非官方用户仓库。包含中文用户常用软件、工具、字体/美化包等。

https://mirrors.tuna.tsinghua.edu.cn/help/archlinuxcn/

编辑完后更新GPG key

```
sudo pacman-key --lsign-key "farseerfc@archlinux.org"
sudo pacman -Syyu
```

#### yay接管包管理

```
sudo pacman -S yay
```

```
yay -Sy archlinux-keyring && yay

yay -S fastfetch
yay -Rsu fastfetch #删除软件包及其依赖项
yay -Sc # 清除软件缓存，即 /var/cache/pacman/pkg 目录下的文件
yay -Syyu #更新系统上的所有软件包，包括更新系统
yay -Qqe #列出所有显式安装（-e,explicitly显式安装；-n忽略外部包AU
yay 软件包名或搜索词 _# 从仓库和 AUR 中交互式搜索和安装软件包。_  
yay -Ps _# 显示已安装软件包和系统健康状况的统计数据。_
```

#### 使用yay安装zsh

安装前提：配置archlinun源
yay自动cmake构建github的包并融入到包体系中，之后安装zsh-theme-powerlevel10k-git

```
yay -S zsh zsh-theme-powerlevel10k-git zsh-autosuggestions zsh-syntax-highlighting
```
注：在arch不建议yay` + `oh-my-zsh混用，这样插件路径不一致，容易出错、不加载、难查错，最不推荐。使用的 `yay + zsh` 配置（不使用 oh-my-zsh）加载插件

- 设置 Zsh 为默认 Shell，并运行zsh创建空白~/.zshrc

```
chsh -s /bin/zsh
exec zsh
```

- 刷新配置

```
source ~/.zshrc
```

使用如下文件配置.zshrc

```
# >>> Powerlevel10k instant prompt（必须靠前） <<<
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# >>> vscode shell 集成 <<<
[[ "$TERM_PROGRAM" == "vscode" ]] && . "$(code --locate-shell-integration-path zsh)"

# >>> 基本设置 <<<
export LANG=en_US.UTF-8
export EDITOR=nano

setopt HIST_IGNORE_DUPS
setopt SHARE_HISTORY

HISTSIZE=10000
SAVEHIST=10000
HISTFILE=~/.zsh_history

# 启动补全系统
autoload -Uz compinit && compinit

# >>> 自动建议插件 <<<
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=cyan'
ZSH_AUTOSUGGEST_STRATEGY=(match_prev_cmd completion)
ZSH_AUTOSUGGEST_BUFFER_MAX_SIZE=20

# >>> Powerlevel10k 主题 <<<
# 如果你不希望显示 git 状态信息可以保留下面两行
POWERLEVEL9K_DISABLE_GITSTATUS=true
POWERLEVEL10K_DISABLE_GITSTATUS=true

source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme

# 如果有个性化配置
[[ -f ~/.p10k.zsh ]] && source ~/.p10k.zsh

# >>> 语法高亮插件（务必最后加载） <<<
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# >>> Micromamba 初始化 <<<
export MAMBA_EXE='/usr/bin/micromamba'
export MAMBA_ROOT_PREFIX='/home/sakura/.local/share/mamba'
__mamba_setup="$("$MAMBA_EXE" shell hook --shell zsh --root-prefix "$MAMBA_ROOT_PREFIX" 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__mamba_setup"
else
    alias micromamba="$MAMBA_EXE"  # Fallback on help from micromamba activate
fi
unset __mamba_setup

# 替换 conda 命令（以 micromamba 执行）
alias conda='micromamba'
alias ca='conda activate'

# >>> 解压函数 extract 与别名 <<<
extract () {
  if [ -f "$1" ] ; then
    case "$1" in
      *.tar.bz2)   tar xjf "$1"    ;;
      *.tar.gz)    tar xzf "$1"    ;;
      *.bz2)       bunzip2 "$1"    ;;
      *.rar)       unrar x "$1"    ;;
      *.gz)        gunzip "$1"     ;;
      *.tar)       tar xf "$1"     ;;
      *.tbz2)      tar xjf "$1"    ;;
      *.tgz)       tar xzf "$1"    ;;
      *.zip)       unzip "$1"      ;;
      *.Z)         uncompress "$1" ;;
      *.7z)        7z x "$1"       ;;
      *)           echo "'$1' cannot be extracted via extract()" ;;
    esac
  else
    echo "'$1' is not a valid file"
  fi
}
alias x='extract'
```

- 重新配置p10k

如果你想再次重启配置向导，运行以下程序。你可以随心所欲地做，次数不限。

```text
p10k configure
```

#### yay安装mamba

```
yay micromamba-bin # 安装aur版本，新版本更新及时
micromamba shell init -s zsh
```

这样其实vscode也能识别创造出来的环境

```
conda create -n mnist python=3.12
```

调出vscode命令，然后select python interpreter就可以选择了、

自更新mamba

```
sudo MAMBA_ROOT_PREFIX=/home/sakura/.local/share/mamba micromamba self-update
```

#### maple中文字体

https://font.subf.dev/zh-cn/download/

```
yay -S ttf-maplemono-nf-cn-unhinted
```

#### fastfetch

```
yay fastfetch
```

## zsh 按tab显示补全提示，按 →直接补全所显示的

## Ubuntu清理

[ubuntu清理空间技巧 包含【系统日志、缓存、无用包、内核、VScode、conda、snap、pip】_sudo apt autoremove --purge snapd-CSDN博客](https://blog.csdn.net/m0_50181189/article/details/119855107)

## linux命令(联网要clash tun模式)

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
sudo apt autoclean                # 删除旧版本软件缓存
sudo apt clean                    # 删除系统内所有软件缓存
sudo apt autoremove             # 删除系统不再使用的孤立软件
```
## 刷新终端(安装完新软件后要刷新)

```
sudo apt update
source ~/.bashrc
source ~/.zshrc
```
## 软件安装,常用命令

查看占用

```
du -sh /home/sakura/.local/share/mamba/envs
du -h --max-depth=2 ~ | sort -hr
```

删除python相关cache

```
rm -rf ~/.cache/pip ~/.cache/uv
```



### apt 安装

```bash
sudo apt search package #搜索包 
sudo apt show package #获取包的相关信息，如说明、大小、版本等  
sudo apt depends package #了解使用依赖  
sudo apt rdepends package #查看该包被哪些包依赖  
sudo apt-cache pkgnames  #执行pkgnames子命令列出当前所有可用的软件包 
sudo apt policy package #使用policy命令显示软件包的安装状态和版本信息。
sudo apt install package #安装包  
sudo apt install package=version #安装指定版本的包  
sudo apt install package --reinstall #重新安装包  
sudo apt -f install #修复安装, "-f = --fix-missing"  
sudo apt remove package #删除包
sudo apt purge package  #删除包，包括删除配置文件等
sudo apt autoremove #自动卸载所有未使用的软件包
sudo apt source package #下载该包的源代码   
sudo apt update #更新apt软件源信息  
sudo apt upgrade #更新已安装的包
sudo apt full-upgrade #在升级软件包时自动处理依赖关系  
sudo apt dist-upgrade #升级系统  
sudo apt dselect-upgrade #使用dselect升级  
sudo apt build-dep package #安装相关的编译环境  
sudo apt clean && sudo apt autoclean #清理无用的包
sudo apt clean  #清理已下载的软件包，实际上是清楚/var/cache/apt/archives目录中的软件包
sudo apt autoclean  #删除已经卸载的软件包备份  
sudo apt-get check #检查是否有损坏的依赖 
```

### aptitude安装(更灵活)

```bash
sudo apt install aptitude # 安装
sudo apt update
sudo apt full-upgrade -y
sudo apt update    更新可用的包列表
sudo aptitude upgrade    升级可用的包
sudo aptitude dist-upgrade   将系统升级到新的发行版
sudo aptitude install pkgname    安装包
sudo aptitude remove pkgname     删除包
sudo aptitude purge pkgname  删除包及其配置文件
sudo aptitude search string  搜索包
sudo aptitude show pkgname   显示包的详细信息
sudo aptitude clean  删除下载的包文件
sudo aptitude autoclean  仅删除过期的包文件
```
### fastfetch

看linux发行版信息
```sh
wget https://github.com/fastfetch-cli/fastfetch/releases/latest/download/fastfetch-linux-amd64.deb

sudo apt install /home/sakura/fastfetch-linux-amd64.deb
```

### 编译相关

```
sudo apt install build-essential
```

### 7zip
apt的p7zip-full很方便
```sh
sudo apt install p7zip-full
```
，但上次更新是2016。用官方的7zip手动安装
```sh
sudo apt update
sudo apt install jq # 解析json要用
asset_url=$(curl -s https://api.github.com/repos/ip7z/7zip/releases/latest | jq -r '.assets[] | select(.name | endswith("linux-x64.tar.xz")) | .browser_download_url')
wget -q $asset_url -O 7z.tar.xz
tar xf 7z.tar.xz
mv 7zz /usr/local/bin
```
用7zz命令
### 解压缩与压缩
```
7z
```

## 安装miniconda

```cmd
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

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


### 卸载：
https://blog.csdn.net/Supremelv/article/details/138244275
找到uninstall_cuda
```bash
cd  /usr/local/cuda/bin
sudo ./cuda-uninstaller
```
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

## 虚拟机安装arch

1. 下载iso镜像

https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/2025.06.01/

然后clash开tun模式以便连外网

2. 进入，然后输入archinstall进行配置

```
archinstall
```

具体见https://zhuanlan.zhihu.com/p/25308291469

3. vmware tools安装

https://wiki.archlinuxcn.org/wiki/VMware/%E5%AE%89%E8%A3%85_Arch_Linux_%E4%B8%BA%E8%99%9A%E6%8B%9F%E6%9C%BA

```
sudo pacman -S open-vm-tools gtkmm3
#启动服务 
systemctl start vmtoolsd.service
systemctl start vmware-vmblock-fuse.service
#设置开机启动
systemctl enable vmtoolsd.service
systemctl enable vmware-vmblock-fuse.service
#查询服务状态
systemctl status vmtoolsd.service
systemctl status vmware-vmblock-fuse.service
```

重启虚拟机
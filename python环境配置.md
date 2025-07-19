镜像源的寻找

https://mirrorz.org/about

https://mirror.nju.edu.cn/mirrorz-help/anaconda

# python：`miniforge`

用miniforge（mamba版本的miniconda，优点在于直接使用conda-forge路径)

- 用户模式安装，不要admin。**不勾选path（？？待定）**，regist版本到ide
- 安装后激活环境：

```
conda init powershell bash
```

- 清华源配置（其实没必要，直连也能conda-forge

  先执行 `conda config --set show_channel_urls yes` 生成该文件之后再修改。配置文件在Windows: `C:\Users\<YourUserName>\.condarc`

  ```
  channels:
    - conda-forge
  show_channel_urls: true
  custom_channels:
    auto: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  conda config --set use yes
  ```

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

- 打开任意python文件，选择编译器运行
- 终端提示输入`conda init`

- 创建虚拟环境，并切换为默认环境，以方便删除与管理

```
conda create -n envbase python=3.11 -y
```

- vscode的python插件设置中，设置默认python path为`D:\Program Files\miniconda3\envs\envbase\python.exe`

## python : micromamba[now]

不产生python的base环境

https://zhuanlan.zhihu.com/p/622346839

1. 下载exe

   https://github.com/mamba-org/mamba/releases

2. 重命名并放到指定位置

   假设你想安装到：

   ```
   D:\micromamba
   ```

   - 创建该文件夹（如果还没有）
   - 把下载的 `micromamba-win-64` 文件重命名为 `micromamba.exe`
   - 放进 `D:\micromamba\` 目录

3. 添加 micromamba 所在目录到系统 PATH

   ``` powershell
   setx PATH "$env:PATH;D:\micromamba"
   ```

4. 初始化 shell

   重启powershell使得PATH生效。临时设定MAMBA_ROOT_PREFIX再激活

   ```powershell
   micromamba shell init --shell powershell --root-prefix D:\micromamba
   micromamba shell init --shell cmd.exe --root-prefix D:\micromamba
   ```

   查看是否生效

   ```powershell
   micromamba info
   ```

5. powershell设置简称

   > [!note]
   >
   > powershell配置文件位置查询
   >
   > ```powershell
   > $PROFILE
   > ```

   ```powershell
   Set-Alias conda "micromamba"
   function ca { micromamba activate $Args[0] }
   function ccn { micromamba create -n @Args }
   ```
   
   bash上
   
   ```bash
   alias ca="conda activate" 
   alias conda="micromamba" 
   alias ccn="micromamba create -n"
   ```


6. 配置ustc源

   https://help.mirrorz.org/anaconda/

   > 清华源老是限速

   首先看能读取的config路径

   ```
   micromamba info
   ```

   user config files 的地址就是它读取的地方，channels能看到配置好源时读取的ustc源地址。

   打开.mambarc，配置如下

   conda-forge: https://mirror.sjtu.edu.cn/anaconda/cloud

   ```yaml
   pkgs_dirs:
     - D:\micromamba\pkgs
   custom_channels:
     conda-forge: https://mirror.nju.edu.cn/anaconda/cloud
   use_uv: true
   proxy_servers:
     http: http://localhost:7890
   ```

   然后刷新一下即可

   ```
   micromamba clean -i
   ```

7. 配置pypi源

   https://help.mirrorz.org/pypi/

   配置conda并不能自动配置好pypi，还是要自己配置

   随意创建一个虚拟环境后，`pip config list -v`查看它能读取的配置地址

   ```bash
   For variant 'global', will try loading 'C:\ProgramData\pip\pip.ini'
   For variant 'user', will try loading 'C:\Users\Administrator\pip\pip.ini'
   For variant 'user', will try loading 'C:\Users\Administrator\AppData\Roaming\pip\pip.ini'
   For variant 'site', will try loading 'D:\micromamba\envs\myenv\pip.ini'
   ```

   以下在user层面创立编辑`pip.ini`，即`C:\Users\Administrator\pip\pip.ini`

   ```
   [global]
   index-url = https://mirrors.nju.edu.cn/pypi/simple
   trusted-host = 'mirrors.ustc.edu.cn'
   ```

   配置完成后使用如下命令看是否使用源安装

   ```
   pip config list
   pip install torch -v
   ```

8. 设置uv源

   https://help.mirrorz.org/pypi/

   https://docs.astral.sh/uv/concepts/configuration-files/#env

   设置用户级别配置文件

   ```
   %APPDATA%\uv\uv.toml
   ```

   ```
   [[index]]
   url = "https://mirrors.nju.edu.cn/pypi/simple"
   default = true
   ```

   测试

   ```
   uv pip install torch -v
   ```

> [!NOTE]
>
> LInux设置在~/.config/uv/uv.toml
>
> ```bash
> mkdir -p ~/.config/uv
> nano ~/.config/uv/uv.toml
> ```
>
> **粘贴以下内容**到文件中
>
> ```
> [[index]]
> url = "https://mirrors.zju.edu.cn/pypi/web/simple"
> default = true
> ```

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
conda clean -a -y
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
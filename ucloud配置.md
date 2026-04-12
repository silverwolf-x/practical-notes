# 服务器选择（待更新）

https://www.bing.com/search?q=vps%20%E5%8D%9A%E5%AE%A2

# ucloud服务器配置

## 更新debian基础依赖到debian13及以上

```
sudo apt update && sudo apt upgrade -y && sudo apt full-upgrade -y && sudo apt --purge autoremove -y
```

更换源(不做，防止网络基础更新导致连不上)

```
cat /etc/apt/sources.list
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak.$(date +%F)
sudo sed -i 's/\bbookworm\b/trixie/g' /etc/apt/sources.list
cat /etc/apt/sources.list
```

更新并重启

```
sudo apt update && sudo apt upgrade -y && sudo apt full-upgrade -y && sudo apt --purge autoremove -y && reboot
```

## 安装fastfetch

```
wget https://github.com/fastfetch-cli/fastfetch/releases/latest/download/fastfetch-linux-amd64.deb && sudo dpkg -i fastfetch-linux-amd64.deb
```

## windows写入 SSH 公钥（免密登录）

### 生成并上传ssh公钥

1. 在 **你自己的电脑** 上执行（powershell）：

```powershell
ssh-keygen -t ed25519 -f $HOME/.ssh/ucloud -C "silverwolf"
```

解释：

* `-t ed25519` → 使用 ed25519 算法，比 RSA 更快更安全。
* `-f ~/.ssh/silverwolf` → 指定密钥文件名，不会覆盖默认的 `id_ed25519`。
* `-C "silverwolf"` → 给密钥加个注释，方便识别。

它会生成：

* 私钥：`~/.ssh/silverwolf`（只能留在你电脑上，权限 600）
* 公钥：`~/.ssh/silverwolf.pub`（可以安全复制给服务器）

2. 开一个新powershell查看公钥内容并复制：

   ```bash
   cat $HOME/.ssh/ucloud.pub
   ```

3. 上传公钥

   ```
   ssh root@152.32.145.158
   mkdir -p ~/.ssh && chmod 700 ~/.ssh
   echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOTS6AosdrzexzDfSCRpOHUAf8jPboKX7nYmMoV9MVsV silverwolf" >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   cat ~/.ssh/authorized_keys
   ```

> [!note]
>
> 如果管理员密码变动，需要在宿主机（windows）删除ssh下的knowhost重新登陆，以重新匹配设备指纹。等价操作为
>
> ```
> ssh-keygen -R 152.32.145.158
> ```

### 服务器ssh设置

密码登陆

```
sudo nano /etc/ssh/sshd_config
```

找到相关行，修改为：

```
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2
PasswordAuthentication yes
```

> 注意：去掉 `#`，并确保 `AuthorizedKeysFile` 指向你存放公钥的文件。

保存文件后，重启 SSH 服务：

```
sudo systemctl restart ssh
```

若有更多问题，将/etc/ssh/sshd_config的内容丢给ai修改

### vscode连接

(可能需要的操作)clash用虚拟网卡模式确保能用vpn的网络登陆，加快登陆速度

左下角-打开远程窗口–连接到主机（需要vscode的remotessh插件）–配置文件–添加

```
Host ucloud
  HostName 152.32.145.158
  Port 22
  User root
  IdentityFile C:\Users\Administrator\.ssh\ucloud
```

IdentityFile的ucloud是上一步你自定义的密钥名字，Host是随便填的名字

## python环境配置

用miniforge安装condahttps://github.com/conda-forge/miniforge

## mt5安装

1. 安装 dockerhttps://docs.docker.com/engine/install/debian/

2. 我们使用这个docker镜像

   https://github.com/hpdeandrade/mt5docker安装mt5。

   ```
   mkdir -p mt5 && cd mt5
   ```

   docker-compose.yaml 创建如下

   ```yaml
   services:
     mt5docker:
       image: hpdeandrade/mt5docker:latest
       container_name: mt5docker
       environment:
         - MT5_HOST=0.0.0.0
         - VNC_PWD=hm6PZ2XL
       ports:
         - 5901:5901
         - 6081:6081
         - 8001:8001
       restart: unless-stopped
       stdin_open: true
       tty: true
   ```

   在MT5文件夹内编辑好docker-compose.yaml后

   ```
   cd mt5
   docker compose up -d
   ```

   一些操作

   ```
   # 查看日志
   docker compose logs
   # 停止 MT5 容器
   docker stop mt5docker
   # 启动mt5容器
   docker start mt5docker
   docker restart mt5docker
   # 删除 MT5 容器
   docker stop mt5docker&&docker rm mt5docker
   # 删除 mt5 镜像（谨慎！）
   docker rmi hpdeandrade/mt5:latest
   # 进入容器里的终端
   docker exec -it mt5docker bash
   # 退出
   exit
   ```

   3. 服务器设置防火墙，开放端口6081
   3. 用浏览器连接http://152.32.145.158:6081/vnc.html，打开docker内部的wine页面，完成mt5安装。连接密码在上文docker-compose.yaml里设置的


## mt5登陆与python连接

1. 右键account–开新模拟账户–找ICMarkets公司，然后连接现有交易账户填写账户密码选MT5-2服务器才能联通。联通后外汇数据会实时更新

2. 安装python313环境，安装

```
pip install pymt5linux
```

测试脚本

```
from pymt5linux import MetaTrader5
# http://152.32.145.158:6081/vnc.html
print("Establishing connection...")
mt5 = MetaTrader5(port=8001)
mt5.initialize(login=7347972, server="ICMarketsSC-MT5-2", password="904303@Fsh")
print(mt5.account_info()._asdict())
print("Closing connection...")
mt5.shutdown()
```

## github开启deploykey上传代码

1. 在Organization 总设置里开启了 Deploy Key

2. 生成 SSH Key（服务器专用）

   切换到专用服务器用户（例如 `gitserver`）：

   ```
   # 生成 SSH Key
   ssh-keygen -t ed25519 -C "cloud@leviathan-quantlab"
   ssh-keygen -t ed25519 -C "elaina"
   # 默认路径：~/.ssh/id_ed25519
   # passphrase 可留空（自动化方便）
   cat ~/.ssh/id_ed25519.pub
   ```

   ------

3. 添加 Deploy Key 到仓库

   1. 打开仓库 → **Settings → Deploy keys → Add deploy key**
   2. Title：例如 `Server SSH Key`
   3. Key：粘贴 `~/.ssh/id_ed25519.pub` 内容
   4. 如果需要写权限，勾选 **Allow write access**
   5. 保存

   > 注意：每个仓库的 Deploy Key 都是独立的，如果要推送到多个仓库，需要为每个仓库添加相应 Deploy Key。

   ------

4. 测试 SSH 连接

   ```
   ssh -T git@github.com
   # 应该显示：
   # Hi <repo>! You've successfully authenticated, but GitHub does not provide shell acc
   ```

5  用ssh来下载仓库，即git@github.com开头的链接来git clone

上传代码时提示*** Please tell me who you are.

```
git config --global user.name "cloud"
git config --global user.email "cloud@leviathan-quantlab.com"
```

## uv

### 创建虚拟环境.venv

```
uv python list # 看可用python版本
uv venv -p 3.14
```

### uv venv升级python环境

```
uv venv -p 3.13
uv pip install -r requirements.txt -U
```

## tmux on windows

### 安装

https://6xyun.cn/article/msys2 下载msys2，将在环境变量选项卡中对 `Path` 增加 `C:\msys64\mingw64\bin\` 和 `C:\msys64\usr\bin\` 目录

- 包管理器使用

```
1. 安装软件包
   pacman -S package_name：安装指定的软件包。
   pacman -Sy：自动更新系统并安装所有可升级的软件包。
   pacman -Syu：自动更新系统、升级所有可升级的软件包以及安装新添加的软件源中的软件包。

2. 删除软件包
   pacman -R package_name：删除指定的软件包及其配置文件。
   pacman -Rn package_name：仅删除指定的软件包，保留配置文件。
   pacman -Rsc：删除系统中不再需要的所有软件包及其配置文件。

3. 搜索软件包
   pacman -Qs package_name：搜索系统中是否安装了指定的软件包。
   pacman -Ql package_name：列出系统中已安装的指定软件包的详细信息。

4. 查询软件包信息
   pacman -Si package_name：显示指定软件包的详细信息，包括版本号、依赖关系等。
   pacman -Fl package_name：显示指定软件包的文件列表。

5. 清理缓存和下载的软件包
   pacman -Sc：清理Pacman缓存，即清除本地数据库中的无效或过期的软件包信息。
   pacman -U download_directory/*.pkg.tar.xz：从指定的目录中安装下载的软件包。

6. 查看系统信息
   pacman -Q：显示系统中已安装的所有软件包及其状态。
   pacman -Qe：显示系统中可用的所有扩展和内核模块。
   pacman -Qi：显示系统中所有已安装的软件包的详细信息。

7. 其他常用命令
   pacman -H：显示帮助信息，列出所有可用的Pacman命令及其用法。
   pacman -T：检查系统中是否有未满足的软件包依赖关系。
   pacman -Sdd：删除系统中所有不再需要的软件包及其配置文件，同时进行磁盘空间清理。
```

```
pacman -S tmux
```

### 使用

```bash
tmux new -s book 'python book_data.py' # 新建会话并进入
tmux detach 或 Ctrl + B, D # 仅“分离”当前会话，不删除
tmux ls # 列出所有会话
tmux attach -t book # 重新连接到名为 book 的会话
tmux kill-session -a -t book # 删除除 book 会话外的所有其他会话
tmux kill-session -t book # 删除指定名称的会话
```
```bash
tmux new -s book 'python book_data.py'
tmux new -s upload 'python upload_data.py'
```

## ssh on windows

```
# 安装 OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
# 启动服务
Start-Service sshd
# 设置开机自启
Set-Service -Name sshd -StartupType 'Automatic'
```

这样就能连接了

密钥设置：`C:\ProgramData\ssh`里面设置sshd_config

```
PubkeyAuthentication yes
```

编辑后，重启 SSH 服务

`Restart-Service sshd`

```
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOTS6AosdrzexzDfSCRpOHUAf8jPboKX7nYmMoV9MVsV silverwolf" >> .ssh/authorized_keys
```

测试

```
ssh -i $HOME\.ssh\ucloud Administrator@152.32.144.58 -v
```

## windows服务器设置

### 取消开机弹出服务器管理器

1.点击右上角的“**管理**”—“**服务器管理器属性**”如下图所示

2.在弹出的选项卡内，把“**在登录时不自动启动服务器管理器**”前面的对钩打上，确定。

### 停用Windows Defender Antivirus Service，释放CPU和内存

可自行搜索Defender Control下载

https://defendercontrol.app/#download

## 激活windows

https://github.com/massgravel/Microsoft-Activation-Scripts?tab=readme-ov-file#download--how-to-use-it

有可能会确实组件导致软件无法安装如git或者重启报错，需要重装系统

## windows停止更新

https://github.com/Hudrig0/Windows-Update-Blocker

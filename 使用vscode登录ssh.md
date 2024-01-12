## 下载openssh

[Releases · PowerShell/Win32-OpenSSH (github.com)](https://github.com/PowerShell/Win32-OpenSSH/releases)

直接下载`OpenSSH-Win64.msi`，双击就会自动安装到`C:\Program Files\OpenSSH`

验证：win+R cmd 

```cmd
C:\Users\Administrator\Desktop> ssh
```

## vscode安装插件remote-ssh

安装完成后，vscode 左下角<u>打开远程窗口</u>--><u>连接到主机remote-ssh</u> --> <u>添加新的主机</u>

### 远程连接ip地址

> **ssh [用户名]@[ip地址(:端口)]**

一般而言，用户名为root，端口默认22。例如

```
ssh liver@61.139.65.141:50144
```

然后<u>更新配置</u>，一般保存在`C:\Users\用户名\.ssh\config`，会变成这样

```config
Host 61.139.65.141
 HostName 61.139.65.141
 Port 50144
 User liver
```

> **Host**: 取任意名字
> **HostName**： 这个是真实的域名地址
> **Port**：端口
> **IdentityFile**：这里是id_rsa的地址（如果需要使用ssh公钥/私钥，需要额外链接本地文件）
> **User**：配置使用用户名

例如本例中需要使用密钥，因此最后配置文件应该要更新为

```
Host 61.139.65.141
  HostName 61.139.65.141
  Port 50144
  User liver
  IdentityFile C:\Users\Administrator\.ssh\liver_73_116_id_rsa
```

之后，再次vscode 左下角<u>打开远程窗口</u>--><u>连接到主机remote-ssh</u>，就能选到对应ip名字了

### 创建公钥与连接私钥
理论上，公钥和私钥相互唯一匹配，唯一确定。
1. 使用git创建公钥

[服务器上的 Git - 生成 SSH 公钥](https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5)

要点：默认情况下，用户的 SSH 密钥存储在其 ~/.ssh 目录下

这样就生成了唯一匹配的公钥`id_rsa.pub`和密钥`id_rsa`

2. 公钥上传至远程服务器，并配置

3. 在本地vscode连接时，对应ip的config中，IdentityFile文件为密钥文件
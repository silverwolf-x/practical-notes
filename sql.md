# 数据库相关

## 安装与使用

### navicat

下载https://www.navicat.com.cn/products/

navicat17破解补丁上github查找https://github.com/zarfadev/Navicat_Patch_v17

如果担心“winmm.dll 放置于Navicat安装目录下即可”文件报毒就用navicat16，理论上这个不是病毒

navicat16和破解https://github.lijunyi.xyz/blogs/app/2022/NavicatPremium16.html、

navicat160链接：https://pan.baidu.com/s/1nze8LTrsRNxHi_zM1N9ukw
提取码：xxma

> 勾选 `HOSTS`会修改 `hosts` 文件，添加 `127.0.0.1 activate.navicat.com` 达到屏蔽 `Navicat16` 激活联网
>
> 如果没有添加成功，请自行前往 `C:\Windows\System32\drivers\etc` 手动在 `HOSTS` 文件中添加！！！
>
> ------
>
> 著作权归lijunyi所有 原文链接：https://lijunyi.xyz/blogs/app/2022/NavicatPremium16.html

如果要使用AI功能，可能需要在高级-设置代理

## sql server安装

如果要在本地创建新的sql server程序，就要安装整个微软的大包

### 本地安装（不推荐，有卸载残留难以安装）

- 安装包下载

https://sysin.org/blog/sql-server/来寻找百度网盘安装包，避免官网渠道的注册

- 安装 SQL Server 时提示：

  SQL Server 安装程序遇到以下错误:

  安装程序在运行作业 UpdateResult 时失败。

  错误代码 0x876E0003。

  开启 Windows 自动更新或关闭自动更新、不勾选 “使用 Microsoft Update 检查更新（推荐）” 均无法继续安装。

  解决方法：

  复制iso到新文件夹，因为iso是只读不可编辑

  手动创建 DefaultSetup.ini 放置到安装程序文件夹里的 x64 或者 x86 目录中

  ```ini
  [OPTIONS]
  UpdateEnabled=0
  IAcceptSQLServerLicenseTerms=1
  ```

- 安装指南https://blog.csdn.net/qq_41188880/article/details/105799584

  只勾选最基础的数据库引擎服务即可

- **选择混合模式，然后添加当前用户**

  ![](https://i-blog.csdnimg.cn/blog_migrate/ea94d97991d585bb945f2b5474fe5ec9.png)

- 在`SQL server配置管理器`中，打开`SQL server服务`；并在`SQL server网络配置`中，打开`TCP/IP配置`

> 否则会报错提示`DB-Lib error message 20009, severity 9`

### docker部署sql server

- 安装windows docker

- powershell使用命令

  ```powershell
  docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=YourStrong@Passw0rd" `
    -p 1433:1433 `
    --name my-sqlserver `
    -v D:\DockerData\SQLserver\data:/var/opt/mssql/data `
    -v D:\DockerData\SQLserver\log:/var/opt/mssql/log `
    -v D:\DockerData\SQLserver\backup:/var/opt/mssql/backup `
    -d rapidfort/microsoft-sql-server-2019-ib
  ```

  MSSQL_SA_PASSWORD密码中组合使用字母、数字和符号字符。

  这里挂载D:\DockerData\SQLserver为/var/opt/mssql，方便读入tbl等数据传输

- 连接：localhost，使用sa登陆即可

### 完全卸载

最主要是卸载SQL Swever 202x这个应用，会弹出来删除程序

详细参考https://zhuanlan.zhihu.com/p/690845748

## sqlserver连接

### navicat连接本地sql server(选择windows验证)

- 新建sql server链接，选择windows验证（什么都不用填，或者可以设置连接名作为本地视图名称
- 新建数据库：
  1. 右键这个连接，新建查询/运行sql文件
  2. 将创建数据库的.sql文件复制过来，点击运行。示例文件https://github.com/silverwolf-x/movie-management-system/blob/master/movie_init.sql
  3. 刷新一下就能看到了

### navicat连接远程sql server

其中sqlserver的程序安装包已经内置在安装地址下的msodbcsql_64.msi。

若连接远程sql server，只需要安装这个msi就可以，然后在新建链接输入远程服务器主机即可

> [!NOTE]
>
> [IM002] [Microsoft][ODBC 驱动程序管理器] 未发现数据源名称并且未指定默认驱动程序 (0)
>
> 可能是因为odbc版本太低，进入https://learn.microsoft.com/zh-cn/sql/connect/odbc/download-odbc-driver-for-sql-server
>
> 下载最新的Microsoft ODBC Driver大版本，然后在连接-高级选择这个版本
>
> 出现过Microsoft ODBC Driver 17连接不上docker的sql server而Microsoft ODBC Driver 18可以



## mySQL

### 安装驱动服务

安装参考https://blog.csdn.net/m0_52559040/article/details/121843945

- 截止mySQL8.0安装器，最后一步启动服务上会卡住！请参照下面的reddit的讨论指引安装https://stackoverflow.com/questions/26970454/mysql-configuration-stops-at-starting-server

  > 1. Next -> Execute, Installation hangs on Starting server, so wait for a while to time out (or don't, your choice). When Dialog (might be covered with other windows) popup with message "Configuration of MySQL Server is taking longer than expected..., here click OK (so to wait longer)
  >    Next -> Execute，安装在启动服务器上挂起，因此请等待一段时间以超时（或不超时，您的选择）。当对话框（可能被其他窗口覆盖）弹出消息“MySQL Server 的配置花费的时间比预期的要长...”时，请单击“确定”（因此等待更长的时间）
  > 2. Meanwhile **go to Start -> Control Panel -> Administrative Tools -> Services -> find MySQL56, right click on it -> Properties -> select Log On Tab AND HERE IS BUG -> Although Local System Account was selected, Somehow "This account: Network Service (with some password) was selected -> Select Log on as: Local System Account, Allow service to interact with desktop -> Apply -> Go back on general tab**
  >    同时转到开始 -> 控制面板 -> 管理工具 -> 服务 ->找到 MySQL56，右键单击它 -> 属性 ->选择登录选项卡 这里有错误 -> 虽然选择了本地系统帐户，但不知何故“此帐户：网络服务（带有一些密码）被选中了 -> 选择登录方式：本地系统帐户，允许服务与桌面交互 -> 应用 -> 返回常规选项卡
  > 3. On general tab click on "Start" button to start service and here it is! Service is started! Click on OK to close MySQL56 Properties dialog. Close Services dialog. Close Administrative tools. Close control panel.
  >    在常规选项卡上，单击“开始”按钮开始服务，就在这里！服务开始！单击“确定”关闭 MySQL56 属性对话框。关闭服务对话框。关闭管理工具。关闭控制面板。
  > 4. And by that time (while you were closing those dialogs) when you look at MySQl Installer Dialog all steps are finished and checked: Starting Server, Applying security... Creating user accounts.. Updating Start menu link
  >    到那时（当您关闭这些对话框时），当您查看 MySQl 安装程序对话框时，所有步骤都已完成并检查：启动服务器、应用安全性...创建用户帐户..更新“开始”菜单链接
  > 5. Confirm with Finish -> Next -> Finish
  >    确认完成 -> 下一个 ->完成

- 如果上述过程中断，也能通过自己启用mysql服务来使用，此时设置密码失效，密码为空。如果上述过程顺利完成，密码就是设置的密码

- 修改密码：

  1. navicat的连接右键–命令行界面

  2. 发送

     ```mysql
     ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword123!';
     FLUSH PRIVILEGES;
     ```

     其中NewPassword123!是新密码

  3. 刷新就要重新输入新密码连接了

### 在 MySQL 中导入数据

- 配置 secure_file_priv

`secure_file_priv` 变量会限制 MySQL 的文件操作（比如导入或导出数据）只能在指定的目录下进行。这是为了安全，确保像 `LOAD DATA INFILE` 或 `SELECT ... INTO OUTFILE` 这样的敏感操作只能在受控目录中执行。

- 禁用 `secure_file_priv`

如果想完全取消限制，可以设置`C:\ProgramData\MySQL\MySQL Server 8.0\my.ini`为空值：

```
[mysqld]
secure-file-priv = ""
```

重启 MySQL 后验证：

```
SHOW VARIABLES LIKE 'secure_file_priv'
```

### docker部署

1. pull mysql的docker

2. ```
   docker run -d `
     --name my-mysql `
     -e MYSQL_ROOT_PASSWORD=YourStrongPass123 `
     -p 3306:3306 `
     -v D:\DockerData\MySQLData:/var/lib/mysql `
     -v D:\DockerData\MySQLFiles:/var/lib/mysql-files `
     mysql
   ```

3. 由于secure_file_priv默认在/var/lib/mysql-files/，如果要导入tbl数据，就需要放在对应windows映射位置的相对路径。如D:\DockerData\MySQLFiles:/var/lib/mysql-files 意思是/var/lib/mysql-files映射到D:\DockerData\MySQLFiles
4. /var/lib/mysql 是存储mysql数据的位置，已经双向映射到D:\DockerData\MySQLData。按照同一路径删除部署mysql数据也不会丢失

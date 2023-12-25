---
title: 安装Windows应用msix
---

# 安装Windows应用msix

参考：[Winget 、msix、msixbundle 功能安装 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/632626404#:~:text=解决无法双击安装 msix、msixbundle 软件包问题 下载 Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle,安装 Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle 找到下载文件夹，按住 shift 键，右键-选择“在此处打开Powershell窗口”%2C复制粘贴如下命令回车即可安装 add-appxpackage.Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle)

> 解决无法将“winget”项识别为 cmdlet、函数、脚本文件或可运行程序的名称的问题
> 解决无法双击安装 msix、msixbundle 软件包问题

[应用安装程序 - Microsoft Apps](https://apps.microsoft.com/detail/9NBLGGH4NNS1?rtc=1&hl=zh-cn&gl=CN)

GitHub源[Releases · microsoft/winget-cli (github.com)](https://github.com/microsoft/winget-cli/releases)

用第三方[Microsoft Store - Generation Project](https://store.rg-adguard.net/)下载更快

找到下载文件夹，按住 shift 键，右键-选择“在此处打开Powershell窗口”,复制粘贴如下命令回车即可安装

```text
add-appxpackage .\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle
```

> 如果安装失败，用geek--查看--Microsoft Apps卸载现有的app installer

## 方法2：命令行

以管理员身份运行PowerShell，然后在Windows PowerShell界面中键入如下格式的命令

```
Add-AppxPackage -Path "C:\Path\to\File.Appx"
```


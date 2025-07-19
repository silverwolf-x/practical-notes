使用LTSC版本

## 永久激活windows和office

HEU_KMS_Activatorhttps://github.com/zbezj/HEU_KMS_Activator/releases

联网，打开软件后直接开始，它会自动找寻最优的激活方式

## windows defender关闭

https://iknow.lenovo.com.cn/spider/detail/kd/181036

## MSIX,winget

安装winget(appinstaller)

https://github.com/microsoft/winget-cli

下载msixbundle后，打开powershell,

```
add-appxpackage msixbundle地址
```

## 优化软件

**Windows11轻松设置**https://www.bilibili.com/opus/904672369138729017，禁用windows更新

# EXCEL2024打开第二个excel慢

https://www.cnblogs.com/fanqisoft/p/18473900

在打开第一个Excel文档后，再打开第二个Excel文档时，Excel 会启动一个单独的 Excel 进程，启动完成后再与第一个进程进行合并操作，而不是直接使用第一个Excel进程，所以会造成打开第二个Excel文件卡顿。

解决：注册表禁用DDE

1. 打开注册表编辑器。
2. 导航至以下键：
   `HKEY_CLASSES_ROOT\Excel.Sheet.12\shell\Open\ddeexec`
   `HKEY_CLASSES_ROOT\Excel.Sheet.8\shell\Open\ddeexec`
3. 将它们其中默认项的值更改为如下内容
   `[open("%1" /ou "%u")]`
4. 关闭注册表编辑器。
5. 重新启动 Excel。

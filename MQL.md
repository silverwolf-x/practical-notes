# MQL使用

## MetaTrader5

工具-选项–图标，把最大数据量调成unlimited来下载无穷数据

如运行时需要下载数据但很慢，设置代理服务器，如果代理拦截不了就tun模式然后关闭mt5的代理，让tun拦截

回测下数据时选择新加坡等靠近CN延迟低的地方并切换vpn为新加坡，加快下载速度

## vscode配置mql5插件

安装插件https://github.com/L-I-V/MQL-Tools遵循教程配置如下：

只在MQL5文件夹下运行这个插件，因为它自动向上级寻找metatrader编译器

在左侧的“MQL”按钮中配置Complication为AVX512，其它不用理会，然后Commands在这个工作区生成create configure

这样一来就享受了clang-format的代码提示和跳转

## vscode日期被clang格式化导致报错

前后加注释即可让格式化跳过几行

```cpp
// clang-format off
input datetime InpTrainStartDate = D'2023.01.01 00:00:00';
// clang-format on
```

### 拿更久远的Tick数据

截止20251214，python拿数据等价与mq5的OnStart脚本，只能拿2025年的TIck数据

如果要2023年的，需要单独写EA在OnTick拿数据，并写入SQLite。一个品种3年大概耗时2分钟，数据大小为4GB左右。现在暂时只能在回测设置中设置一个品种和时间来进入OnTick，暂时不知道如何多品种同步下载

EA运行完的附件在Tester/3000里面，要及时转存不然下次EA回测会覆盖

sqlite的db文件之后可以考虑用duckdb读取借助polars转存分块parquet

## 邮箱提醒

SMTP 主机：smtp.gmail.com:587
端口：587
用户名：a0910742982@gmail.com
密码：刚刚生成的 16 位应用专用密码，在google设置里面设置专用密码
anbfspijncqxfmzh

然后在google邮箱设置中设置过滤器，设置拦截规则，并设置转发邮箱

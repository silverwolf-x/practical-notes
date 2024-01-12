# Stable Diffusion一键整合包使用记录

基于[【AI绘画·11月最新】Stable Diffusion整合包v4.5发布！全新加速 解压即用 防爆显存 三分钟入门AI绘画 ☆更新 ☆训练 ☆汉化 秋叶整合包_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1iM4y1y7oA/)

## python环境重装

如果安装了冗余插件，删除插件只会删除插件自身，而不会删除新下载的Python包。因此只能通过“删除重装Python”来实现包的冗余管理。

1. 删除Python文件夹
2. 打开启动器（设置--一般设置--配置模式--高级），然后，高级选项--环境维护--原生组件管理--python--通过启动器安装--确认即可
3. 安装pytorch--最新的cuda版本
4. 一键启动，自动安装现有插件的requirement

如果启动报错，是因为有些软件requirement需要通过外网检验、需要开启启动器代理：设置--代理设置--clash对应端口--将代理应用到环境变量

> **clash对应端口**：clash主页--端口--命令行--仅复制命令--自定义，这样就能看到代理经过的http地址了，一般为http://127.0.0.1:7890

这一套流程下载完后，python在`.ext`文件夹中，可以手动改名为`python`，重启启动器即可。此时安装的是miniconda管理的python。

这样下来，一套纯净的python环境占用约6.33 GB。

## 整合包更新

[【AI绘画】换整合包/自部署WebUI如何搬家设置与模型？设置、预设文件搬迁 - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv24389699/)

![img](https://i0.hdslb.com/bfs/article/794bc69b14214b6267d09cd2ed2ed20af4a04e34.png@1256w_824h_!web-article-pic.webp)

![img](https://i0.hdslb.com/bfs/article/watermark/1a8d7575cb0ab7e429bdd678d32fb222e5d129cb.png@1256w_1222h_!web-article-pic.webp)

## webui设置

**记得操作后保存设置+重载UI**

- 采样方法参数：全部隐藏，只剩下Eular a和DPM++ 2M Karras。基本就用Eular a

- 先在生成页面设置一下我希望的默认样子（如生成图大小为1024*1024），然后，设置--未分类--默认设置--查看更改

## 三视图

```
1girl, simple background, (white background:1.5), multiple views, <lora:charturnbetalora:0.4>, 
```

图生图中，controlnet模型选用control_v11p_sd15_openpose_fp16 [73c2b67d]，预处理器"无"

[【AI绘画】人设自由！人设三视图一键生成 ControlNet实战教程 Stable Diffusion_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1gk4y1h7xF/)
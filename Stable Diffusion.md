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

**sd-webui-prompt-all-in-one**

如果你想备份你的数据，可以将 `storage` 目录复制到其他地方，然后在需要的时候将其复制回来

**记得移outputs**!!!!

还有controlnet模型

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

## 可能的错误

### 1

[stable diffusion更新到1.6.0后wd14-tagger插件报错的解决方案 - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv27758222/)

> 'modules.shared' (most likely due to a circular import) (E:\sd-webui-aki-v4.3\modules\shared.py)

tagger\ui.py

```
# from webui import wrap_gradio_gpu_call
from modules.call_queue import wrap_gradio_gpu_call
```

preload.py

```
# from modules.shared import models_path
from modules import paths

# default_ddp_path = Path(models_path, 'deepdanbooru')
default_ddp_path = Path(paths.models_path, 'deepdanbooru')
```

### 2

> winFileNotFoundError: [WinError 3] 系统找不到指定的路径。: 'D:\\sd-webui-aki-v4.6.1\\localizations'

切换sd版本试一下，之后会下载一些组件。最后切换为最新版本即可



### 3 显存不够？

实测：用xformers flash attention，可以用heartOfAppleXL_v20模型，画1024*1024的图。有点刚刚够可能不要太多promote?

 torch: 2.1.2+cu118  •  xformers: 0.0.23.post1+cu118  



### 4 下载速度

有时候，用全局模式t能接管某些包的下载速度（从500k上升到3M不等的速度），再不行就用tun模式，即使已经设置了代理。

如果出现网速问题或者sha码报错，切换节点再试即可

### 5 fp8

[Kohaku-XL gamma - rev2 | Stable Diffusion Checkpoint | Civitai](https://civitai.com/models/270291/kohaku-xl-gamma)

需要在requirements_versions.txt中，更改requirements_versions.txt，否则会重装
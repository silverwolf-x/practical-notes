---
date: 2026-04-12
title: 迁移笔记本时各笔记有效性更新
---

## python: uv

使用uv不用conda类，原因：2026年uv公认是最好的pip管理工具

https://docs.astral.sh/uv/getting-started/installation/

### uv config

目标：各缓存包都和大多数代码所在盘（E盘）一致，这样pip包就直接从cache拿硬链接就行

直接用3.14及以上，talib的处理用https://github.com/cgohlke/talib-build/releases/tag/v0.6.8

> [!NOTE]
>
> 用github挂载pypi的自己编译仓库
>
> https://chairc.cn/2025/04/04/%E5%A6%82%E4%BD%95%E7%BC%96%E5%86%99Github-Action%E5%92%8Csetup-py%E5%B9%B6%E5%AE%9E%E7%8E%B0%E8%87%AA%E5%8A%A8%E5%8C%96%E4%B8%8A%E4%BC%A0PyPI/

在系统环境中设置`UV_CACHE_DIR`为`D:\uv-cache`

### uv venv

```
uv venv -p
```

升级minor版本，截止20251101

> Upgrades versions to the latest supported patch release. Requires the `python-upgrade` preview feature.
>
> https://docs.astral.sh/uv/reference/cli/#uv-python-upgrade

```
uv venv -p 3.13 --allow-existing
```

### 其它

不设置清华源，连vpn下载包

```
uv pip install torch --torch-backend=auto

git+https://github.com/kernc/backtesting.py.git

--extra-index-url http://pypi.vnpy.com/simple/
TA-Lib; sys_platform != "linux" 
```

### jupyter

一般主动`uv pip install ipykernel`

然后在vscode打开jupyter

## latex: miktex

安装地址：https://miktex.org/download

中文文章的一般用ctex系列，例如

```latex
\documentclass[UTF8,11pt]{ctexart}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{bm}        % bold math symbols
\usepackage{booktabs}  % three-rule tables
\usepackage{graphicx}  % figures
\usepackage{float}     % float placement
\usepackage{geometry}
\geometry{margin=1in}
```

### vscode配置

插件：latexworkshop，建议只在指定工作区使用

编译一般使用xelatex即可，快速兼容性强。搭配latexmk以编译两次让pdf正常得到超链接

```json
 "latex-workshop.latex.tools": [
    {
      "name": "XeLaTeXmk",
      "command": "latexmk",
      "args": [
        "-xelatex",
        "-synctex=1",
        "-shell-escape",
        "-interaction=nonstopmode",
        "-file-line-error",
        "%DOC%"
      ]
    },
  ],
  "latex-workshop.latex.recipes": [
    {
      "name": "XeLaTeXmk",
      "tools": [
        "XeLaTeXmk",
      ]
    },
  ],
  "latex-workshop.latex.autoClean.run": "onSucceeded",
"latex-workshop.latex.clean.method": "glob",
"latex-workshop.latex.clean.fileTypes": [
    "%DOCFILE%.aux",
    "%DOCFILE%.bbl",
    "%DOCFILE%.blg",
    "%DOCFILE%.idx",
    "%DOCFILE%.ind",
    "%DOCFILE%.lof",
    "%DOCFILE%.lot",
    "%DOCFILE%.out",
    "%DOCFILE%.toc",
    "%DOCFILE%.acn",
    "%DOCFILE%.acr",
    "%DOCFILE%.alg",
    "%DOCFILE%.glg",
    "%DOCFILE%.glo",
    "%DOCFILE%.gls",
    "%DOCFILE%.fls",
    "%DOCFILE%.log",
    "%DOCFILE%.fdb_latexmk",
    "%DOCFILE%.snm",
    "%DOCFILE%.synctex(busy)",
    "%DOCFILE%.synctex.gz(busy)",
    "%DOCFILE%.nav",
    "%DOCFILE%.vrb",
    "%DOCFILE%.synctex.gz",
    "%DOCFILE%.run.xml",
    "%DOCFILE%-blx.bib",
    "%DOCFILE%.xdv"
  ],
"latex-workshop.formatting.latex": "tex-fmt",
```

### 格式化工具

 **tex-fmt** vs  **latexindent**

latexindent是基于perl的成熟格式化工具，缺点是大文件会卡死

tex-fmt基于rust快很多https://github.com/WGUNDERWOOD/tex-fmt?tab=readme-ov-file#performance

| **Files** | **Lines** | **Size** | **tex-fmt** | **latexindent** | **latexindent -m** |
| --------- | --------- | -------- | ----------- | --------------- | ------------------ |
| 51        | 94k       | 3.5M     | **0.055s**  | 106s [x1927]    | 127s [x2309]       |

记得下载后自己配置Latex-workshop › Formatting › Tex-fmt: Path路径，github下载tex-fmt后建议放在miktex安装目录下方便统一管理




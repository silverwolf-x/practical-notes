# vscode小技巧

- 监视nvidia-smi刷新：安装`nvitop`包，输入

    ```powershell
    ! pip install nvitop
    nvitop -m full
    ```

    > 展示的模式有三种：
    >
    > 1. auto (默认)
    > 2. compact
    > 3. full

- 一键更新python所有包：安装`pip-review`包

    ```
    pip-review # 看能更新的包详情
    pip-review --auto -C # 自动更新并跳过安装失败的包
    ```

    > options:
    >   -h, --help            show this help message and exit
    >   --verbose, -v         Show more output
    >   --raw, -r             Print raw lines (suitable for passing to pip install)
    >   --interactive, -i     Ask interactively to install updates
    >   --auto, -a            Automatically install every update found
    >   --continue-on-fail, -C
    >                         Continue with other installs when one fails
    >   --freeze-outdated-packages
    >                         Freeze all outdated packages to "requirements.txt" before upgrading them

​	注意pytorch系列的更新要自己到[PyTorch](https://pytorch.org/)找cuda版本的链接，否则pip-review只会更新到cpu版本

```
pip3 uninstall torch torchvision torchaudio
pip3 install ... # 官网自己找
```


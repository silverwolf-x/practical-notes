# docker WSL2 windows

## 安装

https://docs.docker.com/desktop/features/wsl/

用windows desktop选择WSL2后端，它会通过hyper-V注册WSL2下的容器docker-desktop，与其他linux发行版并列

# WSL2-Vmmem with Docker desktop will use 100% CPU

https://github.com/docker/for-win/issues/12968

https://github.com/microsoft/WSL/issues/12711

解决方法：用回官方发WSL2 kernel 6.6.y

https://github.com/microsoft/WSL2-Linux-Kernel



```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": true,
  "features": {
    "buildkit": true,
  },
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}
```


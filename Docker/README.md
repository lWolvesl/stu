# Docker

[回到主页](../README.md)

### Docker镜像加速

```json
{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/"]
}

https://hub-mirror.c.163.com/
```

### 步骤

```shell
vim /etc/docker/daemon.json
systemctl daemon-reload
systemctl restart docker
```

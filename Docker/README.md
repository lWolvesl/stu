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

## 开启外部控制端口

- 可兼容应用
  - Portainer



```service
#在下面文件增加内容
vim /usr/lib/systemd/system/docker.service
#找到ExecStart这行 在后面加上-H tcp://0.0.0.0:2375  其它方式一会docker就挂了 而且重启无效 
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
systemctl daemon-reload
systemctl restart docker
```

或在 `/etc/docker/daemon.json` 中填入

```json
{
  "hosts":["tcp://0.0.0.0:2375"]
}
```


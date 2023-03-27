# Portainer

[回到主页](../README.md)

```shell
docker run -d -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce:latest

docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/ru
n/docker.sock:/var/run/docker.sock portainer/portainer-ce:latest
```


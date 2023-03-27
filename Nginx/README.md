# Nginx

[回到主页](../README.md)

## Nginx 安装 

- Docker

```shell
docker run -itd --network=host -v /data/nginx:/etc/nginx --name nginx --restart=always nginx
```

## Nginx配置

### 普通重定向

```conf
location /gitlab {
    rewrite ^/(.*) https://cdn.gitlab.wolves.top/ permanent;
}
```

### 带 request 的重定向

```conf
if ($request_uri ~* ^(.*)gitlab(.*)$){
    set $gitlabu $2;
    rewrite (.*) https://www.gitlab.wolves.top:46080$gitlabu? permanent;
}
```




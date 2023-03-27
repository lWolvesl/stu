# GITEA

[回到主页](../README.md)

- 搭建

## Docker

```shell
docker run -itd --name gitea -p 56000:3000 -p 56022:22 -v /data/gitea:/data gitea/gitea
```

```nginx
server {
    listen 80;

    server_name gitea.wolves.top;

    rewrite ^(.*)$ https://gitea.wolves.top$1 permanent;
}

server {
    listen 443 ssl;

    server_name gitea.wolves.top;

    ssl_certificate ssl/gitea.wolves.top/gitea.wolves.top.crt;
    ssl_certificate_key ssl/gitea.wolves.top/gitea.wolves.top.key;

    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;
    
    client_max_body_size 512m;

    location / {
        proxy_set_header HOST $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection upgrade;
        proxy_set_header Accept-Encoding gzip;

        proxy_pass http://10.0.4.4:56000/;
    }
}
```


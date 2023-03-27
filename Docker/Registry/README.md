# Registry --Docker

[回到主页](../../README.md)

```shell
docker run --name registry -p 45000:5000 -v /data/registry/v2/:/var/lib/registry/docker/registry/v2/ --privileged=true -d registry:2


apt -y install apache2-utils	#安装htpasswd工具
mkdir /data/registry/auth
htpasswd -Bbn user1 Passw0rd > /data/registry/auth/auth-info

docker run -p 5011:5000 \
--restart=always \
--name registry \
-v /data/registry/v2/:/var/lib/registry/docker/registry/v2/ \
-v /data/registry/auth/:/auth/ \
-v /data/nginx/ssl:/certs \
-e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/docker.wolves.top.crt  \
-e REGISTRY_HTTP_TLS_KEY=/certs/docker.wolves.top.key \
-e "REGISTRY_AUTH=htpasswd" \
-e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
-e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
-d registry:2
```

```nginx
server {
    listen 5012 ssl;
    
    server_name docker.wolves.top docker.io.wolves.top;

    ssl_certificate ssl/docker.wolves.top.crt;
    ssl_certificate_key ssl/docker.wolves.top.key;
    
    ssl_certificate ssl/docker.io.wolves.top.crt;
    ssl_certificate_key ssl/docker.io.wolves.top.key;

     # Recommendations from https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html

    # disable any limits to avoid HTTP 413 for large image uploads
    client_max_body_size 10240M;

    # required to avoid HTTP 411: see Issue #1486 (https://github.com/docker/docker/issues/1486)
    chunked_transfer_encoding on;

    location /v2/ {
      proxy_pass                          http://192.168.31.3:45000;
      proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;
        proxy_buffering off;
        proxy_set_header        X-Real-IP       $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## 删除

```shell
docker exec registry bin/registry garbage-collect /etc/docker/registry/config.yml
```

## 非80

```json
"insecure-registries": ["0.0.0.0/0"],
```



## hyper/docker-registry-web

```shell
docker run -d -p 45001:8080 --name registry-web --link registry -e REGISTRY_URL=http://192.168.31.3:45000/v2 -e REGISTRY_NAME=docker.io.wolves.top hyper/docker-registry-web
```

```nginx
server {
    listen 44443 ssl;

    server_name docker.wolves.top
;

    ssl_certificate ssl/docker.wolves.top.crt;
    ssl_certificate_key ssl/docker.wolves.top.key;

    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_set_header HOST $host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection upgrade;
        proxy_set_header Accept-Encoding gzip;

        proxy_pass http://192.168.31.3:45001/;
    }
}
```


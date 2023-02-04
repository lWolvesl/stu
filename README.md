# 学习档案

- [学习档案](#%E5%AD%A6%E4%B9%A0%E6%A1%A3%E6%A1%88)
  - [NPM](#npm)
    - [镜像](#%E9%95%9C%E5%83%8F)
      - [安装```nrm```](#%E5%AE%89%E8%A3%85nrm)
      - [使用](#%E4%BD%BF%E7%94%A8)
  - [MARKDOWN](#markdown)
    - [生成目录](#%E7%94%9F%E6%88%90%E7%9B%AE%E5%BD%95)
      - [安装](#%E5%AE%89%E8%A3%85)
      - [使用](#%E4%BD%BF%E7%94%A8-1)
  - [泛域名证书申请](#%E6%B3%9B%E5%9F%9F%E5%90%8D%E8%AF%81%E4%B9%A6%E7%94%B3%E8%AF%B7)
  - [Docker](#docker)
    - [Docker镜像加速](#docker%E9%95%9C%E5%83%8F%E5%8A%A0%E9%80%9F)
    - [步骤](#%E6%AD%A5%E9%AA%A4)
  - [Gitlab](#gitlab)
    - [Gitlab 安装](#gitlab-%E5%AE%89%E8%A3%85)
      - [配置](#%E9%85%8D%E7%BD%AE)
      - [读取配置](#%E8%AF%BB%E5%8F%96%E9%85%8D%E7%BD%AE)
      - [查看初始密码](#%E6%9F%A5%E7%9C%8B%E5%88%9D%E5%A7%8B%E5%AF%86%E7%A0%81)
  - [Nginx 安装 [Docker]](#nginx-%E5%AE%89%E8%A3%85-docker)
    - [Nginx配置](#nginx%E9%85%8D%E7%BD%AE)
        - [普通重定向](#%E6%99%AE%E9%80%9A%E9%87%8D%E5%AE%9A%E5%90%91)
        - [带 request 的重定向](#%E5%B8%A6-request-%E7%9A%84%E9%87%8D%E5%AE%9A%E5%90%91)
  - [Portainer [Docker]](#portainer-docker)


## NPM

- [官网](https://nodejs.org/)

### 镜像

- 使用```nrm```更换下载源以加速```npm```下载速度

#### 安装```nrm```

```shell
npm i nrm -g
```

#### 使用

```shell
# 列举所有可用镜像源
nrm ls
# 使用淘宝[例]镜像源
nrm use taobao
```

## MARKDOWN

### 生成目录

#### 安装

- 使用```NPM```安装  - [NPM操作](#npm)

```shell 
npm i doctoc -g
```

#### 使用

```shell 
doctoc README.md
```

## 泛域名证书申请

[nginx - Let's Encrypt 安装配置教程，免费的 SSL 证书 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000017194280)

```shell
git clone https://github.com/letsencrypt/letsencrypt
```

```ruby
cd letsencrypt
./certbot-auto certonly  -d *.you.cn --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```

其中"you.cn"换成你的一级域名即可

|            参数            |                             说明                             |
| :------------------------: | :----------------------------------------------------------: |
|          certonly          |  表示安装模式，Certbot 有安装模式和验证模式两种类型的插件。  |
|          --manual          | 表示手动安装插件，Certbot 有很多插件，不同的插件都可以申请证书，用户可以根据需要自行选择 |
|             -d             | 为那些主机申请证书，如果是通配符，输入 *.you.cn（可以替换为你自己的一级域名） |
| --preferred-challenges dns |                 使用 DNS 方式校验域名所有权                  |
|          --server          | Let's Encrypt ACME v2 版本使用的服务器不同于 v1 版本，需要显示指定。 |


## Docker
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



## Gitlab 

### Gitlab 安装

```shell
docker run -d -p 46080:46080 -p:46022:22 -p 46081:80 -h www.gitlab.wolves.top --name gitlab --restart always -v /data/gitlab-ce/config:/etc/gitlab -v /data/gitlab-ce/logs:/var/log/gitlab -v /data/gitlab-ce/data:/var/opt/gitlab gitlab/gitlab-ce:latest
```

#### 配置

```rb
external_url "https://www.backup.gitlab.wolves.top:46080"
#nginx['listen_port'] = 46080
gitlab_rails['gitlab_shell_ssh_port'] = 46022
letsencrypt['enable'] = false
nginx['ssl_certificate'] = "/etc/gitlab/ssl/www.backup.gitlab.wolves.top.crt"
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/www.backup.gitlab.wolves.top.key"
```

#### 读取配置

```shell
gitlab-ctl reconfigure
```

#### 查看初始密码

```shell
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```







## Nginx 安装 [Docker]

```shell
docker run -itd --network=host -v /data/nginx:/etc/nginx --name nginx --restart=always nginx
```

### Nginx配置

##### 普通重定向

```conf
location /gitlab {
    rewrite ^/(.*) https://cdn.gitlab.wolves.top/ permanent;
}
```

##### 带 request 的重定向

```conf
if ($request_uri ~* ^(.*)gitlab(.*)$){
    set $gitlabu $2;
    rewrite (.*) https://www.gitlab.wolves.top:46080$gitlabu? permanent;
}
```





## Portainer [Docker]

```shell
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/ru
n/docker.sock:/var/run/docker.sock portainer/portainer-ce:latest
```
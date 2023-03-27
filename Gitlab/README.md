# GitLab

[回到主页](../README.md)

## Gitlab 安装

```shell
docker run -d -p 46080:46080 -p:46022:22 -p 46081:80 -h www.gitlab.wolves.top --name gitlab --restart always -v /data/gitlab-ce/config:/etc/gitlab -v /data/gitlab-ce/logs:/var/log/gitlab -v /data/gitlab-ce/data:/var/opt/gitlab gitlab/gitlab-ce:latest
```

### 配置

```rb
external_url "https://www.backup.gitlab.wolves.top:46080"
#nginx['listen_port'] = 46080
gitlab_rails['gitlab_shell_ssh_port'] = 46022
letsencrypt['enable'] = false
nginx['ssl_certificate'] = "/etc/gitlab/ssl/www.backup.gitlab.wolves.top.crt"
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/www.backup.gitlab.wolves.top.key"
```

### 读取配置

```shell
gitlab-ctl reconfigure
```

### 查看初始密码

```shell
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```


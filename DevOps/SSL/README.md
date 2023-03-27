# SSL

[回到主页](../../README.md)

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

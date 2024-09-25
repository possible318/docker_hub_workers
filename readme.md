# 利用CloudFlare搭建DockerHub的镜像

## 1. 起因

最近在使用Docker的时候，发现DockerHub无法正常访问，经过各种搜索，发现大概率是被墙了。为了解决这个问题，我们可以利用CloudFlare搭建DockerHub的镜像，加速访问DockerHub。

## 2. 什么是CloudFlare

[CloudFlare](https://dash.cloudflare.com/)
人称CF大善人，是一个全球性的CDN服务提供商，可以加速网站访问速度，提高网站的安全性。CF提供多项免费服务，包括CDN、防火墙、DDoS防护等。我们这次是要利用CF的`Workers &
Pages`功能，搭建一个通过CF转发DockerHub流量的服务。

## 3. 如何搭建DockerHub的镜像

### 3.1 准备一个域名

对于域名没有特殊要求, 完全可以在阿里云购买一个域名, 价格在10元左右, 但是需要备案, 如果不想备案, 可以在国外的域名注册商购买一个域名,
价格在10美元左右, 不需要备案.

### 3.2 注册CloudFlare账号

我们需要注册一个`CloudFlare`账号，选择免费套餐就行了。

### 3.3 将域名解析到CloudFlare

在域名注册商的控制台中，将域名解析到CloudFlare的CDN服务中。等待域名解析转移成功后，就可以在CloudFlare的控制台中对域名进行配置。

```
我使用的是阿里云的域名, 在阿里云的控制台中, 找到DNS管理, 将域名解析到CF的DNS服务器上. 

cf的DNS服务器地址如下:
alice.ns.cloudflare.com
mcgrory.ns.cloudflare.com
```

### 3.4 在CloudFlare中添加域名并且创建Worker


在CloudFlare的控制台中，添加域名，然后选择`Workers & Pages`功能，创建一个Worker。

```javascript

```

### 3.5 配置Docker

在Docker的配置文件中，将DockerHub的镜像地址修改为CloudFlare的CDN服务地址。

```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "https://xxx.xxx.xxx"
  ]
}
```

### 3.6 测试

重启Docker服务，然后使用Docker拉取镜像，查看下载速度是否有提升。

## 4. 总结

利用CloudFlare搭建DockerHub的镜像，可以加速访问DockerHub，提高下载速度。一般情况下，CloudFlare的CDN服务是免费的，所以可以免费使用CloudFlare的CDN服务来加速访问DockerHub。


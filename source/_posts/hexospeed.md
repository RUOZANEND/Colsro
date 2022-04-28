---
title: Hexo访问速度优化
categories: 
  - Tech
tags:
  - Hexo
  - WEB
  - CDN
date: 2020-06-30 12:45:00
updated: 2020-06-30 12:45:00
thumbnail: /imgs/hexospeed/cover.jpg
pslug: hexospeed
---
相信各位大部分都是把hexo托管到github pages上，然鹅由于某众所周知的原因，github 在国内访问速度不太理想，便水一篇文章写一下我的解决方案。

<!--more-->

本文默认读者已有一个使用Hexo搭建并放置于github pages的博客，故略去部分基础内容。

## 1 国内优化
国内我选择的是又拍云，原因很简单：**又拍云联盟**

又拍云联盟是由又拍云推出的开发者帮助计划，对开发者提供免费且完善的云存储、CDN 等服务，加速个人网站、博客、APP 等项目，每月可免费使用 10GB 存储空间及 15GB 的 CDN 流量（HTTP/HTTPS )对于个人博客来说妥妥够用。

### 1.1 新建云储存
在又拍云控制台点击云储存，新建一个云储存服务并记下服务名称，操作员名称，操作员密码及后面的CNAME地址。
![cclx](/imgs/hexospeed/cclx.png)

### 1.2 安装又拍云插件
进入Hexo根目录，打开命令行使用`npm install hexo-deployer-upyundeploy --save`安装又拍云上传插件。
打开Hexo根目录下_config.yml文件，找到`deploy`字段，添加如下内容：
```
deploy:
  - type: upyun
    serviceName: 服务名称
    operatorName: 操作员名称
    operatorPassword: 操作员密码
```
随后可以使用`hexo cl && hexo g && hexo d`测试是否可用。

## 2 国外优化
这步其实可有可无，因为github在国外的访问速度本就足够快，但为了避免再次发生类似前段时间针对github的中间人攻击，我选择再加上Cloudflare的CDN。

### 2.1 接入Cloudflare
由于CF官方限制免费用户无法使用CNAME接入，我这里选择使用第三方[CFP](http://cdn.bnxb.com)。

使用CF账号登陆之后添加一个cname记录，记录值填写`username.github.io`
![cfjl](/imgs/hexospeed/cfjl.png)
(注:不要照抄，把username换成你的github用户名。。。)

随后会给出一个CNAME记录，记下来。
![cfcname](/imgs/hexospeed/cfcname.png)

### 3 设置域名解析
国内大部分DNS解析服务商都支持分线路解析，请自行将解析记录设置为默认线路CF，国内线路又拍。

## 4 进阶设置

看过我之前文章的应该知道，我把Hexo源文件放在了Coding上，并使用[Coding CI实现Hexo的持续集成与Github和Coding的同步部署](https://blog.zra.ink/posts/codingci)

使用又拍云储存之后，原本的的CI脚本便不在适用了，需要在21行之后另起一行，输入：

```
sh 'hexo d'
```
至此，速度优化便折腾完了，各位不妨一试。
![cover](/imgs/hexospeed/cover.jpg)
Enjoy

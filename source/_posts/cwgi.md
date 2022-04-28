---
title: 利用Cloudflare Workers 制作Google镜像站
categories:
  - Tech
tags:
  - WEB
  - Cloudflare
  - Workers
date: 2020-05-01 09:33:00
updated: 2020-05-01 09:33:00
thumbnail: /imgs/cwgi/cover.jpg
pslug: cwgi
---
今天在gayhub上面闲逛，发现了一个利用Cloudflare Workers 制作Google镜像站的方法。

<!--more-->

Cloudflare Workers是什么各位可以自行搜索，这里不做过多介绍。

## 新建 Workers

### 登陆cf之后点击 Workers

![xjcw](/imgs/cwgi/xjcw.jpg)

### 创建子域名

![xjzym](/imgs/cwgi/xjzym.jpg)

### 点击创建 Workers

![cjcw](/imgs/cwgi/cjcw.jpg)

## 部署代码

### 编辑Workers

将以下代码粘贴至代码框

```js
// Website you intended to retrieve for users.
const upstream = 'www.google.com'

// Custom pathname for the upstream website.
const upstream_path = '/'

// Website you intended to retrieve for users using mobile devices.
const upstream_mobile = 'www.google.com'

// Countries and regions where you wish to suspend your service.
const blocked_region = ['KP', 'SY', 'PK', 'CU']

// IP addresses which you wish to block from using your service.
const blocked_ip_address = ['0.0.0.0', '127.0.0.1']

// Whether to use HTTPS protocol for upstream address.
const https = true

// Whether to disable cache.
const disable_cache = true

// Replace texts.
const replace_dict = {
    '$upstream': '$custom_domain',
    '//google.com': ''
}

addEventListener('fetch', event => {
    event.respondWith(fetchAndApply(event.request));
})

async function fetchAndApply(request) {

    const region = request.headers.get('cf-ipcountry').toUpperCase();
    const ip_address = request.headers.get('cf-connecting-ip');
    const user_agent = request.headers.get('user-agent');

    let response = null;
    let url = new URL(request.url);
    let url_hostname = url.hostname;

    if (https == true) {
        url.protocol = 'https:';
    } else {
        url.protocol = 'http:';
    }

    if (await device_status(user_agent)) {
        var upstream_domain = upstream;
    } else {
        var upstream_domain = upstream_mobile;
    }

    url.host = upstream_domain;
    if (url.pathname == '/') {
        url.pathname = upstream_path;
    } else {
        url.pathname = upstream_path + url.pathname;
    }

    if (blocked_region.includes(region)) {
        response = new Response('Access denied: WorkersProxy is not available in your region yet.', {
            status: 403
        });
    } else if (blocked_ip_address.includes(ip_address)) {
        response = new Response('Access denied: Your IP address is blocked by WorkersProxy.', {
            status: 403
        });
    } else {
        let method = request.method;
        let request_headers = request.headers;
        let new_request_headers = new Headers(request_headers);

        new_request_headers.set('Host', url.hostname);
        new_request_headers.set('Referer', url.hostname);

        let original_response = await fetch(url.href, {
            method: method,
            headers: new_request_headers
        })

        let original_response_clone = original_response.clone();
        let original_text = null;
        let response_headers = original_response.headers;
        let new_response_headers = new Headers(response_headers);
        let status = original_response.status;
        
        if (disable_cache) {
            new_response_headers.set('Cache-Control', 'no-store');
        }

        new_response_headers.set('access-control-allow-origin', '*');
        new_response_headers.set('access-control-allow-credentials', true);
        new_response_headers.delete('content-security-policy');
        new_response_headers.delete('content-security-policy-report-only');
        new_response_headers.delete('clear-site-data');
        
        if(new_response_headers.get("x-pjax-url")) {
            new_response_headers.set("x-pjax-url", response_headers.get("x-pjax-url").replace("//" + upstream_domain, "//" + url_hostname));
        }
        
        const content_type = new_response_headers.get('content-type');
        if (content_type.includes('text/html') && content_type.includes('UTF-8')) {
            original_text = await replace_response_text(original_response_clone, upstream_domain, url_hostname);
        } else {
            original_text = original_response_clone.body
        }
        
        response = new Response(original_text, {
            status,
            headers: new_response_headers
        })
    }
    return response;
}

async function replace_response_text(response, upstream_domain, host_name) {
    let text = await response.text()

    var i, j;
    for (i in replace_dict) {
        j = replace_dict[i]
        if (i == '$upstream') {
            i = upstream_domain
        } else if (i == '$custom_domain') {
            i = host_name
        }

        if (j == '$upstream') {
            j = upstream_domain
        } else if (j == '$custom_domain') {
            j = host_name
        }

        let re = new RegExp(i, 'g')
        text = text.replace(re, j);
    }
    return text;
}


async function device_status(user_agent_info) {
    var agents = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];
    var flag = true;
    for (var v = 0; v < agents.length; v++) {
        if (user_agent_info.indexOf(agents[v]) > 0) {
            flag = false;
            break;
        }
    }
    return flag;
}
```
点击左上角修改项目名（可选）
点击右下角保存并部署

![xgdm](/imgs/cwgi/xgdm.jpg)

确认部署

![bs](/imgs/cwgi/bs.jpg)

点击预览查看

![yl](/imgs/cwgi/yl.jpg)

## 绑定域名
### 添加路由

回到域名管理，选择你的域名

点击Workers，点击添加路由

![ly](/imgs/cwgi/ly.jpg)

路由填写你需要解析的域名，Workers选择刚刚新建的Workers项目名
我这里以`gogoogle.ml`举例，注意在域名后面填写`/*`

![qr](/imgs/cwgi/qr.jpg)

### 添加解析

这个是可以随便填写的，无论你写的什么，cf都会绑定到Workers上面

![jx](/imgs/cwgi/jx.jpg)

## 预览
!!!
<center>
[btnyellow href="http://gogoogle.ml/" target="blank"]点击预览[/btnyellow]</center>
!!!




---
title: OpenSSL自签证书
categories:
  - Note
tags:
  - Shell
  - OpenSSL
date: 2020-12-20 20:22:00
updated: 2020-12-20 20:22:00
thumbnail: /imgs/openssl-ca/cover.jpg
pslug: openssl-ca
---
本地搭建程序时需要用到https，记录一下过程，以免遗忘。

<!--more-->

## 生成SSL证书
一般来说，这是最常见的用法。
### 生成 CA 密钥对：
genrsa：使用rsa
ecparm：使用ecdsa
2048：密钥强度（必须末尾）
prime256v1：密钥曲线

```bash
#RSA:
openssl genrsa -out RSACA.key 2048
#ECC:
openssl ecparam -genkey -name prime256v1 -out ECCCA.key
```

### 生成 CA 根证书：
-nodes：不使用密码
-days 3650：有效时长
-subj：附加信息，其中：
C=国家代码，ST=省份，L=城市
O=企业/组织名，OU=部门，CN=通用名称（CN为必填）

```bash
#RSA:
openssl req -sha256 -new \
	-key RSACA.key -out RSACA.crt \
	-nodes -x509 -days 3650 \
	-subj "/C=CN/ST=ST/L=L/O=O/OU=OU/CN=RSACA"
#ECC:
openssl req -sha256 -new \
	-key ECCCA.key -out ECCCA.crt \
	-nodes -x509 -days 3650 \
	-subj "/C=CN/ST=ST/L=L/O=O/OU=OU/CN=ECCCA"
```

### 生成csr：
-sha256：签名哈希算法
```bash
#RSA:
openssl req -new -sha256 -newkey rsa:2048 \
	-nodes -keyout example.com.key -out example.com.csr \
	-subj "/C=CN/ST=ST/L=L/O=O/OU=OU/CN=example.com"
#ECC:
openssl ecparam -name prime256v1 -out example.com.key
openssl req -new -sha256 -newkey ec:example.com.key \
	-nodes -keyout example.com.key -out example.com.csr \
	-subj "/C=CN/ST=ST/L=L/O=O/OU=OU/CN=example.com"
```
需要注意将CN换成需要生成ssl的域名。

### 使用CA签发证书：
```bash
#RSA:
openssl x509 -req -days 3650 \
	-CA RSACA.crt -CAkey RSACA.key -CAcreateserial \
	-in example.com.csr -out example.com.crt
#ECC:
openssl x509 -req -days 3650 \
	-CA ECCCA.crt -CAkey ECCCA.key -CAcreateserial \
	-in example.com.csr -out example.com.crt
```
接下来即可使用自签证书部署。

## 生成代码签名证书
具体流程和生成CA根证书类似，但由于apksigner要求密钥为pk8，便需要转换，具体如下：
### 生成签名证书
```bash
#生成密钥对：
openssl genrsa -out Android.key 2048
#生成证书：
openssl req -sha256 -new \
	-key Android.key -out Android.crt \
	-nodes -x509 -days 3650 \
	-subj "/CN=Android"
```
### 将key转换为pk8
```bash
openssl pkcs8 -in Android.key -topk8 -outform DER -out Android.pk8 -nocrypt
```
### apksigner签名指令
```bash
apksigner sign --key Android.pk8 --cert Android.crt example.apk
```

Enjoy!
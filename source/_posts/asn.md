---
title: "好耶，是ASN！"
categories:
  - Note
tags:
  - BGP
  - ASN
date: 2022-03-16 15:33:00
updated: 2022-03-17 15:33:00
thumbnail: /imgs/asn/cover.jpg
pslug: asn
---
第一次知道ASN应该是高一的时候，但由于本人是个穷比，也没精力放在其他地方，就慢慢淡忘了，直到年初看见某大佬在讨论网络质量时才回想起来

<!--more-->

## 注册 ASN

博主最开始是找的Soha大佬想申请RIPE的ASN，毕竟RIPE可以通过网页管理whois信息，不像隔壁APNIC只能使用邮件或者联系lir代为更新，但是，我卡到账单地址上面了qwq。

随后便联系了ZX，然后，几个小时没回信息，心都差点凉透了，不过最后大晚上终于回复了。



### 注册材料

在ZX这里申请APNIC的ASN你需要准备：

1. 身份证明材料（身份证/护照/驾驶证等等，如果是企业申请，需要公司执照和法人身份证明）

2. 组织名称

3. ASN{标识，全名}

4. ASN所在国家

5. 联系人{姓名，住址，电话}

6. 邮箱{联系人，noc，abuse}

7. 两个上游ASN

8. ASN需要部署到的物理位置

9. 管理密码（可选）

这里说一下，邮箱必须真实有效，因为APNIC每隔半年会发送邮件核实有效性，身份信息、上游AS和机房位置都是保密的，如果你不知道上游和物理位置可以直接让zx帮你写，还可以PY一个/44的ipv6段。另外吐槽一下，大部分信息都可以瞎JIER写，但ZX没说，于是~~我被群友开盒了~~(bushi

所有材料准备完成之后大概需要三天时间等待APNIC审核并发放ASN，但我tm历经坎坷等了三周还签了三份合同才tm拿到

### 修改信息

如果注册完成之后需要修改邮箱/联系人等信息，使用之前设置的管理密码给APNIC发邮件更新，具体可以查看：[Sending updates by email APNIC](https://www.apnic.net/manage-ip/using-whois/updating-whois/objects/email-updates/) ，如果没有设置密码可以联系LIR进行更新。如需增加网站之类的信息需要前往：[PeeringDB](https://www.peeringdb.com/)进行设置。



## BGP广播

拿到ASN之后第一件事当然是找家服务商接入后广播233

### Vultr

Vultr可以说是我这种穷孩子首选了，而且简单直白无需过多操作

直接前往[BGP](https://my.vultr.com/bgp/)页面申请开通即可（别忘了验证邮件地址）

等待审核通过之后即可开始广播，我这里使用Debian/BIRD2作为演示

首先使用apt进行安装：

```bash
apt install bird2 -y
```

vim编辑 `/etc/bird/bird.conf`

```config
router id 10.0.0.1;        #公网ip,当作标识

define BOGON_ASNS = [
    0,                        # RFC 7607
    23456,                    # RFC 4893 AS_TRANS
    64496..64511,                # RFC 5398 and documentation/example ASNs
    64512..65534,                # RFC 6996 Private ASNs
    65535,                    # RFC 7300 Last 16 bit ASN
    65536..65551,                # RFC 5398 and documentation/example ASNs
    65552..131071,                # RFC IANA reserved ASNs
    4200000000..4294967294,        # RFC 6996 Private ASNs
    4294967295                # RFC 7300 Last 32 bit ASN
];

filter sample_import
int set reject_private_asn; {
    if bgp_path ~ BOGON_ASNS then reject;
    accept;
}

protocol bgp vultr
{
    local as 00000;   #你的asn
    source address 实例的ipv6地址;
    multihop 2;        #服务商提供的信息
    neighbor 2001:19f0:ffff::1 as 64515;    #服务商提供的信息
    password "passwd";     #设置的bgp密码，没有请删除
    ipv6 {
        import filter sample_import;
        export all;
        graceful restart on;
    };
}

protocol static
{
    ipv6;
    route 需要广播的地址 via 实例的ipv6地址;
}

protocol device
{
    scan time 5;
}

protocol direct
{
    interface "dummy*";
    ipv6 {
        import all;
    };
}
```

保持配置之后启动bird即可，然后我们还需要新建一个虚拟网卡，以便实例使用你广播出去的ip地址

```bash
ip link add dev dummy0 type dummy
ip link set dummy0 up
ip addr add dev dummy0 yourip

# 重启bird
systemctl restart bird

# 查看广播情况
birdc s r
birdc s p
```



### Tunnelbroker

我使用的是 tunnelbroker.ch 的服务，方法与上述大同小异

修改上述配置 `protocol`中的AS,client ip,server ip即可，不做过多叙述（记得删除密码



## 结语

说实话， 申请这个ASN纯属是一时上头，没有过过多思考，单纯我想要，但是后面冷静下来一想，上学那会学的知识都忘完了，路由交换什么的全忘了，现在改个配置都全靠抄。

写这篇权当是记录，看看就好，也劝各位小白，不要上头

## END



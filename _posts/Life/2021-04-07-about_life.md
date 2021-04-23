---
layout: post
title: 近来的生活
category: 生活
tags: Life
keywords: 生活 感想
typora-root-url: ..\..\
typora-copy-images-to: ..\..\public\zimage
---


![](/public/zimage/164-1617870807726.jpg)

![](/public/zimage/164.jpg)


![](/public/zimage/head.jpg)

## 简介

* TOC
{:toc}
![](/public/zimage/2021-04-06_160027.png)

### B1

####  C1

#### C2


##### D1

###### D2

###### E1






### Hello Word


![](/public/zimage/head.jpg)

### BA

####  BC1

#### BC2


##### BD1

###### BD2

###### BE1

```code
background-color: #fd6969;

.col-md-4 {
    width: 33.3%;
}

@media (min-width: 1200px)
.col-lg-4 {
    width: 25%;
}

@media (min-width: 1200px)
.col-lg-8 {
    width: 75%;
}

```
```

问题
某次家中台式机Ubuntu正常速度git push代码，但Macbook上git pull速度只有～10k。

尝试过的无效方法
xx开启全局代理模式，重开终端，问题依旧
更换代理服务器国家，全局模式，重开终端，问题依旧
在新终端设置http_proxy和https_proxy，问题依旧
注释掉hosts文件中的http://github.global.ssl.fastly.net和http://github.com两行，问题依旧
有效办法
在网站https://www.ipaddress.com/分别搜索“github.global.ssl.fastly.net”和“github.com”，查到的IP与我之前在hosts存储的对应IP不同，使用新IP替换旧IP
使用“sudo killall -HUP mDNSResponder”命令刷新mac的DNS
由于看到github的两个IP属地为美国，因此将代理服务器切换至美国，并开启全局模式（实际验证PAC模式问题依旧），重新打开一个终端，git pull速度大幅提升

```

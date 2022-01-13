```
layout: post
title: Qcom_Issue_经验之谈
category: 技术
tags: Qcom 
keywords: Qcom
typora-root-url: ..\..\
typora-copy-images-to: ..\..\public\zimage\qcom_issue
```



## 目录
 * TOC
 {:toc}






##  常遇问题回复Tip

### GNSS抓取Modem-Log的Tip
```
【Bug2go GNSS_V9.cfg 抓取Modem LOG 提示】
modem log was collected using QC_default.cfg as log mask, so there is very few GNSS msg in QXDM log and we can not further check it from modem perspective
( whether there is any interferance, whether HW performance is good, whether any error from modem or GNSS engine... )

can you please help to use GNSS_V9.cfg as log mask
( Bug2Go -> System Debug Settings -> diag_mdlog v2 -> Config file -> GNSS_V9.cfg ) to collect one more B2G log?
and if possible, please help to side by side test it on REF device and collect pass log for comparison

much appreciated

```



### GPS_Provider列表中去除 network的操作 Tip

```
【GPS定位的Provider列表中去除 network的操作 Tip】
In order to get location details in simulated environment, please disable Network Location so test continues on GPS. Below are the steps:
(1) Go to settings->Location->Advanced->Google Location Accuracy and change it from ON to OFF
(2) adb reboot (or power cycle the device)
(3) make sure settings->Location->Advanced->Google Location Accuracy->OFF
(4) settings->location->advanced->Carrier Location -> must be enabled
(5) After that check only GPS location provider enabled by inputing below command in adb shell
adb root
adb shell su 0 settings get secure location_providers_allowed
and it should output gps
```



### WIFI详情开关描述

```
【WIFI详情开关描述】
Settings >System > About phone > tap "Build number" 4 times >Developer options
Setting > System > Advanced > Developer options >Enable WiFi Verbose Logging  [toogle open]

```


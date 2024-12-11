---
layout: post
title: Qcom_Issue_经验之谈
category: 技术
tags: Qcom 
keywords: Qcom
typora-root-url: ..\..\
typora-copy-images-to: ..\..\public\zimage\qcom_issue
---


## 目录
 * TOC
 {:toc}






## 常用notepad++搜索技巧Tip



### 查看手机打开应用记录与时间日志

```
ActivityTaskManager: START u0
```



### 查看指定应用的应用权限信息

```
Package [com.whatsapp]      // bugreport 中搜索该名称
Package [com.baidu.map.location]   // 普通搜索,正则搜索搜不到
```



```
  Package [com.baidu.map.location] (b179259):
    userId=10154
    pkg=Package{1769e1e com.baidu.map.location}
    codePath=/product/priv-app/BaiduNetworkLocation
    resourcePath=/product/priv-app/BaiduNetworkLocation
    legacyNativeLibraryDir=/product/priv-app/BaiduNetworkLocation/lib
    extractNativeLibs=true
    primaryCpuAbi=arm64-v8a
    secondaryCpuAbi=null
    cpuAbiOverride=null
    versionCode=52 minSdk=14 targetSdk=30
    minExtensionVersions=[]
    versionName=5.2.0
    usesNonSdkApi=false
    splits=[base]
    apkSigningVersion=1
    applicationInfo=PackageImpl{1769e1e com.baidu.map.location}
    flags=[ SYSTEM HAS_CODE ]
    privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION ALLOW_AUDIO_PLAYBACK_CAPTURE PRIVILEGED PRODUCT PRIVATE_FLAG_ALLOW_NATIVE_HEAP_POINTER_TAGGING ]
    forceQueryable=false
    queriesPackages=[]
    dataDir=/data/user/0/com.baidu.map.location
    supportsScreens=[small, medium, large, xlarge, resizeable, anyDensity]
    usesLibraries:
      com.android.location.provider
    usesLibraryFiles:
      /system/framework/com.android.location.provider.jar
    timeStamp=2009-01-01 08:00:00
    firstInstallTime=2009-01-01 08:00:00
    lastUpdateTime=2009-01-01 08:00:00
    signatures=PackageSignatures{ce2a6ff version:1, signatures:[2505c087], past signatures:[]}
    installPermissionsFixed=true
    pkgFlags=[ SYSTEM HAS_CODE ]
    install permissions:
      android.permission.WRITE_SETTINGS: granted=true
      android.permission.INSTALL_LOCATION_PROVIDER: granted=true
      android.permission.RECEIVE_BOOT_COMPLETED: granted=true
      android.permission.BLUETOOTH: granted=true
      android.permission.INTERNET: granted=true
      android.permission.WRITE_SECURE_SETTINGS: granted=true
      android.permission.CHANGE_WIFI_STATE: granted=true
      android.permission.ACCESS_NETWORK_STATE: granted=true
      android.permission.INTERACT_ACROSS_USERS: granted=true
      android.permission.ACCESS_WIFI_STATE: granted=true
    User 0: ceDataInode=6185 installed=true hidden=false suspended=false distractionFlags=0 stopped=false notLaunched=false enabled=0 instant=false virtual=false
      overlay paths:
        /product/overlay/NavigationBarModeGestural/NavigationBarModeGesturalOverlay.apk
        /data/resource-cache/com.android.systemui-neutral-kbpM.frro
        /data/resource-cache/com.android.systemui-accent-m2Cs.frro
      legacy overlay paths:
        /product/overlay/NavigationBarModeGestural/NavigationBarModeGesturalOverlay.apk
      gids=[3002, 3003]
      runtime permissions:  【运行时权限】
        android.permission.ACCESS_FINE_LOCATION: granted=true, flags=[ SYSTEM_FIXED|GRANTED_BY_DEFAULT]
        android.permission.BLUETOOTH_CONNECT: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED]
        android.permission.READ_EXTERNAL_STORAGE: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED|RESTRICTION_SYSTEM_EXEMPT|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.ACCESS_COARSE_LOCATION: granted=true, flags=[ SYSTEM_FIXED|GRANTED_BY_DEFAULT]
        android.permission.BLUETOOTH_ADVERTISE: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED]
        android.permission.WRITE_EXTERNAL_STORAGE: granted=true, flags=[ GRANTED_BY_DEFAULT|RESTRICTION_SYSTEM_EXEMPT|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.ACCESS_BACKGROUND_LOCATION: granted=true, flags=[ SYSTEM_FIXED|GRANTED_BY_DEFAULT|RESTRICTION_SYSTEM_EXEMPT|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.BLUETOOTH_SCAN: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED]
    User 900: ceDataInode=0 installed=false hidden=false suspended=false distractionFlags=0 stopped=false notLaunched=false enabled=0 instant=false virtual=false
      gids=[3002, 3003]
      runtime permissions:
        android.permission.ACCESS_FINE_LOCATION: granted=true, flags=[ SYSTEM_FIXED|GRANTED_BY_DEFAULT]
        android.permission.BLUETOOTH_CONNECT: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED]
        android.permission.READ_EXTERNAL_STORAGE: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED|RESTRICTION_SYSTEM_EXEMPT|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.ACCESS_COARSE_LOCATION: granted=true, flags=[ SYSTEM_FIXED|GRANTED_BY_DEFAULT]
        android.permission.BLUETOOTH_ADVERTISE: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED]
        android.permission.WRITE_EXTERNAL_STORAGE: granted=true, flags=[ GRANTED_BY_DEFAULT|RESTRICTION_SYSTEM_EXEMPT|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.ACCESS_BACKGROUND_LOCATION: granted=true, flags=[ SYSTEM_FIXED|GRANTED_BY_DEFAULT|RESTRICTION_SYSTEM_EXEMPT|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.BLUETOOTH_SCAN: granted=true, flags=[ GRANTED_BY_DEFAULT|REVOKE_WHEN_REQUESTED]

```





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

### NVram值查看

```
1.手机处于 Fastboot 模式 下的 Qcom 启动方式 重启

2. 打开 Qcom_QXDM  => Option => Communication => Dialg 标签 选中 List 中的 item =>点击ok 

3. 连接成功 后点击 View-> Common ->  Nv Broswer  打开 NV浏览器

4. 在 Nv Broswer 输入 Search ID (5596)  后点击  Read 就能显示出当前 NVItem 值 ( 5596 :高通GPS DPO 动态功率选择功能的开关) 



DPO 之前查看的是 NV值NV5596 的值是 1 就确保 开启了 
DPO有要求要6颗还是8颗卫星信号稳定在28db以上 持续10分钟
Qcom-> GPS MeatureMent(Fastoot_Qcom_Module) 中的 表头为C的字段(TRK SRCH DPO)
GPS的功耗模式是 cold start(全功耗)->tracking->LPM->DPO      




```

#### NV值列表

```
[ NvItem Id          - 5596 ]
[ NvItem Description - CGPS Dynamic Power Optimization Control ] DPO功耗设置


[ NvItem Id          - 74137 ]
[ NvItem Description - GNSS Forced Multiband Engagement Config ]




```


### QCC来下载XTRA辅助定位数_移除限制

#### 老平台移除XTRA
```
拉取  /system/vendor/etc/gps.conf  或者 /vendor/etc/gps.conf  文件 然后设置 XTRA_TEST_ENABLED = 1 和 XTRA_THROTTLE_ENABLED = 0 到文件 
重新导入覆盖源文件就可以移除xtra下载限制

adb root
adb remount
adb pull /system/vendor/etc/gps.conf .

adb pull /vendor/etc/gps.conf 

open gps.conf and add two configs at the end of file:
XTRA_TEST_ENABLED = 1
XTRA_THROTTLE_ENABLED = 0



adb push gps.conf /system/vendor/etc/gps.conf

adb push gps.conf /vendor/etc/gps.conf

adb reboot




```

####  8450_8475新近平台移除XTRA
```

导入本地的 test_cfg.xml 文件到  /data/user_de/0/com.qualcomm.qti.qdma/files/ 文件夹下重启生效

adb root
adb remount
adb push test_cfg.xml /data/user_de/0/com.qualcomm.qti.qdma/files/
adb push test_cfg.xml /data/user_de/0/com.qti.qcc/files/
adb reboot


adb root && adb remount && adb push test_cfg.xml /data/user_de/0/com.qti.qcc/files/  && adb reboot
 

```

##### test_cfg.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!--
   Copyright (c) 2017 Qualcomm Technologies, Inc.
   All rights reserved.
   Confidential and Proprietary - Qualcomm Technologies, Inc.
 -->

<!--
 NOTE:
Put this file in /data/data/com.qualcomm.qti.qdma/files/ folder and  /data/user_de/0/com.qti.qcc/files/ folder
(adb push test_cfg.xml /data/data/com.qualcomm.qti.qdma/files/test_cfg.xml), 
(adb push test_cfg.xml /data/data/com.qti.qcc/files/test_cfg.xml), 
and reboot device. 
This file will be removed after loaded.

If throttling enable=false, throttling mechanism will be disabled.
-->

<qdma_test_config>
<throttling
enable="false"
duration_seconds="2"
allowed_bytes="6291456"
immediate_request="100000"
periodic_request="10000"
integrity_duration_seconds="100"
integrity_request="3000"
/>
<property_vendor.qti.qdma.enabled
value="1"/>
</qdma_test_config>




```


##### Qcom 检测是否移除Xtra下载限制

```
1. 连接网络
2. 在该网络下设置代理proxy 为    xxxxx.pac  保证能连接上外网 youtube
3. 安装 Gps_test.apk 
4. 一直执行 Clear assist data 后 查看手机打印如下 
 【within limit  : 高通xtra数据在下载限制内 可以继续下载xtra数据】
 【out of limit  : 高通xtra数据下载达到限制一天三次 禁止下载】

adb logcat | grep "check XTRA server request rate limit"
01-12 05:16:07.043  2830  2842 D LocSvc_xtra2: onRequestXtraData:42] check XTRA server request rate limit. within limit. execute request.
01-12 05:16:18.610  2830  2842 D LocSvc_xtra2: onRequestXtraData:42] check XTRA server request rate limit. within limit. execute request.   【xtra下载无限制】


01-12 05:16:18.610  2830  2842 D LocSvc_xtra2: onRequestXtraData:42] check XTRA server request rate limit. out of limit.    【xtra下载次数存在限制】



```

##### Qcm_GPS正常工作检测

```
1. 确认  /vendor/bin/xtra-daemon   和 /vendor/bin/lowi-server 进程正常运行
xtra-daemon    /vendor/bin/xtra-daemon 是 Qcom GPS xtra辅助数据下载的可执行进程
lowi-server    /vendor/bin/lowi-server 是高通项目把Modem扫描到的WIFI结果 注入到与TE交互的Modem包中的工具
  【       adb root && adb remount && adb push izat.conf  /vendor/etc/izat.conf   && adb reboot       】
   如果不正常运行说明可能配置文件可能存在问题 提高通case 更新分支  izat.conf   [/vendor/etc/izat.conf] 查看是否能work 


 adb shell ps -A | grep -E "xtra-daemon|lowi-server"
 
gps           2213  2099    2530328   8308 0                   0 S lowi-server
gps           2214  2099    2502208  11024 0                   0 S xtra-daemon


```


```
【gps 相关进程】
adb shell ps -A | grep -E "gps"


 adb shell ps -A | grep -E "gps"
gps           1487     1    3006868  12868 0                   0 S android.hardware.gnss-aidl-service-qti
gps           2048     1    2331216   6348 0                   0 S qsap_location
gps           2094     1    2284712   5616 0                   0 S mlid
gps           2099     1    2206596   5820 0                   0 S loc_launcher
gps           2213  2099    2530328   8308 0                   0 S lowi-server
gps           2214  2099    2502208  11024 0                   0 S xtra-daemon
gps           6728  2099    2521472   8604 0                   0 S xtwifi-client


xtra-daemon    /vendor/bin/xtra-daemon 是 Qcom GPS xtra辅助数据下载的可执行进程
lowi-server    /vendor/bin/lowi-server 是高通项目把Modem扫描到的WIFI结果 注入到与TE交互的Modem包中的工具
----------------------

【wifi 相关进程】
adb shell ps -A | grep -E "wifi"

wifi          1499     1    2292068  15396 0                   0 S android.hardware.wifi-service
wifi          1988     1    2266020   6444 0                   0 S wificond
system        1993     1    2323036   6844 0                   0 S wifidisplayhalservice
gps           6728  2099    2521472   8604 0                   0 S xtwifi-client
wifi         15038     1    2274900   8548 0                   0 S wpa_supplicant



【bt 相关进程】
 adb shell ps -A | grep -E "bluetooth"
bluetooth     1478     1    2568676   8244 0                   0 S android.hardware.bluetooth@aidl-service-qti
u0_a397       6823  1374    7791284 102368 0                   0 S com.bluetooth.aptxmode
bluetooth    30276  1374    8354704 135540 0                   0 S com.android.bluetooth

```




##### Qcom Xtra GPS辅助数据下载成功检查
```

adb logcat | grep -e "check XTRA server request rate limit" -e "XTRA download request"  -e "QUERY_XTRA_INFO_REQ" -e "doProcessXtraData" -e "XTRA server:" -e "successfully download file, size:" -e "injectXtraData success"  -e "locAPIGnssDeleteAidingData" -e "Used In Fix:"  -e "GnssManager: GNSS" 
【正确打印1】
01-12 05:50:29.723  2830  2849 I LocSvc_ApiV02: <--- globalEventCb line 233 QMI_LOC_EVENT_QUERY_XTRA_INFO_REQ_IND_V02    【请求Xtra数据】
01-12 05:50:29.723  2830  2849 V LocSvc_LBSApiV02: eventCb:58] client = 0xb40000793be71f00, event id = 214, event name = QMI_LOC_EVENT_QUERY_XTRA_INFO_REQ_IND_V02 payload = 0x79365edd08
01-12 05:50:29.723  2830  2849 V LocSvc_IzatApiV02:  eventCb:211]: Got an QMI_LOC_EVENT_QUERY_XTRA_INFO_REQ_IND_V02
01-12 05:50:29.723  2830  2849 D LocSvc_IzatApiV02: eventCb:155]: XTRA download request
01-12 05:50:29.723  2830  2842 W LocSvc_xtra2: updateXtraServers:437] XTRA server: https://path3.xtracloud.net/xtra3Mgrbej.bin https://path1.xtracloud.net/xtra3Mgrbej.bin https://path2.xtracloud.net/xtra3Mgrbej.bin
01-12 05:50:29.723  2830  2842 D LocSvc_xtra2: onRequestXtraData:42] check XTRA server request rate limit. within limit. execute request.
01-12 05:50:30.478  2830  2842 D LocSvc_xtra2: doProcessXtraData:206] successfully download file, size: 37367
01-12 05:50:30.478  2830  2842 D LocSvc_xtra2: doProcessXtraData:218] XTRA data file version number:3
01-12 05:50:30.481  2830  2842 D LocSvc_xtra2: doProcessXtraData:239] Send Periodic Txn
【正确打印2】
01-12 05:52:44.465  2830  2849 I LocSvc_ApiV02: <--- globalEventCb line 233 QMI_LOC_EVENT_QUERY_XTRA_INFO_REQ_IND_V02
01-12 05:52:44.465  2830  2849 V LocSvc_LBSApiV02: eventCb:58] client = 0xb40000793be71f00, event id = 214, event name = QMI_LOC_EVENT_QUERY_XTRA_INFO_REQ_IND_V02 payload = 0x79365edd08
01-12 05:52:44.465  2830  2849 V LocSvc_IzatApiV02:  eventCb:211]: Got an QMI_LOC_EVENT_QUERY_XTRA_INFO_REQ_IND_V02
01-12 05:52:44.465  2830  2849 D LocSvc_IzatApiV02: eventCb:155]: XTRA download request
01-12 05:52:44.466  2830  2842 W LocSvc_xtra2: updateXtraServers:437] XTRA server: https://path1.xtracloud.net/xtra3Mgrbej.bin https://path2.xtracloud.net/xtra3Mgrbej.bin https://path3.xtracloud.net/xtra3Mgrbej.bin
01-12 05:52:44.466  2830  2842 D LocSvc_xtra2: onRequestXtraData:42] check XTRA server request rate limit. within limit. execute request.
01-12 05:52:45.423  2830  2842 D LocSvc_xtra2: doProcessXtraData:206] successfully download file, size: 37367
01-12 05:52:45.423  2830  2842 D LocSvc_xtra2: doProcessXtraData:218] XTRA data file version number:3
01-12 05:52:45.426  2830  2842 D LocSvc_xtra2: doProcessXtraData:239] Send Periodic Txn


adb logcat | grep -e "check XTRA server request rate limit" -e "XTRA download request"  -e "QUERY_XTRA_INFO_REQ" -e "doProcessXtraData" -e "XTRA server:" -e "successfully download file, size:" -e "injectXtraData success"  -e "locAPIGnssDeleteAidingData" -e "Used In Fix:"  -e "GnssManager: GNSS" 
【失败Log】 并不打印 QUERY_XTRA_INFO_REQ 相关Log  

```



### Xtra下载失效恢复Tip

```
will ask reporter to test it another day and collect one more B2G log 
( each device was allowed to download XTRA data 3 times per day )

Please use 【userdebug version】 to retest as below :
Steps to change debug level & disable xtra throttling :
1) adb root
2) adb remount
3) adb pull /vendor/etc/gps.conf
4) add these two items in gps.conf
XTRA_TEST_ENABLED = 1
XTRA_THROTTLE_ENABLED = 0
5) Change DEBUG_LEVEL to 5 & push it back and reboot. If DEBUG_LEVEL is already 5 no need to do anything.
6) adb shell settings put global captive_portal_mode 0
7) begin CTS test .
```





### WIFI详情开关描述

```
【WIFI详情开关描述】
Settings >System > About phone > tap "Build number" 4 times >Developer options
Setting > System > Advanced > Developer options >Enable WiFi Verbose Logging  [toogle open]

```



### 抓取视频复现

```
open wifi verbose as below:
Settings >System > About phone > tap "Build number" 4 times >Developer options
Setting > System > Advanced > Developer options >Enable WiFi Verbose Logging  [toogle open]

capture screenshot video command as below:
    adb root
	adb shell screenrecord /sdcard/demo.mp4 
	adb pull  /sdcard/demo.mp4  
	
```





### 抓取Tcp-Log的Tip

```
【Steps for collecting tcpdump as below】
adb root
adb remount -R 【(NOTE: device will reboot if not remounted yet)】
adb root
adb remount
adb shell
tcpdump -i any -s 0 -w /system/tcpdumpout.pcap    【(run the test case)】
^C    【(Control C to stop logging)】
exit
adb pull /system/tcpdumpout.pcap .
【 share the "tcpdumpout.pcap" file here.】
```





### adb-Log的Tip

```
1.  open verbose wifi button
Setting > System > Advanced > Developer options >Enable WiFi Verbose Logging [toogle open] 
2. open bug2go to capture
3. if you havenot bug2go , please input command as below to capture Log
adb logcat > Log.txt
```



### adb命令描述测试Supl使能

```

adb root
adb disable-verity
adb reboot
adb root
adb remount
adb pull /etc/gps_debug.conf
adb pull  /system/etc/gps_debug.conf
adb pull   /vendor/etc/gps_debug.conf
Make 【#SUPL_MODE=1】 To 【SUPL_MODE=1】 on local dir   【[Mtk]在MccMnc=00101 的情况下 SUPL无法启动时 检查该配置 SUPL_MODE=1 是否打开】
adb push .\gps_debug.conf  /etc/
adb push .\gps_debug.conf  /system/etc/
adb push .\gps_debug.conf  /vendor/etc/
adb shell 
═════════════ in adb shell ═════════════
cd /etc/
chmod 644 gps_debug.conf
cat gps_debug.conf    【make sure SUPL_MODE=1 in the file 】
═════════════ out adb shell ═════════════
adb reboot


```


#### gps_debug.conf

```

# Sample file for use for on device debug override only
# Prefer frameworks/base/core/res/res/values/config.xml and
# frameworks/base/core/res/res/values-mcc*-mnc*/config.xml

################################
##### AGPS server settings #####
################################
# FOR SUPL SUPPORT, set the following
# SUPL_HOST=supl.google.com or IP
# SUPL_PORT=7275

# supl version 2.0
# SUPL_VER=0x20000

#SUPL_MODE is a bit mask set in config.xml per carrier by default.
#If it is uncommented here, this value will overwrite the value from
#config.xml.
#MSA=0X2
#MSB=0X1
#SUPL_MODE=1       【在进行SUPL测试时候 MccMnc=00101 时候 需要把该值打开 才能建立起SUPL连接】

# Emergency SUPL, 1=enable, 0=disable
#SUPL_ES=0

#Choose PDN for Emergency SUPL
#1 - Use emergency PDN
#0 - Use regular SUPL PDN for Emergency SUPL
#USE_EMERGENCY_PDN_FOR_EMERGENCY_SUPL=0

####################################
#  LTE Positioning Profile Settings
####################################
# 0: Enable RRLP on LTE(Default)
# 1: Enable LPP_User_Plane on LTE
# 2: Enable LPP_Control_Plane
# 3: Enable both LPP_User_Plane and LPP_Control_Plane
#LPP_PROFILE = 2                  【同时使能 LPP_CP  LPP_UP  , LPP_PROFILE = 8 】

##################################################
# Select Positioning Protocol on A-GLONASS system
##################################################
# 0x1: RRC CPlane
# 0x2: RRLP UPlane
# 0x4: LLP Uplane
#A_GLONASS_POS_PROTOCOL_SELECT = 0

# Below bit mask configures how GPS functionalities
# should be locked when user turns off GPS on Settings
# Set bit 0x1 if MO GPS functionalities are to be locked
# Set bit 0x2 if NI GPS functionalities are to be locked
# default - non is locked for backward compatibility
#GPS_LOCK = 0

################################
##### PSDS download settings #####
################################
# For wear devices only.
# Enable periodic PSDS download once a day.
# true: Enable periodic PSDS download
# false: Disable periodic PSDS download
#ENABLE_PSDS_PERIODIC_DOWNLOAD=false




```




```
adb root
adb push ./WCNSS_qcom_cfg.ini    /vendor/etc/wifi/WCNSS_qcom_cfg.ini
adb reboot
```



```

adb root
adb remount
adb pull /vendor/etc/wifi/WCNSS_qcom_cfg.ini   .

// add   gindoor_channel_support=1 in end of WCNSS_qcom_cfg.ini
gindoor_channel_support=1

adb push ./WCNSS_qcom_cfg.ini  /vendor/etc/wifi/
adb reboot
adb pull /vendor/etc/wifi/WCNSS_qcom_cfg.ini 

```



### 蓝牙|Wifi共存问题抓取Log的Tip

```
since it's WiFi/BT coexistence issue, please help to collect logs below ( to avoid QC asking for logs again and again ):

(1) Sync your device time to standard time 
(2) BT OTA logs // optional 
(3) BT HCI logs
(4) use attached BT_WLAN_BTC_Mask.cfg as QXDM log mask ( you can push BT_WLAN_BTC_Mask.cfg to /sdcard/ then select it from Bug2Go - Settings - System Debug Settings - diag_mdlog - Config file (log mask) )
(5) Every time wifi is turned on, executing the following to enable BTC logs: 

adb shell iwpriv wlan0 dump 13 6 6 1
adb shell iwpriv wlan0 dump 2 6 6 1
(6) adb shell WifiLogger_app > wifilogger.txt 
(7) WiFi sniffer logs
(8) Bug2Go log ( including wlan driver log and firmware log )
(9) Issue Timestamp

much appreciated

```


### Selinux检查开启关闭

```
FastBoot 模式下
fastboot oem config cmdl androidboot.selinux=enforcing       【开启SeLinux】
fastboot oem config cmdl androidboot.selinux=permissive      【关闭SeLinux】

adb shell getenforce   【permissive__表示关闭】 【enforcing__表示开启】

```

###  Settings.apk导入命令

```
Settings.apk导入命令

adb root && adb remount && adb shell settings put global wifi_verbose_logging_enabled 1 && adb push Settings.apk /system_ext/priv-app/Settings/


```

```
Settings.apk 全局搜索命令

adb root
adb  shell
su
find  -name "*Settings.apk"
【  ./system_ext/priv-app/Settings/Settings.apk  】

```



### Android.mk 文件编辑记录

Android.mk 离 存在 if else 等相关的逻辑判断流程,用于控制编译流程,但缺点是编译复杂 编译速度慢

```

打印相关Log 

$(warning 'zukgit-begin')
$(warning 'zukgit-end')
$(warning "zukgit LOCALPATH is $(LOCAL_PATH)")
$(warning "zukgit TARGET_PRODUCT is $(TARGET_PRODUCT) ")

$(warning " prebuilt_stdcxx_path is $(prebuilt_stdcxx_path)")
$(warning " uvc_util_src_files is $(uvc_util_src_files)")
$(warning " LOCAL_C_INCLUDES is $(LOCAL_C_INCLUDES)")
$(warning " LOCAL_SRC_FILES is $(LOCAL_SRC_FILES)")
$(warning " LOCAL_LDFLAGS is $(LOCAL_LDFLAGS)")
$(warning " LOCAL_SHARED_LIBRARIES is $(LOCAL_SHARED_LIBRARIES)")
$(warning " LOCAL_STATIC_LIBRARIES is $(LOCAL_STATIC_LIBRARIES)")



```



```

ifeq ($(findstring  applex, $(strip $(TARGET_PRODUCT))), applex)        // 如果当前项目名称是applex  那么  执行
        LOCAL_INIT_RC=hidl/$(HIDL_INTERFACE_VERSION)/AAAAAA.rc
        $(warning "Supplicant [A] LOCAL_INIT_RC is $(LOCAL_INIT_RC) TARGET_PRODUCT is $(TARGET_PRODUCT) ")
else    // 当前项目名称非 applex 的情况
        LOCAL_INIT_RC=hidl/$(HIDL_INTERFACE_VERSION)/BBBBBB.rc
        $(warning "Supplicant [B] LOCAL_INIT_RC is $(LOCAL_INIT_RC) TARGET_PRODUCT is $(TARGET_PRODUCT) ")
endif

```


```

// 如果当前的 项目名称  $(TARGET_PRODUCT)  不是 skyline【ifneq】    那么就执行 ifneq 语句  ,  如果是 skyline 那么跳过ifneq 语句

ifneq ($(findstring  skyline, $(strip $(TARGET_PRODUCT))), skyline)
DEVICE_MANIFEST_TXAS_FILES += \
    device/xxxx/xxxx/xxxxx.xml
endif

```




### Android.bp 文件说明

https://blog.csdn.net/FranzKafka95/article/details/136002469



1. Android.bp 文件不存在 if else 相关的编译流程, 当该文件是 json 类型的子集,有点是 编译速度快 高度快...


2. Android.bp 不存在 if else  流程控制  , 但中设计了一个 soong_config_module_type{} 的 模块对象 , 用来匹配在 .mk 文件中定义的宏开关 来 动态 
控制代码的编译宏的开关 。
soong_config_module_type{} 定义了结构体的数据类型 
soong_config_string_variable{ }  // 定义在 soong_config_module_type 中定义的 variables menu 字符串(非纯数字字符串)变量的 可选值
xxxx_hal_cc_defaults{}  定义了当前需要配置的数据 的具体的值 



3. Android.bp 文件的 xxxx_hal_cc_defaults 会去 Soong 配置编译系统中去匹配 , 如果有定义这个宏 那么该宏对应的本地结构体设置为 true(1)
   这个  true(1) 标识 当前编译系统存在这个 编译开关 , 当前 ifdef 只关心当前编译开关是否存在,而不关心该开关是是true还是false
   在代码中宏预编译的部分  , 【ifdef 在代码中 用于判断是否存在这个编译宏开关而不关心这个编译开关是true还是false..】


```

Android.bp  



#声明一个soong编译配置模块
#name表明该模块的名称
#config_namespace表名该编译配置模块所属的命名空间，用于在Makefile中使用
#modlue_type用于表明该编译配置模块所附属的编译配置
#variables，bool_variables，value_variables表明该编译配置模块所支持的选项类型
#bool_variables用于定义一个表征bool类型的配置项
#value_variables 用于定义一个可传递的配置项，通过%s进行获取
#properties用于表明该编译配置模块最终影响的可选项，其来自于moudle_type中的可选项

soong_config_module_type {
    name: "wifi_hal_cc_defaults",
    module_type: "cc_defaults",
    config_namespace: "wifi",   // 用于标识 在 .MK 文件 定义的前缀 
	
	variables: [              //  枚举字符串 值  需要在结构体 soong_config_string_variable 定义可选范围
        "hidl_product_version", // WIFI_HIDL_PRODUCT_VERSION
    ],
    bool_variables: [           //  bool值的宏开关
        "hidl_feature_aware", // WIFI_HIDL_FEATURE_AWARE
        "hidl_feature_dual_interface", // WIFI_HIDL_FEATURE_DUAL_INTERFACE
        "hidl_feature_disable_ap", // WIFI_HIDL_FEATURE_DISABLE_AP
    ],
    value_variables: [       // 用于定义一个可传递的配置项的宏，通过%s进行获取
        "hal_interface_combinations", // WIFI_HAL_INTERFACE_COMBINATIONS
    ],
    properties: [
        "cppflags",
    ],
}


soong_config_string_variable {
    name: "hidl_product_version",
    values: ["v3" , "v2" ,"v1"] ,
}





wifi_hal_cc_defaults {
    name: "xxxxxxx-cppflags-defaults",
    soong_config_variables: {   // 在 soong 编译配置系统中匹配 , 如果有定义 那么就在当前设置编译开关
        hidl_feature_aware: {       // 如果发现当前 WIFI_HIDL_FEATURE_AWARE 这个开关有定义【！ 注意是有定义】 那么 就打开当前项目的编译开关 -DWIFI_HIDL_FEATURE_AWARE
            cppflags: ["-DWIFI_HIDL_FEATURE_AWARE"],
        },
        hidl_feature_dual_interface: {    //  如果在 Soong 发现了该编译开关【不管配置为true , 还是 false】都在当前项目上 添加编译选项  "-DWIFI_HIDL_FEATURE_DUAL_INTERFACE"
            cppflags: ["-DWIFI_HIDL_FEATURE_DUAL_INTERFACE"],
        },
        hidl_feature_disable_ap: {
            cppflags: ["-DWIFI_HIDL_FEATURE_DISABLE_AP"],
        },
        hal_interface_combinations: {
            cppflags: ["-DWIFI_HAL_INTERFACE_COMBINATIONS=%s"],
        },
    },
}



```



#### Android.bp 本地编辑编译记录

##### wlan.mk定义编译开关宏_1
/device/qcom/wlan/taro/wlan.mk

```
1.在 .mk 文件中定义了 编译开关宏的值 并传输给了 Soong 编译配置 如下
/device/qcom/wlan/taro/wlan.mk  中 定义了配置开关

WIFI_TEST1_FALSE_BOOL := false                       //  Bool宏_false    bool_variables
WIFI_TEST2_TRUE_BOOL := true                         //  Bool宏_true     bool_variables
WIFI_TEST3_VALUE_VARIABLES_STRING := HELLO WORLD { A B C}   //  动态传递字符串的宏  (包含 数字字符串)   value_variables
WIFI_TEST4_VALUE_VARIABLES_NUMBER := 100                          //   Menu宏_{v3,v2,v1}  (在Soong 该项不能是纯数字) variables , 在Android.bp 可以定义 menu 可选值范围
WIFI_TEST5_VARIABLES := v2



```

##### board_config_wifi.mk定义了需要传输到Soong配置的mk编译宏_2
/build/make/core/board_config_wifi.mk

```
/build/make/core/board_config_wifi.mk     .mk文件中定义了 需要传递给 soong 配置信息的变量 


ifdef WIFI_TEST1_FALSE_BOOL
   // 使用 soong_config_set 定义namespace 是 wifi 的 字面变量 test_a_feature_support的值是 true 在 Soong配置系统中
   // 也可以定义为false  定义为flase 那么 Soong 就不会主动匹配  Android.bp 中匹配的项就不会被定义 ,最终 #if def 就为 false, 不执行
    $(call soong_config_set,wifi,test1_false_bool,false【这里定义fasle 就不会发送到Android.bp和 Soong】)   
    $(warning " zukgit WIFI_TEST1_FALSE_BOOL is $(WIFI_TEST1_FALSE_BOOL)")

endif

ifdef WIFI_TEST2_TRUE_BOOL
    $(call soong_config_set,wifi,test2_true_bool,true)   //  .mk 编译 和 Soong 编译配置 关联处   传输到 Android.bp
    $(warning " zukgit  WIFI_TEST2_TRUE_BOOL is $(WIFI_TEST2_TRUE_BOOL)")
endif


ifdef WIFI_TEST3_VALUE_VARIABLES_STRING
    $(call soong_config_set,wifi,test3_value_variables_string,$(WIFI_TEST3_VALUE_VARIABLES_STRING))  //  传输变量值
    $(warning " zukgit_1024 WIFI_TEST3_VALUE_VARIABLES_STRING is $(WIFI_TEST3_VALUE_VARIABLES_STRING)")
endif

ifdef WIFI_TEST4_VALUE_VARIABLES_NUMBER
    $(call soong_config_set,wifi,test4_value_variables_number,$(WIFI_TEST4_VALUE_VARIABLES_NUMBER))  //  传输变量值
    $(warning " zukgit_1024 WIFI_TEST4_VALUE_VARIABLES_NUMBER is $(WIFI_TEST4_VALUE_VARIABLES_NUMBER)")
endif

ifdef WIFI_TEST5_VARIABLES
    $(call soong_config_set,wifi,test5_variables,$(WIFI_TEST5_VARIABLES))     //  传输变量值
    $(warning " zukgit_1024 WIFI_TEST5_VARIABLES is $(WIFI_TEST5_VARIABLES)")
endif


//打印的输出值:
//build/make/core/board_config_wifi.mk:106: warning: " zukgit_1024 WIFI_TEST1_FALSE_BOOL is false"
//build/make/core/board_config_wifi.mk:111: warning: " zukgit_1024 WIFI_TEST2_TRUE_BOOL is true"
//build/make/core/board_config_wifi.mk:117: warning: " zukgit_1024 WIFI_TEST3_VALUE_VARIABLES_STRING is HELLO { A B C }"
//build/make/core/board_config_wifi.mk:123: warning: " zukgit_1024 WIFI_TEST4_VALUE_VARIABLES_NUMBER is 100"
//build/make/core/board_config_wifi.mk:129: warning: " zukgit_1024 WIFI_TEST5_VARIABLES is v2"

```

##### Android.bp配置对应的Soong编译类型结构体_3

hardware/interfaces/wifi/aidl/default/Android.bp


分别对下面三个结构体进行配置 使得Android.bp 能接受来自 Soong配置编译系统的变量
1.soong_config_module_type{} 定义了结构体的数据类型 
2.soong_config_string_variable{ }  // 定义在 soong_config_module_type 中定义的 variables menu 字符串(非纯数字字符串)变量的 可选值
3.xxxx_hal_cc_defaults{}  定义了当前需要配置的数据 的具体的值 

1. soong_config_module_type 定义了结构体的数据类型

```


soong_config_module_type {
    name: "wifi_hal_cc_defaults",
    module_type: "cc_defaults",
    config_namespace: "wifi",  // 【这里定义了 来自 .mk 的宏命名空间 前缀是 WIFI 】
    bool_variables: [     // 【 这里定义 Bool值 变量】
        "test1_false_bool", // WIFI_TEST1_FALSE_BOOL
        "test2_true_bool", // WIFI_TEST2_TRUE_BOOL
    ],
    value_variables: [   // 【这里定义从.mk传递过来的字符串 】
        "test3_value_variables_string", // WIFI_TEST3_VALUE_VARIABLES_STRING
        "test4_value_variables_number", // WIFI_TEST4_VALUE_VARIABLES_NUMBER
    ],
    
    variables: [    //【这里定义 字符串 枚举值 (必须非纯数字字符串)】
        "test5_variables", // WIFI_TEST5_VARIABLES
    ],
    properties: [
        "cppflags",
    ],
}


```



2.soong_config_string_variable{ }  
// 定义在 soong_config_module_type 中定义的 variables menu 字符串(非纯数字字符串)变量的 可选值

```

soong_config_string_variable {
    name: "test5_variables",
    values: ["v3" , "v2" ,"v1"] ,
}



```

3.xxxx_hal_cc_defaults{}  定义了当前需要配置的数据 的具体的值 

```

wifi_hal_cc_defaults {
    name: "android.hardware.wifi-service-cppflags-defaults",
    soong_config_variables: {
    
         test1_false_bool: {   // bool 值进行 匹配 , 只有为 true 时 才定义当前Android.bp的 Flag --> WIFI_TEST1_FALSE_BOOL
            cppflags: ["-DWIFI_TEST1_FALSE_BOOL"],
        },
        
        test2_true_bool: {
            cppflags: ["-DWIFI_TEST2_TRUE_BOOL"],
        },
        
        test3_value_variables_string: {  // 字符串值 进行 匹配 %s 用于从mk和soong传递字符串到 Android.bp 
            cppflags: ["-DWIFI_TEST3_VALUE_VARIABLES_STRING=%s"],
        },   
        test4_value_variables_number: {  // 字符串值(纯数字字符串) 匹配 %s 用于从mk和soong传递字符串到 Android.bp 
            cppflags: ["-DWIFI_TEST4_VALUE_VARIABLES_NUMBER=%s"],
        },
    
    
    // menu 枚举值 , 必须是非数字字符串 ，如果在mk定义的在Android.bp soong_config_string_variable 范围之外会编译报错
    // 例如: mk中定义 WIFI_TEST5_VARIABLES=v100 , 报错 error: <input>: Soong config property "test5_variables" must be one of [v3 v2 v1], found "v100"

        test5_variables: {  
            v3 :{
                cppflags: ["-DWIFI_TEST5_VARIABLES=v3"],
            },
             v2 :{
                cppflags: ["-DWIFI_TEST5_VARIABLES=v2"],
            },
             v1 :{
                cppflags: ["-DWWIFI_TEST5_VARIABLES=v1"],
            },
            conditions_default:{
                cppflags: ["-DWIFI_TEST5_VARIABLES=v1"],
            }
        },

    },
}




```



##### wifi_feature_flags.cpp文件中宏定义中打印宏信息_4

hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp


注意: 每个宏需要自己创建新的【Z_宏】 去打印这个 【Z_宏】 会省去很多编译麻烦

```



#define __PRINT_MACRO(x) #x
#define PRINT_MARCO(x) #x"=" __PRINT_MACRO(x)


#define Z_WIFI_TEST1_FALSE_BOOL (WIFI_TEST1_FALSE_BOOL)
#define Z_WIFI_TEST2_TRUE_BOOL (WIFI_TEST2_TRUE_BOOL)
#define Z_WIFI_TEST3_VALUE_VARIABLES_STRING (WIFI_TEST3_VALUE_VARIABLES_STRING)
#define Z_WIFI_TEST4_VALUE_VARIABLES_NUMBER (WIFI_TEST4_VALUE_VARIABLES_NUMBER)
#define Z_WIFI_TEST5_VARIABLES (WIFI_TEST5_VARIABLES)

# pragma message ("________zukgit_1_____")
# pragma message (PRINT_MARCO(Z_WIFI_TEST1_FALSE_BOOL))
# pragma message (PRINT_MARCO(Z_WIFI_TEST2_TRUE_BOOL))
# pragma message (PRINT_MARCO(Z_WIFI_TEST3_VALUE_VARIABLES_STRING))
# pragma message (PRINT_MARCO(Z_WIFI_TEST4_VALUE_VARIABLES_NUMBER))
# pragma message (PRINT_MARCO(Z_WIFI_TEST5_VARIABLES))
# pragma message ("________zukgit_2_____")



```


##### 编译对应模块查看预编译打印信息_5

```

lunch xxxx && mmm hardware/interfaces/wifi/aidl/default/



打印Log如下:


hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:134:10: warning: ________zukgit_1_____ [-W#pragma-messages]
  134 | # pragma message ("________zukgit_1_____")
      |          ^
  //     $(call soong_config_set,wifi,test1_false_bool,!!!false!!!【这里定义fasle 就不会发送到Android.bp和 Soong】)   
  // WIFI_TEST1_FALSE_BOOL 没有打印出来是因为 在 mk传给soong时设置为false 
hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:135:10: warning: Z_WIFI_TEST1_FALSE_BOOL=(WIFI_TEST1_FALSE_BOOL) [-W#pragma-messages]
  135 | # pragma message (PRINT_MARCO(Z_WIFI_TEST1_FALSE_BOOL))  

      |          ^
hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:136:10: warning: Z_WIFI_TEST2_TRUE_BOOL=(1) [-W#pragma-messages]
  136 | # pragma message (PRINT_MARCO(Z_WIFI_TEST2_TRUE_BOOL))
      |          ^
hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:137:10: warning: Z_WIFI_TEST3_VALUE_VARIABLES_STRING=(HELLO { A B C }) [-W#pragma-messages]
  137 | # pragma message (PRINT_MARCO(Z_WIFI_TEST3_VALUE_VARIABLES_STRING))
      |          ^
hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:138:10: warning: Z_WIFI_TEST4_VALUE_VARIABLES_NUMBER=(100) [-W#pragma-messages]
  138 | # pragma message (PRINT_MARCO(Z_WIFI_TEST4_VALUE_VARIABLES_NUMBER))
      |          ^
hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:139:10: warning: Z_WIFI_TEST5_VARIABLES=(v2) [-W#pragma-messages]
  139 | # pragma message (PRINT_MARCO(Z_WIFI_TEST5_VARIABLES))
      |          ^
hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:140:10: warning: ________zukgit_2_____ [-W#pragma-messages]
  140 | # pragma message ("________zukgit_2_____")
      |          ^
7 warnings generated.




```








#### 预编译Log的打印 

#pragma message("___1___zukgit")   用于打印当前地址的预编译Log


```

  // define Z_旧宏  (旧宏)  只打印 Z_旧宏的数据
  // 新定义一个Z_宏 来输出这个宏 避免编译报错   
  //   error: pragma message requires parenthesized string  【血泪史_得到的方法..新定义一个宏】
  //   error: too many arguments provided to function-like macro invocation
  // : error: use of undeclared identifier 'legacyToChipConcurrencyComboList'
  
  
#define __PRINT_MACRO(x) #x
#define PRINT_MARCO(x) #x"=" __PRINT_MACRO(x)


#define Z_WIFI_TEST1_FALSE_BOOL (WIFI_TEST1_FALSE_BOOL)   // define Z_旧宏  (旧宏) 
#define Z_WIFI_TEST2_TRUE_BOOL (WIFI_TEST2_TRUE_BOOL)
#define Z_WIFI_TEST3_VALUE_VARIABLES_STRING (WIFI_TEST3_VALUE_VARIABLES_STRING)
#define Z_WIFI_TEST4_VALUE_VARIABLES_NUMBER (WIFI_TEST4_VALUE_VARIABLES_NUMBER)
#define Z_WIFI_TEST5_VARIABLES (WIFI_TEST5_VARIABLES)

# pragma message ("________zukgit_1_____")
# pragma message (PRINT_MARCO(Z_WIFI_TEST1_FALSE_BOOL))
# pragma message (PRINT_MARCO(Z_WIFI_TEST2_TRUE_BOOL))
# pragma message (PRINT_MARCO(Z_WIFI_TEST3_VALUE_VARIABLES_STRING))
# pragma message (PRINT_MARCO(Z_WIFI_TEST4_VALUE_VARIABLES_NUMBER))
# pragma message (PRINT_MARCO(Z_WIFI_TEST5_VARIABLES))
# pragma message ("________zukgit_2_____")



// 打印示例: 
// hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:134:10: warning: ________zukgit_1_____ [-W#pragma-messages]
// hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:135:10: warning: Z_WIFI_TEST1_FALSE_BOOL=(WIFI_TEST1_FALSE_BOOL) [-W#pragma-messages]
// hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:136:10: warning: Z_WIFI_TEST2_TRUE_BOOL=(1) [-W#pragma-messages]
// hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:137:10: warning: Z_WIFI_TEST3_VALUE_VARIABLES_STRING=(HELLO { A B C }) [-W#pragma-messages]
// hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:138:10: warning: Z_WIFI_TEST4_VALUE_VARIABLES_NUMBER=(100) [-W#pragma-messages]
// hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:139:10: warning: Z_WIFI_TEST5_VARIABLES=(v2) [-W#pragma-messages]
// hardware/interfaces/wifi/aidl/default/wifi_feature_flags.cpp:140:10: warning: ________zukgit_2_____ [-W#pragma-messages]
 
 


```




### 打开 Wifi_Verbose详情开关

```

【WIFI详情开关描述】
Settings >System > About phone > tap "Build number" 4 times >Developer options
Setting > System > Advanced > Developer options >Enable WiFi Verbose Logging  [toogle open]


```



### Qcom查看SAR打印Log 


```

adb root && adb remount && adb shell setprop persist.radio.ctbk_log 5  && adb shell setprop persist.vendor.radio.ctbk_log 5   &&  adb shell setprop log.tag.QCSDK D
adb root && adb disable-verity && adb reboot bootloader
fastboot oem config cmdl androidboot.selinux=permissive 
fastboot reboot 
adb logcat | grep -e "SARCTRL" -e "MDMCTBK" -e "QCSDK"

```

Qcom-Sar 相关adb 命令
```
Qcom-Sar 相关adb 命令

adb root
adb disable-verity
adb reboot
adb wait-for-device
adb root
adb remount
adb shell setprop persist.radio.ctbk_log 5
adb shell setprop persist.vendor.radio.ctbk_log 5
adb push libmdmcutback.lib.so /vendor/lib/libmdmcutback.so
adb push libmdmcutback.lib64.so /vendor/lib64/libmdmcutback.so
adb pull /vendor/lib/libmdmcutback.so libmdmcutback.lib.so 
adb pull /vendor/lib64/libmdmcutback.so  libmdmcutback.lib64.so
adb push ctbk_cfg.xml /vendor/etc/motorola/mdmctbk/ctbk_cfg.xml
adb reboot

 adb root && adb remount && adb shell setprop persist.radio.ctbk_log 5 && adb shell setprop persist.vendor.radio.ctbk_log 5 && adb reboot




关闭sar服务命令:
adb root && adb shell setprop persist.vendor.radio.disable_sar  1  && adb reboot


打开sar服务命令
adb root  &&  adb shell setprop persist.vendor.radio.disable_sar  0 && adb reboot


删除原有的 aplogd 开机Log
adb root && adb remount &&  adb shell  "rm -fr /data/vendor/aplogd/*"


导出 aplogd 
adb root && adb remount && adb pull /data/vendor/aplogd


导出 bug2go
adb root && adb remount && adb pull /data/vendor/bug2go/

删除原有的 bug2go Log
adb root && adb remount &&  adb shell  "rm -fr /data/vendor/bug2go/*"


设置蓝牙测试模式
adb root && adb remount &&  adb  shell   setprop  persist.vendor.radio.btsar_test_mode  true  

 
导入 libmdmcutback.so 
adb root && adb remount && adb push libmdmcutback.so /vendor/lib64/libmdmcutback.so && adb reboot 


```

```

adb push ./ctbk_cfg.xml /vendor/etc/motorola/mdmctbk/
adb push ./ctbk_cfg.xml /etc/motorola/mdmctbk/
adb push ./ctbk_cfg.xml /system/etc/motorola/mdmctbk/
adb push ./rowe_ctbk_cfg.xml /system/etc/motorola/mdmctbk/

cat /vendor/etc/motorola/mdmctbk/ctbk_cfg.xml
cat /etc/motorola/mdmctbk/ctbk_cfg.xml
cat /system/etc/motorola/mdmctbk/ctbk_cfg.xml
cat /system/etc/motorola/mdmctbk/rowe_ctbk_cfg.xml



adb pull  /vendor/etc/motorola/mdmctbk | adb pull /etc/motorola/mdmctbk/ | adb pull /system/etc/motorola/mdmctbk/ 


```


查看生效的sar配置文件
```
adb logcat | grep "SARCTRL : targetFilepath"


: /system/etc/motorola/mdmctbk/rowe_ctbk_cfg.xml
Stream-m.txt:16202:07-25 18:10:32.238  2364  2364 D SARCTRL : targetFilepath: /system/etc/motorola/mdmctbk/rowe_ctbk_cfg.xml

```

5g_split_band 区分

```
5g_split_band 区分

              band_index   channel     frequency    middle_freq_range  band_name
5G band1      band1        36~48       5150~5250    5180~5240          U-NII-1
5G band2      band2        52~64       5250~5350    5260~5320          U-NII-2A
5G band3      band3        100~144     5470~5725    5500~5700          U-NII-2C
5G band4      band4        149~165     5745~5850    5745~5825          U-NII-3
```

```

fcc wifi tx pwr 5g split band  在测试Feature 时
1. 5GHZ At Head     只在  ModemDebug 中的 table = 4   ## State4_5GHZ 5G && At head && WLAN only 状态下生效
2. 5GHZ BodyWorn    只在  ModemDebug 中的 table = 6   ## State6 5GHZ 5G && BodyWorn && WLAN only 状态下生效
3. 5GHZ Hand Held   只在  ModemDebug 中的 table = 9   ## State9_5GHZ 5G && Hand-Held && WLAN only 状态下生效
4. 5GHZ At Head     只在  ModemDebug 中的 table = 10  ##State10_5GHZ 5G && USB Connected && WLAN only 状态下生效







```


### wifi_tx0_sensor_config配置

```

        <!--from back view-->    BackView 为启始
        <sensor type="cap_bottom_right"         index="2">CapSense Ch0</sensor> <!--ANT0, CS0-->    【  000001 】1
        <sensor type="cap_bottom_left"          index="3">CapSense Ch1</sensor> <!--ANT1, CS4-->    【  000010 】2
        <sensor type="cap_top_left"             index="4">CapSense Ch4</sensor> <!--ANT6&7, CS5-->  【  000100 】4
        <sensor type="cap_top_right"            index="5">CapSense Ch2</sensor> <!--ANT3&4, CS6-->  【  001000 】8
        <sensor type="cap_top_middle"           index="6">CapSense Ch3</sensor> <!--ANT5, CS7-->    【  010000 】16
        <sensor type="xxxxx"                      index="7">CapSense ChXXX</sensor> <!--ANT5, CS7-->【  100000 】32

(cap_top_middle|cap_top_left)
 <wifi_tx0_sensor_config>20</wifi_tx0_sensor_config>
 
 
 
 
cap_top_middle (CapSense Ch3)(ANT5, CS7)    === 【  010000 】 === 16
cap_top_left   (CapSense Ch4)(ANT6&7, CS5)  === 【  000100 】 === 4

16 | 4 == 20 



 <wifi_tx0_sensor_config>24 </wifi_tx0_sensor_config>
 01001000===72   S4 S7   最小的是1开始数    tx0_sensor : S4         tx0_sensor : S7
 00011000===24   S4 S5 
 【8_7_6_5_4_3_2_1】  S4==第4== SarSensor index="4"    <sensor index="4">
 【8_7_6_5_4_3_2_1】  S5==第5== SarSensor index="5"	   <sensor index="5">



                      【8_7_6_5_4_3_2_1】 
                       00011000===24
 <!--from back view-->                  从backview 开始算第一个 , 所以顶一个的index 不一定为1 可能为2 
  <sensor index="2">
  <sensor index="3">
  <sensor index="4">
  <sensor index="5">     √(S4)
  <sensor index="6">     √(S5)
  <sensor index="7">
  <sensor index="8">     
  <sensor index="9">
  
  
                      【8_7_6_5_4_3_2_1】 
                       01001000===72               从backview 开始算第一个
  <sensor index="1">
  <sensor index="2">
  <sensor index="3">
  <sensor index="4">     √(S4)
  <sensor index="5">
  <sensor index="6">
  <sensor index="7">     √(S7)
  <sensor index="8">
  
  
  
  

  
```



### MTK SarContrl Log 开关

```

adb root && adb remount && adb shell setprop persist.radio.ctbk_log 5  && adb shell setprop persist.vendor.radio.ctbk_log 5   &&  adb shell setprop log.tag.QCSDK D
adb root && adb disable-verity && adb reboot bootloader
fastboot oem config cmdl androidboot.selinux=permissive 
fastboot reboot 
adb logcat | grep -e "SARCTRL" -e "MDMCTBK" -e "QCSDK" -e "setBtTxPower"


```

#### MTK平台打开BtSar开关
```
编译开关  

# MTK BT SAR
MSSI_MTK_BT_SAR_SUPPORT = yes

```


#### MTK平台查看 SarWifi服务接口

```
MTK 平台 Sar 服务正常进程查询

adb root && adb remount && adb shell cat  /vendor/bin/hw/motorola.hardware.sarwifi-srv
有打印乱码  有对应文件  /vendor/bin/hw/motorola.hardware.sarwifi-srv


adb shell service list  | grep sar 
有打印服务   xxxx.hardware.sarwifi.IxxxWifi/default: [xxxx.hardware.sarwifi.IxxxWifi]


adb shell ps  -ATMf | grep sar
打印 两个进程 一个客户端 一个服务端
system          1060  1060     1    1 2024-01-04 15:16:22 ?        00:00:00 xxxx.hardware.sarwifi-srv
u0_a127         2394  2394   964   17 2024-01-04 15:16:31 ?        00:00:00 com.xxxx.sarxcontrol

```



#### MTK txpowerctrl 配置文件 路径
```
adb root && adb remount 
adb  pull  /vendor/firmware/txpowerctrl.cfg
adb push ./txpowerctrl.cfg   /vendor/firmware/


```


###  Qcom抓取SniffLog

```
 高通项目的 IPLog Tool 已经能抓取sniffLog 了 , MTK 平台还不行, 需要抓取建议使用Qcom设备

```

<img src="/public/zimage/qocm_issue/sniffLog_1.jpg"/>



### Qcom更换regdb.bin文件

```

导入命令:
adb root && adb remount && adb shell "mount -o rw,remount /vendor/firmware_mnt" && adb push .\regdb.bin  /vendor/firmware_mnt/image/ && adb reboot


检测 regdb.bin的加载
adb logcat -b all | grep regdb.bin

11:48:33.532  1535  1597 I cnss-daemon: wlfw_send_bdf_download_req: BDF file : regdb.bin
11:48:33.532     0     0 W cnss-daemon: wlfw_send_bdf_download_req: BDF file : regdb.bin

```

### iw设置WIFI国家码
```
adb shell
cmd wifi force-country-code enabled GT                // 设置成GT 危地马拉

iw reg get     // 查看 国家码设置
______________________________________________________________________
global
country 00: DFS-UNSET
        (2402 - 2472 @ 40), (6, 20), (N/A)
        (2457 - 2482 @ 20), (6, 20), (N/A), AUTO-BW, PASSIVE-SCAN
        (2474 - 2494 @ 20), (6, 20), (N/A), NO-OFDM, PASSIVE-SCAN
        (5170 - 5250 @ 80), (6, 20), (N/A), AUTO-BW, PASSIVE-SCAN
        (5250 - 5330 @ 80), (6, 20), (0 ms), DFS, AUTO-BW, PASSIVE-SCAN
        (5490 - 5730 @ 160), (6, 20), (0 ms), DFS, PASSIVE-SCAN
        (5735 - 5835 @ 80), (6, 20), (N/A), PASSIVE-SCAN
        (57240 - 63720 @ 2160), (N/A, 0), (N/A)



phy#0 (self-managed)
country GT: DFS-FCC
        (2402 - 2482 @ 40), (N/A, 30), (N/A), AUTO-BW
        (5170 - 5250 @ 80), (N/A, 30), (N/A), AUTO-BW
        (5250 - 5330 @ 80), (N/A, 24), (0 ms), DFS, AUTO-BW
        (5490 - 5730 @ 160), (N/A, 24), (0 ms), DFS, AUTO-BW
        (5735 - 5875 @ 80), (N/A, 30), (N/A), AUTO-BW
______________________________________________________________________



iw dev                     // 查看热点频段信息
______________________________________________________________________
phy#0
        Interface p2p0
                ifindex 23
                wdev 0x2
                addr 66:11:a4:cc:ce:13
                type P2P-device
        Interface wlan0
                ifindex 22
                wdev 0x1
                addr 3e:33:32:dc:74:81
                ssid   30XXX 
                type AP
                channel 157 (5785 MHz), width: 80 MHz, center1: 5775 MH
______________________________________________________________________


```


### 查看高通 Build_ID

```
1.在 项目的 AOSP/vendor/qcom/nonhlos  目录会生成高通的 Non-HLOS  在改目录编译完成时会产出 Ver_Info.txt 
文件  该 Ver_Info.txt  标注了 高通项目的 Build_ID  , 该文件是由 nonhlos 目录中的X子目录中的 build.py生成

## 
cd ./vendor/qcom/nonhlos  && zfilesearch_D6.sh  Ver_Info    

================      Begin================
 【index : 000001】   【 MD5(32):  c18537f8601ee408c751b58ffd3b85e9 】 Size[        1605] AOSP/Vendor_Part/vendor/qcom/nonhlos/XXXSubXXX/common/build/Ver_Info.txt
================      End================

╤╤╤╤╤╤╤╤╤╤Ver_Info.txt╤╤╤╤╤╤╤╤╤╤╤╤ 

{
    "Image_Build_IDs": {
        "adsp": "ADSP.HT.5.7-01095.2-WAIPIO-2", 
        "aop": "AOP.HO.4.0-00482-WAIPIO_E-1", 
        "apps_LE": "LE.UM.5.3.1.r1-13600-genericarmv8-64.0-1", 
        "apps_kernel": "KERNEL.PLATFORM.1.0.r1-12400-kernel.0-1", 
        "apps_qssi12": "LA.QSSI.12.0.r1-09300-qssi.0-1", 
        "apps_qssi13": "LA.QSSI.13.0.r1-07600.03-qssi.0-1", 
        "apps_vendor": "LA.VENDOR.1.0.r1-18400-WAIPIO.QSSI13.0-1", 
        "boot": "BOOT.MXF.2.0-00925-WAIPIO-1", 
        "btfm_CHK": "BTFM.CHE.2.1.6-00066-QCACHROMZ-1", 
        "btfm_che": "BTFM.CHE.3.2.1-00269-QCACHROMZ-2", 
        "btfm_msl": "BTFW.MOSELLE.1.1.1-00185-MSL_PATCHZ-1", 
        "camera": "CAMERA.LA.2.0.r1-08100-WAIPIO.0-1", 
        "cdsp": "CDSP.HT.2.8.1-00057-NETRANI-1", 
        "common": "Netrani.LA.1.0.r1-00280-STD.PROD-1", 
        "cpucp": "CPUCP.FW.1.0-00105-WAIPIO.EXT-1", 
        "cpuss_vm": "CPUSS.CPUSYS.VM.1.0-00024-WAIPIO.EXT-1", 
        "display": "DISPLAY.LA.2.0.r1-08700-WAIPIO.0-1", 
        "glue": "GLUE.NETRANI_LA.1.0-00130-NOOP_TEST-1", 
        "modem": "MPSS.DE.2.1-00247.19-NETRANI_GEN_PACK-1", 
        "tz": "TZ.XF.5.18-00293-SPF.WAIPIO-2", 
        "tz_apps": "TZ.APPS.1.18-00297-SPF.WAIPIO-1", 
        "video": "VIDEO.LA.2.0.r1-06500-WAIPIO.0-1", 
        "wlan_hl": "WLAN.HL.3.4.3-00199.1-QCAHLSWMTPLZ-1", 
        "wlan_msl": "WLAN.MSL.2.0-00587.10-QCAMSLSWPLZ-1"
    }, 
    "Metabuild_Info": {
        "Meta_Build_ID": "Netrani.LA.1.0.r1-00280-STD.PROD-1", 
        "Product_Flavor": "['asic']", 
        "Time_Stamp": "2022-12-06 12:16:34"
    }, 
    "Version": "1.0"
}


╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```


### Qcom_kernel-panic之提供.elf文件

```
1. 在 artifactory 中对应版本的  33.60-23/10dc7-0f770 大版本数字小版本数字都对应的 fastboot_xxx_intcfg-test-keys_global_US 解压版本文件中

mbm-ci\DEBUG\hypvm.elf
mbm-ci\DEBUG\mon.elf
mbm-ci\DEBUG\qsee.elf
等 其他 elf 文件 提交给 Qcom

2. 在 对应版本的 ReleaseNoteXXXX.html 中找到 Modem Release Notes
SM6450 de21.01 Release DE2.1-6450-01.57 就是 对应的 Modem  版本 
Build Artifacs:中  m6450n/binaries/DExxxx_m6450n_binaries.tar.gz   对应的 二进制文档
DExxxx_m6450n_binaries.tar.gz 有对应的 Modem Release 编译版本  里面有 modem_proc 信息  
提供　DExxxx_m6450n_binaries.tar.gz 给Qcom

3. 下载对应的  ReleaseNoteXXXX.html 中的 Vendor 代码部分  只更新一个仓库去拿最新的 about.html 

repo init -u xxxxxx
repo sync vendor/qcom/nonhlos/Netrani.XXX/common      ##     只拉取 一个 git 分支

tip: 存在 正确 about.html 文件的目录中 存在如下唯一的 Ver_Info.txt 文件
【 cd ./vendor/qcom/nonhlos/ && find . -name "*Ver_Info*" && find . -name "*about.html*" 】
./AAA/common/build/Ver_Info.txt
./111/common/build/about.html
./AAA/common/build/about.html  【与 Ver_Info同目录 是正确的 about.html   repo sync 此目录】
./222/common/build/about.html


vendor/qcom/nonhlos/Netrani.XXX/common/build/Ver_Info.txt

4.在 XXX-ramdump.zip 文件夹中存在  summary.txt 里面包含了 版本的信息

 ro.build.fingerprint    : XXXXXX60-23/10dc7-0f770:userdebug
 ro.build.version.qcom   : XXXX.XXX.1.0.R1.11.00.00.816.199
 version-baseband        : M6450N_DE21_34.290.01.57R 
 
```

### wifi断连reason列表

```
/external/iw/reason.c#7


static const char *reason_table[] = {
	[1] = "Unspecified",
	[2] = "Previous authentication no longer valid",
	[3] = "Deauthenticated because sending station is leaving (or has left) the IBSS or ESS",
	[4] = "Disassociated due to inactivity",
	[5] = "Disassociated because AP is unable to handle all currently associated STA",
	[6] = "Class 2 frame received from non-authenticated station",
	[7] = "Class 3 frame received from non-authenticated station",
	[8] = "Disassociated because sending station is leaving (or has left) the BSS",
	[9] = "Station requesting (re)association is not authenticated with responding station",
	[10] = "Disassociated because the information in the Power Capability element is unacceptable",
	[11] = "Disassociated because the information in the Supported Channels element is unacceptable",
	[13] = "Invalid information element",
	[14] = "MIC failure",
	[15] = "4-way handshake timeout",
	[16] = "Group key update timeout",
	[17] = "Information element in 4-way handshake different from (Re-)associate request/Probe response/Beacon",
	[18] = "Multicast cipher is not valid",
	[19] = "Unicast cipher is not valid",
	[20] = "AKMP is not valid",
	[21] = "Unsupported RSNE version",
	[22] = "Invalid RSNE capabilities",
	[23] = "IEEE 802.1X authentication failed",
	[24] = "Cipher Suite rejected per security policy",
	[31] = "TS deleted because QoS AP lacks sufficient bandwidth for this QoS STA due to a change in BSS service characteristics or operational mode",
	[32] = "Disassociated for unspecified QoS-related reason",
	[33] = "Disassociated because QAP lacks sufficient bandwidth for this STA",
	[34] = "Disassociated because of excessive frame losses and/or poor channel conditions",
	[35] = "Disassociated because QSTA is transmitting outside the limits of its polled TXOPs",
	[36] = "Requested from peer QSTA as the QSTA is leaving the QBSS (or resetting)",
	[37] = "Requested from peer QSTA as it does not want to use Traffic Stream",
	[38] = "Requested from peer QSTA as the QSTA received frames indicated Traffic Stream for which it has not set up",
	[39] = "Requested from peer QSTA due to time out",
	[40] = "Requested from peer QSTA as the QSTA is leaving the QBSS (or resetting)",
	[41] = "Requested from peer QSTA as it does not want to receive frames directly from the QSTA",
	[42] = "Requested from peer QSTA as the QSTA received DLP frames for which it has not set up",
	[43] = "Requested from peer QSTA as it does not want to use Block Ack",
	[44] = "Requested from peer QSTA as the QSTA received frames indicated Block Acknowledgement policy for which it has not set up",
	[45] = "Peer QSTA does not support the requested cipher suite",
};




```


### 查看安卓Msi和Vendor版本

```

adb shell getprop | grep first_api
adb shell getprop | grep api

[ro.board.api_level]: [31]
[ro.board.first_api_level]: [31]    ——————————————【 Vendor 底层版本 31=Android 12】

[ro.product.first_api_level]: [33]  ——————————————【 MSI 上层版本    33=Android 13】
[ro.vendor.api_level]: [31]

T+S 配置 MSI=T Vendor=S 配置

U     API34： Android 14 (Developer Preview)
T     API33： Android 13 
S     API32： Android 12
S     API31： Android 12
R     API30： Android 11
Q     API29： Android 10.0 Android Q
P     API28： Android 9.0
      API27： Android 8.1 
O     API26： Android 8.0 O
      API25： Android 7.1 N 
N     API24： Android 7.0 N 
M     API23： Android 6.0 M
      API22： Android 5.1.1 L 
L     API21： Android 5.0.1 L 
      API20： Andrroid 4.4W.2
K     API19： Android 4.4 KitKat 
J     API18： Android 4.3 Jelly Bean 
      API17： Android 4.2 Jelly Bean 
      API16： Android 4.1 Jelly Bean 
      API15： Android 4.0.3 - 4.0.4 Ice Cream Sandwich 
      API14： Android 4.0 - 4.0.2 Ice Cream Sandwich 
      API13： Android 3.2 Honeycomb 
      API12： Android 3.1 Honeycomb 
      API11： Android 3.0 Honeycomb 
      API10： Android 2.3.3-2.3.7 Gingerbread 
      API9：  Android 2.3 - 2.3.2 Gingerbread 
      API8：  Android 2.2 - 2.2.3 Froyo 
      API7：  Android 2.1 Éclair 
      API6：  Android 2.0.1 Éclair 
      API5：  Android 2.0 Éclair 
      API4：  Android 1.6 Donut 
      API3：  Android 1.5 Cupcake 
      API2：  Android 1.1 Petit Four 
	  API1：  Android 1.0 SDK API level 1


```


### user 版本转 debug版本

```
在fastboot模式下 执行 刷入 boot 分区 debug-boot.img 的 操作 就可以把 user 版本转为 debug 版本

fastboot flash boot debug-boot.img




在fastboot模式下 执行 刷入vendor_boot 分区 debug-boot.img 的 操作 就可以把 挂载 /vendor/firmware/ 目录

fastboot flash vendor_boot vendor_boot-debug.img

```


### 查看国家码频段 

adb shell cmd wifi force-country-code enabled US       【设置国家码 US 】


adb shell cmd wifi force-country-code enabled CN       【设置国家码 CN 】


adb shell wpa_cli get_capability freq   【查看当前国家码对应的频段信息】



```
wpa_cli -h   帮助信息打印 

wpa_cli [-p<path to ctrl sockets>] [-i<ifname>] [-hvBr] [-a<action file>] \
        [-P<pid file>] [-g<global ctrl>] [-G<ping interval>] \
        [-s<wpa_client_socket_file_path>] [command..]
  -h = help (show this usage text)
  -v = shown version information
  -a = run in daemon mode executing the action file based on events from
       wpa_supplicant
  -r = try to reconnect when client socket is disconnected.
       This is useful only when used with -a.
  -B = run a daemon in the background
  default path: /data/vendor/wifi/wpa/sockets
  default interface: first interface found in socket path
commands:
  status [verbose] = get current WPA/EAPOL/EAP status
  ifname = get current interface name
  ping = pings wpa_supplicant
  relog = re-open log-file (allow rolling logs)
  note <text> = add a note to wpa_supplicant debug log
  mib = get MIB variables (dot1x, dot11)
  help [command] = show usage help
  interface [ifname] = show interfaces/select interface
  level <debug level> = change debug level
  license = show full wpa_cli license
  quit = exit wpa_cli
  set = set variables (shows list of variables when run without arguments)
  dump = dump config variables
  get <name> = get information
  driver_flags = list driver flags
  logon = IEEE 802.1X EAPOL state machine logon
  logoff = IEEE 802.1X EAPOL state machine logoff
  pmksa = show PMKSA cache
  pmksa_flush = flush PMKSA cache entries
  reassociate = force reassociation
  reattach = force reassociation back to the same BSS
  preauthenticate <BSSID> = force preauthentication
  identity <network id> <identity> = configure identity for an SSID
  password <network id> <password> = configure password for an SSID
  new_password <network id> <password> = change password for an SSID
  pin <network id> <pin> = configure pin for an SSID
  otp <network id> <password> = configure one-time-password for an SSID
  psk_passphrase <network id> <PSK/passphrase> = configure PSK/passphrase for an SSID
  passphrase <network id> <passphrase> = configure private key passphrase
    for an SSID
  sim <network id> <pin> = report SIM operation result
  bssid <network id> <BSSID> = set preferred BSSID for an SSID
  bssid_ignore <BSSID> = add a BSSID to the list of temporarily ignored BSSs
  bssid_ignore clear = clear the list of temporarily ignored BSSIDs
  bssid_ignore = display the list of temporarily ignored BSSIDs
  blacklist = deprecated alias for bssid_ignore
  log_level <level> [<timestamp>] = update the log level/timestamp
  log_level = display the current log level and log options
  list_networks = list configured networks
  select_network <network id> = select a network (disable others)
  enable_network <network id> = enable a network
  disable_network <network id> = disable a network
  add_network = add a network
  remove_network <network id> = remove a network
  set_network <network id> <variable> <value> = set network variables (shows
    list of variables when run without arguments)
  get_network <network id> <variable> = get network variables
  dup_network <src network id> <dst network id> <variable> = duplicate network variables
  list_creds = list configured credentials
  add_cred = add a credential
  remove_cred <cred id> = remove a credential
  set_cred <cred id> <variable> <value> = set credential variables
  get_cred <cred id> <variable> = get credential variables
  save_config = save the current configuration
  disconnect = disconnect and wait for reassociate/reconnect command before
    connecting
  reconnect = like reassociate, but only takes effect if already disconnected
  scan = request new BSS scan
  scan_results = get latest scan results
  abort_scan = request ongoing scan to be aborted
  bss <<idx> | <bssid>> = get detailed scan result info
  get_capability <eap/pairwise/group/key_mgmt/proto/auth_alg/channels/freq/modes> = get capabilities
  reconfigure = force wpa_supplicant to re-read its configuration file
  terminate = terminate wpa_supplicant
  interface_add <ifname> <confname> <driver> <ctrl_interface> <driver_param>
    <bridge_name> <create> <type> = adds new interface, all parameters but
    <ifname> are optional. Supported types are station ('sta') and AP ('ap')
  interface_remove <ifname> = removes the interface
  interface_list = list available interfaces
  ap_scan <value> = set ap_scan parameter
  scan_interval <value> = set scan_interval parameter (in seconds)
  bss_expire_age <value> = set BSS expiration age parameter
  bss_expire_count <value> = set BSS expiration scan count parameter
  bss_flush <value> = set BSS flush age (0 by default)
  ft_ds <addr> = request over-the-DS FT with <addr>
  wps_pbc [BSSID] = start Wi-Fi Protected Setup: Push Button Configuration
  wps_pin <BSSID> [PIN] = start WPS PIN method (returns PIN, if not hardcoded)
  wps_check_pin <PIN> = verify PIN checksum
  wps_cancel Cancels the pending WPS operation
  wps_nfc [BSSID] = start Wi-Fi Protected Setup: NFC
  wps_nfc_config_token <WPS|NDEF> = build configuration token
  wps_nfc_token <WPS|NDEF> = create password token
  wps_nfc_tag_read <hexdump of payload> = report read NFC tag with WPS data
  nfc_get_handover_req <NDEF> <WPS> = create NFC handover request
  nfc_get_handover_sel <NDEF> <WPS> = create NFC handover select
  nfc_report_handover <role> <type> <hexdump of req> <hexdump of sel> = report completed NFC handover
  wps_reg <BSSID> <AP PIN> = start WPS Registrar to configure an AP
  wps_ap_pin [params..] = enable/disable AP PIN
  wps_er_start [IP address] = start Wi-Fi Protected Setup External Registrar
  wps_er_stop = stop Wi-Fi Protected Setup External Registrar
  wps_er_pin <UUID> <PIN> = add an Enrollee PIN to External Registrar
  wps_er_pbc <UUID> = accept an Enrollee PBC using External Registrar
  wps_er_learn <UUID> <PIN> = learn AP configuration
  wps_er_set_config <UUID> <network id> = set AP configuration for enrolling
  wps_er_config <UUID> <PIN> <SSID> <auth> <encr> <key> = configure AP
  wps_er_nfc_config_token <WPS/NDEF> <UUID> = build NFC configuration token
  ibss_rsn <addr> = request RSN authentication with <addr> in IBSS
  sta <addr> = get information about an associated station (AP)
  all_sta = get information about all associated stations (AP)
  list_sta = list all stations (AP)
  deauthenticate <addr> = deauthenticate a station
  disassociate <addr> = disassociate a station
  chan_switch <cs_count> <freq> [sec_channel_offset=] [center_freq1=] [center_freq2=] [bandwidth=] [blocktx] [ht|vht] = CSA parameters
  suspend = notification of suspend/hibernate
  resume = notification of resume/thaw
  roam <addr> = roam to the specified BSS
  p2p_find [timeout] [type=*] = find P2P Devices for up-to timeout seconds
  p2p_stop_find = stop P2P Devices search
  p2p_asp_provision <addr> adv_id=<adv_id> conncap=<conncap> [info=<infodata>] = provision with a P2P ASP Device
  p2p_asp_provision_resp <addr> adv_id=<adv_id> [role<conncap>] [info=<infodata>] = provision with a P2P ASP Device
  p2p_connect <addr> <"pbc"|PIN> [ht40] = connect to a P2P Device
  p2p_listen [timeout] = listen for P2P Devices for up-to timeout seconds
  p2p_group_remove <ifname> = remove P2P group interface (terminate group if GO)
  p2p_group_add [ht40] = add a new P2P group (local end as GO)
  p2p_group_member <dev_addr> = Get peer interface address on local GO using peer Device Address
  p2p_prov_disc <addr> <method> = request provisioning discovery
  p2p_get_passphrase = get the passphrase for a group (GO only)
  p2p_serv_disc_req <addr> <TLVs> = schedule service discovery request
  p2p_serv_disc_cancel_req <id> = cancel pending service discovery request
  p2p_serv_disc_resp <freq> <addr> <dialog token> <TLVs> = service discovery response
  p2p_service_update = indicate change in local services
  p2p_serv_disc_external <external> = set external processing of service discovery
  p2p_service_flush = remove all stored service entries
  p2p_service_add <bonjour|upnp|asp> <query|version> <response|service> = add a local service
  p2p_service_rep asp <auto> <adv_id> <svc_state> <svc_string> [<svc_info>] = replace local ASP service
  p2p_service_del <bonjour|upnp> <query|version> [|service] = remove a local service
  p2p_reject <addr> = reject connection attempts from a specific peer
  p2p_invite <cmd> [peer=addr] = invite peer
  p2p_peers [discovered] = list known (optionally, only fully discovered) P2P peers
  p2p_peer <address> = show information about known P2P peer
  p2p_set <field> <value> = set a P2P parameter
  p2p_flush = flush P2P state
  p2p_cancel = cancel P2P group formation
  p2p_unauthorize <address> = unauthorize a peer
  p2p_presence_req [<duration> <interval>] [<duration> <interval>] = request GO presence
  p2p_ext_listen [<period> <interval>] = set extended listen timing
  p2p_remove_client <address|iface=address> = remove a peer from all groups
  vendor_elem_add <frame id> <hexdump of elem(s)> = add vendor specific IEs to frame(s)
    0: Probe Req (P2P), 1: Probe Resp (P2P) , 2: Probe Resp (GO), 3: Beacon (GO), 4: PD Req, 5: PD Resp, 6: GO Neg Req, 7: GO Neg Resp, 8: GO Neg Conf, 9: Inv Req, 10: Inv Resp, 11: Assoc Req (P2P), 12: Assoc Resp (P2P)
  vendor_elem_get <frame id> = get vendor specific IE(s) to frame(s)
    0: Probe Req (P2P), 1: Probe Resp (P2P) , 2: Probe Resp (GO), 3: Beacon (GO), 4: PD Req, 5: PD Resp, 6: GO Neg Req, 7: GO Neg Resp, 8: GO Neg Conf, 9: Inv Req, 10: Inv Resp, 11: Assoc Req (P2P), 12: Assoc Resp (P2P)
  vendor_elem_remove <frame id> <hexdump of elem(s)> = remove vendor specific IE(s) in frame(s)
    0: Probe Req (P2P), 1: Probe Resp (P2P) , 2: Probe Resp (GO), 3: Beacon (GO), 4: PD Req, 5: PD Resp, 6: GO Neg Req, 7: GO Neg Resp, 8: GO Neg Conf, 9: Inv Req, 10: Inv Resp, 11: Assoc Req (P2P), 12: Assoc Resp (P2P)
  wfd_subelem_set <subelem> [contents] = set Wi-Fi Display subelement
  wfd_subelem_get <subelem> = get Wi-Fi Display subelement
  fetch_anqp = fetch ANQP information for all APs
  stop_fetch_anqp = stop fetch_anqp operation
  interworking_select [auto] = perform Interworking network selection
  interworking_connect <BSSID> = connect using Interworking credentials
  interworking_add_network <BSSID> = connect using Interworking credentials
  anqp_get <addr> <info id>[,<info id>]... = request ANQP information
  gas_request <addr> <AdvProtoID> [QueryReq] = GAS request
  gas_response_get <addr> <dialog token> [start,len] = Fetch last GAS response
  hs20_anqp_get <addr> <subtype>[,<subtype>]... = request HS 2.0 ANQP information
  nai_home_realm_list <addr> <home realm> = get HS20 nai home realm list
  hs20_icon_request <addr> <icon name> = get Hotspot 2.0 OSU icon
  fetch_osu = fetch OSU provider information from all APs
  cancel_fetch_osu = cancel fetch_osu command
  sta_autoconnect <0/1> = disable/enable automatic reconnection
  tdls_discover <addr> = request TDLS discovery with <addr>
  tdls_setup <addr> = request TDLS setup with <addr>
  tdls_teardown <addr> = tear down TDLS with <addr>
  tdls_link_status <addr> = TDLS link status with <addr>
  wmm_ac_addts <uplink/downlink/bidi> <tsid=0..7> <up=0..7> [nominal_msdu_size=#] [mean_data_rate=#] [min_phy_rate=#] [sba=#] [fixed_nominal_msdu] = add WMM-AC traffic stream
  wmm_ac_delts <tsid> = delete WMM-AC traffic stream
  wmm_ac_status = show status for Wireless Multi-Media Admission-Control
  tdls_chan_switch <addr> <oper class> <freq> [sec_channel_offset=] [center_freq1=] [center_freq2=] [bandwidth=] [ht|vht] = enable channel switching with TDLS peer
  tdls_cancel_chan_switch <addr> = disable channel switching with TDLS peer <addr>
  signal_poll = get signal parameters
  signal_monitor = set signal monitor parameters
  pktcnt_poll = get TX/RX packet counters
  reauthenticate = trigger IEEE 802.1X/EAPOL reauthentication
  wnm_sleep <enter/exit> [interval=#] = enter/exit WNM-Sleep mode
  wnm_bss_query <query reason> [list] [neighbor=<BSSID>,<BSSID information>,<operating class>,<channel number>,<PHY type>[,<hexdump of optional subelements>] = Send BSS Transition Management Query
  raw <params..> = Sent unprocessed command
  flush = flush wpa_supplicant state
  driver <command> = driver private commands
  radio_work = radio_work <show/add/done>
  vendor <vendor id> <command id> [<hex formatted command argument>] = Send vendor command
  neighbor_rep_request [ssid=<SSID>] [lci] [civic] = Trigger request to AP for neighboring AP report (with optional given SSID in hex or enclosed in double quotes, default: current SSID; with optional LCI and location civic request)
  erp_flush = flush ERP keys
  mac_rand_scan <scan|sched|pno|all> enable=<0/1> [addr=mac-address mask=mac-address-mask] = scan MAC randomization
  get_pref_freq_list <interface type> = retrieve preferred freq list for the specified interface type
  p2p_lo_start <freq> <period> <interval> <count> = start P2P listen offload
  p2p_lo_stop = stop P2P listen offload
  dpp_qr_code report a scanned DPP URI from a QR Code
  dpp_bootstrap_gen type=<qrcode> [chan=..] [mac=..] [info=..] [curve=..] [key=..] = generate DPP bootstrap information
  dpp_bootstrap_remove *|<id> = remove DPP bootstrap information
  dpp_bootstrap_get_uri <id> = get DPP bootstrap URI
  dpp_bootstrap_info <id> = show DPP bootstrap information
  dpp_bootstrap_set <id> [conf=..] [ssid=<SSID>] [ssid_charset=#] [psk=<PSK>] [pass=<passphrase>] [configurator=<id>] [conn_status=#] [akm_use_selector=<0|1>] [group_id=..] [expiry=#] [csrattrs=..] = set DPP configurator parameters
  dpp_auth_init peer=<id> [own=<id>] = initiate DPP bootstrapping
  dpp_listen <freq in MHz> = start DPP listen
  dpp_stop_listen = stop DPP listen
  dpp_configurator_add [curve=..] [key=..] = add DPP configurator
  dpp_configurator_remove *|<id> = remove DPP configurator
  dpp_configurator_get_key <id> = Get DPP configurator's private key
  dpp_configurator_sign conf=<role> configurator=<id> = generate self DPP configuration
  dpp_pkex_add add PKEX code
  dpp_pkex_remove *|<id> = remove DPP pkex information
  dpp_controller_start [tcp_port=<port>] [role=..] = start DPP controller
  dpp_controller_stop = stop DPP controller
  dpp_chirp own=<BI ID> iter=<count> = start DPP chirp
  dpp_stop_chirp = stop DPP chirp
  all_bss = list all BSS entries (scan results)
  

```

#### 查看当前手机国家码

```

adb shell cmd wifi get-country-code &&  adb shell wpa_cli get_capability freq

adb shell cmd wifi get-country-code     // 打印当前国家码
Wifi Country Code = CN      //  命令输出
adb shell wpa_cli get_capability freq    // 查看当前 CN的频段


adb shell cmd wifi force-country-code enabled US  // 设置当前国家码


adb shell cmd wifi get-country-code  // 打印当前国家码
Wifi Country Code = US    //  命令输出

adb shell wpa_cli get_capability freq    // 查看当前 US的频段

```

#### 指定当前手机热点的频段

```
 // 指定当前热点在 5180 这个频段打开热点
adb shell cmd wifi force-softap-channel enabled 5180

channel: 36 band: 2
2G freq: [2412, 2417, 2422, 2427, 2432, 2437, 2442, 2447, 2452, 2457, 2462, 2467, 2472]
5G freq: [5180, 5200, 5220, 5240, 5745, 5765, 5785, 5805, 5825]
5G DFS: [5260, 5280, 5300, 5320]
6G freq: []
60G freq: []


```



#### 设置CN国家码频段
adb shell cmd wifi force-country-code enabled US       

adb shell cmd wifi force-country-code enabled CN  【设置国家码 US 】

adb shell wpa_cli get_capability freq   【CN国家码对应的频段信息】

adb shell cmd wifi get-country-code   【查看设置的国家码】

```
# wpa_cli get_capability freq

adb shell cmd wifi get-country-code
Wifi Country Code = CN


Using interface 'wlan0'
Mode[G] Channels:    Mode[A] Channels:        Mode[A] Channels:           Mode[B] Channels:
  1 = 2412 MHz        36 = 5180 MHz                                        1 = 2412 MHz
  2 = 2417 MHz        40 = 5200 MHz                                        2 = 2417 MHz
  3 = 2422 MHz        44 = 5220 MHz                                        3 = 2422 MHz
  4 = 2427 MHz        48 = 5240 MHz                                        4 = 2427 MHz
  5 = 2432 MHz        52 = 5260 MHz (DFS)                                  5 = 2432 MHz
  6 = 2437 MHz        56 = 5280 MHz (DFS)                                  6 = 2437 MHz
  7 = 2442 MHz        60 = 5300 MHz (DFS)                                  7 = 2442 MHz
  8 = 2447 MHz        64 = 5320 MHz (DFS)                                  8 = 2447 MHz
  9 = 2452 MHz       149 = 5745 MHz                                        9 = 2452 MHz
 10 = 2457 MHz       153 = 5765 MHz                                       10 = 2457 MHz
 11 = 2462 MHz       157 = 5785 MHz                                       11 = 2462 MHz
 12 = 2467 MHz       161 = 5805 MHz                                       12 = 2467 MHz
 13 = 2472 MHz       165 = 5825 MHz                                       13 = 2472 MHz

```

#### 设置US国家码频段
adb shell cmd wifi force-country-code enabled US       
adb shell wpa_cli get_capability freq   【US国家码对应的频段信息】



```
adb shell cmd wifi get-country-code &&  adb shell wpa_cli get_capability freq

adb shell cmd wifi get-country-code
Wifi Country Code = US

 # wpa_cli get_capability freq
Using interface 'wlan0'

Mode[G] Channels:   Mode[A] Channels:         Mode[A] Channels:      Mode[B] Channels:
 1 = 2412 MHz        36 = 5180 MHz             1  = 5955 MHz           1 = 2412 MHz       
 2 = 2417 MHz        40 = 5200 MHz             5  = 5975 MHz           2 = 2417 MHz
 3 = 2422 MHz        44 = 5220 MHz             9  = 5995 MHz           3 = 2422 MHz
 4 = 2427 MHz        48 = 5240 MHz             13 = 6015 MHz           4 = 2427 MHz
 5 = 2432 MHz        52 = 5260 MHz (DFS)       17 = 6035 MHz           5 = 2432 MHz 
 6 = 2437 MHz        56 = 5280 MHz (DFS)       21 = 6055 MHz           6 = 2437 MHz 
 7 = 2442 MHz        60 = 5300 MHz (DFS)       25 = 6075 MH            7 = 2442 MHz
 8 = 2447 MHz        64 = 5320 MHz (DFS)       29 = 6095 MHz           8 = 2447 MHz 
 9 = 2452 MHz       100 = 5500 MHz (DFS)       33 = 6115 MHz           9 = 2452 MHz 
10 = 2457 MHz       104 = 5520 MHz (DFS)       37 = 6135 MHz          10 = 2457 MHz 
11 = 2462 MHz       108 = 5540 MHz (DFS)       41 = 6155 MHz          11 = 2462 MHz 
                    112 = 5560 MHz (DFS)       45 = 6175 MHz              
                    116 = 5580 MHz (DFS)       49 = 6195 MHz              
                    120 = 5600 MHz (DFS)       53 = 6215 MHz              
                    124 = 5620 MHz (DFS)       57 = 6235 MHz              
                    128 = 5640 MHz (DFS)       61 = 6255 MHz              
                    132 = 5660 MHz (DFS)       65 = 6275 MHz              
                    136 = 5680 MHz (DFS)       69 = 6295 MHz              
                    140 = 5700 MHz (DFS)       73 = 6315 MHz
                    144 = 5720 MHz (DFS)       77 = 6335 MHz
                    149 = 5745 MHz             81 = 6355 MHz
                    153 = 5765 MHz             85 = 6375 MHz
                    157 = 5785 MHz             89 = 6395 MHz
                    161 = 5805 MHz             93 = 6415 MHz
                    165 = 5825 MHz             97 = 6435 MHz
                                              101 = 6455 MHz
                                              105 = 6475 MHz
                                              109 = 6495 MHz
                                              113 = 6515 MHz
                                              117 = 6535 MHz
                                              121 = 6555 MHz
                                              125 = 6575 MHz
                                              129 = 6595 MHz
                                              133 = 6615 MHz
                                              137 = 6635 MHz
                                              141 = 6655 MHz
                                              145 = 6675 MHz
                                              149 = 6695 MHz
                                              153 = 6715 MHz
                                              157 = 6735 MHz
                                              161 = 6755 MHz
                                              165 = 6775 MHz
                                              169 = 6795 MHz
                                              173 = 6815 MHz
                                              177 = 6835 MHz
                                              181 = 6855 MHz
                                              185 = 6875 MHz
                                              189 = 6895 MHz
                                              193 = 6915 MHz
                                              197 = 6935 MHz
                                              201 = 6955 MHz
                                              205 = 6975 MHz
                                              209 = 6995 MHz
                                              213 = 7015 MHz
                                              217 = 7035 MHz
                                              221 = 7055 MHz
                                              225 = 7075 MHz
                                              229 = 7095 MHz
                                              233 = 7115 MHz











```



#### 设置BR巴西国家码频段
adb shell cmd wifi force-country-code enabled BR       
adb shell wpa_cli get_capability freq   【BR巴西国家码对应的频段信息】
adb shell cmd wifi get-country-code


```
adb shell cmd wifi force-country-code enabled BR && adb shell cmd wifi get-country-code &&  adb shell wpa_cli get_capability freq

adb shell cmd wifi get-country-code &&  adb shell wpa_cli get_capability freq

adb shell cmd wifi get-country-code
Wifi Country Code = BR



# adb shell wpa_cli get_capability freq
Using interface 'wlan0'

Mode[G] Channels:   Mode[A] Channels:         Mode[A] Channels:      Mode[B] Channels:
 1 = 2412 MHz        36 = 5180 MHz             1  = 5955 MHz           1 = 2412 MHz       
 2 = 2417 MHz        40 = 5200 MHz             5  = 5975 MHz           2 = 2417 MHz
 3 = 2422 MHz        44 = 5220 MHz             9  = 5995 MHz           3 = 2422 MHz
 4 = 2427 MHz        48 = 5240 MHz             13 = 6015 MHz           4 = 2427 MHz
 5 = 2432 MHz        52 = 5260 MHz (DFS)       17 = 6035 MHz           5 = 2432 MHz 
 6 = 2437 MHz        56 = 5280 MHz (DFS)       21 = 6055 MHz           6 = 2437 MHz 
 7 = 2442 MHz        60 = 5300 MHz (DFS)       25 = 6075 MH            7 = 2442 MHz
 8 = 2447 MHz        64 = 5320 MHz (DFS)       29 = 6095 MHz           8 = 2447 MHz 
 9 = 2452 MHz       100 = 5500 MHz (DFS)       33 = 6115 MHz           9 = 2452 MHz 
10 = 2457 MHz       104 = 5520 MHz (DFS)       37 = 6135 MHz          10 = 2457 MHz 
11 = 2462 MHz       108 = 5540 MHz (DFS)       41 = 6155 MHz          11 = 2462 MHz 
                    112 = 5560 MHz (DFS)       45 = 6175 MHz              
                    116 = 5580 MHz (DFS)       49 = 6195 MHz              
                    120 = 5600 MHz (DFS)       53 = 6215 MHz              
                    124 = 5620 MHz (DFS)       57 = 6235 MHz              
                    128 = 5640 MHz (DFS)       61 = 6255 MHz              
                    132 = 5660 MHz (DFS)       65 = 6275 MHz              
                    136 = 5680 MHz (DFS)       69 = 6295 MHz              
                    140 = 5700 MHz (DFS)       73 = 6315 MHz
                    144 = 5720 MHz (DFS)       77 = 6335 MHz
                    149 = 5745 MHz             81 = 6355 MHz
                    153 = 5765 MHz             85 = 6375 MHz
                    157 = 5785 MHz             89 = 6395 MHz
                    161 = 5805 MHz             93 = 6415 MHz
                    165 = 5825 MHz             97 = 6435 MHz
                                              101 = 6455 MHz
                                              105 = 6475 MHz
                                              109 = 6495 MHz
                                              113 = 6515 MHz
                                              117 = 6535 MHz
                                              121 = 6555 MHz
                                              125 = 6575 MHz
                                              129 = 6595 MHz
                                              133 = 6615 MHz
                                              137 = 6635 MHz
                                              141 = 6655 MHz
                                              145 = 6675 MHz
                                              149 = 6695 MHz
                                              153 = 6715 MHz
                                              157 = 6735 MHz
                                              161 = 6755 MHz
                                              165 = 6775 MHz
                                              169 = 6795 MHz
                                              173 = 6815 MHz
                                              177 = 6835 MHz
                                              181 = 6855 MHz
                                              185 = 6875 MHz
                                              189 = 6895 MHz
                                              193 = 6915 MHz
                                              197 = 6935 MHz
                                              201 = 6955 MHz
                                              205 = 6975 MHz
                                              209 = 6995 MHz
                                              213 = 7015 MHz
                                              217 = 7035 MHz
                                              221 = 7055 MHz
                                              225 = 7075 MHz
                                              229 = 7095 MHz
                                              233 = 7115 MHz




```




### Wifi断开 disconnect reason 列表

```
/**
 * Logs when a Wifi connection drops.
 *
 * Logged from:
 *   frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiMetrics.java
 */
message WifiDisconnectReported {

    enum FailureCode {
        UNKNOWN = 0;

        // Wifi supplicant failure reason codes (IEEE Std 802.11-2016, 9.4.1.7, Table 9-45).
        // See ISupplicantStaIfaceCallback.java:ReasonCode
        UNSPECIFIED = 1;
        PREV_AUTH_NOT_VALID = 2;
        DEAUTH_LEAVING = 3;
        DISASSOC_DUE_TO_INACTIVITY = 4;
        DISASSOC_AP_BUSY = 5;
        CLASS2_FRAME_FROM_NONAUTH_STA = 6;
        CLASS3_FRAME_FROM_NONASSOC_STA = 7;
        DISASSOC_STA_HAS_LEFT = 8;
        STA_REQ_ASSOC_WITHOUT_AUTH = 9;
        PWR_CAPABILITY_NOT_VALID = 10;
        SUPPORTED_CHANNEL_NOT_VALID = 11;
        BSS_TRANSITION_DISASSOC = 12;
        INVALID_IE = 13;
        MICHAEL_MIC_FAILURE = 14;
        FOURWAY_HANDSHAKE_TIMEOUT = 15;
        GROUP_KEY_UPDATE_TIMEOUT = 16;
        IE_IN_4WAY_DIFFERS = 17;
        GROUP_CIPHER_NOT_VALID = 18;
        PAIRWISE_CIPHER_NOT_VALID = 19;
        AKMP_NOT_VALID = 20;
        UNSUPPORTED_RSN_IE_VERSION = 21;
        INVALID_RSN_IE_CAPAB = 22;
        IEEE_802_1X_AUTH_FAILED = 23;
        CIPHER_SUITE_REJECTED = 24;
        TDLS_TEARDOWN_UNREACHABLE = 25;
        TDLS_TEARDOWN_UNSPECIFIED = 26;
        SSP_REQUESTED_DISASSOC = 27;
        NO_SSP_ROAMING_AGREEMENT = 28;
        BAD_CIPHER_OR_AKM = 29;
        NOT_AUTHORIZED_THIS_LOCATION = 30;
        SERVICE_CHANGE_PRECLUDES_TS = 31;
        UNSPECIFIED_QOS_REASON = 32;
        NOT_ENOUGH_BANDWIDTH = 33;
        DISASSOC_LOW_ACK = 34;
        EXCEEDED_TXOP = 35;
        STA_LEAVING = 36;
        END_TS_BA_DLS = 37;
        UNKNOWN_TS_BA = 38;
        TIMEOUT = 39;
        PEERKEY_MISMATCH = 45;
        AUTHORIZED_ACCESS_LIMIT_REACHED = 46;
        EXTERNAL_SERVICE_REQUIREMENTS = 47;
        INVALID_FT_ACTION_FRAME_COUNT = 48;
        INVALID_PMKID = 49;
        INVALID_MDE = 50;
        INVALID_FTE = 51;
        MESH_PEERING_CANCELLED = 52;
        MESH_MAX_PEERS = 53;
        MESH_CONFIG_POLICY_VIOLATION = 54;
        MESH_CLOSE_RCVD = 55;
        MESH_MAX_RETRIES = 56;
        MESH_CONFIRM_TIMEOUT = 57;
        MESH_INVALID_GTK = 58;
        MESH_INCONSISTENT_PARAMS = 59;
        MESH_INVALID_SECURITY_CAP = 60;
        MESH_PATH_ERROR_NO_PROXY_INFO = 61;
        MESH_PATH_ERROR_NO_FORWARDING_INFO = 62;
        MESH_PATH_ERROR_DEST_UNREACHABLE = 63;
        MAC_ADDRESS_ALREADY_EXISTS_IN_MBSS = 64;
        MESH_CHANNEL_SWITCH_REGULATORY_REQ = 65;
        MESH_CHANNEL_SWITCH_UNSPECIFIED = 66;

        // ClientModeImpl error codes
        // Defined in /frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiMetrics.java
        IFACE_DESTROYED = 10000;
        WIFI_DISABLED = 10001;
        SUPPLICANT_DISCONNECTED = 10002;
        CONNECTING_WATCHDOG_TIMER = 10003;
        ROAM_WATCHDOG_TIMER = 10004;
    }
	
```


### Linux_Kill_Signal  终止信号

```
kill送出一个特定的信号 （signal）给行程id为pid的行程根据该信号而做特定的动作，若没有指定，预设是送出终止 （TERM）的信号

-s （signal）: 其中常用的讯号有 HUP （1）,KILL （9）,TERM （15）,分别代表着重跑，砍掉,结束; 详细的信号可以用 kill -l （见下结果，可用数字带入）

-p : 印出pid，并不送出信号
-l （signal）: 列出所有可用的信号名称

```




```
这个就是kill -l的查询结果：

1. 所有的名称都是以SIG开头的；
2. 每个信号名称前面有1个数字，用半边括号包围；
3. 没有32和33号信号。
4. 34号信号名为SIGRTMIN，接下来的15个信号分别为SIGRTMIN+1~SIGRTMIN+15，64号信号名为SIGRTMAX，它之前的14个信号分别为SIGRTMAX-14~SIGRTMAX-1；

5.信号的命名和值和历史原因有关，这个在man手册上写了，在不同的标准中加入了不同的信号，
基本上每个信号都有对应的事件，但是这些信号个问题，就是不支持排队，
也就是说如果同时出现好几个信号，那么只有1个信号能处理，其他的信号会丢失。如果修改这个机制的话，可能会导致天下大乱，
然后大神们就想出一种办法，增加了信号的类型，新增的信号类型与旧的信号类型有所区别，那就是新增的信号类型支持排队，
它们即使同时来好几个也不会丢失，那么顺理成章的，旧的信号就成了不可靠信号，新的信号就是可靠信号（在手册中的说法是普通信号和实时信号，
只不过按照我的理解我更倾向于叫它们不可靠信号和可靠信号）。

6.新增的这些信号并没有对应任何一个实际的事件，它们根据实际情况来使用（手册上是这么写的），
因此就不好给它们取名字了，索性就把新增的这些信号划分到一个范围里，
最小的值叫做 SIGRTMIN，最大的值叫 SIGRTMAX，其中RT就是Real-Time，然后用SIGRTMIN+n和SIGRTMAX-n的方法来表示它们


1) SIGHUP          2) SIGINT            3) SIGQUIT            4) SIGILL
5) SIGTRAP         6) SIGABRT           7) SIGBUS             8) SIGFPE
9) SIGKILL         10) SIGUSR1          11) SIGSEGV           12) SIGUSR2
13) SIGPIPE        14) SIGALRM          15) SIGTERM           16) SIGSTKFLT
17) SIGCHLD        18) SIGCONT          19) SIGSTOP           20) SIGTSTP
21) SIGTTIN        22) SIGTTOU          23) SIGURG            24) SIGXCPU
25) SIGXFSZ        26) SIGVTALRM        27) SIGPROF           28) SIGWINCH
29) SIGIO          30) SIGPWR           31) SIGSYS            34) SIGRTMIN
35) SIGRTMIN+1     36) SIGRTMIN+2       37) SIGRTMIN+3        38) SIGRTMIN+4
39) SIGRTMIN+5     40) SIGRTMIN+6       41) SIGRTMIN+7        42) SIGRTMIN+8
43) SIGRTMIN+9     44) SIGRTMIN+10      45) SIGRTMIN+11       46) SIGRTMIN+12
47) SIGRTMIN+13    48) SIGRTMIN+14      49) SIGRTMIN+15       50) SIGRTMAX-14
51) SIGRTMAX-13    52) SIGRTMAX-12      53) SIGRTMAX-11       54) SIGRTMAX-10
55) SIGRTMAX-9     56) SIGRTMAX-8       57) SIGRTMAX-7        58) SIGRTMAX-6
59) SIGRTMAX-5     60) SIGRTMAX-4       61) SIGRTMAX-3        62) SIGRTMAX-2
63) SIGRTMAX-1     64) SIGRTMAX        

```


| 信号 | 值 | 默认动作 | 含义 |
| ---- | ---- | ---- | ---- |
| SIGHUP | 1 | 终止 | 终端挂起或者控制进程终止。该信号在用户终端连接（正常或非正常）退出时发出，通常是在终端的控制进程结束时通知同一会话内的各个作业与控制终端不再关联。 |
| SIGINT | 2 | 终止 | 来自键盘的中断信号，如Ctrl+C，或者break键被按下，但是笔记本上可能没有break键。 |
| SIGQUIT | 3 | 终止，并进行Core_dump | （后面解释什么是Core_dump）来自键盘的退出信号，与SIGINT类似，但是由 |
| SIGILL | 4 | 终止，并进行Core_dump | 非法指令（可执行文件本身发生错误，或者试图执行数据段，或堆栈溢出时发出） |
| SIGTRAP | 5 | 终止，并进行Core_dump | 由断点指令或其它陷阱（trap）指令产生. |
| SIGABRT | 6 | 终止，并进行Core_dump | 由abort(3)发出的终止指令（这个括号带个3指的是可以用man |
| SIGBUS | 7 | 终止，并进行Core_dump | 总线错误（错误的内存访问，包括内存地址对齐(alignment)出错。比如访问一个四个字长的整数, |
| SIGFPE | 8 | 终止，并进行Core_dump | 浮点异常。在发生致命的算术运算错误时发出。不仅包括浮点运算错误, |
| SIGKILL | 9 | 终止 | kill信号。【该信号不能被忽略、阻塞以及自定义处理方法】，如果发现某个进程结束不了可以用这个信号将其杀死 |
| SIGUSR1 | 10 | 终止 | 给用户使用的信号1 |
| SIGSEGV | 11 | 终止，并进行Core_dump | 无效的内存参考，当程序访问没有访问权限的内存区域，或者访问非可读的内存区域时，产生该信号，如数组越界。它与SIGBUS的区别是，SIGSEGV是对合法地址的非法访问，而SIGBUS访问的本身就是非法的地址。 |
| SIGUSR2 | 12 | 终止 | 给用户使用的信号2 |
| SIGPIPE | 13 | 终止 | 管道破裂：往管道写数据的时候读端已经关闭，或者在socket通信的时候，写数据的时候读端已经关闭 |
| SIGALRM | 14 | 终止 | 来自alarm(2)的定时器信号 |
| SIGTERM | 15 | 终止 | 终止信号。kill命令的默认方式，与SIGKILL不同的是，该信号可以被阻塞或者自定义处理方式 |
| SIGSTKFLT | 16 | 终止 | 协处理器上的堆栈故障（未使用） |
| SIGCHLD | 17 | 忽略 | 子进程停止(stopped)或者终止 |
| SIGCONT | 18 | 继续运行 | 让一个停止(stopped)的进程继续执行。本信号不能被阻塞 |
| SIGSTOP | 19 | 暂停运行 | 暂停进程。该信号不能被忽略、阻塞以及自定义处理方法。 |
| SIGTSTP | 20 | 暂停运行 | 由tty发出的停止（stopped）信号，如Ctrl+Z |
| SIGTTIN | 21 | 暂停运行 | tty输入用于后台进程。其实就是指后台进程试图从终端读取数据，比如从stdin读数据。 |
| SIGTTOU | 22 | 暂停运行 | tty输出用于后台进程。其实就是指后台进程试图向终端读写数据，比如向stdout写数据。 |
| SIGURG | 23 | 忽略 | 有”紧急”数据或带外数据out-of-band到达socket时产生。带外数据是socket编程的知识点。 |
| SIGXCPU | 24 | 终止，并进行Core_dump | 超过CPU时间资源限制。这个限制可以由getrlimit/setrlimit来读取/改变。 |
| SIGXFSZ | 25 | 终止，并进行Core_dump | 文件大小超出限制。 |
| SIGVTALRM | 26 | 终止 | 虚拟闹钟。 |
| SIGPROF | 27 | 终止 | Profiling定时器到 |
| SIGWINCH | 28 | 忽略 | 窗口大小改变 |
| SIGIO | 29 | 终止 | I/O准备就绪 |
| SIGPWR | 30 | 终止 | Power failure（电源失败？） |
| SIGSYS | 31 | 终止，并进行Core_dump | Bad argument to routine 非法的系统调用 |







### QCOM_NV_AAGPS

```
NVITEM ID,DESCRIPTION,FULL NAME,CATEGORY
1913,AAGPS Maximum Frequency Uncertainty System Oscillator,aagps_max_osc_unc,AAGPS
1914,AAGPS Maximum Oscillator Uncertainty Rate,aagps_max_osc_unc_rate,AAGPS
1915,AAGPS Processing Losses,aagps_processing_losses,AAGPS
1916,AAGPS Maximum Platform Velocity,aagps_max_platform_velocity,AAGPS
1917,AAGPS Maximum Platform Acceleration,aagps_max_platform_accl,AAGPS
1918,AAGPS Default QoS Time,aagps_default_qos_time,AAGPS
1919,AAGPS Rapid Fix Report Maximum Latency,aagps_rapid_fix_report_max_latency,AAGPS
1920,AAGPS Positioning Modes Supported,aagps_positioning_modes_supported,AAGPS
1921,AAGPS Default Reference Time Uncertainty,aagps_default_ref_time_unc,AAGPS
1922,AAGPS Default Reference Position Uncertainty,aagps_default_ref_position_unc,AAGPS
1923,AAGPS Application Based Tracking GPS-Idle Threshold,aagps_app_tracking_gpsidle_thsld,AAGPS
1924,AAGPS GPS Lock Control,aagps_gps_lock_control,AAGPS
1925,AAGPS Default URL,aagps_default_url,AAGPS
1926,AAGPS Default IP Address,aagps_default_ip_address,AAGPS
1927,AAGPS Transport Type,aagps_transport_type,AAGPS
1929,AAGPS 2G MO-LR Support,aagps_2g_mo_lrsupport,AAGPS
1930,AAGPS Emergency Services Support,aagps_emergency_services_spprt,AAGPS
1934,AAGPS Protocol Selection,aagps_protocol_select,AAGPS
1935,AAGPS Application Based Tracking Periodic Request Delay Margin,aagps_app_trkg_periodic_req_dly_margin,AAGPS
1936,AAGPS Default QoS UNC,aagps_default_qos_unc,AAGPS
1937,AAGPS Application Based Tracking GPS-On Threshold,aagps_app_tracking_gpson_thsld,AAGPS
1940,AAGPS MT LR Support,aagps_mt_lrsupport,AAGPS
1959,AAGPS Default IP Port,aagps_default_ip_port,AAGPS
1960,AAGPS 3G MO LR Support,aagps_3g_mo_lrsupport,AAGPS
1961,AAGPS Development Test Control,aagps_development_test_control,AAGPS
1988,AAGPS Development Test Control 2,aagps_development_test_control2,AAGPS
1989,AAGPS Development Test Control 3,aagps_development_test_control3,AAGPS
1990,AAGPS Development Test Control 4,aagps_development_test_control4,AAGPS
1991,AAGPS Development Test Control 5,aagps_development_test_control5,AAGPS
2784,AAGPS Default Allow Rrc,aagps_default_allow_rrc,AAGPS
2785,AAGPS Default Mtlr Guard Timer,aagps_default_mtlr_guard_timer,AAGPS
2786,AAGPS Default Smlc Comm Timeout,aagps_default_smlc_comm_timeout,AAGPS
2787,AAGPS Default Presupl Ue Timer1 Value,aagps_default_presupl_ue_timer1_value,AAGPS
2788,AAGPS Default Presupl Ue Timer2 Value,aagps_default_presupl_ue_timer2_value,AAGPS
2789,AAGPS Default Presupl Ue Timer3 Value,aagps_default_presupl_ue_timer3_value,AAGPS
3297,AAGPS RTI Validity Duration,aagps_rti_validity_dur,AAGPS
3494,AAGPS Development Test Control 6,aagps_development_test_control6,AAGPS
3758,AAGPS Use Transport Security,aagps_use_transport_security,AAGPS
4529,AAGPS XTRA Enabled,aagps_xtra_enabled,AAGPS
4530,AAGPS XTRA Download Interval,aagps_xtra_download_interval,AAGPS
4531,AAGPS XTRA Number Of Download Attempts,aagps_xtra_num_download_attempts,AAGPS
4532,AAGPS XTRA Primary Server URL,aagps_xtra_primary_server_url,AAGPS
4533,AAGPS XTRA Secondary Server URL,aagps_xtra_secondary_server_url,AAGPS
4534,AAGPS XTRA Time Between Attempts,aagps_xtra_time_between_attempts,AAGPS
4535,AAGPS XTRA Tertiary Server URL,aagps_xtra_tertiary_server_url,AAGPS
4536,AAGPS XTRA Auto Download Enabled,aagps_xtra_auto_download_enabled,AAGPS
4537,AAGPS XTRA Time Information Enabled,aagps_xtra_time_info_enabled,AAGPS
4538,AAGPS XTRA Time Information Uncertainty Threshold,aagps_xtra_time_info_unc_thresh,AAGPS
4539,AAGPS XTRA Time Information Delay Threshold,aagps_xtra_time_info_delay_thresh,AAGPS
4540,AAGPS XTRA Primary SNTP Server URL,aagps_xtra_primary_sntp_server_url,AAGPS
4541,AAGPS XTRA Secondary SNTP Server URL,aagps_xtra_secondary_sntp_server_url,AAGPS
4542,AAGPS XTRA Tertiary SNTP Server URL,aagps_xtra_tertiary_sntp_server_url,AAGPS
4544,AAGPS IPC DM Thread Mask,aagps_ipc_dm_thread_mask,AAGPS
6256,AAGPS Global Altitude,aagps_global_altitude,AAGPS
6257,AAGPS Global Altitude Uncertainty,aagps_global_alt_unc,AAGPS
6263,AAGPS Acquisition Timer,aagps_acquisition_timer,AAGPS
```











### QCOM_NV_GPS
```
NVITEM ID,DESCRIPTION,FULL NAME,CATEGORY
400,GPSOne Capabilities,gps1_capabilitie,GPS
401,GPSOne PDE TCP Address,gps1_pde_addre,GPS
402,GPSOne Position Determination Services Lockout,gps1_allowed,GPS
403,GPSOne Preferred Transport Mechanism,gps1_pde_transport,GPS
404,GPSOne Mobile vs PDE based Position Calculations,gps1_mobile_calc,GPS
426,GPSOne PDE Port,gps1_pde_port,GPS
443,GPSOne RF Delay,gps1_gps_rf_delay,GPS
444,GPSOne CDMA RF Delay,gps1_cdma_rf_delay,GPS
449,GPSOne GPS RF Loss,gps1_gps_rf_lo,GPS
452,GPSOne Lock,gps1_lock,GPS
555,GPSOne LO Calibration Offset,gps1_lo_cal,GPS
556,GPSOne Antenna Offset,gps1_ant_off_db,GPS
557,GPSOne PCS RF Delay,gps1_pcs_rf_delay,GPS
622,GPS RXF Coarse Grain DC I Offset,gps_rxf_cg_ioffset,GPS
626,GPS RXF Coarse Grain DC Q Offset,gps_rxf_cg_qoffset,GPS
630,GPS RXF Fine Grain DC I Offset,gps_rxf_fg_ioffset,GPS
634,GPS RXF Fine Grain DC Q Offset,gps_rxf_fg_qoffset,GPS
638,GPS DAC Controller Estimator I Offset,gps_dacc_est_ioffset,GPS
642,GPS DAC Controller Estimator Q Offset,gps_dacc_est_qoffset,GPS
646,GPS DAC Controller I Accumulator Gain Step 0,gps_dacc_iaccum0,GPS
650,GPS DAC Controller I Accumulator Gain Step 1,gps_dacc_iaccum1,GPS
654,GPS DAC Controller I Accumulator Gain Step 2,gps_dacc_iaccum2,GPS
658,GPS DAC Controller I Accumulator Gain Step 3,gps_dacc_iaccum3,GPS
662,GPS DAC Controller I Accumulator Gain Step 4,gps_dacc_iaccum4,GPS
666,GPS DAC Controller Q Accumulator Gain Step 0,gps_dacc_qaccum0,GPS
670,GPS DAC Controller Q Accumulator Gain Step 1,gps_dacc_qaccum1,GPS
674,GPS DAC Controller Q Accumulator Gain Step 2,gps_dacc_qaccum2,GPS
678,GPS DAC Controller Q Accumulator Gain Step 3,gps_dacc_qaccum3,GPS
682,GPS DAC Controller Q Accumulator Gain Step 4,gps_dacc_qaccum4,GPS
686,GPS DAC Controller Gain Multiply,gps_dacc_gain_mult,GPS
703,GPS Mismatch Compensation A Offset,gps_mis_comp_a_offset,GPS
706,GPS Mismatch Compensation B Offset,gps_mis_comp_b_offset,GPS
710,Low Bias Update NV Value Count,lo_bias_update_cnt,GPS
736,GPS Doppler Calibrated Standard Deviation,gps_dopp_sdev,GPS
823,GPSOne Privacy,gps1_privacy,GPS
824,GPSOne Network Access Control,gps1_net_acce,GPS
825,GPSOne Cell Based Position Determination,gps1_cellbased_sm,GPS
826,GPSOne Teleservice ID,gps1_teleservice_id,GPS
831,GPSOne Network Data Burst Packet Size,gps1_net_dbm_size,GPS
836,GPS Transmitter Gain Attenuation Limit,gps_tx_gain_atten_limit,GPS
981,C1 GPS Mismatch Compensation A Offset,c1_gps_mis_comp_a_offset,GPS
983,C1 GPS Mismatch Compensation B Offset,c1_gps_mis_comp_b_offset,GPS
986,C1 GPS DAC Controller I Accumulator Gain Step 0,c1_gps_dacc_iaccum0,GPS
989,C1 GPS DAC Controller I Accumulator Gain Step 1,c1_gps_dacc_iaccum1,GPS
992,C1 GPS DAC Controller I Accumulator Gain Step 2,c1_gps_dacc_iaccum2,GPS
995,C1 GPS DAC Controller I Accumulator Gain Step 3,c1_gps_dacc_iaccum3,GPS
998,C1 GPS DAC Controller I Accumulator Gain Step 4,c1_gps_dacc_iaccum4,GPS
1001,C1 GPS DAC Controller Q Accumulator Gain Step 0,c1_gps_dacc_qaccum0,GPS
1004,C1 GPS DAC Controller Q Accumulator Gain Step 1,c1_gps_dacc_qaccum1,GPS
1007,C1 GPS DAC Controller Q Accumulator Gain Step 2,c1_gps_dacc_qaccum2,GPS
1010,C1 GPS DAC Controller Q Accumulator Gain Step 3,c1_gps_dacc_qaccum3,GPS
1013,C1 GPS DAC Controller Q Accumulator Gain Step 4,c1_gps_dacc_qaccum4,GPS
1032,GPS Receive Chain Configuration,gps_rf_config,GPS
1349,BC6 GPSOne RF Delay,bc6_gps1_rf_delay,GPS
1389,BC5 GPSOne RF Delay,bc5_gps1_rf_delay,GPS
1467,BC4 GPSOne RF Delay,bc4_gps1_rf_delay,GPS
1545,BC3 GPSOne RF Delay,bc3_gps1_rf_delay,GPS
1624,BC1 GPSOne RF Delay,bc1_gps1_rf_delay,GPS
1713,BC0 GPSOne RF Delay,bc0_gps1_rf_delay,GPS
1944,GPSOne Call Related Functionality,gps1_call_related,GPS
1993,GPSOne Vx LCS Agent,gps1_vx_lcs_agent,GPS
1994,GPSOne Vx Application Trusted Settings,gps1_vx_app_trusted_settings,GPS
1995,GPSOne Vx Dedicated SMS Teleservice Identifier,gps1_vx_ni_teleservice_id,GPS
1996,GPSOne Vx LCS Agent Prev6 Only,gps1_vx_lcs_agent_prev6_only,GPS
1997,GPSOne Vx MO Max Duration,gps1_vx_mo_max_duration,GPS
1998,OOS Operation Preference,oosoperationpref,GPS
1999,GPSOne Vx GPS During Voice Call,gps1_vx_gps_during_voice_call,GPS
2506,Appendix 5 Preferences,appx5_pref,GPS
2783,GPS1 Min Num Svs,gps1_min_num_svs,GPS
3293,GPSOne Position Report,gps1_position_report,GPS
3354,GPSOne MS-Based Back Off Factor,gps1_msb_back_off_factor,GPS
3355,GPSOne MS-Based Back Off Minimum,gps1_msb_back_off_min,GPS
3356,GPSOne MS-Based Back Off Maximum,gps1_msb_back_off_max,GPS
3357,GPSOne MS-Based Back Off Reset,gps1_msb_back_off_reset,GPS
3358,GPSOne Enable MS-B Throttling,gps1_msb_throttle_enable,GPS
3430,GPSOne Number Of Searcher Tasks,gps1_num_searcher_tasks,GPS
3478,GPSOne Cross Correlation Threshold dbhz,gps1_cross_corr_threshold_dbhz,GPS
3479,GPSOne Timer Threshold,gps1_cme_t_track_threshold,GPS
3480,GPSOne V2 Handoff Enable,gps1_vx_lcsagent_v2_handoff_enable,GPS
3481,Block MO session for duration,gps1_vx_lcsagent_mo_backoff_duration,GPS
3486,Sigma Reject,sigmareject,GPS
3487,Velocity Noise In One Sec Drive,velnoiseinonesecdrive,GPS
3488,Velocity Noise In One Sec Pedestrian,velnoiseinonesecped,GPS
3489,Velocity Noise In One Sec Static,velnoiseinonesecstatic,GPS
3490,Extra Clock Bias Error,extraclockbiaserror,GPS
3491,KF Configuration Mask,configmask,GPS
3492,Max Propagation By KF,maxpropagationwonewmeas,GPS
3493,KF Configuration Control Flag,configcontrolflag,GPS
3520,GPSOne Seed Position Option,gps1_seedpos_option,GPS
3756,GPSOne Dynamic Mode,gps1_dynamic_mode,GPS
4100,GPSOne NMEA Output,gps1_nmea_output,GPS
4200,GPS DRX Mode Selection,gps_drx_mode_sel,GPS
4266,GPSOne CME Maximum Throttle Duration,gps1_cme_max_throttle_duration,GPS
4272,BC 15 GPSOne RF Delay,bc15_gps1_rf_delay,GPS
4359,GPSOne Password,gpsone_password,GPS
4394,GPSOne Security Update Rate,gps1_sec_update_rate,GPS
4627,GPSOne XTRA Enabled,gps1_xtra_enabled,GPS
4628,GPSOne XTRA Download Interval,gps1_xtra_download_interval,GPS
4629,GPSOne XTRA Number Of Download Attempts,gps1_xtra_num_download_attempts,GPS
4630,GPSOne XTRA Time Between Attempts,gps1_xtra_time_between_attempts,GPS
4631,GPSOne XTRA Auto Download Enabled,gps1_xtra_auto_download_enabled,GPS
4632,GPSOne XTRA Primary Server URL,gps1_xtra_primary_server_url,GPS
4633,GPSOne XTRA Secondary Server URL,gps1_xtra_secondary_server_url,GPS
4634,GPSOne XTRA Tertiary Server URL,gps1_xtra_tertiary_server_url,GPS
4695,CGPS 1X PDE Server Address IPV4,cgps_1x_pde_server_addr_ipv4,GPS
4696,CGPS 1X PDE Server Address IPV6,cgps_1x_pde_server_addr_ipv6,GPS
4697,CGPS 1X PDE Server Address URL,cgps_1x_pde_server_addr_url,GPS
4698,CGPS 1X MPC Server Address IPV4,cgps_1x_mpc_server_addr_ipv4,GPS
4699,CGPS 1X MPC Server Address IPV6,cgps_1x_mpc_server_addr_ipv6,GPS
4700,CGPS 1X MPC Server Address URL,cgps_1x_mpc_server_addr_url,GPS
4701,CGPS UMTS PDE Server Address IPV4,cgps_umts_pde_server_addr_ipv4,GPS
4702,CGPS UMTS PDE Server Address IPV6,cgps_umts_pde_server_addr_ipv6,GPS
4703,CGPS UMTS PDE Server Address URL,cgps_umts_pde_server_addr_url,GPS
4704,CGPS 1X PDE Server Port,cgps_1x_pde_server_port,GPS
4705,CGPS 1X MPC Server Port,cgps_1x_mpc_server_port,GPS
4706,CGPS UMTS PDE Server Port,cgps_umts_pde_server_port,GPS
4707,CGPS MO Method,cgps_mo_method,GPS
4708,CGPS NMEA Sentence Type,cgps_nmea_sentence_type,GPS
4709,CGPS Max OSC Uncertainty Active System,cgps_max_osc_unc_active_sys,GPS
4710,CGPS Max OSC Uncertainty RGS,cgps_max_osc_unc_rgs,GPS
4711,CGPS Max OSC Uncertainty Temperature Table,cgps_max_osc_unc_temp_tbl,GPS
4712,CGPS Max OSC Uncertainty RGS Old,cgps_max_osc_unc_rgs_old,GPS
4713,CGPS Max Carrier Code Filter,cgps_max_carrier_code_filter,GPS
4714,CGPS Max Integrate MS,cgps_max_integrate_ms,GPS
4715,CGPS ME Reserved 1,cgps_me_reserved1,GPS
4716,CGPS ME Reserved 2,cgps_me_reserved2,GPS
4717,CGPS ME Reserved 3,cgps_me_reserved3,GPS
4718,CGPS ME Reserved 4,cgps_me_reserved4,GPS
4906,CGPS DGPS Corrections Allowed,cgps_dgps_corrections_allowed,GPS
4907,CGPS Maximum DGPS Interval,cgps_maximum_dgps_interval,GPS
4908,CGPS Use FDIC,cgps_use_fdic,GPS
4909,CGPS Altitude Hold Mode,cgps_altitude_hold_mode,GPS
4910,CGPS PDOP Mask,cgps_pdop_mask,GPS
4911,CGPS 2D PDOP Mask,cgps_2d_pdop_mask,GPS
4912,CGPS Reference Mode,cgps_reference_mode,GPS
4913,CGPS Operation Mode,cgps_operation_mode,GPS
4914,CGPS Elevation Mask,cgps_elevation_mask,GPS
4915,CGPS Max Almanac Age In Weeks,cgps_max_almanac_age_in_weeks,GPS
4916,CGPS Steering On Startup,cgps_steering_on_startup,GPS
4927,GPSOne XTRA Time Info Enabled,gps1_xtra_time_info_enabled,GPS
4928,GPSOne XTRA Time Info Uncertainty Threshold,gps1_xtra_time_info_unc_thresh,GPS
4929,GPSOne XTRA Time Info Delay Threshold,gps1_xtra_time_info_delay_thresh,GPS
4930,GPSOne XTRA Primary SNTP Server URL,gps1_xtra_primary_sntp_server_url,GPS
4931,GPSOne XTRA Secondary SNTP Server URL,gps1_xtra_secondary_sntp_server_url,GPS
4932,GPSOne XTRA Tertiary SNTP Server URL,gps1_xtra_tertiary_sntp_server_url,GPS
5034,CGPS QoS Override Accuracy Threshold,cgps_qos_override_acc_threshold,GPS
5035,CGPS QoS Override Time,cgps_qos_override_time,GPS
5046,CGPS Memory Configuration,cgps_mem_config,GPS
5047,CGPS NMEA Configuration Information,cgps_nmea_config_info,GPS
5154,GPS Engine Type,gps_engine_type,GPS
5289,CGPS VCO Age,cgps_vco_age,GPS
5290,CGPS Optimistic Position Uncertainty,cgps_optimistic_punc,GPS
5594,CGPS SBAS Enabled,cgps_sbas_enabled,GPS
5595,CGPS SBAS User Preference,cgps_sbas_user_preference,GPS
5596,CGPS Dynamic Power Optimization Control,cgps_dpo_control,GPS
5599,CGPS On Demand Enabled,cgps_on_demand_enabled,GPS
5849,CGPS SBAS WAAS Area Node 1,cgps_sbas_waas_area_node1,GPS
5850,CGPS SBAS WAAS Area Node 2,cgps_sbas_waas_area_node2,GPS
5851,CGPS SBAS WAAS Area Node 3,cgps_sbas_waas_area_node3,GPS
5852,CGPS SBAS WAAS Area Node 4,cgps_sbas_waas_area_node4,GPS
5853,CGPS SBAS WAAS Area Node 5,cgps_sbas_waas_area_node5,GPS
5854,CGPS SBAS WAAS Area Node 6,cgps_sbas_waas_area_node6,GPS
5855,CGPS SBAS EGNOS Area Node 1,cgps_sbas_egnos_area_node1,GPS
5856,CGPS SBAS EGNOS Area Node 2,cgps_sbas_egnos_area_node2,GPS
5857,CGPS SBAS EGNOS Area Node 3,cgps_sbas_egnos_area_node3,GPS
5858,CGPS SBAS EGNOS Area Node 4,cgps_sbas_egnos_area_node4,GPS
5859,CGPS SBAS EGNOS Area Node 5,cgps_sbas_egnos_area_node5,GPS
5860,CGPS SBAS EGNOS Area Node 6,cgps_sbas_egnos_area_node6,GPS
5861,CGPS SBAS MSAS Area Node 1,cgps_sbas_msas_area_node1,GPS
5862,CGPS SBAS MSAS Area Node 2,cgps_sbas_msas_area_node2,GPS
5863,CGPS SBAS MSAS Area Node 3,cgps_sbas_msas_area_node3,GPS
5864,CGPS SBAS MSAS Area Node 4,cgps_sbas_msas_area_node4,GPS
5865,CGPS SBAS MSAS Area Node 5,cgps_sbas_msas_area_node5,GPS
5866,CGPS SBAS MSAS Area Node 6,cgps_sbas_msas_area_node6,GPS
5867,CGPS SBAS GAGAN Area Node 1,cgps_sbas_gagan_area_node1,GPS
5868,CGPS SBAS GAGAN Area Node 2,cgps_sbas_gagan_area_node2,GPS
5869,CGPS SBAS GAGAN Area Node 3,cgps_sbas_gagan_area_node3,GPS
5870,CGPS SBAS GAGAN Area Node 4,cgps_sbas_gagan_area_node4,GPS
5871,CGPS SBAS GAGAN Area Node 5,cgps_sbas_gagan_area_node5,GPS
5872,CGPS SBAS GAGAN Area Node 6,cgps_sbas_gagan_area_node6,GPS
5881,GPSOne GPS RFIC IM2Cal IM2DAC I Channel,gps1_gps_rfic_im2cal_im2dac_i_channel,GPS
5882,GPSOne GPS RFIC IM2Cal IM2DAC Q Channel,gps1_gps_rfic_im2cal_im2dac_q_channel,GPS
6213,GPSOne Ephemeris Request Curve Fit Interval,gps1_er_curve_fit_interval,GPS
6214,CGPS Ephemeris Request Enable,cgps_er_enable,GPS
6215,CGPS Ephemeris Request Start Time,cgps_er_start_time,GPS
6216,CGPS Ephemeris Request Slot Interval,cgps_er_slot_interval,GPS
6217,CGPS Ephemeris Request Slot Periods,cgps_er_slot_period,GPS
6264,CGPS Minimum GPS Week Number,cgps_minimum_gps_week_number,GPS
6267,CGPS QWIP Loc Engine Config,cgps_qwip_loc_engine_config,GPS
6268,CGPS QWIP Server Config,cgps_qwip_server_config,GPS
6269,CGPS QWIP Tile Config,cgps_qwip_tile_config,GPS
6270,CGPS QWIP Reserved 1,cgps_qwip_reserved_1,GPS
6271,CGPS QWIP Reserved 2,cgps_qwip_reserved_2,GPS
6272,CGPS XTRA_T Control,cgps_xtra_t_control,GPS
6273,GPS Default Operating Mode,gps_default_operating_mode,GPS
6274,GPS Default TBF,gps_default_tbf,GPS
6339,CGPS ME Shallow Integration Length Stage 1,cgps_me_shallow_integ_stage1,GPS
6340,CGPS ME Shallow Integration Length Stage 2,cgps_me_shallow_integ_stage2,GPS
6341,CGPS ME Shallow Integration Length Stage 3,cgps_me_shallow_integ_stage3,GPS
6430,CGPS Vel UNC Mask,cgps_vel_unc_mask,GPS
6469,CGPS QWIP Hepe Parameters,cgps_qwip_hepe_params,GPS
6486,CGPS UTC GPS Time Offset,cgps_utc_gps_time_offset,GPS
6755,CGPS Sensors Enable,cgps_sensors_enable,GPS
6758,GNSS GPS RxD Enable,gnss_GPS_RxD_Enable,GPS
6759,GNSS System Control,gnss_System_Control,GPS
6760,GNSS GLO Control,gnss_glo_control,GPS
6761,GNSS RXD Path Loss,gnss_rxd_path_loss,GPS
6762,GNSS HBW Path Loss,gnss_hbw_path_loss,GPS
6763,GNSS GLO Path Loss Center Minus7,gnss_GLO_Path_Loss_Center_minus7,GPS
6764,GNSS GLO Path Loss Center 0,gnss_glo_path_loss_center_0,GPS
6765,GNSS GLO Path Loss Center Plus6,gnss_glo_path_loss_center_plus6,GPS
6766,GNSS LBW RXD GroUp Delay DIFF,gnss_lbw_rxd_group_delay_diff,GPS
6767,GNSS Timed Output Mode Selection,gnss_timed_output_mode_selection,GPS
6768,GNSS Timed Output Fixed Bias Offset,gnss_timed_output_fixed_bias_offset,GPS
6769,GNSS Max M RXD Tasks,gnss_max_m_rxd_tasks,GPS
6770,GNSS Max GNSS Tasks,gnss_max_gnss_tasks,GPS
6771,GNSS Shallow First Dwell Duration,gnss_shallow_first_dwell_duration,GPS
6772,GNSS Shallow Successive Dwells Duration,gnss_shallow_successive_dwells_duration,GPS
6773,GNSS Deep First Dwell Duration,gnss_deep_first_dwell_duration,GPS
6774,GNSS Deep Second Dwell Duration,gnss_deep_second_dwell_duration,GPS
6775,GNSS Deep Third Dwell Duration,gnss_deep_third_dwell_duration,GPS
6776,GNSS Verification Dwell Duration,gnss_verification_dwell_duration,GPS
6777,GNSS Probationary Scan Dwell Duration,gnss_probationary_scan_dwell_duration,GPS
6778,GNSS Acquisition Sampling Mode,gnss_acquisition_sampling_mode,GPS
6779,GNSS PP Reserved,gnss_pp_reserved,GPS
6780,GNSS Max Num GPS SV Measurements In Fix,gnss_Max_Num_GPS_SV_Measurements_in_Fix,GPS
6781,GNSS Spcfc GPS SV Measures Not In Fix,gnss_Spcfc_GPS_SV_Measures_Not_in_Fix,GPS
6782,GNSS Max Num GLO SV Measurements In Fix,gnss_Max_Num_GLO_SV_Measurements_in_Fix,GPS
6783,GNSS Spcfc GLO SV Measures Not In Fix,gnss_Spcfc_GLO_SV_Measures_Not_in_Fix,GPS
6784,GNSS Disable GGTB Constraint In WLS,gnss_disable_ggtb_constraint_in_wls,GPS
6785,GNSS Disable GLO Only KF,gnss_disable_glo_only_kf,GPS
6786,GNSS Enable Extended On feature For GNSSS,gnss_enable_extended_on_feature_for_gnss,GPS
6787,GNSS Spcfc GLO SV Nav Data Not Decoded,gnss_Spcfc_GLO_SV_Nav_Data_Not_Decoded,GPS
6788,GNSS Spcfc GPS SV Nav Data Not Decoded,gnss_Spcfc_GPS_SV_Nav_Data_Not_Decoded,GPS
6789,GNSS NMEA Sentence Type,gnss_nmea_sentence_type,GPS
6790,GNSS NMEA Extended Sentence Type,gnss_801_b_protocol_support,GPS
6791,GNSS 801 B Protocol SUpport,gnss_801_b_protocol_support,GPS
6792,GNSS SUpL Version,gnss_supl_version,GPS
6793,GNSS RRLP8 RRC8 SUpported,gnss_rrlp8_rrc8_supported,GPS
6794,GNSS DPO Dwell Duration,gnss_dpo_dwell_duration,GPS
6795,CGPS DPO Entry Criteria Control,cgps_dpo_entry_criteria_control,GPS
6796,CGPS Default Region Size,cgps_default_region_size,GPS
6797,CGPS Use QWIP For ODP,cgps_use_qwip_for_odp,GPS
6816,GNSS MGP Error Recovery,gnss_mgp_error_recovery,GPS
6817,GNSS HBW Control,gnss_hbw_control,GPS
6835,CGPS Sensors SPI Opinterval,cgps_sensors_spi_opinterval,GPS
6836,CGPS Sensors SPI Detectthreshold,cgps_sensors_spi_detectthreshold,GPS
6837,SNSD Motion Scale Factor,snsd_motion_scale_factor,GPS
6838,SNSD Motion Zmd Window Samples,snsd_motion_zmd_window_samples,GPS
6839,SNSD Motion Zmd Method,snsd_motion_zmd_method,GPS
6840,SNSD Motion transient length,snsd_motion_transient_length,GPS
6845,GNSS Embedded XTRA Client Enabled,gnss_embedded_xtra_client_enabled,GPS
6846,GNSS WWAN Network Preference,gnss_wwan_network_preference,GPS
6847,GNSS Medium Preferences Number,gnss_medium_preferences_number,GPS
6848,GNSS Medium Preference List,gnss_medium_preference_list,GPS
6849,GNSS Auto NMEA Comport Enabled,gnss_auto_nmea_comport_enabled,GPS
6851,GNSS Embedded XTRA Time Client Enabled,gnss_embedded_xtra_time_client_enabled,GPS
6852,GNSS 1xUP MSA Trusted Mode Disable,gnss_1xup_msa_trusted_mode_disable,GPS
7141,GNSS Enable External Confidence Infor,gnss_enable_external_confidence_infor,GPS
7144,GNSS XSPI Injection Timeout,gnss_xspi_injection_timeout,GPS
7163,CGPS SM Supl Network Params,cgps_sm_supl_network_params,GPS
7165,GNSS OEM Feature Mask,gnss_oem_feature_mask,GPS
65603,Sleep Time Tag Offset To Add To Network Time,/nv/item_files/gps/cgps/me/cgps_sleep_timetag_offset,GPS
65604,gpsOneXTRA  Preferred Maximum Valid Age,/nv/item_files/gps/cgps/xtra/cgps_xtra_preferred_max_valid_age,GPS
65685,GNSS PDCOMM LBS APN Profile List,/nv/item_files/gps/cgps/pdcomms/gnss_pd_comms_lbs_apn_profile_list,GPS
65686,GNSS PDCOMM XTRA APN Profile List,/nv/item_files/gps/cgps/pdcomms/gnss_pd_comms_xtra_apn_profile_list,GPS
65688,MGP External LNA State Control,/cgps/nv/item_files/me/gnss_external_lna_state,GPS
65689,MGP Clock Source Control,/cgps/nv/item_files/me/gnss_clock_source,GPS
65690,MGP Notch Filter Control,/cgps/nv/item_files/me/gnss_notch_filter_control,GPS
65691,MGP GSM Blanking Control,/cgps/nv/item_files/me/gnss_gsm_blanking_control,GPS
65692,MGP Added Frequency Offset,/cgps/nv/item_files/me/gnss_added_frequency_offset,GPS
65732,GNSS NMEA Sentence Type,/nv/item_files/gps/cgps/sm/gnss_nmea_sentence_type,GPS
65733,XTRA-T Feature Control,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_feature_control,GPS
65734,XTRA-T Primary Server Address,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_primary_svr_address,GPS
65735,XTRA-T Primary Server Port,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_primary_svr_port,GPS
65736,XTRA-T Secondary Server Address,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_secondary_svr_address,GPS
65737,XTRA-T Secondary Server Port,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_secondary_svr_port,GPS
65738,XTRA-T User Session Control,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_user_session_control,GPS
65739,XTRA-T Time Injection Control,/cgps/nv/item_files/ulp/tle/gnss_xtrat_time_injection_control,GPS
65740,XTRA-T Client Token,/cgps/nv/item_files/ulp/tle/gnss_xtrat_client_token,GPS
65741,XTRA-T Data Transfer Max Elapsed Time Threshold,/cgps/nv/item_files/ulp/tle/gnss_xtrat_elapsed_threshold,GPS
65742,XTRA-T Periodic Upload Live Count,/cgps/nv/item_files/ulp/tle/gnss_xtrat_periodic_upload_livecount,GPS
65743,XTRA-T Data Transfer Max Elapsed Retry,/cgps/nv/item_files/ulp/tle/gnss_xtrat_dataxfr_num_max_elapsed_retry,GPS
65744,XTRA-T Last Download GPS Week,/cgps/nv/item_files/ulp/tle/gnss_xtrat_last_download_gps_week,GPS
65745,XTRA-T Last Download GPS Second,/cgps/nv/item_files/ulp/tle/gnss_xtrat_last_download_gps_sec,GPS
65746,XTRA-T Database Age Threshold,/cgps/nv/item_files/ulp/tle/gnss_tle_database_age_threshold,GPS
65747,XTRA-T Database Age Threshold Unit,/cgps/nv/item_files/ulp/tle/gnss_tle_database_age_threshold_unit,GPS
65748,XTRA-T Time Injection Time Uncertainty,/cgps/nv/item_files/ulp/tle/gnss_xtrat_time_inject_tunc,GPS
65749,XTRA-T Minimum Time Between Server Access,/cgps/nv/item_files/ulp/tle/gnss_xtrat_min_time_server_access,GPS
65750,XTRA-T Upload Session Control,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_upload_sess_control,GPS
65751,TLE Cell-ID Level 2 Default Radius,/cgps/nv/item_files/ulp/tle/gnss_tle_cellid_l2_dft_radius,GPS
65752,TLE Cell-ID Level 3 Default Radius,/cgps/nv/item_files/ulp/tle/gnss_tle_cellid_l3_dft_radius,GPS
65753,TLE Cell-ID Level 4 Default Radius,/cgps/nv/item_files/ulp/tle/gnss_tle_cellid_l4_dft_radius,GPS
65754,XTRA-T Primary Request Uniform Resource Identifier,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_primary_request_uri,GPS
65755,XTRA-T Secondary Request Uniform Resource Identifier,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_secondary_request_uri,GPS
65756,XTRA-T Use Transport Security,/cgps/nv/item_files/ulp/tle/gnss_xtrat_use_transport_security,GPS
65775,GNSS Send No_Fix Report Control,/nv/item_files/gps/cgps/sm/gnss_send_no_fix_report,GPS
65786,Peak Processing Disable LTE Detection Increase,/nv/item_files/gps/cgps/me/gnss_pp_disable_lte_detection_increase,GPS
65811,NV to enable/disable SUPL as 1x up protocol,/nv/item_files/gps/cgps/sm/gnss_1x_up_supl_enable,GPS
66034,GNSS ME Cell DB Max Timetag Age,/nv/item_files/gps/cgps/me/gnss_me_celldb_max_timetag_age_sec,GPS
67206,BP Amplitude limits to reset engine after jammer removal,/nv/item_files/gps/cgps/me/cgps_me_bp_amp_reset_limits,GPS
67217,AGPS RAT Preference,/nv/item_files/gps/cgps/sm/gnss_agps_rat_pref_config,GPS
67225,Use LPP when on LTE,/nv/item_files/gps/cgps/sm/gnss_lpp_enable,GPS
67272,GNSS GPS to GLONASS RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_rfgd_glo_ns,GPS
67273,GNSS GPS GLO RF Group Delay Uncertainty,/cgps/nv/item_files/me/gnss_me_rfgd_glo_uncertainty_ns,GPS
67274,GNSS TauGPS Uncertainty Bound,/cgps/nv/item_files/me/gnss_me_valid_taugps_unc_bound_ns,GPS
67297,Main WWAN TX Antenna IM Jamming Power,/nv/item_files/gps/cgps/me/gnss_main_wwan_tx_antenna_im_jamming_power,GPS
67298,Auxiliary WWAN TX Antenna IM Jamming Power,/nv/item_files/gps/cgps/me/gnss_aux_wwan_tx_antenna_im_jamming_power,GPS
67299,GNSS WLAN & BT Delta Tx IM Jamming Power,/nv/item_files/gps/cgps/me/gnss_wlan_bt_delta_tx_antenna_im_jamming_power,GPS
67336,GNSS GPS 5Ms IQSUM Logging Enable,/cgps/nv/item_files/me/gnss_me_gps_5ms_iqsum_logging,GPS
69672,GNSS Peak Antenna Gain,/nv/item_files/gps/cgps/me/gnss_peak_antenna_gain,GPS
69688,GNSS LTE B13 TX GPS XCORR,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_gps_xcorr_mask_increase,GPS
69711,XTRA-T Keep Warm Timeout,/cgps/nv/item_files/ulp/tle/gnss_xtrat_keep_warm_timeout,GPS
69713,GNSS LTE B13 TX GLO ACI Mask,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_aci_mask_increase,GPS
69714,GNSS LTE B13 TX GPS/SBAS C/No Threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_gps_sbas_cno_threshold,GPS
69715,GNSS LTE B13 TX GLONASS Frequency -7 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_neg_seven_cno_threshold,GPS
69716,GNSS LTE B13 TX GLONASS Frequency -6 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_neg_six_cno_threshold,GPS
69717,GNSS LTE B13 TX GLONASS Frequency -5 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_neg_five_cno_threshold,GPS
69718,GNSS LTE B13 TX GLONASS Frequency -4 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_neg_four_cno_threshold,GPS
69719,GNSS LTE B13 TX GLONASS Frequency -3 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_neg_three_cno_threshold,GPS
69720,GNSS LTE B13 TX GLONASS Frequency -2 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_neg_two_cno_threshold,GPS
69721,GNSS LTE B13 TX GLONASS Frequency -1 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_neg_one_cno_threshold,GPS
69722,GNSS LTE B13 TX GLONASS Frequency 0 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_zero_cno_threshold,GPS
69723,GNSS LTE B13 TX GLONASS Frequency 1 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_plus_one_cno_threshold,GPS
69724,GNSS LTE B13 TX GLONASS Frequency 2 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_plus_two_cno_threshold,GPS
69725,GNSS LTE B13 TX GLONASS Frequency 3 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_plus_three_cno_threshold,GPS
69726,GNSS LTE B13 TX GLONASS Frequency 4 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_plus_four_cno_threshold,GPS
69727,GNSS LTE B13 TX GLONASS Frequency 5 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_plus_five_cno_threshold,GPS
69728,GNSS LTE B13 TX GLONASS Frequency 6 C/No threshold,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_frequency_plus_six_cno_threshold,GPS
69740,GNSS QMI Service Configuration,/nv/item_files/gps/cgps/qmi/gnss_qmi_config,GPS
69742,GNSS LTE B13 TX GPS/SBAS  C/No offset,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_gps_cno_offset,GPS
69743,GNSS LTE B13 TX GLONASS C/No offset,/nv/item_files/gps/cgps/me/gnss_lte_b13_tx_glo_glo_cno_offset,GPS
69746,Configure premium service,/nv/item_files/gps/cgps/sm/gnss_config_premium_service,GPS
69748,LPP CP transport retransmission timeout control,/nv/item_files/gps/cgps/sm/gnss_lpp_cp_timeout,GPS
70192,Select A-Glonass positioning protocols,/nv/item_files/gps/cgps/sm/gnss_assisted_glo_protocol_select,GPS
70203,TLE Downloaded CellDb Pages,/cgps/nv/item_files/ulp/tle/gnss_tle_cell_db_pages,GPS
70204,TLE Downloaded RegionDb Pages,/cgps/nv/item_files/ulp/tle/gnss_tle_region_db_pages,GPS
70205,TLE Self Learned PosDb Pages,/cgps/nv/item_files/ulp/tle/gnss_tle_selflearned_db_pages,GPS
70206,TLE Enable/Disable Write To Storage,/cgps/nv/item_files/ulp/tle/gnss_tle_write_to_storage_control,GPS
70207,XTRA-T Enable/Disable simultaneous XTRA/XTRA-T,/cgps/nv/item_files/ulp/tle/gnss_xtrat_enable_simultaneous_xtra_xtrat,GPS
70236,Select Emergency Services Protocol,/nv/item_files/gps/cgps/sm/gnss_emergency_session_protocol_select,GPS
70237,GNSS Spectrum Analyzer Control,/cgps/nv/item_files/me/gnss_span_control,GPS
70238,GNSS Spectrum Analyzer Job Timers,/cgps/nv/item_files/me/gnss_span_timers,GPS
70247,PDCOMMS Emergnecy LBS Profile List,/nv/item_files/gps/cgps/pdcomms/gnss_pd_comms_emergency_lbs_apn_profile_list,GPS
70325,system processing loss for BeiDou RF chain. unit 0.1 db,/nv/item_files/gps/cgps/me/gnss_bds_path_loss,GPS
70326,Configures the capabilities of the receiver.,/nv/item_files/gps/cgps/me/gnss_config,GPS
70336,To configure the maximum value of dynamic HEPE limit.,/nv/item_files/gps/cgps/sm/gnss_lm_hepe_threshold,GPS
70366,APQ pga gain without WWAN info,/nv/item_files/gps/cgps/me/gnss_apq_pga_cal,GPS
71552,Spectral amplitude of white clock acceleration noise,/cgps/nv/item_files/pe/gnss_nv_clock_freq_process_noise,GPS
71553,Spectral amplitude of white clock frequency noise,/cgps/nv/item_files/pe/gnss_nv_clock_phase_process_noise,GPS
71555,gnss unavail indication timeout in seconds,/nv/item_files/gps/cgps/sm/gm_gnss_unavail_ind_timeout,GPS
71559,The item is to provide the following options to set 2D or 3D flag for,/nv/item_files/gps/cgps/pe/gnss_nv_2d_set_opt,GPS
71562,GNSS GM Motion Detection Sources,/nv/item_files/gps/cgps/sm/gm_motion_detection_sources,GPS
71563,GNSS GM Position QoS Session Timeout,/nv/item_files/gps/cgps/sm/gm_gnss_position_qos_session_timeout,GPS
71573,GNSS GM Position Sources,/nv/item_files/gps/cgps/sm/gm_position_sources,GPS
71582,It configures the capabilities of the receiver,/cgps/nv/item_files/me/gnss_sp_on_heap_alloc_mode,GPS
71583,GNSS SP On Heap Allocation Fail Test ID,/cgps/nv/item_files/me/gnss_sp_on_heap_alloc_fail_test_id,GPS
71584,GNSS SP On Heap Allocation Fail Buf Num,/cgps/nv/item_files/me/gnss_sp_on_heap_alloc_fail_buf_num,GPS
71599,GNSS Supl UDP Enable Select,/nv/item_files/gps/cgps/sm/gnss_supl_udp_enable_select,GPS
72511,GNSS GM High Responsiveness Config,/nv/item_files/gps/cgps/sm/gm_high_responsiveness_config,GPS
72514,GNSS GM Max Pos Unc Accepted,/nv/item_files/gps/cgps/sm/gm_gnss_max_pos_unc_accepted,GPS
72533,GNSS NV EFS SM E911 Config,/nv/item_files/gps/cgps/sm/gnss_nv_efs_sm_e911_config,GPS
72535,GNSS GM Use NW Asst Fixes,/nv/item_files/gps/cgps/sm/gm_use_nw_asst_fixes,GPS
72544,Time in ms to wait before reattempting XTRA-T session after previous,/nv/item_files/gps/cgps/ulp/tle/gnss_xtrat_conn_failure_retry_period,GPS
72545,GNSS EFS SDP Charger Control Enabled,/nv/item_files/gps/cgps/sdp/sdp_charger_ctrl_enabled,GPS
72546,Position pinning re-pin threshold in meters,/cgps/nv/item_files/pe/gnss_nv_pospinning_repin_thresh,GPS
72557,GNSS 5GHZ Wlan Main WWAN Ttx Antenna IM Jamming Power,/nv/item_files/gps/cgps/me/gnss_5ghz_wlan_main_wwan_tx_antenna_im_jamming_power,GPS
72558,GNSS 5GHZ Wlan AUX WWAN Tx Antenna IM Jamming Power,/nv/item_files/gps/cgps/me/gnss_5ghz_wlan_aux_wwan_tx_antenna_im_jamming_power,GPS
72559,CGPS SM GM CPI Request Rate,/nv/item_files/gps/cgps/sm/gm_cpi_request_rate,GPS
72568,GNSS TLE Test Control,/cgps/nv/item_files/ulp/tle/gnss_tle_test_control,GPS
72569,GM Medium Responsiveness Value,/nv/item_files/gps/cgps/sm/gm_med_resp_backoff,GPS
72570,GM Challenging GPS Env Backoff Config,/nv/item_files/gps/cgps/sm/gm_challenging_gps_env_backoff_config,GPS
72571,GM Motion State Speed Config,/nv/item_files/gps/cgps/sm/gm_motion_state_speed_config,GPS
72572,GM Challenging GPS Env Motion Sensing Distance Config,/nv/item_files/gps/cgps/sm/gm_challenging_gps_env_motion_sensing_dist_config,GPS
72575,GNSS Span Timers Extended,/cgps/nv/item_files/me/gnss_span_timers_extended,GPS
72600,GNSS WLAN Tx Indication Mode,/nv/item_files/gps/cgps/me/gnss_wlan_tx_indication_mode,GPS
72601,MGP Dynamic PGA Configuration,/cgps/nv/item_files/me/gnss_dynamic_pga_config,GPS
72606,GNSS ME Ext Time Transfer Offset NS,/nv/item_files/gps/cgps/me/gnss_me_ext_time_transfer_offset_ns,GPS
72613,GNSS MP CNO Weighing Factor,/cgps/nv/item_files/me/gnss_mp_cno_weighing_factor,GPS
72622,"If bit 0 is set to 1, force crash the device when QMI_PDS response contains an error. Bit 0 should only be enabled during internal testing to collect crash dumps. Bit 1-31 are reserved.",/cgps/nv/item_files/sm/gnss_lbs_config,GPS
73511,MGP PGA Backoff Config,/cgps/nv/item_files/me/gnss_pga_backoff_config,GPS
73536,GNSS FP Exception Config,/nv/item_files/gps/cgps/common/gnss_fp_exception_config,GPS
73541,GNSS  Meas Full Meas Block Control,/nv/item_files/gps/cgps/pe/gnss_nv_rpt_full_meas_blk,GPS
73542,GNSS  Meas QMI Reporting Control,/nv/item_files/gps/cgps/sm/gnss_meas_qmi_reporting_enable_control,GPS
73543,Gnss PE XTRA Memory Release Time Interval,/cgps/nv/item_files/pe/gnss_nv_xtra_mem_rel_time_interval,GPS
73547,GNSS WWAN ME Config,/nv/item_files/gps/cgps/me/gnss_wwan_me_config,GPS
73549,GNSS GIT IPC Sleep Ms,/cgps/nv/item_files/me/gnss_git_ipc_sleep_ms,GPS
73550,GNSS GIT IPC Sleep Ms,/cgps/nv/item_files/me/gnss_git_cpu_busy_thresh_ms,GPS
73555,GNSS Blanking in LTE Transmission Control,/nv/item_files/gps/cgps/me/gnss_lte_tx_gnss_blanking_control,GPS
73565,Gnss Tle Tlm Upload Check Timer Period,/cgps/nv/item_files/ulp/tle/gnss_tle_tlm_upload_check_timer_period,GPS
73566,Gnss TleTlm Upload Cell Count Threshold,/cgps/nv/item_files/ulp/tle/gnss_tle_tlm_upload_cellcount_threshold,GPS
73568,Gnss GPM Config1,/nv/item_files/gps/cgps/me/gnss_gpm_config1,GPS
73575,GNSS TM Config AGNSS Tx Delay,/nv/item_files/gps/cgps/sm/tm_config_agnss_transmission_delay,GPS
73578,Used for simulated error cases for memory allocation in NF and CD,/cgps/nv/item_files/pe/gnss_nf_cd_memalloc_failure,GPS
73579,Used for simulated error cases for memory allocation in MC,/cgps/nv/item_files/me/gnss_mc_memalloc_failure,GPS
73580,Advanced FDIC usage mask,/nv/item_files/gps/cgps/pe/gnss_nv_advanced_fdic,GPS
73586,GNSS Emergency Support Config,/nv/item_files/gps/cgps/sm/gnss_emergency_support_config,GPS
73601,gpsOne GNSS 5Ms GNSS IQSUM Logging Enable,/cgps/nv/item_files/me/gnss_me_gnss_5ms_iqsum_logging,GPS
73603,GNSS SM LPP GRT 24 OTDOA Ad Support,/nv/item_files/gps/cgps/sm/gnss_sm_lpp_grt_24_otdoa_ad_support,GPS
73613,Location Batching Geofence Radius,/nv/item_files/gps/cgps/sm/batching_gf_radius,GPS
73614,Location Batching Config Mask,/nv/item_files/gps/cgps/sm/batching_config_mask,GPS
73615,GNSS Pedestrian Dead Reckoning Timeout,/nv/item_files/gps/cgps/pe/gnss_nv_ped_dr_timeout,GPS
73639,GNSS Tle Tlm Upload Force Timer Period,/cgps/nv/item_files/ulp/tle/gnss_tle_tlm_upload_force_timer_period,GPS
73640,GNSS Tle Tlm Tdp Mem Check Threshold,/cgps/nv/item_files/ulp/tle/gnss_tle_tlm_tdp_mem_check_threshold,GPS
73644,GNSS Pedestrian DR PLE Control,/cgps/nv/item_files/pe/gnss_nv_pdr_subfeature_control,GPS
73646,GNSS EFT Control,/nv/item_files/gps/cgps/me/gnss_eft_control,GPS
73652,enable registration of GM to the LOWI service on LPASS,/nv/item_files/gps/cgps/sm/gm_lowi_register_enable,GPS
73653,subscribe to different events supported on LOWI LPASS,/nv/item_files/gps/cgps/sm/gm_lowi_subscription_mask,GPS
73654,configure how the GM registers with the LOWI service on LPASS,/nv/item_files/gps/cgps/sm/gm_lowi_reg_config,GPS
73663,a threshold for the number of self-learned cells allowed to be stored,/nv/item_files/gps/cgps/ulp/tle/gnss_tle_tlm_visited_cell_history_size,GPS
73665,Sfs Write Interval Factor,/nv/item_files/gps/cgps/pe/gnss_nv_sfs_write_factor,GPS
73672,enable/disable Enhanced TBF5 feature,/nv/item_files/gps/cgps/me/gnss_etbf5_config,GPS
73691,GNSS GM COMP Logging Rate Secs,/nv/item_files/gps/cgps/sm/gm_comp_logging_rate_secs,GPS
73692,IOD Configuration Switches,/nv/item_files/gps/cgps/sm/iod_configuration_switches,GPS
73695,GNSS NV Position Crosscheck Distance,/nv/item_files/gps/cgps/pe/gnss_nv_position_crosscheck_distance,GPS
73709,GNSS SAMLite feature control,/cgps/nv/item_files/samlite/gnss_nv_samlite_control,GPS
73710,GNSS OTDOA Enabled,/nv/item_files/gps/cgps/sm/gnss_otdoa_control,GPS
73715,GNSS Me Automotive Config,/nv/item_files/gps/cgps/me/gnss_automotive,GPS
73718,GNSS ME CPE Control,/cgps/nv/item_files/me/gnss_me_cpe_control,GPS
73722,GM Filter WLAN Freq Mask,/nv/item_files/gps/cgps/sm/gm_filter_wlan_freq_mask,GPS
73734,GNSS SM AON Default GNSS Position Qos Session Timeout,/nv/item_files/gps/cgps/sm/aon_default_gnss_position_qos_session_timeout,GPS
73735,GNSS SM AON Default CPI Request Rate,/nv/item_files/gps/cgps/sm/aon_default_cpi_request_rate,GPS
73736,GNSS SM AON Default Challenging GPS Env Backoff,/nv/item_files/gps/cgps/sm/aon_default_challenging_gps_env_backoff_config,GPS
73737,GNSS AON Default Challenging GPS Env Motion Sensing Distance,/nv/item_files/gps/cgps/sm/aon_default_challenge_gps_env_motion_sensing_dist_config,GPS
73741,GNSS GPS to Beidou RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_rfgd_bds_ns,GPS
73742,GNSS GPS to Beidou RF Group Delay uncertainty,/cgps/nv/item_files/me/gnss_me_rfgd_bds_uncertainty_ns,GPS
73743,GNSS Beidou to Glonass RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_rfgd_bds2glo_ns,GPS
73744,GNSS Beidou to Glonass RF Group Delay uncertainty,/cgps/nv/item_files/me/gnss_me_rfgd_bds2glo_uncertainty_ns,GPS
73745,GPS to Galileo RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2gal_ns,GPS
73746,GNSS GPS to Galileo RF Group Delay Uncertainty,/cgps/nv/item_files/me/gnss_me_rfgd_gps2gal_uncertainty_ns,GPS
73747,GNSS Galileo to Beidou RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gal2bds_ns,GPS
73748,GNSS Galileo to Beidou RF Group Delay uncertainty,/cgps/nv/item_files/me/gnss_me_rfgd_gal2bds_uncertainty_ns,GPS
73749,GNSS Galileo to Glonass RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gal2glo_ns,GPS
73750,GNSS Galileo to Glonass RF Group Delay uncertainty,/cgps/nv/item_files/me/gnss_me_rfgd_gal2glo_uncertainty_ns,GPS
73751,system processing loss for Galileo RF chain. Unit 0.1 dB,/nv/item_files/gps/cgps/me/gnss_gal_path_loss,GPS
73765,GPS Supplement Config From SIM,/nv/item_files/gps/cgps/sm/supl_config_from_sim,GPS
73772,Enable/disable AGNSS acquisition optimization feature.,/cgps/nv/item_files/me/gnss_me_agnss_acq_optimization,GPS
73786,GNSS MC Power Config,/nv/item_files/gps/cgps/me/gnss_mc_power_config,GPS
73800,Loc Supported Min Interval,/cgps/nv/item_files/loc/loc_supported_min_interval,GPS
73827,GNSS PD Comms SSL Cert Key Length,/nv/item_files/gps/cgps/pdcomms/gnss_pd_comms_ssl_cert_key_length,GPS
73860,GNSS XTRA-T Maximum AP response time,/cgps/nv/item_files/ulp/tle/gnss_tle_max_ap_response_time_period_sec,GPS
73861,GNSS TDP Download and Positioning Control,/cgps/nv/item_files/ulp/tle/gnss_tle_tdp_downld_pos_control,GPS
73862,GNSS SM NHZ Config,/nv/item_files/gps/cgps/sm/gnss_nhz_config,GPS
73863,GNSS SM TM Lppe Up Config,/nv/item_files/gps/cgps/sm/gnss_lppe_up_config,GPS
73869,GNSS Tech Sel LPPM Config,/nv/item_files/gps/cgps/sm/tech_sel_lppm_config,GPS
73872,GNSS Tech Sel Wifi Config,/nv/item_files/gps/cgps/sm/tech_sel_wifi_assist_config,GPS
73877,GNSS Me Periodic Reg Dump Config,/cgps/nv/item_files/me/gnss_periodic_reg_dump_config,GPS
73888,GNSS LPPe CP Config,/nv/item_files/gps/cgps/sm/gnss_lppe_cp_config,GPS
73897,GNSS SM Lpp Rel12 2Timer Support,/nv/item_files/gps/cgps/sm/gnss_sm_lpp_rel12_2timer_support,GPS
73898,GNSS ME DR Sync Pulse Width(ms),/cgps/nv/item_files/me/gnss_me_drsync_pulsewidth_config,GPS
73899,GNSS XTRA uax3 config,/nv/item_files/gps/cgps/xtra/gnss_uax3_config,GPS
73902,GNSS ME BP Jammer Configuration,/cgps/nv/item_files/me/gnss_bp_jammer_config,GPS
73905,GNSS Me Enable Non-dedicated Task Grid Log,/cgps/nv/item_files/me/gnss_enable_non_dedicated_task_grid_log,GPS
73913,GNSS Me Enable BUP Flag,/cgps/nv/item_files/me/gnss_enable_bup_flag,GPS
73914,GNSS  Samlite Mag Factory Cal Params,/nv/item_files/gps/cgps/samlite/gnss_nv_samlite_mag_fac_cal,GPS
73917,GNSS ME CatM LTE Coex Config,/cgps/nv/item_files/me/gnss_me_catm_lte_gnss_coex_config,GPS
73930,GNSS LPPM Unit Test Config,/cgps/nv/item_files/pe/gnss_lppm_unit_test_config,GPS
73933,GNSS Fusion CSM Data Upload Threshold,/nv/item_files/gps/cgps/sm/gnss_fusion_csm_data_upload_threshold,GPS
73934,GNSS Fusion CSM SB Buffer Size,/nv/item_files/gps/cgps/sm/gnss_fusion_csm_sb_buffer_size,GPS
73935,GNSS Fusion CSM Maximum Number of SSID Supported,/nv/item_files/gps/cgps/sm/gnss_fusion_csm_max_ssid_supported,GPS
73938,GNSS SM Concurrent Sessions Support Config,/nv/item_files/gps/cgps/sm/gnss_concurrent_sessions_support_config,GPS
73941,GNSS  SM Fusion CSM Venue Tracking Resp Sec,/nv/item_files/gps/cgps/sm/gnss_fusion_csm_venue_tracking_resp_sec,GPS
73947,GNSS ME Raw SV Data Allowed,/cgps/nv/item_files/me/raw_SV_data_allowed,GPS
73951,GNSS SM Assisted Bds Protocol Enable Mask,/nv/item_files/gps/cgps/sm/assisted_bds_protocol_enable_mask,GPS
73960,GNSS Me Enable ESR Mitigation,/cgps/nv/item_files/me/gnss_me_enable_esr_mitigation,GPS
73961,Gnss TLE Max Num Cells Per Upload,/cgps/nv/item_files/ulp/tle/gnss_tle_max_num_cells_per_upload,GPS
73962,GNSS XTRAT Max Num Uploads Per Day,/cgps/nv/item_files/ulp/tle/gnss_xtrat_max_num_uploads_per_day,GPS
73963,GNSS XTRAT Max Num Downloads Per Day,/cgps/nv/item_files/ulp/tle/gnss_xtrat_max_num_downloads_per_day,GPS
73967,GNSS XTRAT Min Time Server Access Quota Exceeded,/cgps/nv/item_files/ulp/tle/gnss_xtrat_min_time_server_access_quota_exceeded,GPS
73972,GNSS SM Fusion Baro Weight,/cgps/nv/item_files/sm/gnss_fusion_csm_baro_weight,GPS
73973,GNSS SM Fusion Baro Sample Rate(Hz),/cgps/nv/item_files/sm/gnss_fusion_csm_baro_rate_hz,GPS
73974,GNSS SM CSM Test Control,/cgps/nv/item_files/sm/gnss_fusion_csm_test_control,GPS
73975,GNSS ME F3 Reduction,/cgps/nv/item_files/me/gnss_me_f3_reduction,GPS
73976,GNSS ME WIFI-GNSS Critical Demod,/cgps/nv/item_files/me/gnss_enable_wifi_gnss_crit_demod,GPS
73977,GNSS Gen9 Reserved 1,/cgps/nv/item_files/me/gnss_gen9_general_1,GPS
73978,GNSS Gen9 Reserved 2,/cgps/nv/item_files/me/gnss_gen9_general_2,GPS
73987,GNSS PE Test Feature Config,/cgps/nv/item_files/pe/gnss_test_feature_config,GPS
73994,GNSS PE Enable Static Pinning,/nv/item_files/gps/cgps/pe/gnss_nv_enable_static_pinning,GPS
74003,/nv/item_files/gps/cgps/sm/gm_drive_to_ped_predictor_enable,/nv/item_files/gps/cgps/sm/gm_drive_to_ped_predictor_enable,GPS
74004,/cgps/nv/item_files/sm/gnss_fusion_csm_operation_mode,/cgps/nv/item_files/sm/gnss_fusion_csm_operation_mode,GPS
74007,/nv/item_files/gps/cgps/pe/gnss_nv_vehicle_dr_timeout,/nv/item_files/gps/cgps/pe/gnss_nv_vehicle_dr_timeout,GPS
74015,GNSS Loc OTB Buff Threshold,/nv/item_files/gps/cgps/loc/loc_otb_buffer_threshold,GPS
74016,GNSS LM HEPE Threshold for DR Fixes,/nv/item_files/gps/cgps/sm/gnss_lm_hepe_threshold_dr_fixes,GPS
74017,GNSS ME DPO Control,/cgps/nv/item_files/me/gnss_dpo_control,GPS
74025,GNSS ME GPS-GLONASS RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2glo_ns,GPS
74031,GNSS Tech Sel LPPM TBM Profile,/nv/item_files/gps/cgps/sm/tech_sel_lppm_tbm_profile,GPS
74044,GNSS ME Long Coh Ctrl,/cgps/nv/item_files/me/gnss_long_coh_ctrl,GPS
74054,GNSS Debug Multiband Config,/cgps/nv/item_files/me/gnss_debug_multiband_config,GPS
74055,GNSS ME Multiband Config,/nv/item_files/gps/cgps/me/gnss_multiband_config,GPS
74059,GNSS ME Vote To Disable LTE MicroSleep,/cgps/nv/item_files/me/gnss_vote_to_disable_lte_microsleep,GPS
74067,GNSS ME BDS SV Blacklisting Control,/cgps/nv/item_files/me/gnss_bds_sv_blacklist,GPS
74075,GNSS SAMLite Sensor Streaming Test Mode,/cgps/nv/item_files/samlite/gnss_nv_sensor_stream_test_mode_cfg,GPS
74076,GNSS SAMLite Tilt-Based Magnetometer Calibration Budget Config,/cgps/nv/item_files/samlite/gnss_nv_tilt_mag_cal_budget_cfg,GPS
74082,GNSS ME DPO IQ sums logging enabled,/cgps/nv/item_files/me/gnss_me_dpo_iq_sums_logging_enable,GPS
74083,/nv/item_files/gps/cgps/me/gnss_me_L1toL5_rfgd_ns,/nv/item_files/gps/cgps/me/gnss_me_L1toL5_rfgd_ns,GPS
74084,/cgps/nv/item_files/me/gnss_me_L1toL5_rfgd_unc_ns,/cgps/nv/item_files/me/gnss_me_L1toL5_rfgd_unc_ns,GPS
74085,/nv/item_files/gps/cgps/me/gnss_me_E1toE5a_rfgd_ns,/nv/item_files/gps/cgps/me/gnss_me_E1toE5a_rfgd_ns,GPS
74086,/cgps/nv/item_files/me/gnss_me_E1toE5a_rfgd_unc_ns,/cgps/nv/item_files/me/gnss_me_E1toE5a_rfgd_unc_ns,GPS
74096,GNSS MultiBand Test Control,/cgps/nv/item_files/pe/gnss_mb_test_control,GPS
74098,GNSS GPS L5 Path Loss,/nv/item_files/gps/cgps/me/gnss_gps_L5_path_loss,GPS
74099,GNSS GAL E5a Path Loss,/nv/item_files/gps/cgps/me/gnss_gal_E5a_path_loss,GPS
74107,GNSS B14 MTJ Mitigation in Peak Processing,/nv/item_files/gps/cgps/me/gnss_me_b14_mtj_mitigate_pp,GPS
74118,GNSS SW Blanking DP threshold,/cgps/nv/item_files/me/gnss_sw_blank_dp_thr,GPS
74119,GNSS SW Blanking CP threshold,/cgps/nv/item_files/me/gnss_sw_blank_cp_thr,GPS
74121,GNSS SM SHA TLS Support,/nv/item_files/gps/cgps/sm/gnss_sm_sha_tls_support,GPS
74131,GNSS PE RTI Health Expiration Period,/cgps/nv/item_files/pe/gnss_rti_health_expiration_period,GPS
74132,GTP External Provider Cell Throttle Period,/cgps/nv/item_files/ulp/tle/gnss_tle_external_provider_cell_throttle_period,GPS
74133,GTP External Provider Upload Cell Threshold,/cgps/nv/item_files/ulp/tle/gnss_tle_external_provider_upload_cell_threshold,GPS
74137,GNSS Forced Multiband Engagement Config,/nv/item_files/gps/cgps/me/gnss_me_forced_mb_engagement_config,GPS
74138,GNSS Acquisition Power Mode Config,/cgps/nv/item_files/me/gnss_me_acq_power_mode_config,GPS
74140,/nv/item_files/gps/cgps/sm/gnss_sm_agps_features,/nv/item_files/gps/cgps/sm/gnss_sm_agps_features,GPS
74178,GNSS SDP Delayed Sensors Off Time,/cgps/nv/item_files/sdp/sdp_delayed_snsoff_time,GPS
74179,GNSS SLIM Test Mode,/cgps/nv/item_files/slim/gnss_nv_slim_test_cfg,GPS
74180,Enable TIS testing on Band5 frequency,/nv/item_files/gps/cgps/me/gnss_me_tis_band5_controls,GPS
74184,NV to control the datum in which NMEA sentences are reported,/nv/item_files/gps/cgps/sm/gnss_control_datum_nmea_sentence,GPS
74191,/nv/item_files/gps/cgps/sm/gnss_control_custom_dynamic_HEPE_usage,/nv/item_files/gps/cgps/sm/gnss_control_custom_dynamic_HEPE_usage,GPS
74192,/nv/item_files/gps/cgps/sm/gtp_wwan_fix_timer_duration,/nv/item_files/gps/cgps/sm/gtp_wwan_fix_timer_duration,GPS
74193,/nv/item_files/gps/cgps/sm/gtp_wwan_min_interval_btwn_req,/nv/item_files/gps/cgps/sm/gtp_wwan_min_interval_btwn_req,GPS
74194,/nv/item_files/gps/cgps/sm/bypass_gnss_aiding_check,/nv/item_files/gps/cgps/sm/bypass_gnss_aiding_check,GPS
74210,/nv/item_files/gps/cgps/loc/loc_accuracy_threshold_for_high_accuracy,/nv/item_files/gps/cgps/loc/loc_accuracy_threshold_for_high_accuracy,GPS
74214,/nv/item_files/gps/cgps/me/gnss_me_nhz_cfg,/nv/item_files/gps/cgps/me/gnss_me_nhz_cfg,GPS
74215,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node1,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node1,GPS
74216,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node2,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node2,GPS
74217,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node3,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node3,GPS
74218,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node4,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node4,GPS
74219,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node5,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node5,GPS
74220,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node6,/nv/item_files/gps/cgps/pe/gnss_nv_sbas_sdcm_area_node6,GPS
74222,/cgps/nv/item_files/me/gnss_me_sdcm_config,/cgps/nv/item_files/me/gnss_me_sdcm_config,GPS
74225,/nv/item_files/gps/cgps/me/gnss_me_e911_constellation_disablement,/nv/item_files/gps/cgps/me/gnss_me_e911_constellation_disablement,GPS
74230,GNSS Allow GNSS Signal Measurement Report during LTE B13/B14,/nv/item_files/gps/cgps/me/gnss_me_allow_gnss_signals_in_b13b14,GPS
74231,/nv/item_files/gps/cgps/sm/bypass_xtra_validity_check,/nv/item_files/gps/cgps/sm/bypass_xtra_validity_check,GPS
74234,/nv/item_files/gps/cgps/sm/gnss_e911_extension_window,/nv/item_files/gps/cgps/sm/gnss_e911_extension_window,GPS
74235,/nv/item_files/gps/cgps/sm/gnss_lock_ctrl_for_oem,/nv/item_files/gps/cgps/sm/gnss_lock_ctrl_for_oem,GPS
74244,/cgps/nv/item_files/me/gnss_me_B1itoB2a_rfgd_unc_ns,/cgps/nv/item_files/me/gnss_me_B1itoB2a_rfgd_unc_ns,GPS
74245,/nv/item_files/gps/cgps/me/gnss_me_B1itoB2a_rfgd_ns,/nv/item_files/gps/cgps/me/gnss_me_B1itoB2a_rfgd_ns,GPS
74246,/nv/item_files/gps/cgps/me/gnss_bds_b2a_path_loss,/nv/item_files/gps/cgps/me/gnss_bds_b2a_path_loss,GPS
74247,/cgps/nv/item_files/me/gnss_me_hw_capability_config,/cgps/nv/item_files/me/gnss_me_hw_capability_config,GPS
74248,/cgps/nv/item_files/ulp/tle/gnss_tle_last_download_time,/cgps/nv/item_files/ulp/tle/gnss_tle_last_download_time,GPS
74249,/cgps/nv/item_files/ulp/tle/gnss_tle_restricted_region_version,/cgps/nv/item_files/ulp/tle/gnss_tle_restricted_region_version,GPS
74250,/cgps/nv/item_files/ulp/tle/gnss_tle_restricted_regions,/cgps/nv/item_files/ulp/tle/gnss_tle_restricted_regions,GPS
74251,/cgps/nv/item_files/ulp/tle/gnss_tle_request_rate_for_day,/cgps/nv/item_files/ulp/tle/gnss_tle_request_rate_for_day,GPS
74252,/cgps/nv/item_files/ulp/tle/gnss_tle_max_num_requests_per_day,/cgps/nv/item_files/ulp/tle/gnss_tle_max_num_requests_per_day,GPS
74253,/cgps/nv/item_files/ulp/tle/gnss_tle_client_controls,/cgps/nv/item_files/ulp/tle/gnss_tle_client_controls,GPS
74255,/nv/item_files/gps/cgps/me/gnss_multiband_configuration,/nv/item_files/gps/cgps/me/gnss_multiband_configuration,GPS
74256,/nv/item_files/gps/cgps/common/gnss_diag_buffer_cfg,/nv/item_files/gps/cgps/common/gnss_diag_buffer_cfg,GPS
74260,/cgps/nv/item_files/me/gnss_otfsa_scan_timers,/cgps/nv/item_files/me/gnss_otfsa_scan_timers,GPS
74264,/nv/item_files/gps/cgps/sm/gnss_nmea_extended_sentence_type_v2,/nv/item_files/gps/cgps/sm/gnss_nmea_extended_sentence_type_v2,GPS
74271,/nv/item_files/gps/cgps/me/gnss_reserved,/nv/item_files/gps/cgps/me/gnss_reserved,GPS
74272,/cgps/nv/item_files/pe/gnss_nv_data_report_mask,/cgps/nv/item_files/pe/gnss_nv_data_report_mask,GPS
74293,/nv/item_files/gps/cgps/sm/gnss_control_integer_second_boundary_aligned_NMEA_report,/nv/item_files/gps/cgps/sm/gnss_control_integer_second_boundary_aligned_NMEA_report,GPS
74295,/nv/item_files/gps/cgps/pe/gnss_nv_vehicle_dr_heve_limit,/nv/item_files/gps/cgps/pe/gnss_nv_vehicle_dr_heve_limit,GPS
74302,/nv/item_files/gps/cgps/pe/gnss_ffae_config,/nv/item_files/gps/cgps/pe/gnss_ffae_config,GPS
74312,GNSS Early Exit Timer per Sub ID,/nv/item_files/gps/cgps/sm/gnss_early_exit_timer_config,GPS
74316,/nv/item_files/gps/cgps/pe/gnss_dgnss_config,/nv/item_files/gps/cgps/pe/gnss_dgnss_config,GPS
74326,/nv/item_files/gps/cgps/me/gnss_l5only_cfg,/nv/item_files/gps/cgps/me/gnss_l5only_cfg,GPS
74334,/nv/item_files/gps/cgps/sm/aagps_positioning_modes_supported_ext,/nv/item_files/gps/cgps/sm/aagps_positioning_modes_supported_ext,GPS
74335,/nv/item_files/gps/cgps/me/gnss_multiband_e911_control,/nv/item_files/gps/cgps/me/gnss_multiband_e911_control,GPS
74336,/cgps/nv/item_files/me/gnss_me_GpsCx2ToCx4_rfgd_unc_ns,/cgps/nv/item_files/me/gnss_me_GpsCx2ToCx4_rfgd_unc_ns,GPS
74337,/nv/item_files/gps/cgps/me/gnss_me_GpsCx2ToCx4_rfgd_ns,/nv/item_files/gps/cgps/me/gnss_me_GpsCx2ToCx4_rfgd_ns,GPS
74341,/nv/item_files/gps/cgps/sm/gnss_lpp_rel13_config,/nv/item_files/gps/cgps/sm/gnss_lpp_rel13_config,GPS
74351,/cgps/nv/item_files/me/gnss_me_gpm_cmfeature_config,/cgps/nv/item_files/me/gnss_me_gpm_cmfeature_config,GPS
74352,/nv/item_files/gps/cgps/pe/gnss_nv_immediate_assist_req_config,/nv/item_files/gps/cgps/pe/gnss_nv_immediate_assist_req_config,GPS
74358,/cgps/nv/item_files/me/gnss_me_qwes_license_cfg,/cgps/nv/item_files/me/gnss_me_qwes_license_cfg,GPS
74367,/nv/item_files/gps/cgps/me/gnss_aux_config,/nv/item_files/gps/cgps/me/gnss_aux_config,GPS
74369,/cgps/nv/item_files/pe/gnss_nv_env_bearing_config,/cgps/nv/item_files/pe/gnss_nv_env_bearing_config,GPS
74373,/nv/item_files/gps/cgps/me/gnss_me_BdsB1iToB1c_rfgd_ns,/nv/item_files/gps/cgps/me/gnss_me_BdsB1iToB1c_rfgd_ns,GPS
74374,/cgps/nv/item_files/me/gnss_me_BdsB1iToB1c_rfgd_unc_ns,/cgps/nv/item_files/me/gnss_me_BdsB1iToB1c_rfgd_unc_ns,GPS
74375,/nv/item_files/gps/cgps/me/gnss_bds_b1c_path_loss,/nv/item_files/gps/cgps/me/gnss_bds_b1c_path_loss,GPS
74380,/nv/item_files/gps/cgps/me/gnss_navic_path_loss,/nv/item_files/gps/cgps/me/gnss_navic_path_loss,GPS
74381,/nv/item_files/gps/cgps/me/gnss_peak_antenna_gain_l5,/nv/item_files/gps/cgps/me/gnss_peak_antenna_gain_l5,GPS
74382,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2navic_ns,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2navic_ns,GPS
74383,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2navic_uncertainty_ns,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2navic_uncertainty_ns,GPS
74396,/cgps/nv/item_files/me/gnss_me_snprintf_enable,/cgps/nv/item_files/me/gnss_me_snprintf_enable,GPS
74397,/nv/item_files/gps/cgps/me/gnss_me_gpsL1toL2_rfgd_ns,/nv/item_files/gps/cgps/me/gnss_me_gpsL1toL2_rfgd_ns,GPS
74398,/cgps/nv/item_files/me/gnss_me_gpsL1toL2_rfgd_unc_ns,/cgps/nv/item_files/me/gnss_me_gpsL1toL2_rfgd_unc_ns,GPS
74399,/nv/item_files/gps/cgps/me/gnss_me_galE1toE5b_rfgd_ns,/nv/item_files/gps/cgps/me/gnss_me_galE1toE5b_rfgd_ns,GPS
74400,/cgps/nv/item_files/me/gnss_me_galE1toE5b_rfgd_unc_ns,/cgps/nv/item_files/me/gnss_me_galE1toE5b_rfgd_unc_ns,GPS
74401,/nv/item_files/gps/cgps/me/gnss_gps_L2C_path_loss,/nv/item_files/gps/cgps/me/gnss_gps_L2C_path_loss,GPS
74402,/nv/item_files/gps/cgps/me/gnss_gal_E5b_path_loss,/nv/item_files/gps/cgps/me/gnss_gal_E5b_path_loss,GPS
74416,configuring PPS parameter,/nv/item_files/gps/cgps/me/gnss_me_drsync_pulse_config,GPS
74421,32 bits OEM specific identification number,/nv/item_files/gps/cgps/pe/oem_id,GPS
74422,32 bits special code for DGNSS,/nv/item_files/gps/cgps/pe/dgnss_special_code,GPS
74431,GNSS GAL E1 Blanking Disablement,/nv/item_files/gps/cgps/me/gnss_me_gale1_blank_disable,GPS
74475,Power Optimized Acquisition Config,/nv/item_files/gps/cgps/me/gnss_me_power_optimized_acq_config,GPS
74482,/nv/item_files/gps/cgps/sm/gnss_control_fix_operation,/nv/item_files/gps/cgps/sm/gnss_control_fix_operation,GPS
74496,Minimum time interval between polynomial reporting bursts in milliseconds,/nv/item_files/gps/cgps/pe/gnss_nv_min_poly_report_tbb,GPS
74497,Maximum number of polynomial reports in each burst,/nv/item_files/gps/cgps/pe/gnss_nv_max_poly_per_burst,GPS
74498,GNSS Jammer Detection,/nv/item_files/gps/cgps/me/gnss_l1_jammer_ctrl,GPS
74499,GNSS Power Optimization Control,/nv/item_files/gps/cgps/me/gnss_power_optimization_control,GPS
74502,Enable or Disable Anti Spoofing Bit check for Automatic GPS Min Week,/nv/item_files/gps/cgps/me/gnss_me_disable_as_bit_check_for_amw,GPS
74514,BDS B2A Capability SV List,/nv/item_files/gps/cgps/me/gnss_me_bds_b2a_capability_sv_list,GPS
74516,GNSS L5 Fast Track Control,/nv/item_files/gps/cgps/me/gnss_l5_fast_track_control,GPS
74539,GNSS Reference Noise Level (in 0.01 dB),/nv/item_files/gps/cgps/me/gnss_ref_noise_level,GPS
74540,GNSS Force Reference Noise Level Reset ReInit,/nv/item_files/gps/cgps/me/gnss_force_rnl_reset_reinit_at_session_start,GPS
74543,GNSS ME Path Control,/nv/item_files/gps/cgps/me/gnss_me_path_control,GPS
74546,B1c To B2a Group Delay Ns,/nv/item_files/gps/cgps/me/gnss_me_rfgd_b1c2b2a_ns,GPS
74547,B1c To B2a Group Delay Unc Ns,/nv/item_files/gps/cgps/me/gnss_me_rfgd_b1c2b2a_unc_ns,GPS
74550,SAMLite Gyro Calibration Duration (secs),/nv/item_files/gps/cgps/samlite/gnss_nv_samlite_gyro_cal_duration,GPS
74553,Gps To B1c Group Delay Ns,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2b1c_ns,GPS
74554,B1c Direct Track Control,/nv/item_files/gps/cgps/me/gnss_me_b1c_track_control,GPS
74560,GNSS Non Standard Configuration,/nv/item_files/gps/cgps/common/gnss_non_std_cfg,GPS
74571,/nv/item_files/gps/cgps/sm/tech_sel_odcpi_timers,/nv/item_files/gps/cgps/sm/tech_sel_odcpi_timers,GPS
74572,Gnss Nav Data Nmea Ctrl,/nv/item_files/gps/cgps/me/gnss_nav_data_nmea_control,GPS
74575,/nv/item_files/gps/cgps/sm/gnss_ipc_buf_mask,/nv/item_files/gps/cgps/sm/gnss_ipc_buf_mask,GPS
74581,GNSS Location Control Configuration,/nv/item_files/gps/cgps/loc/gnss_control,GPS
74582,Gps To B1c Group Delay Unc Ns,/nv/item_files/gps/cgps/me/gnss_me_rfgd_gps2b1c_unc_ns,GPS
74586,GNSS GAL MTJ Mitigation Configuration,/nv/item_files/gps/cgps/me/gnss_me_gal_mtj_mitigation_config,GPS
74588,GNSS GAL MTJ Mitigation Max Dwell Configuration,/nv/item_files/gps/cgps/me/gnss_me_gal_mtj_mitigation_max_dwell_config,GPS
74609,GNSS ME Frequency Estimation Tuning,/nv/item_files/gps/cgps/me/gnss_me_frequency_estimation_tuning,GPS
74611,GNSS BDS B2B Path Loss,/nv/item_files/gps/cgps/me/gnss_bds_B2b_path_loss,GPS
74626,GNSS Acquisition Configuration,/nv/item_files/gps/cgps/me/gnss_me_acquisition_only_config,GPS
74634,configuring sleep clock tunc rate,/nv/item_files/gps/cgps/me/gnss_me_config_sleep_clock_tunc_rate,GPS
74635,Faster GNSS TTFF Ctrl,/nv/item_files/gps/cgps/me/faster_ttff_ctrl,GPS
74637,GNSS Common IPC Logging Control,/nv/item_files/gps/cgps/common/gnss_ipc_payload_thread_mask,GPS
74670,GNSS Broadcast Assistance Data Configuration,/nv/item_files/gps/cgps/sm/gnss_broadcast_ad_cfg,GPS
74674,GNSS ME F3 MPPS Reduction,/nv/item_files/gps/cgps/me/gnss_me_f3_mpps_reduction_mask,GPS
74685,GNSS Minimum Week Number,/nv/item_files/gps/cgps/me/gnss_me_min_week,GPS
74686,GNSS Log Control Configuration,/nv/item_files/gps/cgps/common/gnss_log_control_mask,GPS
74690,NV to select diag log masks for reporting diagnostic messages,/nv/item_files/gps/cgps/common/auto_valuetier_loctech_diag_log_control,GPS
74702,GNSS PE OSNMA FEATURE CONFIG,/nv/item_files/gps/cgps/pe/gnss_pe_osnma_feature_config,GPS
74710,Fast GNSS Low Power Mode Entry Config,/nv/item_files/gps/cgps/me/gnss_me_fast_gnss_low_power_entry,GPS
74713,GNSS Constellation Control,/nv/item_files/gps/cgps/me/gnss_constellation_ctrl,GPS
74748,GNSS Dynamic Blanking Calibration,/nv/item_files/gps/cgps/me/gnss_me_dynamic_blanking_cal,GPS
74775,NavIC L5 to NavIC L1 RF Group Delay,/nv/item_files/gps/cgps/me/gnss_me_NavicL5ToNavicL1_rfgd_ns,GPS
74792,GNSS Delta Signal Strength Diff,/nv/item_files/gps/cgps/me/gnss_me_delta_signal_strength_diff,GPS
74804,Constellation Preferred,/nv/item_files/gps/cgps/me/constellation_preferred,GPS
74829,Gnss Enhanced XTRA,/nv/item_files/gps/cgps/pe/gnss_nv_pe_gnss_enhanced_xtra,GPS
74856,Displays measured GPS L1 to Navic L1 group delay.,/nv/item_files/gps/cgps/me/gnss_me_GpsL1ToNavicL1_rfgd_ns,GPS
74858,Displays measured GPS L1 to Navic L1 group delay uncertainity (Theoritical).,/cgps/nv/item_files/me/gnss_me_GpsL1ToNavicL1_rfgd_unc_ns,GPS
74859,Used to store the system processing loss for Navic L1 RF chain (1 unit value representing 0.1 db).,/nv/item_files/gps/cgps/me/gnss_navic_l1_path_loss,GPS
74862,/nv/item_files/gps/cgps/sm/gm_optimization_bitmask,/nv/item_files/gps/cgps/sm/gm_optimization_bitmask,GPS
74871,Used to store the decision to enable/disable NavIC L5 acquisition during Catastrophic signal conditions.,/nv/item_files/gps/cgps/me/gnss_me_catastrophic_navic_l5_config,GPS
79394,/nv/item_files/gps/cgps/sm/gnss_text_to_emergency_window,/nv/item_files/gps/cgps/sm/gnss_text_to_emergency_window,GPS
```

### Qcom_NV_WLAN

```
NVITEM ID,DESCRIPTION,FULL NAME,CATEGORY
3363,WLAN Enable Power Save Mode,wlan_enable_ps_mode,WLAN
4122,WLAN QoS Mode,wlan_qos_mode,WLAN
4202,WLAN OEM Index,wlan_oem_index,WLAN
4361,WLAN Multidomain Capability Preference,wlan_multidomain_capability_pref,WLAN
4527,WLAN Enable BT Coexistence,wlan_enable_bt_coex,WLAN
4678,WLAN MAC Address,wlan_mac_address,WLAN
5824,WWAN Access Over WIFI Preferred,wwan_access_over_wifi_preferred,WLAN
5825,WIFI Local Breakout Allowed,wifi_local_breakout_allowed,WLAN
6249,WLAN Atheros Specific Configuration,wlan_atheros_specific_cfg,WLAN
6260,WLAN CPU Flow Control Configuration,wlan_cpu_flow_control_cfg,WLAN
6428,WLAN UUID,wlan_uuid,WLAN
6842,WLAN Pal Link Pref,wlan_pal_link_pref,WLAN
65613,WLAN RSSI OFFSET,/nv/item_files/wlan/rssi_offsets,WLAN
65614,WLAN Regulatory Domain FCC Info,/nv/item_files/wlan/reg_domain_fcc_info,WLAN
65615,WLAN Regulatory Domain ETSI Info,/nv/item_files/wlan/reg_domain_etsi_info,WLAN
65616,WLAN Regulatory Domain Japan Info,/nv/item_files/wlan/reg_domain_japan_info,WLAN
65617,WLAN Regulatory Domain World Info,/nv/item_files/wlan/reg_domain_world_info,WLAN
65618,WLAN Regulatory Domain N Anmer Exc Info,/nv/item_files/wlan/reg_domain_n_amer_exc_info,WLAN
65619,WLAN Regulatory Domain APAC Info,/nv/item_files/wlan/reg_domain_apac_info,WLAN
65620,WLAN Regulatory Domain Korea Info,/nv/item_files/wlan/reg_domain_korea_info,WLAN
65621,WLAN Regulatory Domain Hi 5ghz Info,/nv/item_files/wlan/reg_domain_hi_5ghz_info,WLAN
65622,WLAN Regulatory Domain No 5ghz Info,/nv/item_files/wlan/reg_domain_no_5ghz_info,WLAN
65623,WLAN Field Image,/nv/item_files/wlan/field_image,WLAN
65624,WLAN Rate To Power Table,/nv/item_files/wlan/rate_to_power_table,WLAN
65625,WLAN QFuse Data,/nv/item_files/wlan/wlan_qfuse_data,WLAN
65809,WLAN RSSI Channel Offsets,/nv/item_files/wlan/rssi_channel_offsets,WLAN
65817,WLAN Default Country,/nv/item_files/wlan/default_country,WLAN
65888,TX_BB_FILTER_MODE,/nv/item_files/wlan/tx_bb_filter_mode,WLAN
65889,OFDM_CMD_POWER_OFFSET,/nv/item_files/wlan/ofdm_cmd_power_offset,WLAN
66037,Tx insertion loss,/nv/item_files/wlan/tx_insertion_loss,WLAN
66038,Packet Type power limits,/nv/item_files/wlan/packet_type_power_limits,WLAN
66039,TPC PDADC OFFSET,/nv/item_files/wlan/tpc_pdadc_offsets,WLAN
66040,TPC POWET TABLE,/nv/item_files/wlan/tpc_power_table,WLAN

```

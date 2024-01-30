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
拉取  /system/vendor/etc/gps.conf 文件 然后设置 XTRA_TEST_ENABLED = 1 和 XTRA_THROTTLE_ENABLED = 0 到文件 
重新导入覆盖源文件就可以移除xtra下载限制

adb root
adb remount
adb pull system/vendor/etc/gps.conf .



open gps.conf and add two configs at the end of file:
XTRA_TEST_ENABLED = 1
XTRA_THROTTLE_ENABLED = 0



adb push gps.conf system/vendor/etc/gps.conf
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



### adb命令描述模型Tip

```

adb root
adb disable-verity
adb reboot
adb root
adb remount
adb pull /etc/debug_gps.conf
Make 【#SUPL_MODE=1】 To 【SUPL_MODE=1】 on local dir 
adb push .\gps_debug.conf  /etc/
adb shell 
═════════════ in adb shell ═════════════
cd /etc/
chmod 644 gps_debug.conf
cat gps_debug.conf    【make sure SUPL_MODE=1 in the file 】
═════════════ out adb shell ═════════════
adb reboot


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

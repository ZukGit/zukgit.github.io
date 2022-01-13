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


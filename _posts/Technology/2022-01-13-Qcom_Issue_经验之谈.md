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



### 查看SAR打印Log 


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



### 更换regdb.bin文件

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


4.在 XXX-ramdump.zip 文件夹中存在  summary.txt 里面包含了 版本的信息

 ro.build.fingerprint    : XXXXXX60-23/10dc7-0f770:userdebug
 ro.build.version.qcom   : XXXX.XXX.1.0.R1.11.00.00.816.199
 version-baseband        : M6450N_DE21_34.290.01.57R 
 
``` 

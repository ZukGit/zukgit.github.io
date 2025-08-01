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


### 查看产品是否支持DBS双频并发

```


adb shell dumpsys wifi    | grep  "STA + AP"
STA + AP Concurrency Supported: false              【不支持DBS】


adb shell dumpsys wifi    | grep  "STA + AP"
STA + AP Concurrency Supported: true               【支持DBS】


```




### 查看Qcom产品是否支持GPS_L5信号

```

打开NVBrowser 查看 NV74255  gnss_multiband_configuration


// 不支持 L5 的 读取值出来为0  如下
INPUT,VALUE,NAME,SIZE,TYPE
0,0,gnss_multiband_configuration,32,Uint32



// 支持L5 的 读取出来的值非0 如下

39,39,gnss_multiband_configuration,32,Uint32



```

#### Qcom_搜索北斗卫星NV配置


```

插入PRC SIM 卡 或者 配置  NV 74572 为0  进行搜索 北斗卫星信号

74572,Gnss Nav Data Nmea Ctrl,/nv/item_files/gps/cgps/me/gnss_nav_data_nmea_control,GPS

打开NVBrowser 查看 NV74572  gnss_nv_efs_me_gnss_nav_data_nmea_ctrl

NVITEM ID,DESCRIPTION,FULL NAME,CATEGORY
74572,Gnss Nav Data Nmea Ctrl,/nv/item_files/gps/cgps/me/gnss_nav_data_nmea_control,GPS


INPUT,VALUE,NAME,SIZE,TYPE
0,0,gnss_nv_efs_me_gnss_nav_data_nmea_ctrl,32,Uint32  【 NV74572 配置为0 可以搜索到北斗】



```


### 打开当前网速显示 
```

adb root && adb shell motsettings put global internet_speed_switch 1


```


### 查看MTK产品是否支持GPS_L5信号

```

adb shell getprop | grep l5
[vendor.debug.gps.support.l5]: [1]         // 1__支持    2___不支持


```


### 查看MTK产品的目标ALPS分支

```

  adb shell "getprop | grep alps | grep vendor"


[ro.vendor.mediatek.version.branch]: [alps-mp-u0.mp1.tc2sp3]           // 当前的branch分支是 mp1.tc2sp3
[ro.vendor.mediatek.version.release]: [alps-mp-u0.mp1.tc2sp3-V2.111]   //  当前release的版本是V2.111


```

### 查看MTK产品WIFI的固件和Chip信息

```

 adb shell "getprop | grep nep | grep wlan"

[vendor.wlan.firmware.version]: [t-neptune-sp-soc7_0-2409-tc2sp-RAYAS_SOC7_0_E1_ASIC_MT6637-20241129154852]


```

```
adb shell getprop | grep chip
[persist.vendor.connsys.chipid]: [0x6878]
[persist.vendor.connsys.fm_chipid]: [connac2x]
[vendor.connsys.adie.chipid]: [0x6637]
[vendor.connsys.bt.adie.chipid]: [0x6637]
[vendor.connsys.fm.adie.chipid]: [0x6637]
[vendor.connsys.gps.adie.chipid]: [0x6637]
[vendor.connsys.wifi.adie.chipid]: [0x6637]

```


### 查看高通产品WIFI的固件和Chip信息

```


// 【adb shell for循环命令】
adb root && adb remount && adb shell "while true; do   echo -n 'sar_sensor_distance=_____'; getprop vendor.ril.radio.sar_sensor_distance ;  echo -n '_____'; done;"
adb root && adb remount && adb shell "while true; do   echo -n 'context_orientation=_____'; getprop persist.sys.context_orientation ;  echo -n '_____'; done;"




//  【 cmd 发送命令给到 adb shell 执行, 执行效果是 不需要进入到 adb shell里面去】
adb root && adb remount && adb shell "while true; do iwpriv wlan0 driver stat; sleep 1; done;"    【 cmd 不进入adb shell里面 去执行 adb shell的命令】

adb root && adb remount && adb shell " iwpriv wlan0 version"




wlan0     version:Host SW:2.0.9.23B, FW:3.0.2.0.32767.2, HW:WCN6750_V2, Board ver: 8 Ref design id: 0, Customer id: 0, 
Project id: 0, Board Data Rev: 21, REG DB: 0:0, BDF REG DB: 0:0


version【版本】:Host SW:2.0.9.23B
FW【固件】:3.0.2.0.32767.2
HW【硬件】:WCN6750_V2



```


```


adb root && adb remount && adb shell "cat /d/icnss/stats"




        ind_register_req: 1
       ind_register_resp: 1
        ind_register_err: 0
                 cap_req: 1
                cap_resp: 1
                 cap_err: 0
      pin_connect_result: 1
                 cfg_req: 2
                cfg_resp: 2
             cfg_req_err: 0
                mode_req: 4
               mode_resp: 4
            mode_req_err: 0
                 ini_req: 2
                ini_resp: 2
             ini_req_err: 0
   recovery.pdr_fw_crash: 0
 recovery.pdr_host_error: 0
  recovery.root_pd_crash: 0
recovery.root_pd_shutdown: 0

<------------------ PM stats ------------------->
              pm_suspend: 0
          pm_suspend_err: 0
               pm_resume: 0
           pm_resume_err: 0
        pm_suspend_noirq: 0
    pm_suspend_noirq_err: 0
         pm_resume_noirq: 0
     pm_resume_noirq_err: 0
           pm_stay_awake: 813
                pm_relax: 813

<------------------ IRQ stats ------------------->
CE_ID  IRQ  Request     Free   Enable  Disable
   0:    0        0        0        0        0
   1:    0        0        0        0        0
   2:    0        0        0        0        0
   3:    0        0        0        0        0
   4:    0        0        0        0        0
   5:    0        0        0        0        0
   6:    0        0        0        0        0
   7:    0        0        0        0        0
   8:    0        0        0        0        0
   9:    0        0        0        0        0
  10:    0        0        0        0        0
  11:    0        0        0        0        0

<---------------- FW Capability ----------------->
Chip ID: 0x1
Chip family: 0xb
Board ID: 0xff
SOC Info: 0x40140120
Firmware Version: 0x3020ffff
Firmware Build Timestamp: 2025-03-01 10:01

【WLAN.MSL.3.0.2固件平台()  HL/MSL/HMT/GNG】  在  Vendor_Side/vendor/qcom/nonhlos/WLAN.MSL.3.0.2  存在这个MSL平台固件目录
Firmware Build ID: QC_IMAGE_VERSION_STRING=WLAN.MSL.3.0.2-00128-QCAMSLSWPLZ-5.98084.2.101227.2   【 固件详细信息 】 
RD card chain cap: 2
PHY HE channel width cap: 2
PHY QAM cap: 2

<----------------- Events stats ------------------->
                  Events           Posted        Processed
           SERVER_ARRIVE                1                1
             SERVER_EXIT                0                0
                FW_READY                1                1
         REGISTER_DRIVER                1                1
       UNREGISTER_DRIVER                0                0
         PD_SERVICE_DOWN                0                0
      FW_EARLY_CRASH_IND                0                0
           IDLE_SHUTDOWN                1                1
            IDLE_RESTART                1                1
            FW_INIT_DONE                1                1
      QDSS_TRACE_REQ_MEM                1                1
         QDSS_TRACE_SAVE                0                0
         QDSS_TRACE_FREE                0                0
          M3_DUMP_UPLOAD                0                0
        IMS_WFC_CALL_IND                0                0
        WLFW_TWC_CFG_IND                0                0
     QDSS_TRACE_REQ_DATA                0                0
    SUBSYS_RESTART_LEVEL                2                2

Serial Number: 0x0
State: 0x49048f(FW CONN | POWER ON | FW READY | DRIVER PROBED | SSR REGISTERED | WLAN FW EXISTS | MODE ON DONE | IMS_CONNECTED | DMS_CONNECTED)

```





### 查看指定应用的应用权限信息

```
Package [com.whatsapp]      // bugreport 中搜索该名称
Package [com.baidu.map.location]   // 普通搜索, 百度地图 正则搜索搜不到
Package [com.autonavi.minimap]    // 普通搜索  高德地图


      runtime permissions:
        android.permission.ACCESS_FINE_LOCATION: granted=false,  【未授权定位权限】flags=[ USER_SET|USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED|524288]
        android.permission.ACCESS_BACKGROUND_LOCATION: granted=false【未授权后台获得位置权限】, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED|RESTRICTION_INSTALLER_EXEMPT]


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


### 在线Android_C运行环境(模拟)


1.  打开网址  https://www.marscode.cn/ide/w74v81pje2vw88

2. 创建C工程并在根目录创建 main.c   把 下文中的 main.c 代码 复制到文件  main.c  中 

3. 在根目录创建 system.prop 文化并把  把 下文中的 system.prop  代码 复制到文件 system.prop   中 

4. 开始模拟编写 Android_C 运行时代码 点击 运行  实时编译  查看结果 方便调试

```


1.  打开网址  https://www.marscode.cn/ide/w74v81pje2vw88

2. 创建C工程并在根目录创建 main.c   把 下文中的 main.c 代码 复制到文件  main.c  中 

3. 在根目录创建 system.prop 文化并把  把 下文中的 system.prop  代码 复制到文件 system.prop   中 

4. 开始模拟编写 Android_C 运行时代码 点击 运行  实时编译  查看结果 方便调试


```


####  Anroid在线编译_main.c

1.  打开网址  https://www.marscode.cn/ide/w74v81pje2vw88

2. 创建C工程并在根目录创建 main.c   把 下文中的 main.c 代码 复制到文件  main.c  中 

3. 在根目录创建 system.prop 文化并把  把 下文中的 system.prop  代码 复制到文件 system.prop   中 

4. 开始模拟编写 Android_C 运行时代码 点击 运行  实时编译  查看结果 方便调试


```
// 当前文件命名为 main.c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <strings.h>
#include <unistd.h>  // 包含getcwd函数所需的头文件
#include <limits.h>  // 包含PATH_MAX常量
#include <time.h>

// _______   常量定义 Begin _______ 

static char CUR_DIR_PATH[1024] = {0};  //  当前工程的根目录  



// _______   常量定义 End _______ 

// 获取当前时间戳  20250122_172323
char* get_current_time_str_with_timezone() {
    time_t rawtime;
    struct tm *timeinfo;
    static char buffer[80];

    // 获取当前的UTC时间
    time_t now = time(NULL);
    struct tm *utc_tm = gmtime(&now);
 
    // 手动计算北京时间的时差（8小时）
    int timezone = 8 * 60 * 60; // 8小时时差转换为秒

    // 调整UTC时间以反映本地时间
    utc_tm->tm_hour += timezone / (60 * 60); // 小时
    utc_tm->tm_min += (timezone % (60 * 60)) / 60; // 分钟
    utc_tm->tm_sec += timezone % 60; // 秒

    // 格式化时间字符串，包括时差
    strftime(buffer, sizeof(buffer), "%Y%m%d_%H%M%S", utc_tm);

    return buffer;
}



/* property_get: returns the length of the value which will never be
** greater than PROPERTY_VALUE_MAX - 1 and will always be zero terminated.
** (the length does not include the terminating zero).
**
** If the property read fails or returns an empty value, the default
** value is used (if nonnull).
*/
int property_get(const char* key, char* value, const char* default_value){
   int SIZE =256;
   int returnCode = 0 ;
   char oneLine[256] = {0};
   char matchValue[256] ={0};
   char curKey[256] ={0};

   char* identifyKey =  strcat(strcpy(curKey, key),"=");
   char propFilePATH[256] ={0};
   strcpy(propFilePATH, CUR_DIR_PATH);
   strcat(propFilePATH, "/system.prop");

   printf("Prop文件路径:  propFilePATH:%s \n",propFilePATH);

    FILE *file = fopen(propFilePATH, "r");
    if (file == NULL) {
        printf("This file does not exist.\n");
        return -1;
    }

     int linenum = 0 ;
        // 读取每行内容至str中
	while(fgets(oneLine, SIZE, file))
	 {
       char *newline = strtok(oneLine, "\n");    // 去除 fgets函数 自动加入的换行符  因此它会返回去除了换行符的字符串
     //   printf("line_%d: 【%s】  \n",++linenum,newline);
        // 判断str中是否包含参数输入字符串
		if(strstr(newline, identifyKey))  // 验证  【key=】   来确保唯一性
		{

    

        //   printf("找到宏:%s  对应的行:%s \n",key,newline);

           char *p = strstr(newline, key);
           char *q = strstr(newline, "=");
           int len = q - p ;
          // printf("len = %d\n",len);
           strncpy(matchValue,p+len+1,len);
          // printf("AA matchValue = %s\n",matchValue);
           strcpy(value,matchValue);
		}
	}

	fclose(file);

    if(matchValue[0] == '\0'  ){  // 没有找到宏
    //    printf("当前没有找到对应宏：%s \n",key);
       if(default_value == NULL){
           returnCode = -1;
           printf(" 当前没有找到对应宏:%s  && default_value = NULL --> returnCode= %d \n",key,returnCode);
   
           return returnCode;
       } else {
            strcpy(value,default_value);
           returnCode =  strlen(value); 
           printf(" 当前没有找到对应宏:%s  && default_value =[%s] --> returnCode= %d  返回对应设置默认值的长度! \n",key,default_value,returnCode);
            return returnCode;
       }
    }



    // printf("matchValue=【%s】 strlen(matchValue) = %d \n",matchValue,strlen(matchValue));
    // printf("____ property_get() Finish____\n");
    return strlen(matchValue); 
}


int append_line_to_file(const char *filename, const char *line) {
    FILE *file = fopen(filename, "a");  // 打开文件用于追加
    if (file == NULL) {
        perror("Error opening file");
        return -1;  // 打开文件失败，返回错误代码
    }
 
 

    // 写入一个换行符后跟要追加的文本
    fprintf(file, "%s\n", line);
 
    fclose(file);  // 关闭文件
    return 0;  // 成功
}



int modifyLineInFile(const char *filePath,  const char *oldLineContent, const char *newLineContent) {
    FILE *file = fopen(filePath, "r");
    if (file == NULL) {
        perror("Error opening file");
        return -1;
    }


    // Temporary file to store updated content
    FILE *tempFile = fopen("temp.txt", "w");
    if (tempFile == NULL) {
        perror("Error opening temporary file");
        fclose(file);
        return -1;
    }

    char buffer[1024];
    int currentLine = 1;

    while (fgets(buffer, 1024, file) != NULL) {

      char *newline = strtok(buffer, "\n");    // 去除 fgets函数 自动加入的换行符  因此它会返回去除了换行符的字符串
     //   printf("line_%d: 【%s】  \n",++linenum,newline);
        // 判断str中是否包含参数输入字符串
		if(strstr(newline, oldLineContent))  // 验证  【key=】   来确保唯一性
		{

         fprintf(tempFile, "%s\n", newLineContent);
		} else {
          fputs(strcat(buffer,"\n"), tempFile);
        }

        currentLine++;
    }

    fclose(file);
    fclose(tempFile);

    // Replace original file with updated file
    remove(filePath);
    rename("temp.txt", filePath);
    return 0;
}



// property_set: returns 0 on success, < 0 on failure
int property_set(const char *key, const char *newValue){
    char curkeyValue[100] = {0};
    char identifyKey[100] = {0};
    char identifyOldKey[100] = {0};
     char* rawPropLine;
    char* replaceNewLine;    //需要替换或者追加的新的内容
     strcpy(identifyKey,key);

    if(property_get(key, curkeyValue,NULL) > 0 ){
        //修改操作

       // printf("Modify_1!! curkeyValue=【%s】\n",curkeyValue); 
       // printf("Modify_2!! identifyKey=【%s】\n",identifyKey); 
       // printf("Modify_3!! newValue=【%s】\n",newValue); 
       replaceNewLine = strcat(strcat(identifyKey, "="),newValue);
     //  printf("Exist! Need Modify from  【key=%s rawValue=%s】 【key=%s newValue=%s】\n",key,curkeyValue , key , newValue ); 

       strcpy(identifyOldKey,key);
       rawPropLine = strcat(strcat(identifyOldKey, "="),curkeyValue); 
     //  printf("Modify!! rawOldLine=【%s】replaceNewLine=【%s】\n",rawPropLine,replaceNewLine); 

       char propFilePATH[256] ={0};
       strcpy(propFilePATH, CUR_DIR_PATH);
       strcat(propFilePATH, "/system.prop");
       return  modifyLineInFile(propFilePATH,rawPropLine,replaceNewLine);
 
    } else {
        // 追加末尾操作

       replaceNewLine = strcat(strcat(identifyKey, "="),newValue);
    //    printf("No Exist! Need Write Append to End!   key=%s curkeyValue=%s Line=【%s】\n",key,identifyKey,replaceNewLine); 
    //  printf("replaceNewLine=%s\n",replaceNewLine); 
     char *newline = strtok(replaceNewLine, "\n");    // 去除 fgets函数 自动加入的换行符  因此它会返回去除了换行符的字符串

       char propFilePATH[256] ={0};
       strcpy(propFilePATH, CUR_DIR_PATH);
       strcat(propFilePATH, "/system.prop");
      return  append_line_to_file(propFilePATH,newline);
    }




    return 0; 
}






int test_property_get() {
     printf("\n════════════  test_property_get() begin\n");
    char propValue1[100] = {0};
    char propValue2[100] = {0};
    char propValue3[100] = {0};
    char propValue4[100] = {0};
    char propValue5[100] = {0};
    char propValue6[100] = {0};

    // 1. property_get 获取存在的宏  默认值为NULL   "wifi.tethering.interface=ap0"
    // 打印:  propValue1  length = 3   propValue1=ap0
    printf("propValue1  length = %d\n",property_get("wifi.tethering.interface", propValue1, NULL));
    printf("propValue1=%s\n",propValue1);

    // 2. property_get 获取存在的宏  默认值为wlan0     "wifi.tethering.interface=ap0"
    // 打印:   propValue2  length = 3    propValue2=ap0
    printf("propValue2  length = %d\n",property_get("wifi.tethering.interface", propValue2, "wlan0"));
    printf("propValue2=%s\n",propValue2);

    // 3. property_get 获取不存在的宏  默认值为 NULL    "wifi.tethering.interfaceAA"
    // 打印:   propValue3 length = -1   propValue3=   
    // 所以:   if(property_get("wifi.tethering.interfaceAA", propValue3,NULL) > 0) 可用于判断是否正确读取出prop
    printf("propValue3 length = %d\n",property_get("wifi.tethering.interfaceAA", propValue3,NULL));
    printf("propValue3=%s\n",propValue3);

    // 4. property_get 获取不存在的宏  默认值为 "wlan0"    "wifi.tethering.interfaceAA"
    // 打印:   propValue4 length = 5   propValue4=wlan0    // 当前读取到的是默认值 返回的是默认值的长度
    printf("propValue4 length = %d\n",property_get("wifi.tethering.interfaceAA", propValue4, "wlan0"));
    printf("propValue4=%s\n",propValue4); 


    // 5.判断是否正确去读取出 宏 key  "wifi.tethering.interface"
    // 打印  Exist!  propValue5=wlan0
    if(property_get("wifi.tethering.interface", propValue5,NULL) > 0 ){
        printf("Exist!  propValue5=%s\n",propValue4); 
    } else {
        printf("No Exist!  propValue5=%s\n",propValue4); 
    }

    // 6.判断是否正确去读取出不存在的宏   "wifi.tethering.interfaceAA"
     // 打印:    No Exist! propValue6=
    if(property_get("wifi.tethering.interfaceAA", propValue6,NULL) > 0 ){
        printf("Exist! propValue6=%s\n",propValue6); 
    } else {
        printf("No Exist! propValue6=%s\n",propValue6); 
    }


   printf("\n════════════ test_property_get() end\n");

    return 0;
}


// property_set: returns 0 on success, < 0 on failure
//  property_set: returns 0 on success, < 0 on failure
int test_property_set() {
    printf("\n════════════  test_property_set() begin  \n");

    int result_code_1 = property_set("wifi.tethering.interface","p2pXXXX10");
    printf("result_code_1=%d\n",result_code_1);


    int result_code_2 = property_set("wifi.interface","testwifi");
    printf("result_code_2=%d\n",result_code_2);


    int result_code_3 = property_set("vendor.rild.libargs","just one word");
    printf("result_code_3=%d\n",result_code_3);


     // -------- get("不存在key")  set("不存在key")  get("不存在key") begin  --------
     // 读取不存在的宏
       char propGetValue_4pre[100] = {0};
      if(property_get("test_moAAAdeAAA", propGetValue_4pre, "GPS") > 0){
        printf("读取prop成功!(读取到了默认值) propGetValue_4pre=%s\n",propGetValue_4pre);
    } else {
        printf("读取prop失败! propGetValue_4pre=%s\n",propGetValue_4pre);
    }

     // 设置宏  并读取宏  
    int result_code_4 = property_set("test_moAAAdeAAA",get_current_time_str_with_timezone());
    printf("result_code_4=%d\n",result_code_4);
    char propGetValue4[100] = {0};
    if(property_get("test_moAAAdeAAA", propGetValue4, NULL) > 0){
        printf("读取prop成功! propGetValue4=%s\n",propGetValue4);
    } else {
        printf("读取prop失败! propGetValue4=%s\n",propGetValue4);
    }
  
     // 读取之前设置了值的宏
       char result_code_4end[100] = {0};
      if(property_get("test_moAAAdeAAA", result_code_4end, "GPS") > 0){
        printf("读取prop成功! result_code_4end=%s\n",result_code_4end);
    } else {
        printf("读取prop失败! result_code_4end=%s\n",result_code_4end);
    }
     // -------- get("不存在key")  set("不存在key")  get("不存在key") end  --------


    printf("\n════════════  test_property_set() end \n");

}




int get_system_info() {
      printf("\n════════════  get_system_info() begin \n");
    char cwd[PATH_MAX];  // 定义一个足够大的字符数组来存储路径

    // 调用getcwd函数获取当前工作目录
    if (getcwd(cwd, sizeof(cwd)) != NULL) {
        printf("当前路径是: %s\n", cwd);
        strcpy(CUR_DIR_PATH,cwd);
    } else {
        perror("当前路径获取失败! 请检查代码! getcwd() error\n ");  // 如果getcwd函数失败，则打印错误信息
    }

       printf("CUR_DIR_PATH=【%s】 \n",CUR_DIR_PATH);
      printf("\n════════════  get_system_info() end \n");

    return 0;
}


int main() {
 printf("\n════════════════════════════════════ main() begin ════════════════════════════════════\n");
 get_system_info();
test_property_get();
test_property_set();

// 在这里 模拟 C 语言( Android )运行编译代码测试环境 zukgit C 在线编译 现在运行
 printf("\n════════════════════════════════════ main() end ════════════════════════════════════\n");

}







```

#### Anroid在线编译_system.prop 

1.  打开网址  https://www.marscode.cn/ide/w74v81pje2vw88

2. 创建C工程并在根目录创建 main.c   把 下文中的 main.c 代码 复制到文件  main.c  中 

3. 在根目录创建 system.prop 文化并把  把 下文中的 system.prop  代码 复制到文件 system.prop   中 

4. 开始模拟编写 Android_C 运行时代码 点击 运行  实时编译  查看结果 方便调试

```
## 当前文件命名为 system.prop   不能包含空行
Build.BRAND=MTK
aaudio.mmap_exclusive_policy=2
aaudio.mmap_policy=2
audio.deep_buffer.media=true
audio.offload.disable=false
wifi.direct.interface=p2p0
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



### 项目SKU_radioid查看

```

cd AOSP && cd ./device && grep -rins "device name=" . | grep vhw

cd ./device && grep -rins "device name=" . | grep vhw     // 索索所有出现的  【device name=】的xml 文件 
 
gedit ./xxxxx/vhw.xml          //  打开当前所有到的 vhw.xml 文件 

搜索关键字 【"radio/.range"】  就能查看当前 device 范围下的 radioid 


示例:
			<string-array name=【"radio/.range"】>
				<item>LATAM</item>
				<item>PRC</item>
				<item>CHINA</item>
				<item>INDIA</item>
			</string-array>

```


### EAP_TYPE类型

```
EAP_TYPE类型 说明: 
https://learn.microsoft.com/zh-cn/windows-server/networking/technologies/extensible-authentication-protocol/network-access?tabs=eap-tls%2Cserveruserprompt-eap-tls%2Ceap-sim

```


| EAP方法 | IANA分配的类型编号EAP_TYPE | 本机Windows支持 |
| ---- | ---- | ---- |
| MD5-Challenge(EAP-MD5) | 4 | ❌ |
| 一次性密码(EAP-OTP) | 5 | ❌ |
| 通用令牌卡(EAP-GTC) | 6 | ❌ |
| EAP-TLS | 13 | ✅ |
| EAP-SIM | 18 | ✅ |
| EAP-TTLS | 21 | ✅ |
| EAP-AKA | 23 | ✅ |
| PEAP | 25 | ✅ |
| EAP-MSCHAPv2 | 26 | ✅ |
| 受保护的一次性密码(EAP-POTP) | 32 | ❌ |
| EAP-FAST | 43 | ❌ |
| 预共享密钥(EAP-PSK) | 47 | ❌ |
| EAP-IKEv2 | 49 | ❌ |
| EAP-AKA' | 50 | ✅ |
| EAP-EKE | 53 | ❌ |
| TEAP | 55 | ✅ |
| EAP-NOOB | 56 | ❌ |




### SUPL证书说明
当前证书列表说明网站:
https://support.apple.com/zh-cn/103494

```
当前证书列表说明网站:

https://support.apple.com/zh-cn/103494

https://www.digicert.com/kb/digicert-root-certificates.htm

```

```
EV 扩展验证（Extended Validation, EV）证书的颁发、管理、验证和使用规范
EV 证书必须使用特定的策略对象标识符（OID）来标识其遵循的认证策略
CA/Browser Forum 的标准，EV 证书的策略 OID 为 2.23.140.1.1
协卡网络信任服务体系 EV 证书策略中定义的证书策略对象标识符为 1.2.156.112570.1.0.3

```


```

证书名称 签发者 类型 密钥大小 签名算法 序列号 到期时间 EV策略

1 VeriSign_Class_3_Public_Primary_Certification_Authority-G5.cer  RSA  2048位 SHA-1  18:DA:D1:9E:26:7D:E8:BB:4A:21:58:CD:CC:6B:3B:4A 20360716_23:59:59 2.16.840.1.

2 DigiCert_Assured_ID_Root_CA RSA 2048位 SHA-1 0C:E7:E0:E5:17:D8:46:FE:8F:E5:60:FC:1B:F0:30:39 2031010_00:00:00 Not_EV

3 DigiCert_Global_Root_G2 RSA 2048位 SHA-256 03:3A:F1:E6:A7:11:A9:A0:BB:28:64:B1:1D:09:FA:E5 20380115_12:00:00 2.16.840.1.114412.2.1

4 DigiCert_Global_Root_G3  ECDSA  384位 SHA-384 05:55:56:BC:F2:5E:A4:35:35:C3:A4:0F:D5:AB:45:72 20380115_12:00:00  2.16.840.1.114412.2.1

5 DigiCert_TLS_RSA4096_Root_G5 RSA 4096位 SHA-384 08:F9:B4:78:A8:FA:7E:DA:6A:33:37:89:DE:7C:CF:8A  20460114_00:00:00  Not_EV

6  DigiCert_TLS_ECC_P384_Root_G5 ECDSA 384位   SHA-384   09:E0:93:65:AC:F7:D9:C8:B9:3E:1C:0B:04:2A:2E:F3 20460114_00:00:00  Not_EV


```

| 证书名称 | 签发者 | 类型 | 密钥大小 | 签名算法 | 序列号 | 到期时间 | EV策略 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1 | VeriSign_Class_3_Public_Primary_Certification_Authority-G5.cer | RSA | 2048位 | SHA-1 | 18:DA:D1:9E:26:7D:E8:BB:4A:21:58:CD:CC:6B:3B:4A | 20360716_23:59:59 | 2.16.840.1. |
| 2 | DigiCert_Assured_ID_Root_CA | RSA | 2048位 | SHA-1 | 0C:E7:E0:E5:17:D8:46:FE:8F:E5:60:FC:1B:F0:30:39 | 2031010_00:00:00 | Not_EV |
| 3 | DigiCert_Global_Root_G2 | RSA | 2048位 | SHA-256 | 03:3A:F1:E6:A7:11:A9:A0:BB:28:64:B1:1D:09:FA:E5 | 20380115_12:00:00 | 2.16.840.1.114412.2.1 |
| 4 | DigiCert_Global_Root_G3 | ECDSA | 384位 | SHA-384 | 05:55:56:BC:F2:5E:A4:35:35:C3:A4:0F:D5:AB:45:72 | 20380115_12:00:00 | 2.16.840.1.114412.2.1 |
| 5 | DigiCert_TLS_RSA4096_Root_G5 | RSA | 4096位 | SHA-384 | 08:F9:B4:78:A8:FA:7E:DA:6A:33:37:89:DE:7C:CF:8A | 20460114_00:00:00 | Not_EV |
| 6 | DigiCert_TLS_ECC_P384_Root_G5 | ECDSA | 384位 | SHA-384 | 09:E0:93:65:AC:F7:D9:C8:B9:3E:1C:0B:04:2A:2E:F3 | 20460114_00:00:00 | Not_EV |



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

#————————————————————————————————————————
# AGPS server settings #
#————————————————————————————————————————
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

#————————————————————————————————————————
#  LTE Positioning Profile Settings
#————————————————————————————————————————
# 0: Enable RRLP on LTE(Default)
# 1: Enable LPP_User_Plane on LTE
# 2: Enable LPP_Control_Plane
# 3: Enable both LPP_User_Plane and LPP_Control_Plane
#LPP_PROFILE = 2                  【同时使能 LPP_CP  LPP_UP  , LPP_PROFILE = 8 】

#————————————————————————————————————————
# Select Positioning Protocol on A-GLONASS system
#————————————————————————————————————————
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

#————————————————————————————————————————
# PSDS download settings #
#————————————————————————————————————————
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


### Qcom_GPS信号L5支持查看_NV74255


```
QXDM > NV Browser > NV74255

74255,/nv/item_files/gps/cgps/me/gnss_multiband_configuration,/nv/item_files/gps/cgps/me/gnss_multiband_configuration,GPS

取值范围: 0---0xffffffff

为0: 则 Qcom该产品不支持 L5 GPS 信号

INPUT,VALUE,NAME,SIZE,TYPE
39【0x27】,39,gnss_multiband_configuration,32,Uint32


 zbitfeature_E0.bat 39
 
#######=============解析   flag=0x27   Begin ===========####
当前解析能力标记位Long(       16进制):0x27
当前解析能力标记位Long( 有符号10进制):39                 (无符号10进制):39          (byte[0x27]_转_int_10进制):[39]
当前解析能力标记位Long( 未补位 2进制):100111
当前解析能力标记位Long( 补位   2进制):0000000000000000000000000000000000000000000000000000000000100111
=============解析能力如下===========
索引:all  bit位:0000000000000000000000000000000000000000000000000000000000100111
索引:000  bit位:0000000000000000000000000000000000000000000000000000000000000001  16进制:0x0000000000000001  10进制:1
索引:001  bit位:0000000000000000000000000000000000000000000000000000000000000010  16进制:0x0000000000000002  10进制:2
索引:002  bit位:0000000000000000000000000000000000000000000000000000000000000100  16进制:0x0000000000000004  10进制:4
索引:005  bit位:0000000000000000000000000000000000000000000000000000000000100000  16进制:0x0000000000000020  10进制:32
=============解析能力结束===========
 
```
Bit位示意图
<img src="/public/zimage/qocm_issue/nv74255_1.jpg"/>

常见配置值列表
<img src="/public/zimage/qocm_issue/nv74255_2.jpg"/>










### Qcom_AGPS模式UP_CP_MSA_MSB_2G_3G支持配置_NV1920

```

1920,AAGPS Positioning Modes Supported,aagps_positioning_modes_supported,AAGPS

NV1920 需要支持的AGPS模式 

默认值为:   65407 (0xFF7F)  
Units – Bit map
Type – UINT32
Range – 0 to 65535


Bit0    0  Standalone
Bit1    1  UP MS-based
Bit2    2  UP MS-assisted
Bit3    3  CP MS-based    (2G)
Bit4    4  CP MS-assisted (2G)
Bit5    5  CP UE-based    (3G)
Bit6    6  CP UE-assisted (3G)
Bit7    7  UP network measurement report (2G)
Bit8    8  UP MS-based    (4G)
Bit9    9  UP MS-assisted (4G)
Bit10   10 CP MS-based    (4G)
Bit11   11 CP MS-assisted (4G)
Bit16   16 Enabling of autonomous fallback for SUPL-MSB
Bit17   17 A-GLONASS UP MS-based for    3G
Bit18   18 A-GLONASS UP MS-assisted for 3G
Bit19   19 A-GLONASS CP MS-based for    3G
Bit20   20 A-GLONASS CP MS-assisted for 3G
Bit21   21 A-GLONASS UP MS-based for    4G
Bit22   22 A-GLONASS UP MS-assisted for 4G
Bit23   23 A-GLONASS CP MS-based for    4G
Bit24   24 A-GLONASS CP MS-assisted for 4G

LPP(4G)_CP 控制Bit位:  Bit10 Bit11  Bit23 Bit24
LPP(4G)_UP 控制Bit位:  Bit8  Bit9   Bit21 Bit22
RRC_3G_CP  控制Bit位:  Bit5  Bit6   Bit19 Bit20
RRC_3G_UP  控制Bit位:               Bit17 Bit18
RRLP(2G)_CP控制Bit位:  Bit3  Bit4
Messure检测控制Bit位:        Bit7    UP network measurement report (2G)

 
 
默认值:  65407  0xFF7F

#######=============解析  65407   flag=0xff7f   Begin ===========####
当前解析能力标记位Long(       16进制):0xff7f
当前解析能力标记位Long( 有符号10进制):65407                 (无符号10进制):65407
当前解析能力标记位Long( 未补位 2进制):1111111101111111
当前解析能力标记位Long( 补位   2进制):0000000000000000000000000000000000000000000000001111111101111111
=============解析能力如下===========
索引:all  bit位:0000000000000000000000000000000000000000000000001111111101111111
索引:000  bit位:0000000000000000000000000000000000000000000000000000000000000001  16进制:0x0000000000000001  10进制:1
索引:001  bit位:0000000000000000000000000000000000000000000000000000000000000010  16进制:0x0000000000000002  10进制:2
索引:002  bit位:0000000000000000000000000000000000000000000000000000000000000100  16进制:0x0000000000000004  10进制:4
索引:003  bit位:0000000000000000000000000000000000000000000000000000000000001000  16进制:0x0000000000000008  10进制:8
索引:004  bit位:0000000000000000000000000000000000000000000000000000000000010000  16进制:0x0000000000000010  10进制:16
索引:005  bit位:0000000000000000000000000000000000000000000000000000000000100000  16进制:0x0000000000000020  10进制:32
索引:006  bit位:0000000000000000000000000000000000000000000000000000000001000000  16进制:0x0000000000000040  10进制:64
索引:008  bit位:0000000000000000000000000000000000000000000000000000000100000000  16进制:0x0000000000000100  10进制:256
索引:009  bit位:0000000000000000000000000000000000000000000000000000001000000000  16进制:0x0000000000000200  10进制:512
索引:010  bit位:0000000000000000000000000000000000000000000000000000010000000000  16进制:0x0000000000000400  10进制:1024
索引:011  bit位:0000000000000000000000000000000000000000000000000000100000000000  16进制:0x0000000000000800  10进制:2048
索引:012  bit位:0000000000000000000000000000000000000000000000000001000000000000  16进制:0x0000000000001000  10进制:4096
索引:013  bit位:0000000000000000000000000000000000000000000000000010000000000000  16进制:0x0000000000002000  10进制:8192
索引:014  bit位:0000000000000000000000000000000000000000000000000100000000000000  16进制:0x0000000000004000  10进制:16384
索引:015  bit位:0000000000000000000000000000000000000000000000001000000000000000  16进制:0x0000000000008000  10进制:32768
=============解析能力结束===========
#######=============解析   flag=0xff7f   End   ===========####

65407 0xFF7F:
索引:000   Bit 00 Standalone
索引:001   Bit 01 UP MS-based
索引:002   Bit 02 UP MS-assisted
索引:003   Bit 03 CP MS-based    (2G)
索引:004   Bit 04 CP MS-assisted (2G)
索引:005   Bit 05 CP UE-based    (3G)
索引:006   Bit 06 CP UE-assisted (3G)
索引:008   Bit 08 UP MS-based    (4G)
索引:009   Bit 09 UP MS-assisted (4G)
索引:010   Bit 10 CP MS-based    (4G)
索引:011   Bit 11 CP MS-assisted (4G)
索引:012 
索引:013 
索引:014 

```

<img src="/public/zimage/qocm_issue/gps_nv1920.jpg"/>


### Qcom_LPP4G网络下CP_UP控制项_NV67725

```
67225,Use LPP when on LTE,/nv/item_files/gps/cgps/sm/gnss_lpp_enable,GPS

NV67225
索引:000   Bit 00  【(Enable LPP(4G)_UP】   Enable LTE User    Plane LPP
索引:001   Bit 01  【(Enable LPP(4G)_CP】   Enable LTE Control Plane LPP
索引:002   Bit 02  【(Enable NR_SA(5G)_UP】 Enable 5G NR_SA User    Plane LPP
索引:003   Bit 03  【(Enable NR_SA(5G)_CP】 Enable 5G NR_SA Control Plane LPP
索引:004   Bit 04   保留
索引:005   Bit 05   保留



// 当前从 NVbrowser 读取到值15  当前NV67725默认值是2 
INPUT,VALUE,NAME,SIZE,TYPE
15,15,gnss_lpp_enable,8,Uint8

15的位解析
#######=============解析 flag=15  flag=0xf    Begin ===========####
索引:all  bit位:0000000000000000000000000000000000000000000000000000000000001111
索引:000  bit位:0000000000000000000000000000000000000000000000000000000000000001  16进制:0x0000000000000001  10进制:1
索引:001  bit位:0000000000000000000000000000000000000000000000000000000000000010  16进制:0x0000000000000002  10进制:2
索引:002  bit位:0000000000000000000000000000000000000000000000000000000000000100  16进制:0x0000000000000004  10进制:4
索引:003  bit位:0000000000000000000000000000000000000000000000000000000000001000  16进制:0x0000000000000008  10进制:8
=============解析能力结束===========

2的位解析
#######=============解析   flag=2  flag=0x2   Begin ===========####
索引:001  bit位:0000000000000000000000000000000000000000000000000000000000000010  16进制:0x0000000000000002  10进制:2
=============解析能力结束===========


```

<img src="/public/zimage/qocm_issue/gps_nv67225.jpg"/>



### Qcom_LPPe4G_UP控制项_NV73863

```

INPUT,VALUE,NAME,SIZE,TYPE
0x1 | LPPE_DBH_ENABLE | LPPE_WLAN_ENABLE | 0x10,0x1 | LPPE_DBH_ENABLE | LPPE_WLAN_ENABLE | 0x10,gnss_lppe_up_config,32,Bitmask



0=NILR(NI) === NetWork Initiate LocationRequest  ===  2G CP Test Scenario (NI)
1=MTLR(NI) === Mobile Terminated LocationRequest  ===  2G CP Test Scenario – MTLR, request additional assist data
2=MOLR(SI) === Mobile Originated Location Request ===  2G CP Test Scenario – MOLR-Location Estimate

UBP === uncompensated barometric pressure  === 无补偿气压技术 ===  用于Z轴定位

DBH ===  Device-Based Hybrid ===  混合3D高精度 定位 

Civic Address  ===   市民地址 === "urn:ietf:params:xml:ns:pidf:geopriv10:civicAddr:ext" per IETF RFC 6848 [15]

```

<img src="/public/zimage/qocm_issue/gps_nv73863.jpg"/>



### Qcom_LPPe4G_CP控制项_NV73863


### Qcom_TTFF加速_NV74302_GNSSffaeConfig

<img src="/public/zimage/qocm_issue/.jpg"/>

```

FFAE === （First Fix Accuracy Enhancement）=== 首次定位精度增强 TTFF

INPUT,VALUE,NAME,SIZE,TYPE
0,0,gnss_ffae_config,32,Uint32


这个NV项是 qcom的一个优化型的一个算法 
开启FFAE定位相关的 ， 
具体的描述就，在信号较弱的情况会拉长定位的时间 给到用户一个比较精确地位置，


```





### Qcom_GPS模式配置查看_NV70326_GnssConfig


<img src="/public/zimage/qocm_issue/nv70326.jpg"/>

```

Qcom通过QXDM_NVBrowser查看GPS_Mode

QXDM > NV Browser > NV70326 
 
Bit位 描述                                                         当前默认值
B0    Reserved,GPS set it to 1                                     1
B1    Controls GLONASS                                             1
B2    Controls BeiDou  outside of the United States                1    【美国外可用北斗】
B3    Controls Galileo outside of the United States                0
B4    Controls BeiDou  worldwide                                   0    【全球可用北斗】
B5    Controls Galileo worldwide[2]                                0
B6    Controls QZSS worldwide                                      0
B7    Reserved, set to 0                                           0
B8    Controls QZSS outside of the United States                   0
B9    Controls QZSS L1S (SLAS) in Japan L1S serviceable area       0
B10   Reserved, set to 0                                           0
B11   Controls Galileo                                             1
B12   Controls NavIC outside of the United States                  0
B13   Controls NavIC worldwide                                     0
B14   Controls GLONASS G1 qualified                                0
B15   Controls GLONASS G1 force-enabled                            0
Other bits Reserved, set to 0

GPS     美国    全球定位系统(Global Positioning System，GPS)      
GLONASS 俄罗斯  格洛纳斯-俄罗斯实现的国家导航系统    
Beidou  中国    北斗      
Galileo 欧洲    伽利略
QZSS    日本    准天顶卫星系统 Quasi-Zenith Satellite System
NAVIC(IRNSS)    印度 NAVIC(Navigation with Indian Constellation)  IRNSS(印度区域导航卫星系统(Indian Regional Navigation Satellite System (IRNSS)NAVIC)


╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤ Qcom GNSS NV70326 GnssConfig 取值列表 ╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤

NV70326 = 
0x803 - GPS/GLO/GAL enabled, BDS force enabled, QZSS/NavIC disabled; 
0x807 - GPS/GLO/GAL enabled, BDS qualified enabled, QZSS/NavIC disabled; 
0x907 - GPS/GLO/GAL enabled, BDS/QZSS qualified enabled*, NavIC disabled (default)
0x1905 -GPS/GAL enabled, BDS/QZSS/NavIC qualified enabled*(0001 1001 0000 0101)
0x1907 -GPS/GLO/GAL enabled, BDS/QZSS/NavIC qualified enabled*

0x803 
索引:B0  GPS
索引:B1  GLONASS
索引:B11 Galileo 


0x807 
索引:B0  GPS
索引:B1  GLONASS
索引:B2  Beidou  ( outside of US )
索引:B11 Galileo 


0x853
索引:B0  GPS
索引:B1  GLONASS
索引:B4  Controls BeiDou  worldwide 
索引:B6  Controls QZSS worldwide 
索引:B11 Galileo 



0x907
索引:B0  GPS
索引:B1  GLONASS
索引:B2  Beidou ( outside of US )
索引:B8  QZSS   ( outside of US ) 
索引:B11 Galileo 


0x1905
索引:B0  GPS
索引:B2  Beidou ( outside of US )
索引:B8  QZSS   ( outside of US ) 
索引:B11 Galileo 
索引:B12 NavIC  ( outside of US )


0x1906
索引:B1  GLONASS
索引:B2  Beidou ( outside of US )
索引:B8  QZSS   ( outside of US ) 
索引:B11 Galileo 
索引:B12 NavIC  ( outside of US )




0x1907
索引:B0  GPS
索引:B1  GLONASS
索引:B2  Beidou ( outside of US )
索引:B8  QZSS   ( outside of US ) 
索引:B11 Galileo 
索引:B12 NavIC  ( outside of US )



0x2853
索引:B0  GPS
索引:B1  GLONASS
索引:B4  Controls BeiDou  worldwide 
索引:B6  Controls QZSS worldwide 
索引:B11 Galileo 
索引:B13 Controls NavIC worldwide


```






### Qcom_LPPe4G_CP控制项_NV73888

```
默认值:0  当前值535
INPUT,VALUE,NAME,SIZE,TYPE
535,535,gnss_lppe_cp_config,32,Uint32


B0    Enable/disable LPPe for CP  
B1    Enable/disable DBH for CP           
B2    Enable/disable WLAN measurements for CP         
B3    Enable/disable SRN BTLE for CP              
B4    Enable/disable UBP for CP                        
B9    Enable/disable LPPe in non-E911 CP NILR sessions              
B10   Enable/disable civic address for CP                                 
Other Reserved


#######=============解析  int=535  hex=0x217   Begin ===========####
索引:000  bit位:0000000000000000000000000000000000000000000000000000000000000001  16进制:0x0000000000000001  10进制:1
索引:001  bit位:0000000000000000000000000000000000000000000000000000000000000010  16进制:0x0000000000000002  10进制:2
索引:002  bit位:0000000000000000000000000000000000000000000000000000000000000100  16进制:0x0000000000000004  10进制:4
索引:004  bit位:0000000000000000000000000000000000000000000000000000000000010000  16进制:0x0000000000000010  10进制:16
索引:009  bit位:0000000000000000000000000000000000000000000000000000001000000000  16进制:0x0000000000000200  10进制:512
 
 
候选值: 535
索引:000   Enable LPPe for CP  
索引:001   Enable DBH for CP     
索引:002   Enable WLAN measurements for CP      
索引:004   Enable UBP for CP          
索引:009   Enable LPPe in non-E911 CP NILR sessions   


```

<img src="/public/zimage/qocm_issue/nv73888.jpg"/>





### Qcom_AGPS_定位模式支持控制项_NV74334

```
 GNSS NV A-GPS positioning modes supported ext

默认值:0    当前值:255
INPUT,  VALUE,   NAME,                                 SIZE,    TYPE
  255,    255,   aagps_positioning_modes_supported_ext,  64,  Uint64


Bit0   Enable/disable AGNSS NR CP LPP GPS MS-BASED
Bit1   Enable/disable AGNSS NR CP LPP GPS MS-ASSISTED
Bit2   Enable/disable AGNSS NR CP LPP GLO MS-BASED
Bit3   Enable/disable AGNSS NR CP LPP GLO MS-ASSISTEDD
Bit4   Enable/disable AGNSS NR UP LPP GPS MS-BASED
Bit5   Enable/disable AGNSS NR UP LPP GPS MS-ASSISTED
Bit6   Enable/disable AGNSS NR UP LPP GLO MS-BASED
Bit7   Enable/disable AGNSS NR UP LPP GLO MS-ASSISTED
Bit8   Enable/disable AGNSS NR RRLP UP GPS MS-BASED
Bit9   Enable/disable AGNSS NR RRLP UP GPS MS-ASSISTED
Bit10  Enable/disable AGNSS NR RRLP UP GLO MS-BASED
Bit11  Enable/disable AGNSS NR RRLP UP GLO MS-ASSISTED
Bit12  Enable/disable AGNSS NR RRLP UP BDS MS-BASED
Bit13  Enable/disable AGNSS NR RRLP UP BDS MS-ASSISTED
Bit14  Enable/disable AGNSS NR LPP UP BDS MS-BASED
Bit20  Enable/disable AGNSS NR LPP CP ECID
Other  bits Reserved



int=255  hex=0xff   
索引:000   Enable AGNSS NR CP LPP GPS MS-BASED
索引:001   Enable AGNSS NR CP LPP GPS MS-ASSISTED
索引:002   Enable AGNSS NR CP LPP GLO MS-BASED
索引:003   Enable AGNSS NR CP LPP GLO MS-ASSISTEDD
索引:004   Enable AGNSS NR UP LPP GPS MS-BASED
索引:005   Enable AGNSS NR UP LPP GPS MS-ASSISTED
索引:006   Enable AGNSS NR UP LPP GLO MS-BASED
索引:007   Enable AGNSS NR UP LPP GLO MS-ASSISTED




```


<img src="/public/zimage/qocm_issue/nv74334.jpg"/>


### Qcom_SUPL版本控制_NV6792

```

ID – 6792
Type – UINT32
Range – NA
Units – NA
Default value – 0x00010000


当权值:   131076 ==  0x20004
INPUT,VALUE,NAME,SIZE,TYPE
131076,131076,gnss_SUPL_Version,32,Uint32


Descripton
This NV item determines the SUPL version used in an SI session, 
and the highest possible SUPL major version used in an NI session.
Supported versions:


0x10000 【65536】–  SUPL 1.0 (default)
0x20000 【131072】– SUPL 2.0 (with LTE supported)
0x20002 【131074】– SUPL 2.0.2 (with LPPe supported)
0x20004 【131076】– SUPL 2.0.4 (with LPPe and 5G NR supported)


// 这个NV值会在 config.xml 或者 gps.conf 中 被覆盖 
This NV item can be written by SUPL_VER from HLOS (carrier config.xml or gps.conf) 
QMI LOC during the bootup or SIM change.


```


<img src="/public/zimage/qocm_issue/nv6792.jpg"/>




### Qcom_SUPL_TLS1.1_TLS1.0_选择_NV3758
```
Descripton
This NV item determines whether to use transport security for SUPL sessions.How to confgure

ID – 3758
Type – UINT8
Range – NA
Units – Bit map
Default value – 【1】


Bit 0 – To enable/disable transport security
        0 – Disables security
		1 – Enables security (default)
Bit 1 – To select the TLS version when SUPL 2.0 is selected for call flow.
        0 – TLS 1.1 (default)1 – TLS 1.0
        1 – TLS 1.0
Bit 2 – To select the SHA version when SUPL2.0 is selected for call flow.
        0 – SHA-256 (default)
		1 – SHA-1

```


### Qcom_SUPL_UP使能_NV65811
```

ID – 65811
Type – UINT8
Range – NA
Units – Bitmap
Default value – 0 (0x0)

Bit 0 – To enable SUPL as user plane protocol on 1X.
         0 – Disables SUPL on 1X (default)
         1 – Enables SUPL on 1X

```


### Qcom_SUPL_TLS1.2_使能_NV74121

```

优先检查 NV74121_TLS1.2 , 没有配置 NV74121 才回去检查NV3758  检查 TLS1.1_TLS1.0
   
ID – 74121
Type – UINT32
Range – 0 to 0xFFFFFFFF
Units – N/A
Default value – Varies across different targets


1 ––––––  TLS 1.2 enabled  –––––– Applicable for targets with C#3880572, including all the devices with
0 ––––––  TLS 1.2 disabled –––––– Applicable for targets without CR#3880572.
 NV#3758 is checked only when NV#74121 is not configured. 


Lower 16 bits are used to configure TLS support.

Bit0          – TLS 1.2 enabled/disabled.
Bit1 to Bit15 – Reserved for newer TLS versions.

```



### Qcom_SUPL_SI(3G)时_CP_UP的选中项_NV4707

```
MO method (UMTS only)
ID – 04707
Type – UINT8
Range – 0 or 1
Units – Enumeration Default value – 1

Descripton
This NV item specifies if the mobile-originated (MO) GPS session 
must use the 【control plane】 session or 【user plane】 session.

0 – Control plane 
1 – User plane

设备SI 发起会话请求时 使用 CP 还是 UP 


当前值: 1  当前使用用户面
INPUT,VALUE,NAME,SIZE,TYPE
1,1,cgps_mo_method,8,Uint8


```

<img src="/public/zimage/qocm_issue/nv4707.jpg"/>


#### Qcom_SUPL_UT1_UT2_UT3_会话超时_NV2787

```
UT1 ---> UE Time1  用户设备时间1   设备UE发送SUPL_START 到得到TE反馈Response的超时时间
UT2 ---> UE Time2  用户设备时间2   设备UE发送SUPL_POS_INIT  到得到TE反馈 SUPL_POS 的超时时间
UT3 ---> UE Time3  用户设备时间3   设备UE发送SUPL_POS  到 TE反馈 SUPL_END的超时时间

PreSUPL UE timer1
ID – 02787
Type – UINT32
Range – 0 to 255 Units – Seconds
Default value – 20  // 默认值时 20秒
INPUT,VALUE,NAME,SIZE,TYPE // 当前读取值10
10,10,aagps_default_presupl_ue_timer1_value,32,Uint32

ID – 02787 Descripton
This NV item is used for SUPL although the name remains PreSUPL UE Timer1 for legacy reasons.
This is the timeout from the UE sending SUPL_START to its receipt of SUPL_RESPONSE from the SLP. 
The same value is also used for the timer used to wait for the initial datalink to be established.


PreSUPL UE timer2
ID – 02788
Type – UINT32
Range – 0 to 255 Units – Seconds
Default value – 20            // 默认值20 
INPUT,VALUE,NAME,SIZE,TYPE   // 当前读取值10
10,10,aagps_default_presupl_ue_timer2_value,32,Uint32

ID – 02788 Descripton
This NV item is used for SUPL although the name remains PreSUPL UE Timer2 for legacy reasons.
This is the timeout from the UE sending SUPL_POS_INIT to its receipt of the first SUPL_POS from the SLP.


PreSUPL UE timer3
ID – 02789
Type – UINT32
Range – 0 to 255 Units – Seconds 
Default value – 20   // 默认值20 
INPUT,VALUE,NAME,SIZE,TYPE  // 当前读取值10 
10,10,aagps_default_presupl_ue_timer3_value,32,Uint32

ID – 02789  Descripton: 
This NV item is used for SUPL although the name remains PreSUPL UE Timer3 for legacy reasons.
This is the timeout from the UE sending SUPL_POS to its receipt of SUPL_END from the SLP, 
or any subsequent SUPL_POS from the SLP.




```





#### Qcom_Xtra_辅助数据服务功能控制_NV4627

```

ID    – 04627
Type  – Boolean
Range – TRUE/FALSE
Units – NA
Default value – TRUE (enabled)


Descripton
This NV item enables/disables the GNSS assistance service feature 【 enables/disables  Qcom GPS Xtra Feature 】.

INPUT,VALUE,NAME,SIZE,TYPE   // 当前默认位true  打开
True,True,gps1_xtra_enabled,8,Bool


```


#### Qcom_Xtra_辅助数据服务下载次数控制_NV4629

```
ID – 04629
Type – UINT8
Range – 0 to 10 Units – NA
Default value – 3


INPUT,VALUE,NAME,SIZE,TYPE   // 即一天下载 xtra的数量限制  为3  , 可以 改成 1000试一试 下次
3,3,gps1_xtra_num_download_attempts,8,Uint8

Descripton
This NV item specifies the number of unsuccessful file download attempts before aborting requests.


How to confgure
The manufacturer chooses this value.
When MPSS detects that XTRA has expired, it generates the on-demand XTRA request. If the
requirement is not met from the APSS client, then MPSS will retry to download XTRA data after 10
minutes—the default value of NV4630.
If MPSS is running but APSS is in low power state, retrying to download XTRA data may lead to APSS wakeup. 
Though APSS wakeup is acceptable, OEM can disable this retry of download by setting NV
4629 to 0. In such case, the next download will be allowed only after 12 hours.




```


#### Qcom_Xtra_辅助数据服务下载时间间隔控制_NV4630

```

ID – 04630  ----- GNSS assistance service tme between atempts 
Type – UINT8 Range:
      1 to 120
      2 to 120 for devices with Location software v22 and later.
Units – Minutes
Default value – 10


Descripton
This NV item specifies the number of minutes between unsuccessful file download requests 
sent to the GNSS assistance service client.


INPUT,VALUE,NAME,SIZE,TYPE   // 下载xtra数据失败 之后重试的时间间隔
10,10,gps1_xtra_time_between_attempts,8,Uint8



```


#### Qcom_Xtra_辅助数据服务下载url配置_NV4632

```

ID    – 04632  ----- GNSS assistance service primary server URL
Type  – 128 char string
Range – NULL-terminated ASCII
Units – NA
Default value – See the following description


Descripton:  QCOm xtra 辅助数据下载定位数据的主url:
This NV item specifies the URL of the primary GNSS assistance service server. The latest server URLs:


v1.0【GPS】 only predictions     (v1.0 devices) – 
https://path1.xtracloud.net/xtra.bin

v2.0 【GPS+GLO】  predictions     (v2.0 devices) – 
https://path1.xtracloud.net/xtra2.bin

v3.0【GPS+GLO+BDS】  predictions  (v3.0 devices) – 
https://path1.xtracloud.net/xtra3grc.bin 

v3.1【GPS+GLO+BDS+GAL+QZSS】  predictions (v3.1 devices and higher) –
https://path1.xtracloud.net/xtra3grcej.bin
 
在url中 g-GPS r-GLO c-DBS e-GAL j-QZSS  i-Navic



Most common GNSS assistance service v3.x files:       可选的xtta辅助数据信息url下载地址
https://path1.xtracloud.net/xtra3gr.bin      (GPS + GLO)
https://path1.xtracloud.net/xtra3grc.bin     (GPS + GLO + BDS) – common for MPSS.JO modems
https://path1.xtracloud.net/xtra3grcj.bin    (GPS + GLO + BDS + QZSS)
https://path1.xtracloud.net/xtra3grcej.bin   (GPS + GLO + BDS + GAL + QZSS)
https://path1.xtracloud.net/xtra3Mgrcej.bin  (GPS + GLO + BDS + GAL + QZSS, Multiband enabled)
https://path1.xtracloud.net/xtra3Mgrbeji.bin (GPS + GLO + BDS + GAL + QZSS + NavIC, common for MPSS.HI modems,dynamically URL selection, no configuration required)




INPUT,VALUE,NAME,SIZE,TYPE   // 当前值是 https://xtrapath1.izatcloud.net/xtra3grcej.bin
https://xtrapath1.izatcloud.net/xtra3grcej.bin, 【INPUT】
https://xtrapath1.izatcloud.net/xtra3grcej.bin,【VALUE】
gps1_xtra_primary_server_url,【NAME】       
368,【SIZE】
Char8[] 【TYPE】


If OEM configures the device with different constellation combination in NV#70326, change the filename accordingly.

如果OEM在 nv#70326 GPS_Mode 中配置了不同星座组合的设备，请相应地更改文件名 使得对应的xtra url 能及时得到辅助数据的支持



ID – 04633  --- GNSS assistance service secondary server URL 
同 NV4632 是一个第二xtra候选的url ,（当第一个url NV4632 不工作时 使用该辅助url）
Type – 128 char string
Range – NULL-terminated ASCII
Units – NA
Default value – See the following description:


INPUT,VALUE,NAME,SIZE,TYPE  // 当前值
https://xtrapath1.izatcloud.net/xtra3grcej.bin,
https://xtrapath1.izatcloud.net/xtra3grcej.bin,
gps1_xtra_primary_server_url,368,Char8[]



ID – 04634  --- GNSS assistance service tertary server URL 
Type – 128 char string
Range – NULL-terminated ASCII
Units – NA
Default value – See the following description

同 NV4632 是一个第三个xtra候选的url ,（当第一个url NV4632 不工作时 使用该辅助url）

INPUT,VALUE,NAME,SIZE,TYPE  // 当前值
https://xtrapath3.izatcloud.net/xtra3grcej.bin,
https://xtrapath3.izatcloud.net/xtra3grcej.bin,
gps1_xtra_tertiary_server_url,  368,   Char8[]


```


#### Qcom_Xtra_辅助数据服务使能NTP时间注入_NV4927

```
ID – 04927  --- GNSS assistance service tmeinjecton enable/disable
Type – UINT8
Range – 0 or 1
Units – Enumeration
Default value – 1 (enabled)


0 – Disables time injection feature 
1 – Enables time injection feature


启用此功能后，定位子系统可以向GNsS辅助服务客户端发送请求，从internet/网络上的服务器检索SNTP时间信息，
并通过适当的API注入。这可以提高定位的速度和精度。
如果系统还没有时间感，则在定位会话期间向客户端发送时间请求。
这通常在异步网络上更常见，因为同步网络上的时间很充裕。


INPUT,VALUE,NAME,SIZE,TYPE
1,1,gps1_xtra_time_info_enabled,8,Uint8

```


#### Qcom_Xtra_时间timeNTP时间注入url_NV4930

```

ID   – 04930  ------- GNSS assistance service primary NTP server URL
Type – 128 char string
Range – NULL-terminated ASCII
Units – NA
Default value – time.xtracloud.net (or set by licensee)


INPUT,VALUE,NAME,SIZE,TYPE  // 当前默认值  time.xtracloud.net
time.xtracloud.net,time.xtracloud.net,gps1_xtra_primary_sntp_server_url,144,Char8[]

```



#### Qcom_Xtra_辅助数据有效时间控制_NV65604


```

ID   – 65604   ---- GNSS assistance service data preferred maximum valid age
Type – UINT16
Range – 24 to 168
Units – Hours
Default value – 168 or 72 (hours)


描述NV项指定GNSS辅助服务数据的首选最大有效时间。
最新手机平台默认为3天，其他平台为7天。
如果在会话开始时，XTRA文件的年龄大于首选的有效年龄，则从Modem请求刷新XTRA文件。
如果需要更频繁地刷新XTRA文件，则oem可以指定较小的值， 如48小时或24小时。


```



### Qcom_Enable_Navic的NV配置项
```

Enable Navic

NV74255   0x27    【gnss_multiband_configuration】                       
NV70326   0x1907
NV4632    https://path1.xtracloud.net/xtra3Mgrbeji.bin
NV4633    https://path2.xtracloud.net/xtra3Mgrbeji.bin
NV4634    https://path3.xtracloud.net/xtra3Mgrbeji.bin


```


### MTK通过命令设置GPS_Mode
```
adb root && adb shell setprop persist.vendor.radio.gps_test_mode 4 && adb reboot 

adb logcat | grep GNSSOPMode      //  查看当前GPS模式的打印
 
0        GPS_GLONASS
1        GPS_BEIDOU
2        GPS_GLONASS_BEIDOU
3        GPS
4        BEIDOU
5        GLONASS
6        GPS_GLONASS_BEIDOU_GALILEO
7        GPS_GALILEO
8        GPS_GLONASS_GALILEO
9        GALILEO
10(0xa)  GPS_GLONASS_BEIDOU_GALILEO_NAVIC
11(0xb)  BEIDOU_GLONASS_GALILEO_NAVIC

 //  MTK 查看当前GPS模式的打印
adb logcat | grep GNSSOPMode    

//  MTK 重启查看打印
adb reboot && adb wait-for-device &&  adb logcat | grep GNSSOPMode  

//输出打印
:2609:04-16 18:31:10.354  1491  1491 D gps_controlller: get_chip_gnss_op_mode: get_chip_gnss_op_mode]: 
mnld GNSSOPMode: 0xa 【GNSSOPMode】  ro.vendor.hw.device xxxx  ro.product.is_prc  ro.carrier XXXX  ro.vendor.hw.radio:XXXX  persist.vendor.radio.gps_test_mode:

```


### MTK通过工模设置 GPS_Mode
```
*#*#3646633#*#*
 
进入MTK工模
 
然后Location - MNL Config Editor
 
选择GnssMode，Edit
 
Config选择Enabled，然后Setting改成4（Beidou Only模式），OK

这步完上面还有一个Set要点一下，然后重启手机
```



### 无线adb

```
// 0. 手机和WIFI 连接同一个 ssid 和 bssid的网络  建议5G( 2.4G有失败经历 )
// 1.  手机连接USB 执行  CMD执行如下命令 
adb  tcpip 5555

// 2. 拔掉 移除 USB
// 3. 检查手机的IP地址  手机WIFI按钮长按进入详情设置 例如: 192.168.1.105
// 4. CMD 执行 adb connect 【P】:5555 命令
adb connect 192.168.1.105:5555

// 5.执行 CMD adb shell  完成无线adb 连接 
adb shell 

```


### Qcom触发并导出ramdump

```

// 触发 Ramdump
adb root && adb remount && adb shell " iwpriv wlan0 setUnitTestCmd 19 1 4 "    


adb reboot fastboot 

// 在 fastboot 模式下  导出 ramdump文件 
fastboot oem ramdump pull all         




```




###  Qcom_Symbols 文件

```
Vendor端 release 目录中包含 所有的 Symbols 文件 的三个文件

1. 排查当前最新 fastboot  和  Symbol 编译生成时间上是否一致

cd ./release  ;  ls -lt |  grep fastboot | head -1 ;  ls -l | grep -e "mmi_kernel_platform"   -e "symbols.tar"    -e "nonhlos_symbols" 

-rw-rw-r-- 1 xxxx xxxx  7429246677  4月  8 18:02 fastboot_skyline_userdebug_15_XXXX.xxxx.250408-test-keys_global_US.tar.gz
-rwxrwxr-x 1 xxxx xxxx   380494576  4月  8 18:03 mmi_kernel_platform_debug_files.tar.gz
-rw-rw-r-- 1 xxxx xxxx    75071684  4月  8 18:03 nonhlos_symbols.tar.gz
-rw-rw-r-- 1 xxxx xxxx  2805411586  4月  8 18:03 symbols.tar.gz


2. 导出当前  Symbols 文件 的三个文件 
Vendor端/release/mmi_kernel_platform_debug_files.tar.gz
Vendor端/release/nonhlos_symbols.tar.gz
Vendor端/release/symbols.tar.gz




```





### Qcom查看WIFI漫游Log 

```

[ROAM_TRIGGER]  触发漫游
[ROAM_RESULT]   漫游结果

0 == C_AP:  Candidate AP          当前选择的漫游目标WIFI
1 == P_AP:  Current connected AP  当前正在连接的WIFI
2 == R_AP:  Reassoscia AP         尝试重关联的WIFI 

04-04 13:57:43.426  wlan: [2389:I:WMA] [19:57:45.138000] [ROAM_TRIGGER]: VDEV[0] Reason: "LOW RSSI"  Cur_Rssi threshold:75 Current AP RSSI: 71
04-04 13:57:43.426  wlan: [2389:I:WMA] [19:57:45.138000] [ROAM_SCAN]: VDEV[0] Scan_type: PARTIAL next_rssi_threshold: 75 dBm {2437 2462 5745 }
04-04 13:57:43.426  wlan: [2389:I:WMA] ============================================================================================================================
04-04 13:57:43.426  wlan: [2389:I:WMA]      AP BSSID           TSTAMP       CH   TY  ETP  RSSI/SCR CU%/SCR TOT_SCR  BL_RSN BL_SRC    BL_TSTAMP       BL_TIMEOUT(ms)
04-04 13:57:43.426  wlan: [2389:I:WMA] ============================================================================================================================
04-04 13:57:43.426  wlan: [2389:I:WMA] d8:6c:63:e1:54:d6 [19:57:45.138000] 5745 P_AP    0  71/0     0/0        0       0       0   [00:00:00.000000]         0
04-04 13:57:43.426  wlan: [2389:I:WMA] d8:6c:63:e1:55:11 [19:57:45.402000] 2462 C_AP    0  43/0     0/0        0       0       0   [00:00:00.000000]         0
04-04 13:57:43.426  wlan: [2389:I:WMA] d8:6c:63:e1:55:0d [19:57:45.618000] 5745 R_AP    0  44/0     0/0        0       0       0   [00:00:00.000000]         0
04-04 13:57:43.426  wlan: [2389:I:WMA] [19:57:45.933000] [ROAM_RESULT]: VDEV[0] SUCCESS


```





### Qcom_WLAN_Log 


#### Qcom_配置FW_WLAN详细Log开关

```

// 查询当前 WCNSS_qcom_cfg.ini  路径
adb shell " find /vendor/etc/wifi -name 'WCNSS_qcom_cfg.ini'  " 
/vendor/etc/wifi/WCNSS_qcom_cfg.ini


adb root
adb remount
adb pull /vendor/etc/wifi/WCNSS_qcom_cfg.ini   


// add   gindoor_channel_support=1 in end of WCNSS_qcom_cfg.ini
gEnablefwlog=1       #启用固件日志(要删除)
gEnablefwlogging=1   #启用详细记录(要删除)

adb  root && adb remount  adb push ./WCNSS_qcom_cfg.ini  /vendor/etc/wifi/ 
adb reboot
adb pull /vendor/etc/wifi/WCNSS_qcom_cfg.ini 

往 WCNSS_qcom_cfg.ini  添加Log开关
gEnablefwlog=1       # 启用固件日志
gEnablefwlogging=1   # 启用详细记录


```

```

adb root
adb remount


// 查询当前 WCNSS_qcom_cfg.ini  路径
adb shell " find /vendor/etc/wifi -name 'WCNSS_qcom_cfg.ini'  " 
/vendor/etc/wifi/qca6750/WCNSS_qcom_cfg.ini


adb pull  /vendor/etc/wifi/qca6750/WCNSS_qcom_cfg.ini   
 
 
// add  value in end of WCNSS_qcom_cfg.ini  在一个value 后面追加 下面的配置 打开 WLAN 详细Log 
gEnablefwlog=1       # 启用固件日志
gEnablefwlogging=1   # 启用详细记录


adb  root && adb remount && adb push ./WCNSS_qcom_cfg.ini  /vendor/etc/wifi/qca6750/
adb reboot

```


#### Qcom_导出cnss_fw_logs文件

```

adb root && adb remount && adb pull /data/vendor/bug2go/

adb root && adb remount && adb pull /data/vendor/aplogd

adb root && adb remount && /data/vendor/wifi/wlan_logs/


adb root && adb remount &&  adb shell  "rm -fr /data/vendor/aplogd/*"
adb root && adb remount &&  adb shell  "rm -fr /data/vendor/bug2go/*"

【FW WLAN_Log  cnss_fw_logs_*.txt】
adb root && adb remount &&  adb shell  "rm -fr /data/vendor/wifi/wlan_logs/*"


检查 cnss_diag 是否运行

adb root && adb shell "ps -A | grep cnss_dia"
system        2419     1    2377348   7984 do_sys_poll         0 S cnss_diag

cnss_diag -q -f & // 后台运行
adb shell "cnss_diag -q -f & "
 



/data/vendor/wifi/wlan_logs 路径文件内容
/data/vendor/wifi/wlan_logs/host_driver_logs_current.txt
/data/vendor/wifi/wlan_logs/cnss_fw_logs_current.txt
/data/vendor/wifi/wlan_logs/txrx_pktlog_current.dat
/data/vendor/wifi/wlan_logs/cnss_fw_logs_000.txt
/data/vendor/wifi/wlan_logs/cnss_fw_logs_001.txt
/data/vendor/wifi/wlan_logs/cnss_fw_logs_002.txt
/data/vendor/wifi/wlan_logs/cnss_fw_logs_003.txt
/data/vendor/wifi/wlan_logs/cnss_fw_logs_004.txt

```



#### Qcom_WCNSS_qcom_cfg.ini 




```



# This file allows user to override the factory
# defaults for the WLAN Driver

gDot11Mode=0
InfraUapsdVoSrvIntv=0
InfraUapsdViSrvIntv=0
InfraUapsdBeSrvIntv=0
InfraUapsdBkSrvIntv=0
gAddTSWhenACMIsOff=1
gEnableApOBSSProt=1
RTSThreshold=1048576
g11dSupportEnabled=0
=============================
#gEnableDFSMasterCap=1
gNeighborScanTimerPeriod=200
gNeighborLookupThreshold=76
FastRoamEnabled=1
RoamRssiDiff=5
gChannelBondingMode5GHz=1
gAllowDFSChannelRoam=1
gSetTxChainmask1x1=1
gSetRxChainmask1x1=1
gWlanMccToSccSwitchMode = 3
gEnableTXSTBC=1
gEnableTxBFeeSAP=1
gEnableTxBFin20MHz=1
gEnableTxSUBeamformer=1
gRrmEnable=1
gVhtAmpduLenExponent=7
gVhtMpduLen=2
====================================
#isP2pDeviceAddrAdministrated=0
gEnableVhtFor24GHzBand=1
gEnableLpassSupport=1
gCountryCodePriority=1
gEnableMuBformee=1
gTDLSExternalControl=1
gEnableTDLSOffChannel=1
gThermalMitigationEnable=0
gChannelBondingMode24GHz=1

===============  Datapath feature set Begin ===============
gVhtRxMCS=2
gVhtTxMCS=2
gEnable2x2=1
gVhtRxMCS2x2=2
gVhtTxMCS2x2=2
dp_tx_ring_size=3072
rx_mode=20
gEnableFastPath=1
TSOEnable=1
======================
#GROEnable=1
ght_mpdu_density=5
gEnableFlowSteering=1
maxMSDUsPerRxInd=8
=======================================
#gEnableNUDTracking=1
dp_rx_fisa_enable=1
dp_rx_flow_search_table_size=128
rpsRxQueueCpuMapList=07
legacy_mode_csum_disable=0

===================  Datapath feature set End ===================

adaptive_dwell_mode_enabled=1
hostscan_adaptive_dwell_mode=1
enable_rtt_mac_randomization=1
gEnableSNRMonitoring=1
gWmiCreditCount=1
AutoChannelSelectWeight=0x00fafafa
bcast_twt=1
gRuntimePM=2
gRuntimePMDelay=500
oem_6g_support_disable=0

gEnableSWLM=1
g_enable_pci_gen=1
ssdp=0
gRArateLimitInterval=600
gEnableSifsBurst=1
gIbssTxSpEndInactivityTime=10
RX_THREAD_UL_CPU_AFFINITY_MASK=0xc0
dp_rx_buff_prealloc_pool=1
dp_rx_refill_buff_pool=1
dp_rx_fst_in_cmem=1

gBpfFilterEnable=1
gActiveUcBpfMode=2
gActiveMcBcBpfMode=1

===================== Configuration Begin =====================

#Enable user triggered SSR
gEnableForceTargetAssert=1

# 1 - Enable the host silent recovery
# 0 - Disable the host silent recovery
gEnableSelfRecovery=1

# turning QC BLM parameters
avoid_list_expiry_time=5
black_list_expiry_time=1
bad_bssid_counter_thresh=10

# Enable SRD channel
etsi13_srd_chan_in_master_mode=7

#Disable Data Rssi threshold trigger
roam_data_rssi_threshold_triggers=0

#Enable GRO feature forcibly
GROEnable=3

#Enable to derive the P2P MAC address from the primary MAC address
isP2pDeviceAddrAdministrated=1

#Disable the DFS master capability.
gEnableDFSMasterCap=0

#Disable sbs
enable_sbs=0

#Don't disconnected when NUD failure   NUD failed （Neighbor Unreachability Detection, NUD） 网络协议中的邻居不可达检测事件处理策略
#0: Driver will not track the NUD failures, and ignore the same.
#1: Driver will track the NUD failures and if honoured will disconnect from
#   the connected BSSID.
#2: Driver will track the NUD failures and if honoured will roam away from
#   the connected BSSID to a new BSSID to retain the data connectivity.
#3: Driver will try to roam to a new AP but if roam fails, disconnect. Related: None
gEnableNUDTracking=2

#disallow DUT create softap on indoor channel although STA on indoor
# All 5G channale is indoor channel when country code is JP
# we find it will create 5G AP when country code is JP via special steps
sta_sap_scc_on_indoor_chan=0

#wlm_latency_flags_ultralow - WLM flags setting for ultralow level
#bit 0: Avoid scan request from HLOS if setting
wlm_latency_flags_ultralow=0xc82


gEnablefwlog=1    
gEnablefwlogging=1   

=====================  Configuration End =====================

END

# Note: Configuration parser would not read anything past the END marker





```




### Qcom_ctbk_cfg.xml_与mk文件表格数量判断

#### Qcom_SarTable数量
State22个表
```
State22 个表 SarTable  从 <Default index="0" >  -->   <State1  index="1" >  -->  <State22  index="22" >

SAR_WIFI_DBS_SUPPORT
SAR_CAP_ABSENT_PWR_MINIMUM
SAR_WIFI_DBS_STANDALONE_DEFAULT
SAR_WIFI_USB_SUPPORT

```

State21个表
```
State21 个表 SarTable  从 <Default index="0" >  -->   <State1  index="1" >  -->  <State21  index="21" >

SAR_WIFI_DBS_SUPPORT
SAR_WIFI_DBS_STANDALONE_DEFAULT
SAR_WIFI_USB_SUPPORT


```

State20个表
```
State20 个表 SarTable  从 <Default index="0" >  -->   <State1  index="1" >  -->  <State20  index="20" >

SAR_WIFI_DBS_SUPPORT
SAR_WIFI_DBS_STANDALONE_DEFAULT
SAR_WIFI_USB_SUPPORT


```


```
State10 个表 SarTable  从 <Default index="0" >  -->   <State1  index="1" >  -->  <State10  index="10" >

 没有定义 SAR_WIFI_DBS_SUPPORT 


```


#### Qcom_SarTable_Version版本

```
【默认支持DBS版本:】
SAR_VERSION=23
SAR_VERSION=33
SAR_VERSION=35
SAR_VERSION=110


【默认不支持DBS版本:】 可手动添加宏 SAR_WIFI_DBS_SUPPORT 达到支持目的
SAR_VERSION=20
SAR_VERSION=22
SAR_VERSION=32
SAR_VERSION=34


【默认支持BT版本:】 在不支持的版本可手动添加宏 MOT_SAR_BLUETOOTH 达到支持目的
SAR_VERSION=34
SAR_VERSION=35




```



#### Qcom_Sar_宏说明

```

<fcc_wifi_tx_pwr_mimo>  <fcc_country_mcc_list> 
SAR_WIFI_FCC_SEPERATE_SUPPORT    在 ctbk_cfg.xml 子中 存在 tag <fcc_wifi_tx_pwr_mimo>  和  <fcc_country_mcc_list>


存在 tag <fcc_wifi_tx_pwr_mimo> 
SAR_WIFI_MIMO_6G_BDF_SUPPORT


// 存在   <wifi_tx0_sensor_config_5ghz>  和   <wifi_tx0_sensor_config_2ghz>
SAR_WIFI_TX0_SUPPORT


// 存在 <bt_tx_pwr>  和  <bt_tx_pwr_wifi_off>
SAR_BLUETOOTH

// 决定了 sar_sta 和 sar_mhs路径  
// adb shell "find /sys/module  -iname '*sar_*'"
// "/sys/module/qca6490/parameters/sar_sta"    "/sys/module/qca6490/parameters/sar_mhs"
SAR_WIFI_WCN6750


// 决定了  热点socket的控制socket节点  "/data/vendor/wifi/hostapd/ctrl/wlan0"   "/data/vendor/wifi/hostapd/ctrl/wlan1"   "/data/vendor/wifi/hostapd/ctrl/wlan2" 
SAR_WIFI_NO_DBS_SOCKET






决定了table的数量 看下面的sartable分析
SAR_WIFI_DBS_SUPPORT
SAR_CAP_ABSENT_PWR_MINIMUM
SAR_WIFI_DBS_STANDALONE_DEFAULT
SAR_WIFI_USB_SUPPORT

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
adb push libmdmcutback.so /vendor/lib64/libmdmcutback.so       ##  直接libmdmcutback.so 导入 Lib64
adb pull /vendor/lib/libmdmcutback.so libmdmcutback.lib.so 
adb pull /vendor/lib64/libmdmcutback.so  libmdmcutback.lib64.so
adb push ctbk_cfg.xml /vendor/etc/motorola/mdmctbk/ctbk_cfg.xml
adb reboot

 adb root && adb remount && adb shell setprop persist.radio.ctbk_log 5 && adb shell setprop persist.vendor.radio.ctbk_log 5 && adb reboot




关闭sar服务命令:

Qcom_关闭sar:
adb root && adb shell setprop persist.vendor.radio.disable_sar  1  && adb reboot


Qcom_打开sar服务命令
adb root  &&  adb shell setprop persist.vendor.radio.disable_sar  0 && adb reboot


MTK_关闭sar:
adb root && adb shell setprop persist.radio.disable_sar_ctrl   1  && adb reboot


MT_打开sar服务命令
adb root && adb shell setprop persist.radio.disable_sar_ctrl   0  && adb reboot





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
Stream-m.txt:16202:07-25 18:10:32.238  2364  2364 D SARCTRL : targetFilepath: /system/etc/xxxx/mdmctbk/rowe_ctbk_cfg.xml

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

adb root && adb remount && adb shell setprop persist.radio.ctbk_log 5  && adb shell setprop persist.vendor.radio.ctbk_log 5   &&  adb shell setprop log.tag.QCSDK D && adb reboot
adb root && adb disable-verity && adb reboot bootloader
fastboot oem config cmdl androidboot.selinux=permissive 
fastboot reboot 
adb logcat | grep -e "SARCTRL" -e "MDMCTBK" -e "QCSDK" -e "setBtTxPower"


```

#### 导入 MtkSarControlService.apk

```

1.关闭 Selinux
fastboot oem config cmdl androidboot.selinux=permissive         【关闭SeLinux】   

2.导入apk 文件
adb root && adb remount &&  adb push  ./MtkSarControlService.apk /system/priv-app/MtkSarControlService/ && adb reboot 
 
3. 进入 Bt TestMode 开关WIFI  
查看 adb logcat |  grep EmHidlService          // 有打印 

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


### MTK 检查txpowerctrl文件
 
 
```
// 请反馈如重启进入Settings页面下命令的输出值  MTK产品 输出内容

adb root && adb remount  && adb shell cmd wifi start-softap Wifi_2412 open -b 2  -f 2412 > NUL 2>&1  &&   adb shell  "cd /data/vendor/wifi/hostapd/ctrl/ ; ls -ld $PWD/* "  &&  adb shell "find /sys/module  -iname '*sar_*'"  && adb shell setprop persist.radio.ctbk_log 5  && adb shell setprop persist.vendor.radio.ctbk_log 5   &&  adb shell setprop log.tag.QCSDK D  && adb shell "getprop | grep -e ro.vendor.hw.device -e ro.boot.radio   -e ro.vendor.hw.radio -e ro.carrier ;cd /vendor/firmware/ ; ls -ld $PWD/*  | grep txpowerctrl    ; cd /system/etc/motorola/mdmctbk ; ls -ld $PWD/*    | grep ctbk_cfg ; cd /vendor/etc/motorola/mdmctbk ;  ls -ld $PWD/*    "  &&   adb root &&   adb reboot && adb wait-for-device  &&  adb shell " logcat | grep -e GNSSOPMode -e ctbk_cfg -e txpowerctrl -e sar_mhs -e sar_sta -e wifi/hostapd/ctrl  "


```

```

// txpowerctrl.cfg  如果不包含所有场景 那么 radioid 对应的SKU 就得配置很多文件
// 请反馈如重启进入Settings页面下命令的输出值
adb root && adb remount  && adb shell cmd wifi start-softap Wifi_2412 open -b 2  -f 2412 > NUL 2>&1  &&   adb shell  "cd /data/vendor/wifi/hostapd/ctrl/ ; ls -ld $PWD/* "  &&  adb shell "find /sys/module  -iname '*sar_*'"  && adb shell setprop persist.radio.ctbk_log 5  && adb shell setprop persist.vendor.radio.ctbk_log 5   &&  adb shell setprop log.tag.QCSDK D  && adb shell "getprop | grep -e ro.vendor.hw.device -e ro.boot.radio   -e ro.vendor.hw.radio -e ro.carrier ;cd /vendor/firmware/ ; ls -ld $PWD/*  | grep txpowerctrl    ; cd /system/etc/motorola/mdmctbk ; ls -ld $PWD/*    | grep ctbk_cfg ; cd /vendor/etc/motorola/mdmctbk ;  ls -ld $PWD/*    "  &&   adb root &&   adb reboot && adb wait-for-device  &&  adb shell " logcat | grep -e GNSSOPMode -e ctbk_cfg -e txpowerctrl -e sar_mhs -e sar_sta -e wifi/hostapd/ctrl  "

Verity is already disabled
Remounted /system as RW
Remounted /system_ext as RW
Remounted /vendor as RW
Remounted /product as RW
Remounted /vendor_dlkm as RW
Remounted /system_dlkm as RW
Remount succeeded
srwxrwx--- 1 wifi wifi 0 2025-04-24 23:07 /data/vendor/wifi/hostapd/ctrl/wlan0    【热点控制的socket   SAR_WIFI_NO_DBS_SOCKET 有关 】
/sys/module/qca_cld3_qca6750/parameters/sar_sta         【监控wifi打开的结点】
/sys/module/qca_cld3_qca6750/parameters/sar_mhs         【监控热点打开的结点】
[ro.boot.radio]: [NA]                
[ro.carrier]: [unknown]
[ro.vendor.hw.device]: [skyline]                 //  txpowerctrl_【ro.boot.radio】.cfg   -->  txpowerctrl_NA.cfg
[ro.vendor.hw.radio]: [NA]                    // 【ro.carrier】_【ro.vendor.hw.radio】_ctbk_cfg.xml   -->  na_ctbk_cfg.xml
-rw-r--r-- 1 root root    3200 2009-01-01 08:00 /vendor/firmware/txpowerctrl.cfg
-rw-r--r-- 1 root root   14163 2009-01-01 08:00 /vendor/firmware/txpowerctrl_DOCOMO.cfg
-rw-r--r-- 1 root root   14163 2009-01-01 08:00 /vendor/firmware/txpowerctrl_JP.cfg
-rw-r--r-- 1 root root   14163 2009-01-01 08:00 /vendor/firmware/txpowerctrl_ROW.cfg
-rw-r--r-- 1 root root   14163 2009-01-01 08:00 /vendor/firmware/txpowerctrl_na.cfg
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/ctbk_cfg.xml
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/docomo_ctbk_cfg.xml
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/jp_ctbk_cfg.xml
-rw-r--r-- 1 root root 17278 2009-01-01 08:00 /system/etc/skyline/mdmctbk/na_ctbk_cfg.xml
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/row_ctbk_cfg.xml
/system/bin/sh: cd: /vendor/etc/skyline/mdmctbk: No such file or directory
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/ctbk_cfg.xml
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/docomo_ctbk_cfg.xml
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/jp_ctbk_cfg.xml
-rw-r--r-- 1 root root 17278 2009-01-01 08:00 /system/etc/skyline/mdmctbk/na_ctbk_cfg.xml
-rw-r--r-- 1 root root 17194 2009-01-01 08:00 /system/etc/skyline/mdmctbk/row_ctbk_cfg.xml
04-18 12:45:11.435  1496  1496 D gps_controlller: get_chip_gnss_op_mode: get_chip_gnss_op_mode] GNSSOPMode: 0xa
04-18 12:45:11.435  1496  1496 D gps_controlller: get_chip_gnss_op_mode: get_chip_gnss_op_mode]: mnld GNSSOPMode: 0xa  ro.vendor.hw.device aito ro.product.is_prc true ro.carrier unknown ro.vendor.hw.radio:NA  persist.vendor.radio.gps_test_mode:
04-18 12:45:14.315     0     0 I [T400733] sub_wifi_thrd: [name:wlan_drv_gen4m_6878&][wlan][733]get_moto_config_file_name:(RLM ERROR) [MOTO]Use moto config file name: txpowerctrl_na.cfg
04-18 12:45:14.316   733   733 I sub_wifi_thrd: [name:wlan_drv_gen4m_6878&][wlan][733]kalRequestFirmware:(INIT INFO) kalRequestFirmware(): txpowerctrl_na.cfg OK
04-18 12:45:19.906     0     0 I [T600735] mtk_wland_threa: [name:wlan_drv_gen4m_6878&][wlan][735]get_moto_config_file_name:(RLM ERROR) [MOTO]Use moto config file name: txpowerctrl_na.cfg
04-18 12:45:19.907     0     0 I [T600735] mtk_wland_threa: [name:wlan_drv_gen4m_6878&][wlan][735]kalRequestFirmware:(INIT INFO) kalRequestFirmware(): txpowerctrl_na.cfg OK
04-18 12:45:31.900  2695  2695 D SARCTRL : targetFilepath: /system/etc/skyline/mdmctbk/na_ctbk_cfg.xml

```
 <img src="/public/zimage/qocm_issue/mtk_txpower_config_tip.jpg"/>




### Qcom_产品打印视图


```
adb root && adb remount  && adb shell cmd wifi start-softap Wifi_2412 open -b 2  -f 2412 > NUL 2>&1  &&   adb shell  "cd /data/vendor/wifi/hostapd/ctrl/ ; ls -ld $PWD/* "  &&  adb shell "find /sys/module  -iname '*sar_*'"  && adb shell setprop persist.radio.ctbk_log 5  && adb shell setprop persist.vendor.radio.ctbk_log 5   &&  adb shell setprop log.tag.QCSDK D  && adb shell "getprop | grep -e ro.vendor.hw.device -e ro.boot.radio   -e ro.vendor.hw.radio -e ro.carrier ;cd /vendor/firmware/ ; ls -ld $PWD/*  | grep txpowerctrl    ; cd /system/etc/motorola/mdmctbk ; ls -ld $PWD/*    | grep ctbk_cfg ; cd /vendor/etc/motorola/mdmctbk ;  ls -ld $PWD/*    "  &&   adb root &&   adb reboot && adb wait-for-device  &&  adb shell " logcat | grep -e GNSSOPMode -e ctbk_cfg -e txpowerctrl -e sar_mhs -e sar_sta -e wifi/hostapd/ctrl  "


```

```
adb <command> > NUL 2>&1     【ADB静默执行】

```


```
// 请反馈如重启进入Settings页面下命令的输出值  Qcom产品 输出内容
restarting adbd as root
Verity is already disabled
Remounted /system as RW
Remounted /system_ext as RW
Remounted /product as RW
Remounted /vendor as RW
Remounted /vendor_dlkm as RW
Remounted /system_dlkm as RW
Remounted /vendor/firmware_mnt as RW
Remounted /vendor/dsp as RW
Remounted /vendor/bt_firmware as RW
Remounted /vendor/fsg as RW
Remount succeeded

srwxrwx--- 1 wifi wifi 0 2025-04-24 23:07 /data/vendor/wifi/hostapd/ctrl/wlan0    【热点控制的socket   SAR_WIFI_NO_DBS_SOCKET 有关 】
/sys/module/qca_cld3_qca6750/parameters/sar_sta         【监控wifi打开的结点】
/sys/module/qca_cld3_qca6750/parameters/sar_mhs         【监控热点打开的结点】
[ro.boot.radio]: [NA]
[ro.carrier]: [unknown]
[ro.vendor.hw.device]: [xxxx]
[ro.vendor.hw.radio]: [NA]
/system/bin/sh: cd: /system/etc/skyline/mdmctbk: No such file or directory
-rw-r--r-- 1 root root 12528 2009-01-01 08:00 /vendor/etc/skyline/mdmctbk/ctbk_cfg.xml

04-17 18:28:54.341  2401  2614 I MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:/vendor/etc/skyline/mdmctbk/unknown_na_ctbk_cfg.xml not found, ignore carrier!
04-17 18:28:54.341  2401  2614 E MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:Error: unable to parse file /vendor/etc/skyline/mdmctbk/na_ctbk_cfg.xml
04-17 18:28:54.341  2401  2614 E MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:try default cfg file /vendor/etc/skyline/mdmctbk/ctbk_cfg.xml
04-17 18:28:54.343  2401  2614 I MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:success to parse default xml file /vendor/etc/skyline/mdmctbk/ctbk_cfg.xml
04-17 18:28:54.344  2401  2614 D MDMCTBK : [0] - addObserver:MdmCutbackFileObserver::addObserver(): path:/sys/module/qca_cld3_qca6750/parameters/sar_sta
04-17 18:28:54.344  2401  2614 D MDMCTBK : [0] - fwpathStateChanged:fwpathStateChanged, path:/sys/module/qca_cld3_qca6750/parameters/sar_sta mask:7
04-17 18:28:54.344  2401  2614 I MDMCTBK : [0] - process_netlink_event_wifi_netd:NetlinkHandler, path:/sys/module/qca_cld3_qca6750/parameters/sar_sta, wifi_file_content is
04-17 18:28:55.345  2401  2614 D MDMCTBK : [0] - addObserver:MdmCutbackFileObserver::addObserver(): path:/sys/module/qca_cld3_qca6750/parameters/sar_mhs
04-17 18:28:55.345  2401  2614 D MDMCTBK : [0] - fwpathStateChanged:fwpathStateChanged, path:/sys/module/qca_cld3_qca6750/parameters/sar_mhs mask:7
04-17 18:28:55.345  2401  2614 I MDMCTBK : [0] - process_netlink_event_wifi_netd:NetlinkHandler, path:/sys/module/qca_cld3_qca6750/parameters/sar_mhs, wifi_file_content is
04-17 18:28:57.819  2401  3061 I MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:/vendor/etc/skyline/mdmctbk/unknown_na_ctbk_cfg.xml not found, ignore carrier!
04-17 18:28:57.819  2401  3061 E MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:Error: unable to parse file /vendor/etc/skyline/mdmctbk/na_ctbk_cfg.xml
04-17 18:28:57.819  2401  3061 E MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:try default cfg file /vendor/etc/skyline/mdmctbk/ctbk_cfg.xml
04-17 18:28:57.820  2401  3061 I MDMCTBK : [0] - MdmCutbackConfigOpenXmlFile:success to parse default xml file /vendor/etc/skyline/mdmctbk/ctbk_cfg.xml
04-17 18:28:59.430  2401  2648 I MDMCTBK : [0] - onEvent:NetlinkHandler, add sar_sta file observer

```

<img src="/public/zimage/qocm_issue/qcom_txpower_config_tip.jpg"/>



### MTK_txpowerctrl.cfg文件解析(场景值)

```
JPScenario;1;2;1;前缀
#scenario_name;scenario_sub_index;applied_way;operation;

JPScenario; === #scenario_name === 场景名字
1;          === #scenario_sub_index === 场景副版本号
2;          === #applied_way ===  使用方式 (1.only_wifi_on 2.only_ioctl) == 1: will be always invoked when wifi on.  2: only be invoked by ioctl(manually)
1;          === #operation ===  操作方式(1.正常值  2.偏移值(取负))   === 1:power level 2:power offset


JPScenario;1;2;1;[2G4,HE,20,20,20,24,24,24,35,35,35,39,39,39,33,33,33,48,48,48,48,48,48][5G,HE,16,16,16,22,22,22,28,28,28,35,35,35,38,38,38,38,38,38,48,48,48][6G,HE6G,12,12,12,18,18,18,25,25,25,28,28,28,28,28,28,28,28,28,28,28,28]

示例:
JPScenario;1;2;1;                                                             【第一部分】        【第二部分】   【第三部分】
[5GBAND1,44]                                                              --> 【5GBAND1           ,  44(one_tx值),   0 空           】
[2G4,HE,20,20,20,24,24,24,35,35,35,39,39,39,33,33,33,48,48,48,48,48,48]   --> 【2G4               ,  HE          ,   21 (tx列表长度)】
[5G,HE,16,16,16,22,22,22,28,28,28,35,35,35,38,38,38,38,38,38,48,48,48]    --> 【5G                ,  HE          ,   21 (tx列表长度)】
[6G,HE6G,12,12,12,18,18,18,25,25,25,28,28,28,28,28,28,28,28,28,28,28,28]  --> 【6G                ,  HE6G        ,   21 (tx列表长度)】
[11,LEGACY,43,32,32,48,48,48,48,48,48]                                    --> 【11(2462MHz)       ,  HE6G        ,   21 (tx列表长度)】
[1-13,30,48,48,48,48,48,48,24,24]                                         --> 【1-13(2412-2462MHz),  空          ,   9  (tx列表长度)】
[2G4,LEGACY,44,40,40,34,34,48,48,48,48]                                   --> 【2G4               ,  LEGACY      ,   9  (tx列表长度)】
[1,HE,48,48,48,48,48,48,48,48,48,31,31,31,48,48,48,48,48,48,48,48,48]     --> 【1(2412MHz)        ,  HE          ,   21 (tx列表长度)】
[5GBAND1,LEGACY,43,36,36,36,36,36,36,48,48]                               --> 【5GBAND1           ,  LEGACY      ,   9  (tx列表长度)】
[5GBAND4,HE,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32]-> 【5GBAND4           ,  HE          ,   21 (tx列表长度)】
[36,HE,48,48,48,48,48,48,48,48,48,34,34,34,48,48,48,48,48,48,48,48,48]    --> 【36(5180MHz)       ,  HE          ,   21 (tx列表长度)】
[64,HE,48,48,48,48,48,48,48,48,48,37,37,37,48,48,48,48,48,48,48,48,48]    --> 【64(5320HHz)       ,  HE          ,   21 (tx列表长度)】
[100,HE,48,48,48,48,48,48,48,48,48,37,37,37,48,48,48,48,48,48,48,48,48]   --> 【100(5500HHz)      ,  HE          ,   21 (tx列表长度)】
[140,HE,48,48,48,48,48,48,48,48,48,37,37,37,48,48,48,48,48,48,48,48,48]   --> 【140(5700HHz)      ,  HE          ,   21 (tx列表长度)】
[6G,HE6G,0,0,0,5,5,5,11,11,11,17,17,17,18,18,18,18,34,34,40,40,40]        --> 【6G                ,  HE6G        ,   21 (tx列表长度)】
                                                                          --> 【??(未找到实例)    ,  CCKL        ,   9  (tx列表长度)】
                                                                          --> 【??(未找到实例)    ,  CCKH        ,   9  (tx列表长度)】
                                                                          --> 【??(未找到实例)    ,  OFDML       ,   9  (tx列表长度)】
                                                                          --> 【??(未找到实例)    ,  OFDMH       ,   9  (tx列表长度)】
                                                                          --> 【??(未找到实例)    ,  CCK         ,   9  (tx列表长度)】
                                                                          --> 【??(未找到实例)    ,  HT          ,   9  (tx列表长度)】
                                                                          --> 【??(未找到实例)    ,  RU          ,   21 (tx列表长度)】
                                                                          --> 【2G??(未找到实例)  ,  EHT         ,   36 (tx列表长度)】
                                                                          --> 【5G??(未找到实例)  ,  EHT         ,   36 (tx列表长度)】
                                                                          --> 【6G_??(未找到实例) ,  EHT         ,   48 (tx列表长度)】



[2G4【第一部分】,CCK【第二部分】,38,48,48,48,48,48,48,48,48【第三部分】]

[ 
【第一部分】(频段标识)                     (PWR_CTRL_CHNL_TYPE_KEY)  ,  
【第二部分】(速率标识||调制方式||唯一tx值) (PWR_CFG_RATE_TAG||CCK||OFDM),    
【第三部分】(具体的tx值列表)               (int tx值)
]



频段索引标识:
Mode[G] Channels:   Mode[A] Channels:        
 1 = 2412 MHz        36 = 5180 MHz           
 2 = 2417 MHz        40 = 5200 MHz           
 3 = 2422 MHz        44 = 5220 MHz           
 4 = 2427 MHz        48 = 5240 MHz           
 5 = 2432 MHz        52 = 5260 MHz (DFS)     
 6 = 2437 MHz        56 = 5280 MHz (DFS)     
 7 = 2442 MHz        60 = 5300 MHz (DFS)     
 8 = 2447 MHz        64 = 5320 MHz (DFS)     
 9 = 2452 MHz       100 = 5500 MHz (DFS)     
10 = 2457 MHz       104 = 5520 MHz (DFS)     
11 = 2462 MHz       108 = 5540 MHz (DFS)     
12 = 2467 MHz       112 = 5560 MHz (DFS)     
13 = 2472 MHz       116 = 5580 MHz (DFS)     
                    120 = 5600 MHz (DFS)     
                    124 = 5620 MHz (DFS)     
                    128 = 5640 MHz (DFS)     
                    132 = 5660 MHz (DFS)     
                    136 = 5680 MHz (DFS)     
                    140 = 5700 MHz (DFS)     
                    144 = 5720 MHz (DFS)     
                    149 = 5745 MHz           
                    153 = 5765 MHz           
                    157 = 5785 MHz           
                    161 = 5805 MHz           
                    165 = 5825 MHz           


【第一部分】                         【第二部分】                    【第三部分】
1_1:  "ALL"                           2_1:  "LEGACY"                  3_1: 第三部分的count数量由1,2部分决定,程序判断不等于指定长度则解析失败
1_2:  "2G4"                           2_2:  "HE"                      3_2: 无
1_3:  "5G"                            2_3:  "AX160"
1_4:  "BANDEDGE2G4"                   2_4:  "EHT"
1_5:  "BANDEDGE5G"                    2_5:  "LEGACY6G"
1_6:  "5GBAND1"                       2_6:  "HE6G"
1_7:  "5GBAND2"                       2_7:  "EHT6G"
1_8:  "5GBAND3"                       2_8:  "only_one_tx值"_无第三部分
1_9:  "5GBAND4"                       2_9:  "tx列表"_第一部分为频段范围
1_10: "6G"                            
1_11: "6GBAND1"                      
1_12: "6GBAND2"                      
1_13: "6GBAND3"                      
1_14: "6GBAND4"                      
1_15: 频段索引标识(1=2412 MHz)
1_16  频段范围(1-13)(2412-)

```




#### MTK_PwrMode6GSupport功耗配置解析

```
vlp.cfg 文件解析

PwrMode6GSupport='J','P'-0,1,1*0,1,1*0,1,1*0,1,1  -->    (;)i=1      (*)j=4    (,)k=3
PwrMode6GSupport='J','P'-1,2,3*4,5,6*7,8,9*10,11,12  --> (;)i=1      (*)j=4    (,)k=3

依次切割 ;-*, 


1. ;作为 i 循环 

2.
切割-的第一部分区分国家码 aucCountryCode
切割-的第二部分区继续进行*的切割

3.
*作为 j 循环 
,作为每个k循环(0..j)的赋值值 k 是通过动态切割,的数量获取

i=1 
j=4
k=2 --> (j==0 i==0) 
k=3 --> (j==1 i==0)  (j==2 i==0) (j==3 i==0)


      ;(i)      *(j)                    ,(k)                        (k_value)
array[i].rSubBand[j].fgPwrMode6GSupport[sub_sub_count] = x_atoi(num_token);
array[0]..aucCountryCode[0] = 'J'
array[0]..aucCountryCode[1] = 'P'
array[0].rSubBand[0].fgPwrMode6GSupport[0] = 1
array[0].rSubBand[0].fgPwrMode6GSupport[1] = 2
array[0].rSubBand[0].fgPwrMode6GSupport[2] = 3
array[0].rSubBand[1].fgPwrMode6GSupport[0] = 4
array[0].rSubBand[1].fgPwrMode6GSupport[1] = 5
array[0].rSubBand[1].fgPwrMode6GSupport[2] = 6
array[0].rSubBand[2].fgPwrMode6GSupport[0] = 7
array[0].rSubBand[2].fgPwrMode6GSupport[1] = 8
array[0].rSubBand[2].fgPwrMode6GSupport[2] = 9
array[0].rSubBand[3].fgPwrMode6GSupport[0] = 10
array[0].rSubBand[3].fgPwrMode6GSupport[1] = 11
array[0].rSubBand[3].fgPwrMode6GSupport[2] = 12

```






#### MTK_VLP_LPI功耗配置

```
VLP ===  Very Low Power   === WIFI 的一个工作模式 , VLP设备适用于室内环境，但其功率更低，通常不超过25mW
LPI ===  Low Power Indoor === WIFI 的一个工作模式 , 其功率较低，通常不超过200毫瓦 200mw
vlp.cfg 文件解析

VLP='J','P'-63,63,63,63,63,14,14,14,14-0
LPI='J','P'-63,63,63,63,63,32,32,32,32-0

'J','P'-63,63,63,63,63,32,32,32,32-0
'J','P'-10,11,12,13,14,15,16,17,18-0

;作为 i 循环 
-作为 j 循环
,作为第k次
i=1
j=3
k=2(j=0)  k=9(j=1) k=1(j=3)


k=2(j=0 i=0) 
array[i].aucCountryCode[0] 
array[i].aucCountryCode[1] 


     i=0                    k.length=9(j=1 i=0)            k_value
array[i].aucPwrLimitSubBand[k] = xxx_atoi(num_token);
array[i].ucPwrUnit = xxx_atoi(subparts[2]);  // -分隔的索引下标为3的那个值0


array[0].aucCountryCode[0] = "J"
array[0].aucCountryCode[1] = "P"
array[0].aucPwrLimitSubBand[0] = 10;
array[0].aucPwrLimitSubBand[1] = 11;
array[0].aucPwrLimitSubBand[2] = 12;
array[0].aucPwrLimitSubBand[3] = 13;
array[0].aucPwrLimitSubBand[4] = 14;
array[0].aucPwrLimitSubBand[5] = 15;
array[0].aucPwrLimitSubBand[6] = 16;
array[0].aucPwrLimitSubBand[7] = 17;
array[0].aucPwrLimitSubBand[8] = 18;
array[0].ucPwrUnit =  0

```



### MTK抓取WIFI ICS硬件Log 

```


MTKlog：
[system]  MTK-logger APP-> enable “mobile Log”
[Wi-Fi]   MTK-logger APP-> enable “Conn-sys Log” 
[Wi-Fi]   MTK-logger APP-> enable “Netlog Log”
[Wi-Fi]   MTK-logger APP log lever->  Set "Driver" as “more” Level
[Wi-Fi]   MTK-logger APP log lever->  Set "Firmware" as “more” Level
[Wi-Fi]   MTK-logger APP log lever->  Set "ICSFW LOG Level" as MAC_ICS       【ICS Log】



```


### MTK抓取GPS_nmea_log

```


MTKlog：

adb  pull /data/debuglogger/                         【 MTK_Log 路径】
adb  pull /data/debuglogger/connsyslog/gpshost       【nmea log 路径】


adb shell " cd /data/debuglogger/connsyslog/gpshost ; ls -l"



adb shell " cd /data/debuglogger/connsyslog/gpshost ; ls -l"
total 7
drwxrwx--- 2 shell log 3452 2025-03-16 06:28 CSLog_2025_0316_062709       【 nmea log 文件路径 】
-rwxrwx--- 1 shell log  793 2025-03-16 06:27 file_tree.txt




```

```


1. 拨号盘输入  *#*#3646633#*#*    打开ENgineerMode
 
2.切换到 Log and Debugging 点击 DebugLoggerUI  
 
3. 在 DebugLoggerUI 停止当前Log录制 , 清除当前已经存在的Log
 
4. 点击 DebugLoggerUI 右上边按钮进入设置界面  MobileLog ModemLog NetworkLog ConsysLog 都打开
   Log Level 选中最大
   
5. 重新启动 Log录制 (DebugLoggerUI主界面的开关按钮)
 
6. 进行复测操作
 
7.回到DebugLoggerUI  停止录制 并 导出当前界面显示的文件路径的Log 并提供该Log
  类似于 [  adb  pull   /data/debuglogger/ ]
  

================== 抓取 NMEA LOG  CSLog_xxx Log 步骤 ==================
Step 1:  ##3646633##    Engineer Mode will be open

Step 2: Log and Debugging  -> DebugLoggerUI -> Settings ->  ConnsysLog (Enable)  -> GPS Log(Host Log) Enable 

Step 3: stop previous recording and delete previous log 

Step 4: restart recoarding to collect MTK Log . 




1. input *#*#3646633#*#* in dial panel to start MTK Engineer Mode
2. enter Engineer Mode - Location - Location Based Service
3. in LocationEM2, lab should be able to select MSA or MSB, 
   select LPP or RRLP and also configure other parameters ( e.g, SUPL Host, SUPL Port, SUPL Version, ... )

```


<img src="/public/zimage/qocm_issue/gps_nmea.jpg"/>






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

--------------
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



### Qcom_更新Firmware固件提交的文件

```
/nonhlos/WLAN.MSL.3.0.2/wlan_proc/【Git目录】
/nonhlos/WLAN.MSL.3.0.2/wlan_proc/build/manifest.xml
/nonhlos/WLAN.MSL.3.0.2/wlan_proc/build/ms/Data.msc
/nonhlos/WLAN.MSL.3.0.2/wlan_proc/build/ms/bin/QCALAMSLNETRANI/wpss.mbn
/nonhlos/WLAN.MSL.3.0.2/wlan_proc/build/ms/bin/QCALAMSLNETRANI/wpss.qdb
/nonhlos/WLAN.MSL.3.0.2/wlan_proc/build/ms/orig_WPSS_PROC_IMG_QCALAMSLNETRANIQ_WLAN_PD.elf

```



### Qcom产品symbols文件

```


[3_1]AOSP_Vendor/release/mmi_kernel_platform_debug_files.tar.gz    【360MB】
[3_2]AOSP_Vendor/release/nonhlos_symbols.tar.gz                    【70MB】
[3_3]AOSP_Vendor/release/symbols.tar.gz                            【2.5GB】


```



```


adb pull  /vendor/firmware_mnt/image/qca6755    // 导出当前版本的固件文件



// 新导入其他Fireware目录下splitbins下的wpss文件和   Data.msc +  wpss* 文件  
// 之后查看 adb root && adb remount && adb shell "cat /d/icnss/stats"  固件版本是否变化 
adb  push ./     /vendor/firmware_mnt/image/qca6755/              





/vendor/firmware_mnt/image/qca6755 # ls -l
total 8784
-rw-rw-rw- 1 23152 system 1160789 2025-03-04 09:59 Data.msc
-rw-rw-rw- 1 23152 system    1813 2025-03-04 09:59 qdss_trace_config.cfg
-rw-rw-rw- 1 23152 system   24278 2025-03-04 09:59 regdb.bin
-rw-rw-rw- 1 23152 system     468 2025-03-04 09:59 wpss.b00
-rw-rw-rw- 1 23152 system    6440 2025-03-04 09:59 wpss.b01
-rw-rw-rw- 1 23152 system  461561 2025-03-04 09:59 wpss.b02
-rw-rw-rw- 1 23152 system    1104 2025-03-04 09:59 wpss.b03
-rw-rw-rw- 1 23152 system     256 2025-03-04 09:59 wpss.b04
-rw-rw-rw- 1 23152 system    4096 2025-03-04 09:59 wpss.b05
-rw-rw-rw- 1 23152 system 6057934 2025-03-04 09:59 wpss.b06
-rw-rw-rw- 1 23152 system  118588 2025-03-04 09:59 wpss.b07
-rw-rw-rw- 1 23152 system  275644 2025-03-04 09:59 wpss.b08
-rw-rw-rw- 1 23152 system  344560 2025-03-04 09:59 wpss.b09
-rw-rw-rw- 1 23152 system  909694 2025-03-04 09:59 wpss.b10
-rw-rw-rw- 1 23152 system    4376 2025-03-04 09:59 wpss.b12
-rw-rw-rw- 1 23152 system    4844 2025-03-04 09:59 wpss.mdt
-rw-rw-rw- 1 23152 system     517 2025-03-04 09:59 wpss.qdb


```



####  Qcom产品symbols文件_mmi_kernel_platform_debug_files.tar.gz

```


Dir[ 4_1    ]    AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules
Dir[ 4_2    ]    AOSP_Vendor/release/mmi_kernel_platform_debug_files/qcom-wifi
Dir[ 4_3    ]    AOSP_Vendor/release/mmi_kernel_platform_debug_files/vmlinux
Dir[ 4_4    ]    AOSP_Vendor/release/mmi_kernel_platform_debug_files


AOSP_Vendor/release/mmi_kernel_platform_debug_files/qcom-wifi/qca_cld3_qca6750.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/vmlinux/vmlinux
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_drm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/camera.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ipam.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_kgsl.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cfg80211.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_video.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sched-walt.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_core.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/kheaders.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/clk-qcom.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/goodix_brl_mmi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_shs.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/aw882xx_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/icnss2.ko          【 symbols的一个关键so】
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ufs-qcom.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bluetooth.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gsim.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mhi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/arm_smmu.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_dma_heaps.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_offload.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qseecom_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-tmc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_wlan.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/frpc-adsprpc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pci-msm-drv.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dwc3-msm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/drm_display_helper.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_iommu_util.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sps_drv.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gunyah_loader.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nxp-nci.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wcd938x_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lpass_cdc_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/usb_f_gsi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_perf.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wcd937x_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcedev-mod_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/smcinvoke_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm-mmrm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/radio-i2c-rtc6226-qca.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/touchscreen_mmi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/minidump.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qrtr.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mem_buf_dev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-scm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-hv-haptics.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_tsens.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mbhc_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-cti.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wsa881x_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/goodix_fod_mmi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_geni_serial.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-dcvs.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wcd_core_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_lpm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/machine_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_mem.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_rm_drv.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_glink.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mmi_info.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cqhci.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_glink_charger.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lpass_cdc_rx_macro_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/slim-qcom-ngd-ctrl.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/tipc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_gpi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcrypto-msm_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/slimbus.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sdhci-msm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qce50_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wsa883x_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wcd9xxx_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bt_fm_slim.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_rpmh.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_aps.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/i2c-msm-geni.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-tpdm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/swr_ctrl_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/btpower.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sx937x_multi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/spi-msm-geni.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lpass_cdc_wsa2_macro_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lpass_cdc_wsa_macro_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sg.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lpass_cdc_va_macro_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_iommu_debug.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mem_buf.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/leds-qpnp-flash-v2.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_ctl.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rproc_qcom_common.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_battery_charger.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/con_dfpar.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lpass_cdc_tx_macro_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/usb_f_qdss.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mmrm_test_module.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_q6v5_pas.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qpnp-lcdb-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_perf_tether.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stm_core.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qmi_helpers.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mmi_charger.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qpnp-power-on.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/utags.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-spmi-wled.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hdcp_qseecom_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/si_core_module.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qnoc-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qnoc-ravelin.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_ipc_logging.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/snd-usb-audio-qmi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bwmon.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/swr_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/usb_f_cdev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gcc-sm4450.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bam_dma.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/swr_dmic_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qrtr-mhi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rbs_fod_mmi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dcc_v2.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_sysstats.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cdsprm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gdsc-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gcc-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/debug-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/phy-msm-snps-hs.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/smmu_proxy_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/memlat.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-replicator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/glink_pkt.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-csr.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pinctrl-msm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-pmu-lib.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/swr_haptics_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-funnel.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/socinfo.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_pm8008-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-cpufreq-hw.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-stm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pinctrl-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rpmh-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stm_nfc_i2c.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/f_fs_ipc_log.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ufshcd-crypto-qti.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/phy-msm-ssusb-qmp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/spmi-pmic-arb.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cnss_utils.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/camcc-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/secure_buffer.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/camcc-sm4450.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pinctrl-ravelin.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-tmc-sec.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_qmi_sensor_v2.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dispcc-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_performance.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rmnet_sch.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hall_pen.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/phy-qcom-ufs.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/usb_f_ccid.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/user_sysfs_private.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pwm-qti-lpg.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_wdt_core.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-tpda.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cpucp_log.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/leds-aw2016.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hdmi_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-i2c-pmic.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pinctrl-spmi-gpio.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-tgu.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dmesg_dumper.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dio8015.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gpucc-sm4450.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dispcc-sm4450.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-amoled-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/tz_log_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-spmi-adc5.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-qmi.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/phy-generic.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/thermal_pause.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/drm_dp_aux_bus.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gic_intr_routing.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pmic-glink-debug.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/icc-bcm-voter.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wlan_firmware_service.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/eud.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_smd.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_stats.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pinctrl_lpi_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-remote-etm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mhi_dev_uci.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/coresight-dummy.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/fsa4480-i2c.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/clk-rpmh.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bcl_pmic5.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ucsi_qti_glink.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gpucc-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/audpkt_ion_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_memshare.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nvmem_qfprom.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_sysmon.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/memory_dump_v2.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rtc-pm8xxx.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/audio_pkt_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_qmp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/debugcc-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/videocc-parrot.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_pmic_glink.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti-ocp-notifier.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/phy-qcom-ufs-qmp-v4-waipio.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sync_fence.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti-fixed-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qrtr-gunyah.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/phy-qcom-ufs-qrbtc-sdm845.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pinctrl-spmi-mpp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bm_adsp_ulog.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rdbg.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_va_minidump.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/debugcc-sm4450.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_ice.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sdpm_clk.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/proxy-consumer.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stub-regulator.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gpr_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/altmode-glink.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_panic_notifier.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qsee_ipc_irq_bridge.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/smp2p.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_qmi_cdev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/smp2p_sleepstate.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_battery_debug.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/icc-debug.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_aoss.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cnss_nl.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-spmi-adc-tm5.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/q6_notifier_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/xhci-sideband.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lvstest.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sys_pm_vx.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stub_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti-regmap-debugfs.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sysmon_subsystem_stats.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/smem.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/adsp_loader_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mmi_sys_temp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_ext_display.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_q6v5.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/spf_core_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mhi_dev_dtr.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_dma_iommu_mapping.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/icc-rpmh.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-pdc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/governor_msm_adreno_tz.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mmi_annotate.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mem_object.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wcd938x_slave_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_rm_booster.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_tlmm_vm_mem_access.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/sensors_class.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_cpuss_sleep_stats.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_cpufreq_cdev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/leds-qpnp-vibrator-ldo.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qrng_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pm8941-pwrkey.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/leds-qti-tri-led.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-spmi-temp-alarm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qpnp_adaptive_charge.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pmic-pon-log.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bcl_soc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hvc_gunyah.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mem_buf_msgq.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/charger-ulog-glink.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-ipcc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/thermal_config.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_glink_smem.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/tmecom-intf.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_ramdump.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cpu_hotplug.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_cpucp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-spmi-pmic.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti-glink-adc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/audio_prm_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-dload-mode.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qrtr-smd.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nfc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/snd_event_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/spmi-pmic-arb-debug.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_sharedmem.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/policy_engine.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/msm_show_resume_irq.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mac802154.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_cpu_vendor_hooks.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_devfreq_cdev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ieee802154.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/debug_symbol.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mmi_relay.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cmd-db.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/iommu-logger.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_hwspinlock.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mmi_stow.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ehset.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/phy-qcom-emu.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dcvs_fp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cnss_prealloc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/governor_gpubw_mon.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/clk-dummy.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mdt_loader.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_virt_wdt.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wcd937x_slave_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nvmem_qcom-spmi-sdam.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ddr_cdev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stm_p_ost.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_soc_wdt.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_userspace_cdev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/reboot-mode.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_ctrl.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qfprom-sys.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-pon.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rq_stats.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-reboot-reason.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qti_thermal_vendor_hooks.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/boot_stats.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hung_task_enh.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cpu_phys_log_map.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stm_p_basic.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stm_ftrace.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/stm_console.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/watchdogtest.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/panel_event_notifier.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_pil_info.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hci_uart.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_logbuf_boot_log.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/r8152.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rfcomm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pdr_interface.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_arm_drv.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom-vadc-common.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/clk_test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_msgq.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_dbl.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/9pnet.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ftdi_sio.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_irq_lend.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qseecom_proxy.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gh_mem_notifier.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/q6_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qcom_logbuf_vendor_hooks.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ipanetm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/qnoc-qos.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/heap_mem_ext_v01.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/q6_pdr_dlkm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/asix.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/macsec.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/l2tp_core.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/usbserial.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/usbnet.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/regmap-kunit.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/gzvm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/kunit.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/can-dev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cdc-acm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ppp_generic.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hidp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cdc_ncm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/8021q.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/kunit-test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ax88179_178a.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/hid-uclogic-test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/usbmon.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/virtio_console.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ptp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/iio-test-format.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/6lowpan.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/clk-gate_test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rfkill.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/btqca.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/zram.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/l2tp_ppp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/virtio_blk.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ieee802154_socket.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/soc-topology-test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/zsmalloc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/wwan.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/can-bcm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ieee802154_6lowpan.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/can.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/aqc111.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pps_core.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/virtio_pci.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/btbcm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/can-raw.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/slcan.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/can-gw.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/virtio_balloon.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cdc_ether.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/rtl8150.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/vmw_vsock_virtio_transport.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/9pnet_fd.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/btsdio.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cctrng.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/dev_addr_lists_test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pptp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/virtio_pci_modern_dev.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/kunit-example-test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/input_test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ppp_mppe.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ppp_deflate.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/mii.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/bsd_comp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/cdc_eem.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nhc_udp.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/slhc.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/vcpu_stall_detector.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/pppox.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/vcan.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/soc-utils-test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/fat_test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ptp_kvm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/r8153_ecm.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/open-dice.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/ext4-inode-test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/time_test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/lib_test.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/diag.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/regmap-raw-ram.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/regmap-ram.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nhc_mobility.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nhc_fragment.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nhc_routing.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nhc_hop.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nhc_ipv6.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/nhc_dest.ko
AOSP_Vendor/release/mmi_kernel_platform_debug_files/dist-modules/libarc4.ko


```



####  Qcom产品symbols文件_nonhlos_symbols.tar.gz


```


Dir[ 2_1    ]nonhlos_symbols/DEBUG
Dir[ 2_2    ]nonhlos_symbols

AOSP_Vendor/release/nonhlos_symbols/orig_WPSS_PROC_IMG_QCALAMSLNETRANIQ_WLAN_PD.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/AUDIO_netrani.adsp.prodQ.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/SENSOR_netrani.adsp.prodQ.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/ROOT_netrani.adsp.prodQ.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/XBLRamDump.dll
AOSP_Vendor/release/nonhlos_symbols/DEBUG/qsee.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/uefi.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/CHARGER_netrani.adsp.prodQ.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/DevPrgD.dll
AOSP_Vendor/release/nonhlos_symbols/DEBUG/UKERNEL_netrani.adsp.prodQ.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/AOP_AAAAANAZO.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/hypvm.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/hypvmperformance.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/mon.elf
AOSP_Vendor/release/nonhlos_symbols/DEBUG/AOP_AAAAANAZO_devcfg.elf
AOSP_Vendor/release/nonhlos_symbols/about.html         【 about.html 固件版本信息 】
AOSP_Vendor/release/nonhlos_symbols/Ver_Info.txt



```



####  Qcom产品symbols文件_symbols.tar.gz


```

symbols.tar.gz  手机本身路径下的文件路径


AllSize_[ 8.8GB ] [ 9058.0MB ]
目录文件总数:[   126   ]
实体文件总数:[  2646   ]
文件类型总数:[    54   ]


文件类型:.0               匹配文件个数:1              类型文件大小:    0.8MB      .0
文件类型:.0-service       匹配文件个数:2              类型文件大小:    1.9MB      .0-service
文件类型:.1-goodixservice  匹配文件个数:1              类型文件大小:    2.2MB      .1-goodixservice
文件类型:.aidl-service    匹配文件个数:1              类型文件大小:    0.5MB      .aidl-service
文件类型:.alarm-service   匹配文件个数:1              类型文件大小:    0.2MB      .alarm-service
文件类型:.allocator-service  匹配文件个数:1              类型文件大小:    0.7MB      .allocator-service
文件类型:.bluetooth@aidl-service-qti  匹配文件个数:1              类型文件大小:    0.3MB      .bluetooth@aidl-service-qti
文件类型:.c2-default-service-dax  匹配文件个数:1              类型文件大小:    0.2MB      .c2-default-service-dax
文件类型:.capabilityconfigstore-service  匹配文件个数:1              类型文件大小:    0.2MB      .capabilityconfigstore-service
文件类型:.clearkey        匹配文件个数:1              类型文件大小:    3.4MB      .clearkey
文件类型:.composer-service  匹配文件个数:1              类型文件大小:    8.6MB      .composer-service
文件类型:.debugutils-service  匹配文件个数:1              类型文件大小:    0.5MB      .debugutils-service
文件类型:.demura-service  匹配文件个数:1              类型文件大小:    0.3MB      .demura-service
文件类型:.erofs           匹配文件个数:6              类型文件大小:    2.3MB      .erofs
文件类型:.example         匹配文件个数:1              类型文件大小:    1.1MB      .example
文件类型:.example_recovery  匹配文件个数:1              类型文件大小:    0.3MB      .example_recovery
文件类型:.exfat           匹配文件个数:2              类型文件大小:    0.3MB      .exfat
文件类型:.f2fs            匹配文件个数:2              类型文件大小:    1.9MB      .f2fs
文件类型:.face-isv        匹配文件个数:1              类型文件大小:    2.7MB      .face-isv
文件类型:.fdr-service     匹配文件个数:1              类型文件大小:    0.2MB      .fdr-service
文件类型:.fingerprint-service-rbs  匹配文件个数:1              类型文件大小:    1.7MB      .fingerprint-service-rbs
文件类型:.gnss-aidl-service-qti  匹配文件个数:1              类型文件大小:    0.3MB      .gnss-aidl-service-qti
文件类型:.health-service  匹配文件个数:1              类型文件大小:    0.2MB      .health-service
文件类型:.mc              匹配文件个数:1              类型文件大小:    9.9MB      .mc
文件类型:.memhal-service  匹配文件个数:1              类型文件大小:    0.2MB      .memhal-service
文件类型:.memtrack-service  匹配文件个数:1              类型文件大小:    0.3MB      .memtrack-service
文件类型:.multihal        匹配文件个数:1              类型文件大小:    1.7MB      .multihal
文件类型:.nfc-service-st  匹配文件个数:1              类型文件大小:    0.4MB      .nfc-service-st
文件类型:.oat             匹配文件个数:27             类型文件大小:   38.1MB      .oat
文件类型:.panel-service   匹配文件个数:1              类型文件大小:    0.3MB      .panel-service
文件类型:.perf2-hal-service  匹配文件个数:1              类型文件大小:    1.0MB      .perf2-hal-service
文件类型:.power-service   匹配文件个数:1              类型文件大小:    0.7MB      .power-service
文件类型:.provider-service_64  匹配文件个数:1              类型文件大小:    0.3MB      .provider-service_64
文件类型:.qspa-service    匹配文件个数:1              类型文件大小:    1.1MB      .qspa-service
文件类型:.qti             匹配文件个数:6              类型文件大小:    8.0MB      .qti
文件类型:.qti_recovery    匹配文件个数:1              类型文件大小:    1.1MB      .qti_recovery
文件类型:.recovery        匹配文件个数:1              类型文件大小:    0.3MB      .recovery
文件类型:.secureprocessor  匹配文件个数:1              类型文件大小:    0.6MB      .secureprocessor
文件类型:.sensorext-service  匹配文件个数:1              类型文件大小:    1.6MB      .sensorext-service
文件类型:.sensorscalibrate-service  匹配文件个数:1              类型文件大小:    0.2MB      .sensorscalibrate-service
文件类型:.service         匹配文件个数:3              类型文件大小:    0.8MB      .service
文件类型:.servicetrackeraidl-service  匹配文件个数:1              类型文件大小:    0.2MB      .servicetrackeraidl-service
文件类型:.sh              匹配文件个数:1              类型文件大小:    0.0MB      .sh
文件类型:.so              匹配文件个数:2130           类型文件大小: 7070.6MB      .so
文件类型:.soter-provision  匹配文件个数:1              类型文件大小:    0.2MB      .soter-provision
文件类型:.storage-service  匹配文件个数:1              类型文件大小:    1.3MB      .storage-service
文件类型:.stylus-service  匹配文件个数:1              类型文件大小:    0.2MB      .stylus-service
文件类型:.suspend-service  匹配文件个数:1              类型文件大小:    3.3MB      .suspend-service
文件类型:.syshealthmon-service  匹配文件个数:1              类型文件大小:    0.3MB      .syshealthmon-service
文件类型:.touch-service   匹配文件个数:1              类型文件大小:    0.4MB      .touch-service
文件类型:.txt             匹配文件个数:1              类型文件大小:    0.0MB      .txt
文件类型:.wifi-service    匹配文件个数:1              类型文件大小:   11.1MB      .wifi-service
文件类型:.wlc-service     匹配文件个数:1              类型文件大小:    0.2MB      .wlc-service
文件类型:unknow           匹配文件个数:423            类型文件大小: 1873.1MB      unknow

AllSize_[ 8.8GB ] [ 9058.0MB ]
目录文件总数:[   126   ]
实体文件总数:[  2646   ]
文件类型总数:[    54   ]



Dir[ 126_1    ] symbols\system\lib64
Dir[ 126_2    ] symbols\vendor\lib64
Dir[ 126_3    ] symbols\system\bin
Dir[ 126_4    ] symbols\vendor\lib64\camera\components
Dir[ 126_5    ] symbols\vendor\lib64\hw
Dir[ 126_6    ] symbols\vendor\bin
Dir[ 126_7    ] symbols\apex\com.android.ondevicepersonalization\lib64
Dir[ 126_8    ] symbols\apex\com.android.media.swcodec\lib64
Dir[ 126_9    ] symbols\apex\com.android.art\lib64
Dir[ 126_10   ] symbols\apex\com.android.btservices\lib64
Dir[ 126_11   ] symbols\recovery\root\system\bin
Dir[ 126_12   ] symbols\vendor\lib64\soundfx
Dir[ 126_13   ] symbols\recovery\root\system\lib64
Dir[ 126_14   ] symbols\apex\com.android.extservices\priv-app\ExtServices-sminus@VVB35V\lib\arm64-v8a
Dir[ 126_15   ] symbols\apex\com.android.neuralnetworks\lib64
Dir[ 126_16   ] symbols\vendor\bin\hw
Dir[ 126_17   ] symbols\apex\com.android.tethering\lib64
Dir[ 126_18   ] symbols\apex\com.android.adservices\lib64
Dir[ 126_19   ] symbols\apex\com.android.extservices\lib64
Dir[ 126_20   ] symbols\apex\com.android.mediaprovider\lib64
Dir[ 126_21   ] symbols\apex\com.android.art\bin
Dir[ 126_22   ] symbols\apex\com.android.resolv\lib64
Dir[ 126_23   ] symbols\apex\com.android.appsearch\lib64
Dir[ 126_24   ] symbols\apex\com.android.uwb\lib64
Dir[ 126_25   ] symbols\ramdisk
Dir[ 126_26   ] symbols\apex\com.android.runtime\bin
Dir[ 126_27   ] symbols\data\nativetest64\VtsHalAudioCoreTargetTest
Dir[ 126_28   ] symbols\apex\com.android.os.statsd\bin
Dir[ 126_29   ] symbols\system\framework\arm64
Dir[ 126_30   ] symbols\data\nativetest64\VtsHalEnvironmentalReverbTargetTest
Dir[ 126_31   ] symbols\data\nativetest64\VtsHalAudioEffectTargetTest
Dir[ 126_32   ] symbols\data\nativetest64\VtsHalVirtualizerTargetTest
Dir[ 126_33   ] symbols\data\nativetest64\VtsHalDownmixTargetTest
Dir[ 126_34   ] symbols\data\nativetest64\VtsHalVolumeTargetTest
Dir[ 126_35   ] symbols\data\nativetest64\VtsHalPresetReverbTargetTest
Dir[ 126_36   ] symbols\data\nativetest64\VtsHalHapticGeneratorTargetTest
Dir[ 126_37   ] symbols\apex\com.android.i18n\lib64
Dir[ 126_38   ] symbols\data\nativetest64\VtsHalNSTargetTest
Dir[ 126_39   ] symbols\data\nativetest64\VtsHalEqualizerTargetTest
Dir[ 126_40   ] symbols\data\nativetest64\VtsHalAECTargetTest
Dir[ 126_41   ] symbols\data\nativetest64\VtsHalVisualizerTargetTest
Dir[ 126_42   ] symbols\data\nativetest64\VtsHalLoudnessEnhancerTargetTest
Dir[ 126_43   ] symbols\data\nativetest64\VtsHalAGC2TargetTest
Dir[ 126_44   ] symbols\data\nativetest64\VtsHalAGC1TargetTest
Dir[ 126_45   ] symbols\apex\com.android.media\lib64\extractors
Dir[ 126_46   ] symbols\data\nativetest64\VtsHalAudioEffectFactoryTargetTest
Dir[ 126_47   ] symbols\apex\com.android.adbd\lib64
Dir[ 126_48   ] symbols\apex\com.android.mediaprovider\priv-app\MediaProvider@VVB35V\lib\arm64-v8a
Dir[ 126_49   ] symbols\system\bin\bootstrap
Dir[ 126_50   ] symbols\apex\com.android.tethering\bin
Dir[ 126_51   ] symbols\apex\com.android.adbd\bin
Dir[ 126_52   ] symbols\apex\com.android.nfcservices\lib64
Dir[ 126_53   ] symbols\apex\com.android.media\lib64
Dir[ 126_54   ] symbols\ramdisk\system\bin
Dir[ 126_55   ] symbols\apex\com.android.conscrypt\lib64
Dir[ 126_56   ] symbols\apex\art_boot_images\javalib\arm64
Dir[ 126_57   ] symbols\system\lib64\bootstrap
Dir[ 126_58   ] symbols\apex\com.xxx.modules.attiqi\lib64
Dir[ 126_59   ] symbols\apex\com.android.runtime\lib64\bionic
Dir[ 126_60   ] symbols\system\lib64\bootstrap\hwasan
Dir[ 126_61   ] symbols\apex\com.android.sdkext\bin
Dir[ 126_62   ] symbols\apex\com.android.runtime\lib64
Dir[ 126_63   ] symbols\apex\com.android.runtime\lib64\bionic\hwasan
Dir[ 126_64   ] symbols\system\xbin
Dir[ 126_65   ] symbols\system\bin\hw
Dir[ 126_66   ] symbols\apex\com.android.os.statsd\lib64
Dir[ 126_67   ] symbols\recovery\root\system\bin\hw
Dir[ 126_68   ] symbols\apex\com.android.tethering\priv-app\TetheringNext@VVB35V\lib\arm64-v8a
Dir[ 126_69   ] symbols\vendor\bin\qmi-framework-tests
Dir[ 126_70   ] symbols\apex\com.android.hardware.cas\bin\hw
Dir[ 126_71   ] symbols\vendor\lib64\mediacas
Dir[ 126_72   ] symbols\vendor\lib64\camera
Dir[ 126_73   ] symbols\vendor\lib64\mediadrm
Dir[ 126_74   ] symbols\system\lib64\drm
Dir[ 126_75   ] symbols\apex\com.android.virt\lib64
Dir[ 126_76   ] symbols\data\nativetest64\modetest
Dir[ 126_77   ] symbols\apex\com.android.media\bin
Dir[ 126_78   ] symbols\apex\com.android.media.swcodec\bin
Dir[ 126_79   ] symbols\apex\com.android.tethering\bin\for-system
Dir[ 126_80   ] symbols\apex\com.android.conscrypt\bin
Dir[ 126_81   ] symbols
Dir[ 126_82   ] symbols\system
Dir[ 126_83   ] symbols\vendor
Dir[ 126_84   ] symbols\apex
Dir[ 126_85   ] symbols\data
Dir[ 126_86   ] symbols\data\nativetest64
Dir[ 126_87   ] symbols\apex\com.android.ondevicepersonalization
Dir[ 126_88   ] symbols\apex\com.android.art
Dir[ 126_89   ] symbols\recovery
Dir[ 126_90   ] symbols\recovery\root
Dir[ 126_91   ] symbols\apex\com.android.media.swcodec
Dir[ 126_92   ] symbols\recovery\root\system
Dir[ 126_93   ] symbols\apex\com.android.btservices
Dir[ 126_94   ] symbols\apex\com.android.extservices
Dir[ 126_95   ] symbols\apex\com.android.extservices\priv-app\ExtServices-sminus@VVB35V
Dir[ 126_96   ] symbols\apex\com.android.extservices\priv-app\ExtServices-sminus@VVB35V\lib
Dir[ 126_97   ] symbols\apex\com.android.extservices\priv-app
Dir[ 126_98   ] symbols\apex\com.android.neuralnetworks
Dir[ 126_99   ] symbols\apex\com.android.tethering
Dir[ 126_100  ] symbols\apex\com.android.mediaprovider
Dir[ 126_101  ] symbols\apex\com.android.adservices
Dir[ 126_102  ] symbols\apex\com.android.runtime
Dir[ 126_103  ] symbols\apex\com.android.resolv
Dir[ 126_104  ] symbols\apex\com.android.appsearch
Dir[ 126_105  ] symbols\apex\com.android.media
Dir[ 126_106  ] symbols\apex\com.android.uwb
Dir[ 126_107  ] symbols\apex\com.android.adbd
Dir[ 126_108  ] symbols\apex\com.android.os.statsd
Dir[ 126_109  ] symbols\system\framework
Dir[ 126_110  ] symbols\apex\com.android.i18n
Dir[ 126_111  ] symbols\apex\com.android.mediaprovider\priv-app
Dir[ 126_112  ] symbols\apex\com.android.mediaprovider\priv-app\MediaProvider@VVB35V
Dir[ 126_113  ] symbols\apex\com.android.mediaprovider\priv-app\MediaProvider@VVB35V\lib
Dir[ 126_114  ] symbols\apex\com.android.nfcservices
Dir[ 126_115  ] symbols\ramdisk\system
Dir[ 126_116  ] symbols\apex\com.android.conscrypt
Dir[ 126_117  ] symbols\apex\art_boot_images\javalib
Dir[ 126_118  ] symbols\apex\art_boot_images
Dir[ 126_119  ] symbols\apex\com.xxxx.modules.attiqi
Dir[ 126_120  ] symbols\apex\com.android.sdkext
Dir[ 126_121  ] symbols\apex\com.android.hardware.cas\bin
Dir[ 126_122  ] symbols\apex\com.android.tethering\priv-app
Dir[ 126_123  ] symbols\apex\com.android.hardware.cas
Dir[ 126_124  ] symbols\apex\com.android.tethering\priv-app\TetheringNext@VVB35V\lib
Dir[ 126_125  ] symbols\apex\com.android.tethering\priv-app\TetheringNext@VVB35V
Dir[ 126_126  ] symbols\apex\com.android.virt


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
adb shell cmd wifi get-allowed-channel  // 查看当前 US的频段

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

        // ClientModeImpl error codes   【frameworks/proto_logging/stats/atoms.proto】
        // Defined in /frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiMetrics.java
        IFACE_DESTROYED = 10000;
        WIFI_DISABLED = 10001;
        SUPPLICANT_DISCONNECTED = 10002;
        CONNECTING_WATCHDOG_TIMER = 10003;
        ROAM_WATCHDOG_TIMER = 10004;

        // New reasons tracking disconnections initiated by wifi framework
        DISCONNECT_GENERAL = 10005; // Framework disconnect, generic reason
        // Disconnecting due to unspecified IP reachability lost.
        DISCONNECT_NUD_FAILURE_GENERIC = 10006;
        // Disconnecting due to IP reachability lost from roaming
        DISCONNECT_NUD_FAILURE_ROAM = 10007;
        // Disconnecting due to IP reachability lost from the CONFIRM command
        DISCONNECT_NUD_FAILURE_CONFIRM = 10008;
        // Disconnecting due to IP reachability lost from kernel check
        DISCONNECT_NUD_FAILURE_ORGANIC = 10009;
        // Connectivity no longer wants this network
        DISCONNECT_UNWANTED_BY_CONNECTIVITY = 10010;
        // Timeout creating the IP client
        DISCONNECT_CREATE_IP_CLIENT_TIMEOUT = 10011;
        DISCONNECT_IP_PROVISIONING_FAILURE = 10012; // IP provisioning failure
        DISCONNECT_P2P_REQUESTED_DISCONNECT = 10013; // Disconnect by P2P
        // Network is removed from the WifiConfigManager
        DISCONNECT_NETWORK_REMOVED = 10014;
        DISCONNECT_NETWORK_UNTRUSTED = 10015; // Network is marked as untrusted
        DISCONNECT_NETWORK_METERED = 10016; // Network is marked as metered
        DISCONNECT_TEMP_DISABLED = 10017; // Network is temporarily disabled
        DISCONNECT_PERM_DISABLED = 10018; // Network is permanently disabled
        DISCONNECT_CARRIER_OFFLOAD_DISABLED = 10019;
        // Disconnecting due to Passpoint terms and conditions page
        DISCONNECT_PASSPOINT_TAC = 10020;
        // Disconnecting due to issues with terms and conditions URL
        DISCONNECT_VNC_REQUEST = 10021;
        // Connected to a network that is already removed
        DISCONNECT_UNKNOWN_NETWORK = 10022;
        // User initiated a new connection
        DISCONNECT_NEW_CONNECTION_USER = 10023;
        // New connection triggered by non-user
        DISCONNECT_NEW_CONNECTION_OTHERS = 10024;
        // Wi-Fi 7 is enabled or disabled for this network
        DISCONNECT_NETWORK_WIFI7_TOGGLED = 10025;
    }
	
```


###  qca_disconnect_reason_codes Vendor驱动端连接失败原因

```

/external/wpa_supplicant_8/src/common/qca-vendor.h


例如:
// vendor:11 代表 QCA_DISCONNECT_REASON_PEER_XRETRY_FAIL    reason:1 代表         UNSPECIFIED = 1;

76:c5:3e:b4:d5:8b locally-generated disconnect e8:13:6e:74:74:f4 cm_id 0xd000002 source 6 reason:1  vendor:11 QCA_DISCONNECT_REASON_PEER_XRETRY_FAIL    

/**
 * enum qca_disconnect_reason_codes - Specifies driver disconnect reason codes.
 * Used when the driver triggers the STA to disconnect from the AP.
 *
 * QCA_DISCONNECT_REASON_UNSPECIFIED = 0,
 * @QCA_DISCONNECT_REASON_UNSPECIFIED: The host driver triggered the
 * disconnection with the AP due to unspecified reasons.
 
 * @QCA_DISCONNECT_REASON_INTERNAL_ROAM_FAILURE = 1,
 * @QCA_DISCONNECT_REASON_INTERNAL_ROAM_FAILURE: The host driver triggered the
 * disconnection with the AP due to a roaming failure. This roaming is triggered
 * internally (host driver/firmware).
 
 * @QCA_DISCONNECT_REASON_EXTERNAL_ROAM_FAILURE = 2
 * @QCA_DISCONNECT_REASON_EXTERNAL_ROAM_FAILURE: The driver disconnected from
 * the AP when the user/external triggered roaming fails.
 *
 * @QCA_DISCONNECT_REASON_GATEWAY_REACHABILITY_FAILURE = 3
 * @QCA_DISCONNECT_REASON_GATEWAY_REACHABILITY_FAILURE: This reason code is used
 * by the host driver whenever gateway reachability failure is detected and the
 * driver disconnects with AP.
 *
 * @QCA_DISCONNECT_REASON_UNSUPPORTED_CHANNEL_CSA = 4
 * @QCA_DISCONNECT_REASON_UNSUPPORTED_CHANNEL_CSA: The driver disconnected from
 * the AP on a channel switch announcement from it with an unsupported channel.
 *
 * @QCA_DISCONNECT_REASON_OPER_CHANNEL_DISABLED_INDOOR = 5
 * @QCA_DISCONNECT_REASON_OPER_CHANNEL_DISABLED_INDOOR: On a concurrent AP start
 * with indoor channels disabled and if the STA is connected on one of these
 * disabled channels, the host driver disconnected the STA with this reason
 * code.
 *
 * @QCA_DISCONNECT_REASON_OPER_CHANNEL_USER_DISABLED = 6
 * @QCA_DISCONNECT_REASON_OPER_CHANNEL_USER_DISABLED: Disconnection due to an
 * explicit request from the user to disable the current operating channel.
 *
 * @QCA_DISCONNECT_REASON_DEVICE_RECOVERY = 7
 * @QCA_DISCONNECT_REASON_DEVICE_RECOVERY: STA disconnected from the AP due to
 * the internal host driver/firmware recovery.
 *
 * @QCA_DISCONNECT_REASON_KEY_TIMEOUT = 8
 * @QCA_DISCONNECT_REASON_KEY_TIMEOUT: The driver triggered the disconnection on
 * a timeout for the key installations from the user space.
 *
 * @QCA_DISCONNECT_REASON_OPER_CHANNEL_BAND_CHANGE = 9
 * @QCA_DISCONNECT_REASON_OPER_CHANNEL_BAND_CHANGE: The dDriver disconnected the
 * STA on a band change request from the user space to a different band from the
 * current operation channel/band.
 *
 * @QCA_DISCONNECT_REASON_IFACE_DOWN = 10
 * @QCA_DISCONNECT_REASON_IFACE_DOWN: The STA disconnected from the AP on an
 * interface down trigger from the user space.
 *
 * @QCA_DISCONNECT_REASON_PEER_XRETRY_FAIL = 11
 * @QCA_DISCONNECT_REASON_PEER_XRETRY_FAIL: The host driver disconnected the
 * STA on getting continuous transmission failures for multiple Data frames.
 *
 * @QCA_DISCONNECT_REASON_PEER_INACTIVITY  = 12
 * @QCA_DISCONNECT_REASON_PEER_INACTIVITY: The STA does a keep alive
 * notification to the AP by transmitting NULL/G-ARP frames. This disconnection
 * represents inactivity from AP on such transmissions.
 * 
 * @QCA_DISCONNECT_REASON_SA_QUERY_TIMEOUT  = 13
 * @QCA_DISCONNECT_REASON_SA_QUERY_TIMEOUT: This reason code is used on
 * disconnection when SA Query times out (AP does not respond to SA Query).
 *
 * @QCA_DISCONNECT_REASON_BEACON_MISS_FAILURE  = 13
 * @QCA_DISCONNECT_REASON_BEACON_MISS_FAILURE: The host driver disconnected the
 * STA on missing the beacons continuously from the AP.
 *
 * @QCA_DISCONNECT_REASON_CHANNEL_SWITCH_FAILURE  = 13
 * @QCA_DISCONNECT_REASON_CHANNEL_SWITCH_FAILURE: Disconnection due to STA not
 * able to move to the channel mentioned by the AP in CSA.
 *
 * @QCA_DISCONNECT_REASON_USER_TRIGGERED  = 13
 * @QCA_DISCONNECT_REASON_USER_TRIGGERED: User triggered disconnection.
 */
enum qca_disconnect_reason_codes {
	QCA_DISCONNECT_REASON_UNSPECIFIED = 0,
	QCA_DISCONNECT_REASON_INTERNAL_ROAM_FAILURE = 1,
	QCA_DISCONNECT_REASON_EXTERNAL_ROAM_FAILURE = 2,
	QCA_DISCONNECT_REASON_GATEWAY_REACHABILITY_FAILURE = 3,
	QCA_DISCONNECT_REASON_UNSUPPORTED_CHANNEL_CSA = 4,
	QCA_DISCONNECT_REASON_OPER_CHANNEL_DISABLED_INDOOR = 5,
	QCA_DISCONNECT_REASON_OPER_CHANNEL_USER_DISABLED = 6,
	QCA_DISCONNECT_REASON_DEVICE_RECOVERY = 7,
	QCA_DISCONNECT_REASON_KEY_TIMEOUT = 8,
	QCA_DISCONNECT_REASON_OPER_CHANNEL_BAND_CHANGE = 9,
	QCA_DISCONNECT_REASON_IFACE_DOWN = 10,
	QCA_DISCONNECT_REASON_PEER_XRETRY_FAIL = 11,
	QCA_DISCONNECT_REASON_PEER_INACTIVITY = 12,
	QCA_DISCONNECT_REASON_SA_QUERY_TIMEOUT = 13,
	QCA_DISCONNECT_REASON_BEACON_MISS_FAILURE = 14,
	QCA_DISCONNECT_REASON_CHANNEL_SWITCH_FAILURE = 15,
	QCA_DISCONNECT_REASON_USER_TRIGGERED = 16,
};


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




### 赛门铁克误杀文件处理

```
Symantec Endpoint Protection(赛门铁克杀毒软件) 如何添加白名单避免被误删、误杀？

一、添加 文件夹 到 例外Exceptions(白名单) ，避免被扫描

https://www.cnblogs.com/onelikeone/p/13035491.html


```

<img src="/public/zimage/qocm_issue/saimen/saimen_1.gif"/>
<img src="/public/zimage/qocm_issue/saimen/saimen_1.jpg"/>
<img src="/public/zimage/qocm_issue/saimen/saimen_2.jpg"/>
<img src="/public/zimage/qocm_issue/saimen/saimen_3.jpg"/>
<img src="/public/zimage/qocm_issue/saimen/saimen_4.jpg"/>




### AppOpsManager_OperationID_AppOp操作id码

显示 gps icon的逻辑是在 SystemUI 通过 AppOpsControllerImpl的来监听Noted op: 【1】 with result 【ignore|allow】 来实现的
```
位置权限的区别
位置权限_始终允许----------------android.permission.ACCESS_BACKGROUND_LOCATION: granted=true   授权后台使用 
位置权限_仅在使用应用时允许------android.permission.ACCESS_BACKGROUND_LOCATION: granted=false  不授权后台使用 
位置权限_使用确切位置True--------android.permission.ACCESS_FINE_LOCATION: granted=true  android.permission.ACCESS_COARSE_LOCATION: granted=true 授权高精度位置
位置权限_使用确切位置False-------android.permission.ACCESS_FINE_LOCATION: granted=false android.permission.ACCESS_COARSE_LOCATION: granted=true 授权模糊位置 
```

```
打印Log

adb shell dumpsys appops  > appops.txt
搜索  Package com.potintest.systestp: 查看对应的模式
adb logcat | grep "AppOpsControllerImpl: Noted op:"

07-31 16:45:58.304  7537  7537 W AppOpsControllerImpl: Noted op: 1 with result ignore for package com.potintest.systestp  【ignore———不允许】
07-31 16:45:58.304  7537  7537 W AppOpsControllerImpl: Noted op: 0 with result allow for package com.potintest.systestp   【allow————允许】


object AppOpModes {
    const val MODE_ALLOWED = AppOpsManager.MODE_ALLOWED  【允许模式】
    const val MODE_IGNORED = AppOpsManager.MODE_IGNORED  【忽略模式】
    const val MODE_ERRORED = AppOpsManager.MODE_ERRORED
    const val MODE_DEFAULT = AppOpsManager.MODE_DEFAULT
    const val MODE_FOREGROUND = AppOpsManager.MODE_FOREGROUND
}



```

```

// AppOpsManager.java - operation ids for logging APPops操作id
enum AppOpEnum {
    APP_OP_NONE = -1;
    APP_OP_COARSE_LOCATION = 0;   【模糊位置】
    APP_OP_FINE_LOCATION = 1;     【精确位置】
    APP_OP_GPS = 2;
    APP_OP_VIBRATE = 3;
    APP_OP_READ_CONTACTS = 4;
    APP_OP_WRITE_CONTACTS = 5;
    APP_OP_READ_CALL_LOG = 6;
    APP_OP_WRITE_CALL_LOG = 7;
    APP_OP_READ_CALENDAR = 8;
    APP_OP_WRITE_CALENDAR = 9;
    APP_OP_WIFI_SCAN = 10;
    APP_OP_POST_NOTIFICATION = 11;
    APP_OP_NEIGHBORING_CELLS = 12;
    APP_OP_CALL_PHONE = 13;
    APP_OP_READ_SMS = 14;
    APP_OP_WRITE_SMS = 15;
    APP_OP_RECEIVE_SMS = 16;
    APP_OP_RECEIVE_EMERGENCY_SMS = 17;
    APP_OP_RECEIVE_MMS = 18;
    APP_OP_RECEIVE_WAP_PUSH = 19;
    APP_OP_SEND_SMS = 20;
    APP_OP_READ_ICC_SMS = 21;
    APP_OP_WRITE_ICC_SMS = 22;
    APP_OP_WRITE_SETTINGS = 23;
    APP_OP_SYSTEM_ALERT_WINDOW = 24;
    APP_OP_ACCESS_NOTIFICATIONS = 25;
    APP_OP_CAMERA = 26;
    APP_OP_RECORD_AUDIO = 27;
    APP_OP_PLAY_AUDIO = 28;
    APP_OP_READ_CLIPBOARD = 29;
    APP_OP_WRITE_CLIPBOARD = 30;
    APP_OP_TAKE_MEDIA_BUTTONS = 31;
    APP_OP_TAKE_AUDIO_FOCUS = 32;
    APP_OP_AUDIO_MASTER_VOLUME = 33;
    APP_OP_AUDIO_VOICE_VOLUME = 34;
    APP_OP_AUDIO_RING_VOLUME = 35;
    APP_OP_AUDIO_MEDIA_VOLUME = 36;
    APP_OP_AUDIO_ALARM_VOLUME = 37;
    APP_OP_AUDIO_NOTIFICATION_VOLUME = 38;
    APP_OP_AUDIO_BLUETOOTH_VOLUME = 39;
    APP_OP_WAKE_LOCK = 40;
    APP_OP_MONITOR_LOCATION = 41;                【监听位置】
    APP_OP_MONITOR_HIGH_POWER_LOCATION = 42;     【监听高精度位置】
    APP_OP_GET_USAGE_STATS = 43;
    APP_OP_MUTE_MICROPHONE = 44;
    APP_OP_TOAST_WINDOW = 45;
    APP_OP_PROJECT_MEDIA = 46;
    APP_OP_ACTIVATE_VPN = 47;
    APP_OP_WRITE_WALLPAPER = 48;
    APP_OP_ASSIST_STRUCTURE = 49;
    APP_OP_ASSIST_SCREENSHOT = 50;
    APP_OP_READ_PHONE_STATE = 51;
    APP_OP_ADD_VOICEMAIL = 52;
    APP_OP_USE_SIP = 53;
    APP_OP_PROCESS_OUTGOING_CALLS = 54;
    APP_OP_USE_FINGERPRINT = 55;
    APP_OP_BODY_SENSORS = 56;
    APP_OP_READ_CELL_BROADCASTS = 57;
    APP_OP_MOCK_LOCATION = 58;
    APP_OP_READ_EXTERNAL_STORAGE = 59;
    APP_OP_WRITE_EXTERNAL_STORAGE = 60;
    APP_OP_TURN_SCREEN_ON = 61;
    APP_OP_GET_ACCOUNTS = 62;
    APP_OP_RUN_IN_BACKGROUND = 63;
    APP_OP_AUDIO_ACCESSIBILITY_VOLUME = 64;
    APP_OP_READ_PHONE_NUMBERS = 65;
    APP_OP_REQUEST_INSTALL_PACKAGES = 66;
    APP_OP_PICTURE_IN_PICTURE = 67;
    APP_OP_INSTANT_APP_START_FOREGROUND = 68;
    APP_OP_ANSWER_PHONE_CALLS = 69;
    APP_OP_RUN_ANY_IN_BACKGROUND = 70;
    APP_OP_CHANGE_WIFI_STATE = 71;
    APP_OP_REQUEST_DELETE_PACKAGES = 72;
    APP_OP_BIND_ACCESSIBILITY_SERVICE = 73;
    APP_OP_ACCEPT_HANDOVER = 74;
    APP_OP_MANAGE_IPSEC_TUNNELS = 75;
    APP_OP_START_FOREGROUND = 76;
    APP_OP_BLUETOOTH_SCAN = 77;
    APP_OP_USE_BIOMETRIC = 78;
    APP_OP_ACTIVITY_RECOGNITION = 79;
    APP_OP_SMS_FINANCIAL_TRANSACTIONS = 80;
    APP_OP_READ_MEDIA_AUDIO = 81;
    APP_OP_WRITE_MEDIA_AUDIO = 82;
    APP_OP_READ_MEDIA_VIDEO = 83;
    APP_OP_WRITE_MEDIA_VIDEO = 84;
    APP_OP_READ_MEDIA_IMAGES = 85;
    APP_OP_WRITE_MEDIA_IMAGES = 86;
    APP_OP_LEGACY_STORAGE = 87;
    APP_OP_ACCESS_ACCESSIBILITY = 88;
    APP_OP_READ_DEVICE_IDENTIFIERS = 89;
    APP_OP_ACCESS_MEDIA_LOCATION = 90;
    APP_OP_QUERY_ALL_PACKAGES = 91;
    APP_OP_MANAGE_EXTERNAL_STORAGE = 92;
    APP_OP_INTERACT_ACROSS_PROFILES = 93;
    APP_OP_ACTIVATE_PLATFORM_VPN = 94;
    APP_OP_LOADER_USAGE_STATS = 95;
    APP_OP_DEPRECATED_1 = 96 [deprecated = true];
    APP_OP_AUTO_REVOKE_PERMISSIONS_IF_UNUSED = 97;
    APP_OP_AUTO_REVOKE_MANAGED_BY_INSTALLER = 98;
    APP_OP_NO_ISOLATED_STORAGE = 99;
    APP_OP_PHONE_CALL_MICROPHONE = 100;
    APP_OP_PHONE_CALL_CAMERA = 101;
    APP_OP_RECORD_AUDIO_HOTWORD = 102;
    APP_OP_MANAGE_ONGOING_CALLS = 103;
    APP_OP_MANAGE_CREDENTIALS = 104;
    APP_OP_USE_ICC_AUTH_WITH_DEVICE_IDENTIFIER = 105;
    APP_OP_RECORD_AUDIO_OUTPUT = 106;
    APP_OP_SCHEDULE_EXACT_ALARM = 107;
    APP_OP_FINE_LOCATION_SOURCE = 108;
    APP_OP_COARSE_LOCATION_SOURCE = 109;
    APP_OP_MANAGE_MEDIA = 110;
    APP_OP_BLUETOOTH_CONNECT = 111;
    APP_OP_UWB_RANGING = 112;
    APP_OP_ACTIVITY_RECOGNITION_SOURCE = 113;
    APP_OP_BLUETOOTH_ADVERTISE = 114;
    APP_OP_RECORD_INCOMING_PHONE_AUDIO = 115;
    APP_OP_NEARBY_WIFI_DEVICES = 116;
    APP_OP_ESTABLISH_VPN_SERVICE = 117;
    APP_OP_ESTABLISH_VPN_MANAGER = 118;
    APP_OP_ACCESS_RESTRICTED_SETTINGS = 119;
    APP_OP_RECEIVE_AMBIENT_TRIGGER_AUDIO = 120;
    APP_OP_RECEIVE_EXPLICIT_USER_INTERACTION_AUDIO = 121;
    APP_OP_RUN_USER_INITIATED_JOBS = 122;
    APP_OP_READ_MEDIA_VISUAL_USER_SELECTED = 123;
    APP_OP_SYSTEM_EXEMPT_FROM_SUSPENSION = 124;
    APP_OP_SYSTEM_EXEMPT_FROM_DISMISSIBLE_NOTIFICATIONS = 125;
    APP_OP_READ_WRITE_HEALTH_DATA = 126;
    APP_OP_FOREGROUND_SERVICE_SPECIAL_USE = 127;
    APP_OP_SYSTEM_EXEMPT_FROM_POWER_RESTRICTIONS = 128;
    APP_OP_SYSTEM_EXEMPT_FROM_HIBERNATION = 129;
    APP_OP_SYSTEM_EXEMPT_FROM_ACTIVITY_BG_START_RESTRICTION = 130;
    APP_OP_CAPTURE_CONSENTLESS_BUGREPORT_ON_USERDEBUG_BUILD = 131;
    APP_OP_BODY_SENSORS_WRIST_TEMPERATURE = 132 [deprecated = true];
    APP_OP_USE_FULL_SCREEN_INTENT = 133;
    APP_OP_CAMERA_SANDBOXED = 134;
    APP_OP_RECORD_AUDIO_SANDBOXED = 135;
    APP_OP_RECEIVE_SANDBOX_TRIGGER_AUDIO = 136;
    APP_OP_RECEIVE_SANDBOXED_DETECTION_TRAINING_DATA = 137 [deprecated = true];
    APP_OP_CREATE_ACCESSIBILITY_OVERLAY = 138;
    APP_OP_MEDIA_ROUTING_CONTROL = 139;
    APP_OP_ENABLE_MOBILE_DATA_BY_USER = 140;
    APP_OP_RESERVED_FOR_TESTING = 141;
    APP_OP_RAPID_CLEAR_NOTIFICATIONS_BY_LISTENER = 142;
    APP_OP_READ_SYSTEM_GRAMMATICAL_GENDER = 143;
    APP_OP_RUN_BACKUP_JOBS = 144 [deprecated = true];
    APP_OP_ARCHIVE_ICON_OVERLAY = 145;
    APP_OP_UNARCHIVAL_CONFIRMATION = 146;
    APP_OP_EMERGENCY_LOCATION = 147;             【紧急位置】
    APP_OP_RECEIVE_SENSITIVE_NOTIFICATIONS = 148;
}

```





### Location.java定位数据分析
```

/frameworks/base/core/java/android/location/Location.java
/frameworks/base/location/java/android/location/LocationManager.java 


GnssLocationProvider: reportLocation Location
[gps【A】 31.226788【B】,121.479635【C】 hAcc=3.1 et=+5d13h27m50s951ms alt=55.800000000000004 vAcc=40.5 vel=0.026765555 sAcc=0.8 bear=23.48 bAcc=179.9]   
 
【A】: provider 可选值(gps network) 
【B】 纬度:
【C】 经度:
【 hAcc】 水平精度
【 et  】 开机持续时间
【 alt 】 海拔高度
【 vAcc】 海拔精度
【 vel 】 速度/每秒
【 sAcc 】速度精度/每秒
【 bear 】方位
【 bAcc 】方位精度



// 使用 GPS 提供方，时间间隔和距离均设为 0 , 即 实时获取到系统的位置信息 仅在位置变化时触发 
// 将 minTime 和 minDistance 设为 0 时，系统会尽可能频繁地推送位置更新
locationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 
    0,  // minTime 单位 毫秒
    0,  // minDistance  单位 米
    locationListener
);




Used-in-fix constellation types: GPS GLONASS QZSS BEIDOU GALILEO
【 0="UNKNOWN"  
【 1="GPS"=GPS-美国全球定位系统 = 全球定位系统(Global Positioning System，GPS)
【 2="SBAS"= SBAS（Satellite-Based Augmentation System），即星基增强系统 =美国WAAS(Wide Area Augmentation System)
【 3="GLONASS"=格洛纳斯-俄罗斯实现的国家导航系统 
【 4="QZSS"=准天顶卫星系统 Quasi-Zenith Satellite System；缩写：QZSS = 日本发展的国家定位系统 
【 5="BEIDOU"=中国北斗卫星导航系统（英文名称：BeiDou Navigation Satellite System，简称BDS）
【 6="GALILEO"= 欧盟实现的 伽利略卫星导航系统（Galileo satellite navigation system）
【 7="IRNSS" = 印度区域导航卫星系统（英语：Indian Regional Navigation Satellite System (IRNSS)、NAVIC）

```



### MTK_agps_profiles_conf2.xml 说明

```

agps_profiles_conf2.xml路径：
 agps_profiles_conf2.xml的device路径：/vendor/etc/gnss/agps_profiles_conf2.xml
 codebase路径：/device/mediatek/vendor/common/agps/agps_profiles_conf2.xml

 
SUPL证书转换方法：
(Android Q ~  with Split Build) 
转换工具在：   alps/device/mediatek/vendor/common/agps/certutil/gen_hash_name/host/linux-x86   


1_do_translate_in_linux.sh
Input: ./CERT/
No white space in the file name
Output A: ./0_pem_md5/
Use this in Android

转好放在：      alps/device/mediatek/vendor/common/agps/certutil/files/cacerts_supl_lab 
Path in Device:   /vendor/etc/security/cacerts_supl/lab



```

#### MTLR
```


agps_profiles_conf2.xml 上面的参数主要是设置UP的以及UP给modem的设置
(0=NILR 1=MTLR 2=MOLR 3=QUERY 4=MLR)是属于CP来的位置请求，它代表不同的主体来拿位置。 

gps_open_ind  type=0 (0=NILR 1=MTLR 2=MOLR 3=QUERY 4=MLR)

0=NILR(NI) === NetWork Initiate LocationRequest  ===  2G CP Test Scenario (NI)
1=MTLR(NI) === Mobile Terminated LocationRequest  ===  2G CP Test Scenario – MTLR, request additional assist data
2=MOLR(SI) === Mobile Originated Location Request ===  2G CP Test Scenario – MOLR-Location Estimate



```



#### MTK_agps_profiles_conf2.xml需求列表
```



1需求：SUPL_START消息中不添加QoP信息
 1是支持，需要修改agps_profiles_conf2.xml中 qop_hacc="0"，

6需求: 为Docomo禁用MT-LR。
  修改 agps_profiles_conf2.xml中 reject_non911_nilr_enable ="true"
  MTLR(NI) === Mobile Terminated LocationRequest
  NILR(NI) === NetWork Initiate LocationRequest
```

### MTK_agpsd版本号查看

```


// 查看当前 mtk_agpsd 文件 大小 时间
adb shell "cd /vendor/bin/ ;  ls -l | grep mtk_agpsd"
-rwxr-xr-x 1 root shell  2749328 2009-01-01 08:00 mtk_agpsd

// 导入新的 mtk_agpsd  文件 
adb root && adb remount &&  adb push  ./mtk_agpsd  /vendor/bin/mtk_agpsd 
 


adb reboot

adb logcat | grep "mtk_agpsd is"

04-02 18:54:34.851  1549  1549 E agps    : [agps] ERR: [MAIN] mtk_agpsd is running ver=4.492.0, submarine_mode=-1



工模 》 location base service 》 FLOW 页面会显示 agps ver

AGPS Ver=[4.492.0]

```


### carrier_id排查

```


一: AOSP carrier_id 的列表 (标识唯一 carrier_id )
https://android.googlesource.com/platform/packages/providers/TelephonyProvider/+/main/assets/latest_carrier_id/carrier_list.textpb

二:可在 AOSP_MSI/packages/apps/CarrierConfig/res/xml/xxxx_config.xml 中去配置当前 SUPL的 mode和 地址 
    <carrier_config mcc="470" mnc="50">
        <string name="gps.supl_host">location2.xxs.ne.cca</string>
        <string name="gps.supl_mode">1</string>
    </carrier_config>
	

三:   /vendor/etc/gps_debug.conf 中   【SUPL_MODE=1 for MSB手机主定位】 【SUPL_MODE=2 for MSA平台主定位】 
Add SUPL_MODE=0x1 or SUPL_MODE=0x2 in /vendor/etc/gps_debug.conf .
    SUPL_MODE=0x1 For MSB 
    SUPL_MODE=0x2 For MSA



adb root
adb disable-verity
adb reboot
adb root
adb remount
adb pull /vendor/etc/gps_debug.conf
Make 【#SUPL_MODE=1】 To 【SUPL_MODE=1】 on local dir  
adb push gps_debug.conf /vendor/etc/
adb shell 
═════════════ in adb shell ═════════════
cd /vendor/etc/
chmod 644 gps_debug.conf
cat gps_debug.conf 【make sure SUPL_MODE=1 in the file 】
═════════════ out adb shell ═════════════
adb reboot


```

/vendor/etc/gps_debug.conf 文件内容 
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
#SUPL_MODE=1

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
#LPP_PROFILE = 2

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
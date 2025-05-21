---
layout: post
title: BroadCast_广播_异常分析
category: 安卓
tags:  BroadCast 广播
keywords: BroadCast 广播
typora-root-url: ..\..\..\
typora-copy-images-to: ..\..\..\public\zimage
---

## 简介
 * TOC
 {:toc}
 
##  广播异常案例分析


### BroadCast没有被Dispatched


BroadCast正常Log:

```

11-09 05:52:40.280 14886 14886 D StatsLog: LAUNCHER_TASK_DISMISS_SWIPE_UP
11-09 05:52:40.380 14886 14920 I Launcher/FeatureForPRC: [#sendToRemote] force stop package, clear all = false component = com.zui.browser/com.lenovo.browser.BrowserActivity#0
11-09 05:52:40.381  1888  3347 V BroadcastQueue: Enqueuing BroadcastRecord{7303e30 com.motorola.deviceguard.ACCELERATE_FORCE_STOP_RECENT_APP_ACTION/u0} for 1 receivers

// ACCELERATE_FORCE_STOP_RECENT_APP_ACTION  被发送到  (3229:com.motorola.deviceguard/u0a285) 这个进程中 
11-09 05:52:40.381  1888  1955 D BroadcastQueue: Dispatched BroadcastRecord{7303e30 com.motorola.deviceguard.ACCELERATE_FORCE_STOP_RECENT_APP_ACTION/u0} to (3229:com.motorola.deviceguard/u0a285). assumeDelivered=true ordered=false costTimeMs=0 fromEnqueueTimeMs=0

// 目标process   (3229:com.motorola.deviceguard/u0a285) 打印接收到了 广播  进行 对应 逻辑的处理   能在这里看到 所花销的时间
11-09 05:52:40.385  3229  3805 I DeviceGuard: [DGMC::AccelerateSmartCleanReceiver] action received: com.motorola.deviceguard.ACCELERATE_FORCE_STOP_RECENT_APP_ACTION

11-09 05:52:40.385  3229  3805 W DeviceGuard: [DGMC::AccelerateSmartCleanReceiver] forceStopApi : true , isAllClear : false
11-09 05:52:40.385  3229  3805 I DeviceGuard: [DGMC::AccelerateSmartCleanReceiver] clearRecentTasks[631]


```


BroadCast异常Log:
表现现象:  当用户手动划掉当前运行的APP时 再次打开任务栏时 发现 划掉的APP 还存在

```
// launcher has call sendToRemote method to send a broadcast to DeviceGuard to remove the task  发送广播 ACCELERATE_FORCE_STOP_RECENT_APP_ACTION  携带baidumap包名称到 AMS 侧
05-16 22:30:24.764  2928  2928 D StatsLog: LAUNCHER_TASK_DISMISS_SWIPE_UP
05-16 22:30:24.838  2928  2964 I Launcher/FeatureForPRC: [#sendToRemote] force stop package, clear all = false component = com.baidu.BaiduMap/com.baidu.baidumaps.WelcomeScreen.Alias0#0


//the broadcast was put into the queue in AMS side  广播进入到AMS侧排队
05-16 22:30:25.531  1940  4854 V BroadcastQueue: Enqueuing BroadcastRecord{979d588 com.motorola.deviceguard.ACCELERATE_FORCE_STOP_RECENT_APP_ACTION/u0} for 1 receivers


// 并没有发现该广播被dispatched 到 对应的 目标进程中 


```





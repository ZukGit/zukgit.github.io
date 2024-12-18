---
layout: post
title: Qcom_GPS_ModemLog_排查分析
category: 技术
tags: GPS Qcom Modem 
keywords: GPS Qcom Modem 
typora-root-url: ..\..\
typora-copy-images-to: ..\..\public\zimage
---



## 简介
 * TOC
 {:toc}


## QXDM_操作


### 过滤搜索包


<img src="/public/zimage/technology/qcom_modem_gps/refliter.jpg">




#### match过滤多关键字(无法实时)


```


1. ctrl + a      // 选中当前所有的item  
2. 右键菜单选中  match (搜索)
3. RegEx Engineer 选中为   Perl 
4. Content to Search 需要选中 Type  Name  Summary 
5. 在 Search For 输入栏输入 搜索关键字   示例 :    【Initiate XTRA-T session|tm_xtra3】

点击  Match 后就可以查看当前过滤出关键字的 item 列表项 


```

![](/public/zimage/qcom_tool/qxdm_search_1.jpg)

![](/public/zimage/qcom_tool/qxdm_search_2.jpg)

#### Find 搜索关键字匹配_item 

```

1. Ctrl+F  // 打开 Find 界面 
2. 输入匹配的关键字   示例:  Initiate XTRA-T session
3. 选中 Contents to Search 中的 Name Summary 
4， 点击 Find 进行查找 

CTRL + N  (下一个匹配match_item)

CTRL + Shift + N  (上一个匹配match_item)


```






### 检查xtra的QXDM_match过滤字符串

过滤关键字__精确
```
XTRA Download Request Blocked|GPS time is invalid|Time injection|sending time info request|pdsm_xtra_inject_time_info|Initiate XTRA-T session|EVENT_GPSONEXTRA_END_DOWNLOAD|EVENT_GPSONEXTRA_START_DOWNLOAD|tm_xtra_is_req_blocked|SESSION start request from TM|SESSION stop request from TM|XTRA data/time valid|XTRA data invalid|XTRA3 FileName selected|injecting XTRA3
```

过滤关键字__模糊
```
xtra|SESSION start request from TM|SESSION stop request from TM
```

#### 正常xtra下载打印的Log_1
```

Match过滤字符串1——————
xtra|SESSION start request from TM|SESSION stop request from TM

Match过滤字符串2——————
XTRA Download Request Blocked|GPS time is invalid|Time injection|sending time info request|pdsm_xtra_inject_time_info|Initiate XTRA-T session|EVENT_GPSONEXTRA_END_DOWNLOAD|EVENT_GPSONEXTRA_START_DOWNLOAD|tm_xtra_is_req_blocked|SESSION start request from TM|SESSION stop request from TM|XTRA data/time valid|XTRA data invalid|XTRA3 FileName selected|injecting XTRA3



════1═════   XTRA data/time valid 数据有效  跳过下载 
07:15:11.182631          GPS SM/Medium                                               [           tm_xtra.c   4693] Initiate XTRA-T session.                                                   
07:15:11.183338          GPS SM/Medium                                               [             lm_tm.c   1465] =LM TASK= Received SESSION start request from TM                           
07:15:11.183413          GPS SM/High                                                 [           tm_xtra.c   4441] XTRA data/time valid. Ask TLE for Pos Unc for XTRA-T if necessary   
07:15:15.283413          GPS SM/Medium                                               [tm_decode_xtra3_data.c   8650] =TM XTRA= tm_xtra3_set_xtra_time_and_validity: GPSWeek=2341 GPSMin=4800 Duration=72h
07:15:15.351235          GPS SM/Medium                                               [             lm_tm.c   1773] =LM TASK= Received SESSION stop request from TM

════2═════   XTRA data/time valid 数据无效  请求下载    tm_xtra_is_req_blocked=FLASE  下载请求未被阻止  (False___不阻止) (True___阻止)
 // Session 开始
07:15:15.871405          GPS SM/Medium                                               [           tm_xtra.c   4693] Initiate XTRA-T session.    
07:15:15.872117          GPS SM/Medium                                               [             lm_tm.c   1465] =LM TASK= Received SESSION start request from TM

// GPS时间无效 请求更新时间
07:15:15.872118          GPS SM/High                                                 [tm_xtra_data_handler.c    153] =TM XTRA= GPS time is invalid
07:15:15.872119          GPS SM/High                                                 [           tm_xtra.c   3027] =TM XTRA= Trigger download - sending time info request to client. Force 1
07:15:15.872120          EVENT_GPSONEXTRA_START_DOWNLOAD                             Trigger Type = DCME
07:15:15.872129          GPS SM/High                                                 [             pdapi.c   2318] =PDSM= pdsm_xtra_inject_time_info()
07:15:15.872191          GPS SM/Medium                                               [           tm_xtra.c   2451] =TM XTRA= Time injection: 1416127823405.000(Week: 2341,Msec: 291023405), unc 20.000, utc 1 src 2
07:15:15.872192          GPS SM/Medium                                               [           tm_xtra.c   2497] =TM XTRA= Time injection to GTS successful

// xtra 更新了GPS 时间
07:15:15.872194          GPS SM/Medium                                               [tm_decode_xtra3_data.c   8650] =TM XTRA= tm_xtra3_set_xtra_time_and_validity: GPSWeek=2341 GPSMin=4800 Duration=72h


// 请求下载数据
07:15:15.877954          GPS SM/High                                                 [           tm_xtra.c   4183] =TM XTRA= XTRA data invalid/expired at time inject, request data download   
// 下载请求未被阻止
07:15:15.877973          GPS SM/Medium                                               [           tm_xtra.c    247] =TM XTRA= tm_xtra_is_req_blocked == FALSE                                  
07:15:15.878086          GPS SM/Medium                                               [           tm_xtra.c    247] =TM XTRA= tm_xtra_is_req_blocked == FALSE
// 请求的 xtra数据  以及执行 注入 injecting XTRA3 数据 
07:15:15.878218          GPS SM/Medium                                               [tm_decode_xtra3_data.c   9874] =TM XTRA= XTRA3 FileName selected = [https://path2.xtracloud.net/xtra3grcej.bin]
07:15:15.878235          GPS SM/Medium                                               [tm_decode_xtra3_data.c   9874] =TM XTRA= XTRA3 FileName selected = [https://path3.xtracloud.net/xtra3grcej.bin]
07:15:15.878251          GPS SM/Medium                                               [tm_decode_xtra3_data.c   9874] =TM XTRA= XTRA3 FileName selected = [https://path1.xtracloud.net/xtra3grcej.bin]
07:15:17.176182          GPS SM/Medium                                               [tm_xtra_data_handler.c    750]  Present XTRA ver=3, injecting XTRA3
// 下载xtra数据成功并注入成功
07:15:17.335100          EVENT_GPSONEXTRA_START_DOWNLOAD                             Trigger Type = Fix Request
07:15:17.335155          EVENT_GPSONEXTRA_END_DOWNLOAD                               End Reason = Success
// Session 结束
07:15:18.102503          GPS SM/Medium                                               [             lm_tm.c   1773] =LM TASK= Received SESSION stop request from TM



════3═════  XTRA data/time valid 数据有效  跳过下载 
07:15:18.621791          GPS SM/Medium                                               [           tm_xtra.c   4693] Initiate XTRA-T session.                                                   
07:15:18.622544          GPS SM/Medium                                               [             lm_tm.c   1465] =LM TASK= Received SESSION start request from TM                           
07:15:18.622642          GPS SM/High                                                 [           tm_xtra.c   4441] XTRA data/time valid. Ask TLE for Pos Unc for XTRA-T if necessary  
07:15:18.922642          GPS SM/Medium                                               [tm_decode_xtra3_data.c   8650] =TM XTRA= tm_xtra3_set_xtra_time_and_validity: GPSWeek=2341 GPSMin=4800 Duration=72h
07:15:20.264567          GPS SM/Medium                                               [             lm_tm.c   1773] =LM TASK= Received SESSION stop request from TM           


════4═════  XTRA data/time valid 数据无效  请求下载    tm_xtra_is_req_blocked=FLASE  下载请求未被阻止  (False___不阻止) (True___阻止)
07:15:24.590178          GPS SM/Medium                                               [           tm_xtra.c   4693] Initiate XTRA-T session.                                                   
07:15:24.590822          GPS SM/Medium                                               [             lm_tm.c   1465] =LM TASK= Received SESSION start request from TM
07:15:15.590832          GPS SM/High                                                 [tm_xtra_data_handler.c    153] =TM XTRA= GPS time is invalid
07:15:15.590842          GPS SM/High                                                 [           tm_xtra.c   3027] =TM XTRA= Trigger download - sending time info request to client. Force 1
07:15:15.590852          EVENT_GPSONEXTRA_START_DOWNLOAD                             Trigger Type = DCME
07:15:15.590862          GPS SM/High                                                 [             pdapi.c   2318] =PDSM= pdsm_xtra_inject_time_info()
07:15:15.590872          GPS SM/Medium                                               [           tm_xtra.c   2451] =TM XTRA= Time injection: 1416127823405.000(Week: 2341,Msec: 291023405), unc 20.000, utc 1 src 2
07:15:15.590882          GPS SM/Medium                                               [           tm_xtra.c   2497] =TM XTRA= Time injection to GTS successful
07:15:15.590885          GPS SM/Medium                                               [tm_decode_xtra3_data.c   8650] =TM XTRA= tm_xtra3_set_xtra_time_and_validity: GPSWeek=2341 GPSMin=4800 Duration=72h
07:15:24.598804          GPS SM/High                                                 [           tm_xtra.c   4183] =TM XTRA= XTRA data invalid/expired at time inject, request data download  
07:15:24.598812          GPS SM/Medium                                               [           tm_xtra.c    247] =TM XTRA= tm_xtra_is_req_blocked == FALSE                                  
07:15:24.598892          GPS SM/Medium                                               [           tm_xtra.c    247] =TM XTRA= tm_xtra_is_req_blocked == FALSE
07:15:24.599012          GPS SM/Medium                                               [tm_decode_xtra3_data.c   9874] =TM XTRA= XTRA3 FileName selected = [https://path3.xtracloud.net/xtra3grcej.bin]
07:15:24.599024          GPS SM/Medium                                               [tm_decode_xtra3_data.c   9874] =TM XTRA= XTRA3 FileName selected = [https://path1.xtracloud.net/xtra3grcej.bin]
07:15:24.599035          GPS SM/Medium                                               [tm_decode_xtra3_data.c   9874] =TM XTRA= XTRA3 FileName selected = [https://path2.xtracloud.net/xtra3grcej.bin]
07:15:25.233715          GPS SM/Medium                                               [tm_xtra_data_handler.c    750]  Present XTRA ver=3, injecting XTRA3
07:15:25.404103          EVENT_GPSONEXTRA_START_DOWNLOAD                             Trigger Type = Fix Request 
07:15:25.404203          EVENT_GPSONEXTRA_END_DOWNLOAD                               End Reason = Success                                                                                     
07:15:31.973953          GPS SM/Medium                                               [             lm_tm.c   1773] =LM TASK= Received SESSION stop request from TM                            
```


#### 异常xtra下载打印的Log_1

```

Match过滤字符串1——————
xtra|SESSION start request from TM|SESSION stop request from TM

Match过滤字符串2——————
XTRA Download Request Blocked|GPS time is invalid|Time injection|sending time info request|pdsm_xtra_inject_time_info|Initiate XTRA-T session|EVENT_GPSONEXTRA_END_DOWNLOAD|EVENT_GPSONEXTRA_START_DOWNLOAD|tm_xtra_is_req_blocked|SESSION start request from TM|SESSION stop request from TM|XTRA data/time valid|XTRA data invalid|XTRA3 FileName selected|injecting XTRA3



════失败下载1═════ GPS time is invalid   GPS时间无效  没有执行注入时间 pdsm_xtra_inject_time_info() 【Time injection】  【inject_time_info】 导致无法下载xtra 数据  (有VPN但仍然无法更新 gps时间)
08:11:55.825717          GPS SM/Medium                                               [           tm_xtra.c   4693]    Initiate XTRA-T session.                                                                 
08:11:55.826498          GPS SM/Medium                                               [             lm_tm.c   1465] =  LM TASK= Received SESSION start request from TM                                         
08:11:55.826509          GPS SM/High                                                 [tm_xtra_data_handler.c    128] =TM XTRA= tm_xtra_data_check_handler entry                                             
08:11:55.826516          GPS SM/High                                                 [tm_xtra_data_handler.c    153] =TM XTRA= GPS time is invalid                                                          
08:11:55.826523          GPS SM/High                                                 [           tm_xtra.c   3027] =  TM XTRA= Trigger download - sending time info request to client. Force 1                
08:11:55.826551          EVENT_GPSONEXTRA_START_DOWNLOAD                             Trigger Type = DCME                                                                                                    
08:12:04.898135          GPS SM/Medium                                               [             lm_tm.c   1773] =  LM TASK= Received SESSION stop request from TM                                          



════失败下载2═════   tm_xtra_is_req_blocked=TRUE   xtra下载请求被阻止(一天三次) test_cfg.xml 解除限制


07:25:24.590178          GPS SM/Medium                                               [           tm_xtra.c   4693] Initiate XTRA-T session.                                                   
07:25:24.590822          GPS SM/Medium                                               [             lm_tm.c   1465] =LM TASK= Received SESSION start request from TM
07:25:15.590832          GPS SM/High                                                 [tm_xtra_data_handler.c    153] =TM XTRA= GPS time is invalid
07:25:15.590842          GPS SM/High                                                 [           tm_xtra.c   3027] =TM XTRA= Trigger download - sending time info request to client. Force 1
07:25:15.590852          EVENT_GPSONEXTRA_START_DOWNLOAD                             Trigger Type = DCME
07:25:15.590862          GPS SM/High                                                 [             pdapi.c   2318] =PDSM= pdsm_xtra_inject_time_info()
07:25:15.590872          GPS SM/Medium                                               [           tm_xtra.c   2451] =TM XTRA= Time injection: 1416127823405.000(Week: 2341,Msec: 291023405), unc 20.000, utc 1 src 2
07:25:15.590882          GPS SM/Medium                                               [           tm_xtra.c   2497] =TM XTRA= Time injection to GTS successful
// xtra GPS 时间 有更新
07:25:15.590883         GPS SM/Medium                                               [           tm_xtra.c   2451] =TM XTRA= Time injection: 1416127823405.000(Week: 2341,Msec: 291023405), unc 20.000, utc 1 src 2
07:25:24.598804          GPS SM/High                                                 [           tm_xtra.c   4183] =TM XTRA= XTRA data invalid/expired at time inject, request data download  
07:25:24.598812          GPS SM/Medium                                               [           tm_xtra.c    247] =TM XTRA= tm_xtra_is_req_blocked == TRUE     【 xtra 下载被阻止】                              
07:25:24.598892          GPS SM/Medium                                               [           tm_xtra.c    247] =TM XTRA= tm_xtra_is_req_blocked == TRUE     【 xtra 下载被阻止】
07:25:24.598992          GPS SM/High                                                 [           tm_xtra.c   3828] =TM XTRA= XTRA Download Request Blocked                                                                                 
07:25:31.973953          GPS SM/Medium                                               [             lm_tm.c   1773] =LM TASK= Received SESSION stop request from TM    



════失败下载3═════  无VPN网络导致下载xtra数据失败(gps时间无法更新)

03:41:37.072078          GPS SM/Medium                                               [           tm_xtra.c   4693] Initiate XTRA-T session.
03:41:37.072749          GPS SM/Medium                                               [             lm_tm.c   1465] =LM TASK= Received SESSION start request from TM
03:41:37.072752          GPS SM/High                                                 [tm_xtra_data_handler.c    128] =TM XTRA= tm_xtra_data_check_handler entry
03:41:37.072759          GPS SM/High                                                 [tm_xtra_data_handler.c    153] =TM XTRA= GPS time is invalid                         
03:41:37.072769          GPS SM/High                                                 [           tm_xtra.c   3027] =TM XTRA= Trigger download - sending time info request to client. Force 1
03:41:37.072800          EVENT_GPSONEXTRA_START_DOWNLOAD                             Trigger Type = DCME                                                                 
// 将近两分半钟 xtra 时间都无法更新  GPSWeek=0 (由于没有网络导致无法更新GPS时间)
03:41:38.166435          GPS SM/Medium                                               [tm_decode_xtra3_data.c   8650] =TM XTRA= tm_xtra3_set_xtra_time_and_validity: GPSWeek=0 GPSMin=0 Duration=0h
03:44:19.166690          GPS SM/Medium                                               [tm_decode_xtra3_data.c   8650] =TM XTRA= tm_xtra3_set_xtra_time_and_validity: GPSWeek=0 GPSMin=0 Duration=0h
03:44:19.704542          GPS SM/Medium                                               [             lm_tm.c   1773] =LM TASK= Received SESSION stop request from TM



```

## QDXM_match 字符串集合收集


### SUPL_CP match_字符串搜索


```
EVENT_IMS_SIP_SESSION|CGPS Report Server Rx|CGPS Report Server Tx|User Triggered

```

<img src="/public/zimage/technology/qcom_modem_gps/supl_string_1.jpg">

File_Url【 <img src="/public/zimage/technology/qcom_modem_gps/supl_string_1.hdf"> 】



```
EVENT_IMS_SIP_SESSION_START      开始打电话
EVENT_IMS_SIP_SESSION_RINGING    响铃
EVENT_IMS_SIP_SESSION_ESTABLISH  接听
EVENT_IMS_SIP_SESSION_TERMINATED 呼叫结束

```






## AGPS_CP 交互

### Qcom_AGPS_CP_WLAN_AP_Measurement


#### TE Log

TE_测试主设备(Spirent_思博伦)_Spirent 8100 移动设备SUPL测试系统_HostLog
```

18:41:43 TCSTATUS: ## LBS ## Received ProvideLocationInformation from the UE.__NL__    【接受到来自UE手机端发来的 ProvideLocationInformation 位置信息】
18:41:43 TCSTATUS: ## LBS ## ON_ASPX LppDataReq Transition__NL__
18:41:43 TCSTATUS: ## LBS ## LbsControlMeasurementReadyInd !!__NL__

18:41:43 TCSTATUS: ## LBS ## ***A final fix is received***__NL__                  【解析 a_gnss_ProvideLocationInformation 错误】
18:41:43 TCSTATUS: ## LBS ## Error received in Measurement Ready Ind...__NL__
18:41:43 TCSTATUS: ## LBS ## Error Cause Reported: Pos_NoMeasurement__NL__
18:41:43 TCSTATUS: ## LBS ## Error Tech Reported: a_gnss_ProvideLocationInformation__NL__
18:41:43 TCSTATUS: ## LBS ## Error Tech Reported: COMMONIE__NL__


18:41:43 TCSTATUS: ## LBS ## Received ProvideLocationInformation from the UE.__NL__

18:41:43 TCSTATUS: ## LBS ## LBS Control Msg sent successfully!__NL__    【 LBS Control Msg 发送出去了】
18:41:43 TCSTATUS: ## LBS ## Position error reported by lbs server__NL__     【发生定位错误】


18:41:43 TCSTATUS: ## LBS ## Size of Simulated APs: 12__NL__    【模拟热点AP有12个】
18:41:43 TCSTATUS: ## LBS ## ############__NL__## Evaluating 2.4GHz WLAN AP’s reported__NL__############__NL__
18:41:43 TCSTATUS: ## LBS ##  WLAN 2.4GHz measurement requested and not reported. Test case will FAIL.__NL__   【 2.4G WIFI measurement 没有数据】



18:41:43 TCSTATUS: ## LBS ## Size of Simulated APs: 6__NL__
18:41:43 TCSTATUS: ## LBS ## ############__NL__## Evaluating 5GHz WLAN AP&apos;s reported__NL__############__NL__
18:41:43 TCSTATUS: ## LBS ##  WLAN 5GHz measurement requested and not reported. Test case will FAIL.__NL__ 【 5GHz WIFI measurement 没有数据】
18:41:43 TCSTATUS: ##  LTE  ## Cell now released    __NL__      【 会话关闭】


```


#### UE_手机Log

1. Host TE 设备发送 LPP REQUEST_LOCATION_INFORMATION msg  定位请求消息去请求  WLAN AP measurement
   TE sent LPP REQUEST_LOCATION_INFORMATION msg and requested for WLAN AP measurement

```

2024 Nov  9  06:23:48.463  [5C]  0x1387  CGPS Report Server Rx
Link Type     = Invalid
Protocol Type = LPP CP
Sequence Num  = 6
Flags         = FIRST_SEGMENT | FINAL_SEGMENT
Message Length= 24

Interpreted PDU:
value LPP-Message ::= 
{
  transactionID 
  {
    initiator locationServer,
    transactionNumber 4
  },
  endTransaction FALSE,
  sequenceNumber 14,
  acknowledgement 
  {
    ackRequested TRUE
  },
  lpp-MessageBody c1 : requestLocationInformation : 
      {
        criticalExtensions c1 : requestLocationInformation-r9 : 
            {
              commonIEsRequestLocationInformation 
              {
                locationInformationType locationMeasurementsRequired,
                additionalInformation onlyReturnInformationRequested,
                qos 
                {
                  verticalCoordinateRequest FALSE,
                  responseTime 
                  {
                    time 16
                  },
                  velocityRequest FALSE
                }
              },
              epdu-RequestLocationInformation 
              {
                {
                  ePDU-Identifier 
                  {
                    ePDU-ID 1
                  },
                  ePDU-Body '000800840843F8C200101000'H
                }
              }
            }
      }
}
value OMA-LPPe-MessageExtension ::= 
{
  lppeCompatibilityLevel 0,
  lppeVersion 
  {
    majorVersion 1,
    minorVersion 0
  },
  lppeMode normal,
  messageExtensionBody requestLocationInformation :     【请求Location_位置数据】
    {
      agnss-RequestLocationInformation     【请求agnss的数据】
      {
        positioningInstructions 
        {
          highAccuracyMethodRequested TRUE
        }
      },
      wlan-ap-RequestLocationInformation     【请求wlan的数据】
      {
        requestedMeasurements { apSSID, apRSSI, apChanFreq, non-serving },
        additionalRequestedMeasurements {  }
      }
    }
}




```

2. Modem 收到消息后发送 WLAN_AP_Request到 定位应用 层面
   执行911 定位请求

```

2024 Nov  9  06:23:48.463  [EF]  0x1544  QMI_MCS_QCSI_PKT
packetVersion = 2
MsgType = Indication
Counter = 277
ServiceId = 16
MajorRev = 2
MinorRev = 172
ConHandle = 0xE23982C0
MsgId = 0x0000007C
QmiLength = 4
Service_LOC {
   ServiceLOCV2 {
      qmiLocEventInjectWifiApDataReq {
         qmiLocEventInjectWifiApDataReqIndTlvs[0] {
            Type = 0x10
            Length = 1
            e911Mode {
               e911Mode = true   【 911 定位请求】
            }
         }
      }
   }
}
```


3. 定位应用 接收到定位的请求Log打印 

定位应用 检测到发生 QMI_LOC_EVENT_INJECT_WIFI_AP_DATA_REQ_IND_V02 事件 执行插入 WLAN_AP measure数据到 定位数据包中
 LOWI-SERVER 执行这个插入数据的过程,但  LOWI-SERVER 没起来 所以 数据没正常插入
```

11-09 06:23:48.449  1448  2680 I LocSvc_ApiV02: <--- globalEventCb line 257 QMI_LOC_EVENT_INJECT_WIFI_AP_DATA_REQ_IND_V02
11-09 06:23:48.449  1448  2680 V LocSvc_LBSApiV02: eventCb:59] client = 0xb400007affa3a7c0, event id = 124, event name = QMI_LOC_EVENT_INJECT_WIFI_AP_DATA_REQ_IND_V02 payload = 0x7ae4d3fc98
11-09 06:23:48.449  1448  1468 V IzatSvc_FreeWifiScanObserver: Send emergency on demand scan request
11-09 06:23:48.450  1448  1468 D MessageQ_Client: sendMessage:173] Send normal message to LOWI-SERVER  【 把拿到的数据发送给  LOWI-SERVER 来完成插入数据】


11-09 06:24:00.746  2869  5851 D WifiNl80211Manager: Scan result ready event  【在 WIFI 层面 其实已经有扫描结果】
11-09 06:24:00.746  2869  5851 D WifiNative: Scan result ready event



```


4.反馈给 Host TE 的 PROVIDE_LOCATION_INFORMATION   定位数据包 由于不包含 wifi measure数据导致了测试项失败 


```


2024 Nov  9  06:24:03.681  [1C]  0x1386  CGPS Report Server Tx
Link Type     = Invalid
Protocol Type = LPP CP
Sequence Num  = 7
Flags         = FIRST_SEGMENT | FINAL_SEGMENT
Message Length= 16

Interpreted PDU:
value LPP-Message ::= 
{
  transactionID 
  {
    initiator locationServer,
    transactionNumber 4
  },
  endTransaction TRUE,
  sequenceNumber 2,
  acknowledgement 
  {
    ackRequested TRUE
  },
  lpp-MessageBody c1 : provideLocationInformation : 
      {
        criticalExtensions c1 : provideLocationInformation-r9 : 
            {
              epdu-ProvideLocationInformation 
              {
                {
                  ePDU-Identifier 
                  {
                    ePDU-ID 1
                  },
                  ePDU-Body '000800A008280000'H
                }
              }
            }
      }
}
value OMA-LPPe-MessageExtension ::= 
{
  lppeCompatibilityLevel 0,
  lppeVersion 
  {
    majorVersion 1,
    minorVersion 0
  },
  lppeMode normal,
  messageExtensionBody provideLocationInformation : 
    {
      wlan-ap-ProvideLocationInformastion 
      {
        wlan-AP-Error targetDeviceErrorCauses :    【这里由于缺失 WIFI的检测数据  所以导致测试项失败】
          {
            cause undefined
          }
      }
    }
}


```





##  EVENT_GPS

```

EVENT_GPS_SESSION_BEGIN                                                        = 0x191, /* 0401 */
EVENT_GPS_SESSION_END                                                          = 0x192, /* 0402 */
EVENT_GPS_WAITING_ON_SA                                                        = 0x193, /* 0403 */
EVENT_GPS_PPM_START                                                            = 0x194, /* 0404 */
EVENT_GPS_PPM_RESULTS                                                          = 0x195, /* 0405 */
EVENT_GPS_PPM_END                                                              = 0x196, /* 0406 */
EVENT_GPS_VISIT_BEGIN                                                          = 0x197, /* 0407 */
EVENT_GPS_VISIT_END                                                            = 0x198, /* 0408 */
EVENT_GPS_CDMA_RESUMED_AFTER_GPS_VISIT                                         = 0x199, /* 0409 */
EVENT_GPS_PD_SESSION_BEGIN                                                     = 0x19A, /* 0410 */
EVENT_GPS_PD_SESSION_END                                                       = 0x19B, /* 0411 - Payload: 1 byte PDSM substate */
EVENT_GPS_IS801_RX                                                             = 0x19C, /* 0412 - Payload, 1 byte msg_type */
EVENT_GPS_IS801_TX                                                             = 0x19D, /* 0413 - Payload, 1 byte msg_type */
EVENT_GPS_IS801_TIMEOUT                                                        = 0x1B8, /* 0440 */
EVENT_GPS_IS801_DISCARD                                                        = 0x1B9, /* 0441 - Payload: 1 byte msg type */
EVENT_GPS_PD_CONNECTION_TIMEOUT                                                = 0x23C, /* 0572 - No payload */
EVENT_GPS_PD_DISCONNECTION_COMPLETE                                            = 0x23D, /* 0573 - No payload */


EVENT_GPS_SRCH_START                                                           = 0x241, /* 0577 - Payload: session_type (1 byte) */
EVENT_GPS_SRCH_END                                                             = 0x242, /* 0578 - No Payload */
EVENT_GPS_PPM_PAUSE                                                            = 0x243, /* 0579 - Payload: pause_reason (1 byte) */
EVENT_GPS_PPM_RESUME                                                           = 0x244, /* 0580 - No Payload */
EVENT_GPS_SA_RECEIVED                                                          = 0x245, /* 0581 - Payload: REF_BIT_NUM (2 bytes),
                                                                                     DR_SIZE     (1byte) */
EVENT_GPS_CLK_ON                                                               = 0x246, /* 0582 - No Payload */
EVENT_GPS_CLK_OFF                                                              = 0x247, /* 0583 - No Payload */
EVENT_GPS_VISIT_REQUEST                                                        = 0x248, /* 0584 - No Payload */
EVENT_GPS_VISIT_RESPONSE                                                       = 0x249, /* 0585 - Payload: response_result (1 byte) */
EVENT_GPS_TA_START                                                             = 0x24A, /* 0586 - No Payload */
EVENT_GPS_DSP_READY                                                            = 0x24B, /* 0587 - No Payload */
EVENT_GPS_DSP_CHANNEL_START                                                    = 0x24C, /* 0588 - Payload: SV_ID         (1 byte),
                                                                                     SRCH_MODE     (1 byte),
                                                                                     CHANNEL_NUM   (1 byte),
                                                                                     RESERVED      (1 byte) */
EVENT_GPS_DSP_CHANNEL_DONE                                                     = 0x24D, /* 0589 - Payload: channel_num (1 byte) */
EVENT_GPS_DSP_STOP                                                             = 0x24E, /* 0590 - No Payload */
EVENT_GPS_DSP_DONE                                                             = 0x24F, /* 0591 - No Payload */
EVENT_GPS_TB_END                                                               = 0x250, /* 0592 - No Payload */
EVENT_GPS_SRCH_LARGE_DOPP_WIN                                                  = 0x251, /* 0593 - Payload: sv_prn_num (1 byte),
                                                                                     srch_mode (1 byte),
                                                                                     dopp_wind (2 byte) */
EVENT_GPS_SRCH_EXCEPTION                                                       = 0x252, /* 0594 - Payload: grid_log_id (2 byte),
                                                                                     exception_type (1 byte) */
EVENT_GPS_SRCH_HW_POLLING1                                                     = 0x253, /* 0595 - Payload: agc_val (2 byte),
                                                                                     dci_off (2 byte),
                                                                                     dcq_off (2 byte),
                                                                                     trk_lo (2 byte),
                                                                                     lo_bias (2 byte) */
EVENT_GPS_SRCH_HW_POLLING2                                                     = 0x254, /* 0596 - Payload: sync80 (2 byte) */
EVENT_GPS_PGI_ACTION_PROCESS                                                   = 0x255, /* 0597 - Payload: pgi_substate (1 byte),
                                                                                     pgi_cmd (1 byte) */
EVENT_GPS_GSC_ACTION_PROCESS                                                   = 0x256, /* 0598 - Payload: gsc_substate (1 byte),
                                                                                     gsc_cmd (1 byte) */
EVENT_GPS_PGI_ABORT                                                            = 0x257, /* 0599 - Payload: pgi_subsate (1 byte) */
EVENT_GPS_GSC_ABORT                                                            = 0x258, /* 0600 - Payload: gsc_subsate (1 byte) */
EVENT_GPS_PD_FIX_START                                                         = 0x259, /* 0601 - Payload: event_log_cnt  (2 byte),
                                                                                     operation_mode (1 byte) */
EVENT_GPS_PD_FIX_END                                                           = 0x25A, /* 0602 - Payload: end_status     (1 byte) */
EVENT_GPS_DATA_DOWNLOAD_START                                                  = 0x25B, /* 0603 - Payload: data_type   (1 byte),
                                                                                     sv_mask     (4 byte) */
EVENT_GPS_DATA_DOWNLOAD_END                                                    = 0x25C, /* 0604 - Payload: end_status      (1 byte) */
EVENT_GPS_PD_SESSION_START                                                     = 0x25D, /* 0605 - Payload: start_source    (4 bit)
                                                                                     operation_mode  (4 bit)
                                                                                     session_type    (4 bit)
                                                                                     privacy         (4 bit)
                                                                                     num_fixed       (2 byte)
                                                                                     fix_period      (2 byte)
                                                                                     nav_data_dl     (4 bit)
                                                                                     prq             (1 byte)
                                                                                     threshold       (2 byte)
                                                                                     transport_type  (4 bit) */
EVENT_GPS_DORMANCY_BEGIN                                                       = 0x25E, /* 0606 - No Payload */
EVENT_GPS_DORMANCY_END                                                         = 0x25F, /* 0607 - No Payload */
EVENT_GPS_PRQ_TIMEOUT                                                          = 0x260, /* 0608 - No Payload */
EVENT_GPS_PD_CONNECTION_START                                                  = 0x261, /* 0609 - No Payload */
EVENT_GPS_PD_CONNECTION_ESTABLISHED                                            = 0x262, /* 0610 - No Payload */
EVENT_GPS_PD_DISCONNECTION_START                                               = 0x263, /* 0611 - No Payload */
EVENT_GPS_FTEST_FIX_START                                                      = 0x264, /* 0612 - Payload: reserved (4 byte) */
EVENT_GPS_FTEST_FIX_END                                                        = 0x265, /* 0613 - Payload: reserved (4 byte) */
EVENT_GPS_PD_POSITION                                                          = 0x266, /* 0614 - No Payload */
EVENT_GPS_E911_START                                                           = 0x267, /* 0615 - No Payload */
EVENT_GPS_E911_END                                                             = 0x268, /* 0616 - No Payload */
EVENT_GPS_DBM_SEND_FAILURE                                                     = 0x269, /* 0617 - No Payload */
EVENT_GPS_UAPDMS_STATE_CHANGE                                                  = 0x26A, /* 0618 - Payload: new_state (1 byte)
EVENT_GPS_PDSM_EVENT_REPORT                                                    = 0x40A, /* 1034 - 6 byte payload */
EVENT_GPS_PD_DEMOD_SESS_START                                                  = 0x444, /* 1092 - 5 byte payload */
EVENT_GPS_PD_DEMOD_SESS_END                                                    = 0x445, /* 1093 - 1 byte payload */
EVENT_GPS_SV_ACQUIRED                                                          = 0x446, /* 1094 - 4 byte payload */
EVENT_GPS_SV_BIT_EDGE_FOUND                                                    = 0x447, /* 1095 - 4 byte payload */
EVENT_GPS_DEMOD_STARTED                                                        = 0x448, /* 1096 - 4 byte payload */
EVENT_GPS_DEMOD_OUT_OF_LOCK                                                    = 0x449, /* 1097 - 3 byte payload */
EVENT_GPS_DEMOD_STOPPED                                                        = 0x44A, /* 1098 - 3 byte payload */
EVENT_GPS_DEMOD_PREAMBLE_FOUND                                                 = 0x44B, /* 1099 - 3 byte payload */
EVENT_GPS_DEMOD_FRAME_SYNC_STATUS                                              = 0x44C, /* 1100 - 4 byte payload */
EVENT_GPS_DEMOD_SUBFRAME                                                       = 0x44D, /* 1101 - 6 byte payload */
EVENT_GPS_DEMOD_EPHEMERIS_COMPLETE                                             = 0x44E, /* 1102 - 1 byte payload */
EVENT_GPS_DEMOD_ALMANAC_COMPLETE                                               = 0x44F, /* 1103 - 1 byte payload */
EVENT_GPS_DEMOD_BIT_EDGE_STATUS                                                = 0x450, /* 1104 - 4 byte payload */
EVENT_GPS_DIAG_APP_TRACKING_START                                              = 0x454, /* 1108 - 4 byte payload */
EVENT_GPS_DIAG_APP_TRACKING_END                                                = 0x455, /* 1109 - 12 byte payload */
EVENT_GPS_DIAG_APP_POSITION_SUCCESS                                            = 0x456, /* 1110 - 16 byte payload */
EVENT_GPS_DIAG_APP_POSITION_FAILURE                                            = 0x457, /* 1111 - 6 byte payload */
EVENT_GPS_JAMMER_DETECTION_TEST_PASS                                           = 0x464, /* 1124 - No payload */
EVENT_GPS_JAMMER_DETECTION_TEST_FAILURE                                        = 0x465, /* 1125 - 8 byte payload */
EVENT_GPS_GET_PARAM                                                            = 0x467, /* 1127 - 8 byte payload */
EVENT_GPS_GET_PARAM_BS_INFO                                                    = 0x468, /* 1128 - 18 byte payload */

EVENT_GPS_CME_POS_REQ                                                          = 0x484, /* 1156 - No payload */
EVENT_GPS_CME_FIX_START                                                        = 0x485, /* 1157 - No payload */
EVENT_GPS_CME_FIX_END                                                          = 0x486, /* 1158 - No payload */
EVENT_GPS_SEED_CLM                                                             = 0x487, /* 1159 - 12 byte payload */
EVENT_GPS_SEED_SID                                                             = 0x488, /* 1160 - 10 byte payload */
EVENT_GPS_SEED_SL                                                              = 0x489, /* 1161 - 11 byte payload */
EVENT_GPS_SEED_GET                                                             = 0x48A, /* 1162 - 13 byte payload */
EVENT_GPS_PD_FALLBACK_MODE                                                     = 0x4B5, /* 1205 - 3 byte payload */
EVENT_GPS_PD_COMM_FAILURE                                                      = 0x4D9, /* 1241 - 2 byte payload */
EVENT_GPS_PD_COMM_DONE                                                         = 0x4DA, /* 1242 - No payload */
EVENT_GPS_PD_EVENT_END                                                         = 0x4DB, /* 1243 - 1 byte payload */
EVENT_GPS_PA_EVENT_CALLBACK                                                    = 0x4DC, /* 1244 - 1 byte payload */
EVENT_GPS_PD_CMD_ERR_CALLBACK                                                  = 0x4DD, /* 1245 - 2 byte payload */
EVENT_GPS_PA_CMD_ERR_CALLBACK                                                  = 0x4DE, /* 1246 - 2 byte payload */
EVENT_GPS_LM_ENTER_SA_RF_VERIF                                                 = 0x4DF, /* 1247 - 1 byte payload */
EVENT_GPS_LM_EXIT_SA_RF_VERIF                                                  = 0x4E0, /* 1248 - 1 byte payload */
EVENT_GPS_LM_ERROR_SA_RF_VERIF                                                 = 0x4E1, /* 1249 - 1 byte payload */
EVENT_GPS_LM_PD_COMPLETE                                                       = 0x4E2, /* 1250 - No payload */
EVENT_GPS_LM_IQ_TEST_COMPLETE                                                  = 0x4E3, /* 1251 - No payload */
EVENT_GPS_LM_SESSION_START                                                     = 0x506, /* 1286 - 1 byte payload */
EVENT_GPS_LM_SESSION_END                                                       = 0x507, /* 1287 - No payload */
EVENT_GPS_LM_FIX_REQUEST_START                                                 = 0x508, /* 1288 - No payload */
EVENT_GPS_LM_FIX_REQUEST_END                                                   = 0x509, /* 1289 - No payload */
EVENT_GPS_LM_PRM_REQUEST_START                                                 = 0x50A, /* 1290 - No payload */
EVENT_GPS_LM_PRM_REQUEST_END                                                   = 0x50B, /* 1291 - No payload */
EVENT_GPS_LM_SESSION_CONTINUE                                                  = 0x50C, /* 1292 - 1 byte payload */
EVENT_GPS_LM_FIX_REQUEST_CONTINUE                                              = 0x50D, /* 1293 - No payload */
EVENT_GPS_LM_PRM_REQUEST_CONTINUE                                              = 0x50E, /* 1294 - No payload */
EVENT_GPS_LM_PPM_REQUEST_CONTINUE                                              = 0x50F, /* 1295 - No payload */
EVENT_GPS_LM_AIDING_DATA_RECEIVED                                              = 0x510, /* 1296 - 1 byte payload */
EVENT_GPS_LM_RC_ON_TIMER_TIMEOUT                                               = 0x511, /* 1297 - No payload */
EVENT_GPS_LM_SHUT_OFF_TIMER_TIMEOUT                                            = 0x512, /* 1298 - No payload */
EVENT_GPS_LM_MGP_ON                                                            = 0x513, /* 1299 - No payload */
EVENT_GPS_LM_MGP_IDLE                                                          = 0x514, /* 1300 - No payload */
EVENT_GPS_LM_MGP_OFF                                                           = 0x515, /* 1301 - No payload */
EVENT_GPSONEXTRA_START_DOWNLOAD                                                = 0x560, /* 1376 - 4 byte payload */
EVENT_GPSONEXTRA_END_DOWNLOAD                                                  = 0x561, /* 1377 - 4 byte payload */
EVENT_GPS_DCME_NEW_SV_ADDED_IN_AA                                              = 0x56B, /* 1387 - 1 byte payload */
EVENT_GPS_DCME_SV_REMOVED_FROM_AA                                              = 0x56C, /* 1388 - 1 byte payload */
EVENT_GPS_DCME_MEAS_CYCLE_START                                                = 0x57A, /* 1402 - No payload */
EVENT_GPS_DCME_MEAS_CYCLE_END                                                  = 0x57B, /* 1403 - No payload */
EVENT_GPS_CME_ENGAGED                                                          = 0x57C, /* 1404 - No payload */
EVENT_GPS_CME_NOT_ENGAGED                                                      = 0x57D, /* 1405 - No payload */
EVENT_GPS_DCME_ENGAGED                                                         = 0x57E, /* 1406 - No payload */
EVENT_GPS_DCME_NOT_ENGAGED                                                     = 0x57F, /* 1407 - No payload */
EVENT_GPS_BLANKING_OFF                                                         = 0x59F, /* 1439 - No payload */
EVENT_GPS_BLANKING_ON                                                          = 0x5A0, /* 1440 - No payload */
EVENT_GPS_OPTIMISTIC_PUNC_START                                                = 0x5A3, /* 1443 - 4 byte payload */
EVENT_GPS_OPTIMISTIC_PUNC_END                                                  = 0x5A4, /* 1444 - 4 byte payload */
EVENT_GPS_SV_SEARCH_STATE                                                      = 0x5AC, /* 1452 - 6 byte payload */
EVENT_GPS_TM_ON_DEMAND_MODE_CHANGE                                             = 0x5AD, /* 1453 - 3 byte payload */
EVENT_GPS_TM_ON_DEMAND_BEGIN                                                   = 0x5AE, /* 1454 - 6 byte payload */
EVENT_GPS_TM_ON_DEMAND_DONE                                                    = 0x5AF, /* 1455 - 1 byte payload */
EVENT_GPS_SBAS_DEMOD_REPORT                                                    = 0x5B1, /* 1457 - 9 byte payload */
EVENT_GPS_EXTERN_COARSE_POS_INJ_START                                          = 0x5B2, /* 1458 - No payload */
EVENT_GPS_EXTERN_COARSE_POS_INJ_END                                            = 0x5B3, /* 1459 - 1 byte payload */
EVENT_GPS_EPH_REREQUEST_TIME                                                   = 0x5B4, /* 1460 - 2 byte payload */
EVENT_GPSXTRA_T_SESS_BEGIN                                                     = 0x640, /* 1600 - 1 byte payload */
EVENT_GPSXTRA_T_SESS_DATA                                                      = 0x641, /* 1601 - 8 byte payload */
EVENT_GPSXTRA_T_SESS_DONE                                                      = 0x642, /* 1602 - 1 byte payload */
EVENT_GPSXTRA_T_SESS_END                                                       = 0x643, /* 1603 - 4 byte payload */
EVENT_GPSONEXTRA_DATA_ACK                                                      = 0x787, /* 1927 */
EVENT_GPSXTRA_T_CONN_BEGIN                                                     = 0x795, /* 1941 */
EVENT_GPSXTRA_T_CONN_CONNECT                                                   = 0x796, /* 1942 */
EVENT_GPSXTRA_T_CONN_DISCONNET                                                 = 0x797, /* 1943 */
EVENT_GPSXTRA_T_CONN_DONE                                                      = 0x798, /* 1944 */
EVENT_GPSXTRA_T_CONN_FAILURE                                                   = 0x799, /* 1945 */
EVENT_GPSXTRA_T_CONN_TYPE                                                      = 0x7AD, /* 1965 */
EVENT_GPSXTRA_T_POSITION_INJECTION                                             = 0x9A4, /* 2468 */
EVENT_GPS_PD_COMM_NW_CONNECTING                                                = 0x9B9, /* 2489 */
EVENT_GPS_PD_COMM_NW_CONNECTED                                                 = 0x9BA, /* 2490 */
EVENT_GPS_PD_COMM_SERVER_CONNECTING                                            = 0x9BB, /* 2491 */
EVENT_GPS_PD_COMM_SERVER_CONNECTED                                             = 0x9BC, /* 2492 */
EVENT_GPS_GTP_TDP_SESS_START                                                   = 0xA6F, /* 2671 */
EVENT_GPS_GTP_TDP_SESS_END                                                     = 0xA70, /* 2672 */

```




### EVENT [ EVENT_GPS_PD_SESSION_START ]

```

start_source = MOBILE_INITIATED
operation_mode = STD_ALONE
session_type = GET_NEW_POS
privacy = NONE
data_dl_type = SINGLE
transport_type = IP
num_fixes = 1
fix_period = 1000
prq = 20
threshold = 500



```




## MSG_GPS
```
1217:8:0:1975:lm_mgp.c:LM: Assertion pz_MgpConfig->e_OperationMode != (uint8)MGP_OPERATION_MAX failed
1218:8:0:2376:lm_mgp.c:LM: Assertion pz_MgpConfig->e_OperationMode != MGP_OPERATION_MAX failed
1219:8:0:3754:lm_mgp.c:LM: Assertion z_lmControl.u_MgpONParamsValid failed
1220:8:0:3827:lm_mgp.c:LM: Assertion e_mgpState != LM_MGP_STATE_INVALID failed
1221:8:0:7180:tm_lpp.c:SM: Assertion p_prot_glo_aa_steering->u_NumSvs == p_prot_glo_aa_sv_dir->u_NumSvs failed

7209:8:7:2754:gps_common.c:gnss_ConvertGpsTime2GloTime: NULL pointer!
11318:2:19:6103:gps_common.c:GNSS-LE validation failed, HEPE:%lu, threshold:%lu, quality:%d
11319:2:19:910:gps_nv_efs.c:CGPS_NV_EFS: Registry Initialization
11319:2:19:910:gps_nv_efs.c:CGPS_NV_EFS: Registry Initialization
11320:2:19:964:gps_nv_efs.c:CGPS_NV_EFS: Registry Initialization
11320:2:19:964:gps_nv_efs.c:CGPS_NV_EFS: Registry Initialization
11321:2:19:1145:gps_nv_efs.c:CGPS_NV_EFS_REG: Registry Item Write Started for item:%d
11321:2:19:1145:gps_nv_efs.c:CGPS_NV_EFS_REG: Registry Item Write Started for item:%d
11322:2:19:1268:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Registry Item Write Started for item:%d
11322:2:19:1268:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Registry Item Write Started for item:%d
11323:2:19:1357:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: Registry Item Write Started for item:%d
11323:2:19:1357:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: Registry Item Write Started for item:%d
11324:2:19:1461:gps_nv_efs.c:GPS_NV_EFS: Registry Init Started for subsystem:%d
11324:2:19:1461:gps_nv_efs.c:GPS_NV_EFS: Registry Init Started for subsystem:%d
11325:2:19:1515:gps_nv_efs.c:GPS_NV_EFS: Registry Item Read Started for item:%d
11325:2:19:1515:gps_nv_efs.c:GPS_NV_EFS: Registry Item Read Started for item:%d
11326:2:19:233:gps_nv_efs.c:CGPS_NV_EFS: Registry Initialization %d
11326:2:19:233:gps_nv_efs.c:CGPS_NV_EFS: Registry Initialization %d
11327:2:19:409:gps_nv_efs.c:CGPS_NV_EFS: mcfg_mkdir /nv returned %d for subsys %d
11327:2:19:409:gps_nv_efs.c:CGPS_NV_EFS: mcfg_mkdir /nv returned %d for subsys %d
11328:2:19:417:gps_nv_efs.c:CGPS_NV_EFS: mcfg_mkdir /nv/item_files returned %d for subsys %d
11328:2:19:417:gps_nv_efs.c:CGPS_NV_EFS: mcfg_mkdir /nv/item_files returned %d for subsys %d
11329:2:19:425:gps_nv_efs.c:CGPS_NV_EFS: mcfg_mkdir /nv/item_files/conf returned %d for subsys %d
11329:2:19:425:gps_nv_efs.c:CGPS_NV_EFS: mcfg_mkdir /nv/item_files/conf returned %d for subsys %d
11330:2:19:187:gps_nv_efs.c:CGPS_NV_EFS_REG: could not add new line to file name file_size %d
11330:2:19:187:gps_nv_efs.c:CGPS_NV_EFS_REG: could not add new line to file name file_size %d
11353:2:19:946:aries_gpsdiag.c:gpsdiag_DoneEventCb: PDSM_PD_EVENT_GPS_DONE
11359:2:19:1052:aries_gpsdiag.c:gpsdiag_StartEventCb: PDSM_PD_EVENT_GPS_BEGIN
11365:2:19:1284:aries_gpsdiag.c:gpsdiag_PaEventsDispatchCb: PDSM_PA_EVENT_GPS_LOCK received
11412:2:19:1951:aries_gpsdiag.c:ApiCmd: CGPS_INIT_CMD
11413:2:19:1964:aries_gpsdiag.c:CGPS_INIT_CMD: ClientType=%d, Client ID %d
11414:2:19:1970:aries_gpsdiag.c:ApiCmd: CGPS_ACTIVATE_CMD
11415:2:19:1982:aries_gpsdiag.c:ApiCmd: CGPS_DEACTIVATE_CMD
11416:2:19:1994:aries_gpsdiag.c:ApiCmd: CGPS_RELEASE_CMD
11418:2:19:2105:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_PD_CMD: Register: %d, %u
11419:2:19:2115:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_PD_CMD: Deregister
11420:2:19:2137:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_PA_CMD: Register
11421:2:19:2147:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_PA_CMD: Deregister
11422:2:19:2169:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_LCS_CMD: Register
11423:2:19:2179:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_LCS_CMD: Deregister
11424:2:19:2201:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_EXT_STAT_CMD: Register
11425:2:19:2211:aries_gpsdiag.c:RegCmd: CGPS_REGISTER_EXT_STAT_CMD: Deregister
11437:2:19:2951:aries_gpsdiag.c:Param: GPSDIAG_PA_GPS_LOCK
11484:2:19:5175:aries_gpsdiag.c:CgpsCmdCode: CGPS_XTRA_INJECT_DATA_CMD pktlen:%d
11485:2:19:5180:aries_gpsdiag.c:CgpsCmdCode: CGPS_XTRA_CLIENT_REGISTER_CMD pktlen:%d
11486:2:19:5184:aries_gpsdiag.c:CgpsCmdCode: CGPS_START_CMD
11487:2:19:5188:aries_gpsdiag.c:CgpsCmdCode: CGPS_END_CMD
11488:2:19:5192:aries_gpsdiag.c:CgpsCmdCode: CGPS_SET_PARAM_CMD
11489:2:19:5196:aries_gpsdiag.c:CgpsCmdCode: CGPS_GET_PARAM_CMD
11490:2:19:5200:aries_gpsdiag.c:CgpsCmdCode: CGPS_GET_LAST_KNOWN_POS_CMD
11491:2:19:5204:aries_gpsdiag.c:CgpsCmdCode: CGPS_GET_PDAPI_VERS_CMD
11492:2:19:5208:aries_gpsdiag.c:CgpsCmdCode: CGPS_NTFY_VRFY_SND_USR_ACTN_CMD
11493:2:19:5212:aries_gpsdiag.c:CgpsCmdCode: CGPS_INJECT_TIME_CMD
11494:2:19:5216:aries_gpsdiag.c:CgpsCmdCode: CGPS_EXTERN_COARSE_POS_INJ_CMD
11495:2:19:5220:aries_gpsdiag.c:CgpsCmdCode: CGPS_LCS_VX_INIT_CLIENT_CMD
11496:2:19:5224:aries_gpsdiag.c:CgpsCmdCode: CGPS_LCS_VX_INIT_CLIENT_APP_INFO_CMD
11497:2:19:5228:aries_gpsdiag.c:CgpsCmdCode: CGPS_LCS_VX_RELEASE_CLIENT_CMD
11498:2:19:5232:aries_gpsdiag.c:CgpsCmdCode: CGPS_LCS_VX_QUERY_CONFIG_CMD
11499:2:19:5236:aries_gpsdiag.c:CgpsCmdCode: CGPS_LCS_VX_GET_CLIENT_TYPE_CMD
11500:2:19:5240:aries_gpsdiag.c:CgpsCmdCode: CGPS_LCS_VX_GET_STATUS_CMD
11501:2:19:5244:aries_gpsdiag.c:CgpsCmdCode: CGPS_QUERY_GPS_STATE_CMD
11501:2:19:5244:aries_gpsdiag.c:CgpsCmdCode: CGPS_QUERY_GPS_STATE_CMD
11502:2:19:5248:aries_gpsdiag.c:CgpsCmdCode: CGPS_TRIGGER_XTRAT_SESSION
11503:2:19:5252:aries_gpsdiag.c:CgpsCmdCode: CGPS_TEST_FORCE_MEM_ALLOC_ERROR
11504:2:19:5256:aries_gpsdiag.c:CgpsCmdCode: CGPS_QUERY_VERSION
11505:2:19:5260:aries_gpsdiag.c:CgpsCmdCode: CGPS_INJECT_EXTERNAL_SPI
11506:2:19:5264:aries_gpsdiag.c:CgpsCmdCode: CGPS_OEM_CONTROL
11507:2:19:5268:aries_gpsdiag.c:CgpsCmdCode: CGPS_OEM_DRSYNC_SET_CFG
11508:2:19:5272:aries_gpsdiag.c:CgpsCmdCode: CGPS_INTERNAL_REQ_1XEVDO_RF_INFO_CMD
11509:2:19:5276:aries_gpsdiag.c:CgpsCmdCode: CGPS_GEOFENCE_IN_MEM_LOG_CMD
11510:2:19:5280:aries_gpsdiag.c:CgpsCmdCode: CGPS_DIAG_BUF_TEST_CMD
12323:4:19:1629:gps_common.c:Measurement already propagated,Skip. Constel:%d,SV:%u Stat:0x%x 
12324:4:19:1823:gps_common.c:Measurement already propagated,Skip double propagation Constel:%d,SV:%u Stat:%0x 
12325:4:19:1851:gps_common.c:FineSpeed PreCheck: Sv=%d has CN0=%d (x10 dB-Hz) and Unc=%d (mm/s)
12326:4:19:4533:gps_common.c:Callback Log report for GM: 0x%x
12327:4:19:4537:gps_common.c:Callback Log report for TM: 0x%x, 0x%x, 0x%x
12328:4:19:5788:gps_common.c:gnss_SendPdrControl: Sending PDR control flag %u from source %u
12329:4:19:5907:gps_common.c:gnss_UpdatePdrControlDb: Accepting new PDR control, input flag %u, source %u, new flag %u
13482:8:19:99:tm_rr_iface.c:CGPS_NV_EFS: Invalid as_id
13497:8:19:1762:cgps_api.c:Bad min GPS wknum %u, using %u instead
13498:8:19:1826:cgps_api.c:sm_GetGpsUtcOffset not supported for OEMPD. QDI needed
13500:8:19:1168:gps_common.c:Null p_GpsMsecs
13501:8:19:1206:gps_common.c:Null p_GpsMsecs
13502:8:19:1261:gps_common.c:Null MeasBlk
13503:8:19:1617:gps_common.c:Null MeasBlk
13504:8:19:1655:gps_common.c:Out of bounds access attempt for array &p_MeasBlk->n_Meas.z_Gps
13505:8:19:1708:gps_common.c:Null ptr
13506:8:19:1803:gps_common.c:Null ptr
13507:8:19:1907:gps_common.c:NULL access in gnss_GloMeasBlkGGRfgdAdjust()
13508:8:19:1912:gps_common.c:zero RF Group delay retrieved
13509:8:19:2011:gps_common.c:Log packet allocation for LOG_CONVERGED_GPS_POSITION_REPORT_C failed
13509:8:19:2011:gps_common.c:Log packet allocation for LOG_CONVERGED_GPS_POSITION_REPORT_C failed
13510:8:19:2091:gps_common.c:Log packet allocation for LOG_CONVERGED_GPS_POSITION_REPORT_C failed
13510:8:19:2091:gps_common.c:Log packet allocation for LOG_CONVERGED_GPS_POSITION_REPORT_C failed
13511:8:19:2546:gps_common.c:Unexpected disrepancy between Gps %d & TimeTick %d Msec diff at TimeTickMsec Ahead of %d
13512:8:19:2841:gps_common.c:can't convert gps time to bds time, parameters invalid
13513:8:19:2882:gps_common.c:can't convert gps time to gal time, parameters invalid
13514:8:19:2914:gps_common.c:CD/NAVIC: verify - converted week number value: %u
13515:8:19:2930:gps_common.c:GNSS/NAVIC: can't convert GPS time to NAVIC time, parameters invalid
13516:8:19:3050:gps_common.c:GNSS/COMMON LogPosRpt: null pointer %u %u %u
13517:8:19:3712:gps_common.c:gnss_LogPositionReport: Input NULL pointer
13518:8:19:3737:gps_common.c:Log packet allocation for GNSS Position Report Failed
13519:8:19:4340:gps_common.c:PosReport1476: TotalBCN %u, Operating Mode1 %u, Operating Mode2 %u
13520:8:19:4476:gps_common.c:PosReport1476_end: TotalBCN %u, Operating Mode1 %u, Operating Mode2 %u
13521:8:19:4541:gps_common.c:Unknown clientThreadId = %d passed
13522:8:19:5166:gps_common.c:*gps_PtrPtr = 0x%X after FREE
13522:8:19:5166:gps_common.c:*gps_PtrPtr = 0x%X after FREE
13523:8:19:5305:gps_common.c:Bad GNSS type %u
13524:8:19:5349:gps_common.c:Bad wknum %u, not in range %u...%u
13525:8:19:5376:gps_common.c:GNSS/COMMON: Null pointer access inwgs84_Ecef2LlaGet() %u %u
13526:8:19:5418:gps_common.c:GNSS/COMMON: Null pointer access ingnss_Wgs84EcefToDtmEcef() %u %u
13527:8:19:5490:gps_common.c:GNSS/COMMON: Null pointer access ingnss_Ecef2LlaGet() %u %u
13528:8:19:5527:gps_common.c:GNSS/COMMON: Invalid Datum typein gnss_Ecef2LlaGet %u
13529:8:19:5795:gps_common.c:gnss_SendPdrControl: Sending PDR control flag to LE failed
13530:8:19:5801:gps_common.c:gnss_SendPdrControl: Sending PDR control flag to SAML failed
13531:8:19:5831:gps_common.c:gnss_InitPdrControlDb: Input pointer NULL
13532:8:19:5873:gps_common.c:gnss_UpdatePdrControlDb: InputPr pointer NULL
13533:8:19:5880:gps_common.c:gnss_UpdatePdrControlDb: Invalid PDR control source, flag %u, source %u
13534:8:19:5939:gps_common.c:Could not allocate log pkt
13535:8:19:6169:gps_common.c:Null constel cfg
13536:8:19:6349:gps_common.c:GNSSMASK: invalid mask pointer.
13537:8:19:6914:gps_common.c:ConvertType %d not supported
13538:8:19:6967:gps_common.c:ConvertType %d not supported
13539:8:19:7670:gps_common.c:IPC -Failure in memory Allocation ipc buff
13540:8:19:7679:gps_common.c:IPC -Failure in memory Allocation ipc cir buff
13541:8:19:7683:gps_common.c:IPC -size - %ld
13542:8:19:3632:gps_common.c:GNSS/COMMON CountAvailMeas: null pointer %u %u %u %u %u
13543:8:19:3385:gps_common.c:GNSS/COMMON LogPosRpt: null pointer %u %u %u %u %u
13544:8:19:916:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid type: %d
13544:8:19:916:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid type: %d
13545:8:19:926:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid table indexes start:%d, end:%d
13545:8:19:926:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid table indexes start:%d, end:%d
13546:8:19:932:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer
13546:8:19:932:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer
13547:8:19:970:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid type: %d
13547:8:19:970:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid type: %d
13548:8:19:980:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid table indexes. start:%d, end:%d
13548:8:19:980:gps_nv_efs.c:CGPS_NV_EFS_INIT: Invalid table indexes. start:%d, end:%d
13549:8:19:986:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer
13549:8:19:986:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer
13550:8:19:993:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid Sub ID %d
13550:8:19:993:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid Sub ID %d
13551:8:19:1044:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13551:8:19:1044:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13552:8:19:1050:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer for item:%d
13552:8:19:1050:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer for item:%d
13553:8:19:1095:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid item index, start:%d, end:%d, item:%d
13553:8:19:1095:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid item index, start:%d, end:%d, item:%d
13554:8:19:1101:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13554:8:19:1101:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13555:8:19:1107:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer for item:%d
13555:8:19:1107:gps_nv_efs.c:CGPS_NV_EFS_READ: NULL Registry pointer for item:%d
13556:8:19:1114:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid Sub ID %d
13556:8:19:1114:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid Sub ID %d
13557:8:19:1158:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid item index, start:%d, end:%d, item:%d
13557:8:19:1158:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid item index, start:%d, end:%d, item:%d
13558:8:19:1164:gps_nv_efs.c:CGPS_NV_EFS_WRITE: data pointer is null
13558:8:19:1164:gps_nv_efs.c:CGPS_NV_EFS_WRITE: data pointer is null
13559:8:19:1170:gps_nv_efs.c:CGPS_NV_EFS_WRITE: NULL Registry pointer for item:%d
13559:8:19:1170:gps_nv_efs.c:CGPS_NV_EFS_WRITE: NULL Registry pointer for item:%d
13560:8:19:1371:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: subsystem not initialized for item:%d
13560:8:19:1371:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: subsystem not initialized for item:%d
13561:8:19:1378:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: Destination buffer size incorrect
13561:8:19:1378:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: Destination buffer size incorrect
13562:8:19:1405:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: could not generate filename
13562:8:19:1405:gps_nv_efs.c:CGPS_NV_EFS_ASYNC_WRITE: could not generate filename
13563:8:19:548:gps_nv_efs.c:CGPS_NV_EFS_REG: Null table pointer.
13563:8:19:548:gps_nv_efs.c:CGPS_NV_EFS_REG: Null table pointer.
13564:8:19:554:gps_nv_efs.c:CGPS_NV_EFS_REG: Null pathname
13564:8:19:554:gps_nv_efs.c:CGPS_NV_EFS_REG: Null pathname
13565:8:19:564:gps_nv_efs.c:CGPS_NV_EFS_REG: invalid range: %d, %d
13565:8:19:564:gps_nv_efs.c:CGPS_NV_EFS_REG: invalid range: %d, %d
13566:8:19:571:gps_nv_efs.c:CGPS_NV_EFS_REG: invalid type: %d
13566:8:19:571:gps_nv_efs.c:CGPS_NV_EFS_REG: invalid type: %d
13567:8:19:238:gps_nv_efs.c:CGPS_NV_EFS_REG: Null table pointer.
13567:8:19:238:gps_nv_efs.c:CGPS_NV_EFS_REG: Null table pointer.
13568:8:19:244:gps_nv_efs.c:CGPS_NV_EFS_REG: Null pathname
13568:8:19:244:gps_nv_efs.c:CGPS_NV_EFS_REG: Null pathname
13569:8:19:254:gps_nv_efs.c:CGPS_NV_EFS_REG: invalid range: %d, %d
13569:8:19:254:gps_nv_efs.c:CGPS_NV_EFS_REG: invalid range: %d, %d
13570:8:19:275:gps_nv_efs.c:CGPS_NV_EFS: No config file available for this type %d
13570:8:19:275:gps_nv_efs.c:CGPS_NV_EFS: No config file available for this type %d
13571:8:19:281:gps_nv_efs.c:CGPS_NV_EFS: Config file name is still NULL
13571:8:19:281:gps_nv_efs.c:CGPS_NV_EFS: Config file name is still NULL
13572:8:19:288:gps_nv_efs.c:CGPS_NV_EFS: Invalid File system specified %d
13572:8:19:288:gps_nv_efs.c:CGPS_NV_EFS: Invalid File system specified %d
13573:8:19:295:gps_nv_efs.c:CGPS_NV_EFS: Invalid Subscription ID specified %d
13573:8:19:295:gps_nv_efs.c:CGPS_NV_EFS: Invalid Subscription ID specified %d
13574:8:19:453:gps_nv_efs.c:CGPS_NV_EFS: Error %d creating config file
13574:8:19:453:gps_nv_efs.c:CGPS_NV_EFS: Error %d creating config file
13575:8:19:477:gps_nv_efs.c:CGPS_NV_EFS:Incorrect File Name length. Limit is :%d, Current size:%d
13575:8:19:477:gps_nv_efs.c:CGPS_NV_EFS:Incorrect File Name length. Limit is :%d, Current size:%d
13576:8:19:489:gps_nv_efs.c:CGPS_NV_EFS: Error %d writing to config file
13576:8:19:489:gps_nv_efs.c:CGPS_NV_EFS: Error %d writing to config file
13577:8:19:503:gps_nv_efs.c:CGPS_NV_EFS: Error %d closing config file
13577:8:19:503:gps_nv_efs.c:CGPS_NV_EFS: Error %d closing config file
13578:8:19:717:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid NV Item specified %d
13578:8:19:717:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid NV Item specified %d
13579:8:19:723:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13579:8:19:723:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13580:8:19:622:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid NV Item specified %d
13580:8:19:622:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid NV Item specified %d
13581:8:19:628:gps_nv_efs.c:CGPS_NV_EFS_READ: subsystem not initialized for item:%d
13581:8:19:628:gps_nv_efs.c:CGPS_NV_EFS_READ: subsystem not initialized for item:%d
13582:8:19:634:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13582:8:19:634:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13583:8:19:641:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid File system specified %d
13583:8:19:641:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid File system specified %d
13584:8:19:648:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid Subscription ID specified %d
13584:8:19:648:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid Subscription ID specified %d
13585:8:19:683:gps_nv_efs.c:CGPS_NV_EFS_READ: EFS read error for item = %d, %d
13585:8:19:683:gps_nv_efs.c:CGPS_NV_EFS_READ: EFS read error for item = %d, %d
13586:8:19:866:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid NV Item specified %d
13586:8:19:866:gps_nv_efs.c:CGPS_NV_EFS_READ: Invalid NV Item specified %d
13587:8:19:872:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13587:8:19:872:gps_nv_efs.c:CGPS_NV_EFS_READ: data pointer is null
13588:8:19:1210:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid item index, start:%d, end:%d, item:%d
13588:8:19:1210:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid item index, start:%d, end:%d, item:%d
13589:8:19:1218:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid NV Item specified %d
13589:8:19:1218:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid NV Item specified %d
13590:8:19:1224:gps_nv_efs.c:CGPS_NV_EFS_WRITE: data pointer is null
13590:8:19:1224:gps_nv_efs.c:CGPS_NV_EFS_WRITE: data pointer is null
13591:8:19:1230:gps_nv_efs.c:CGPS_NV_EFS_WRITE: NULL Registry pointer for item:%d
13591:8:19:1230:gps_nv_efs.c:CGPS_NV_EFS_WRITE: NULL Registry pointer for item:%d
13592:8:19:1237:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid Sub ID %d
13592:8:19:1237:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid Sub ID %d
13593:8:19:770:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid NV Item specified %d
13593:8:19:770:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid NV Item specified %d
13594:8:19:776:gps_nv_efs.c:CGPS_NV_EFS_WRITE: subsystem not initialized for item:%d
13594:8:19:776:gps_nv_efs.c:CGPS_NV_EFS_WRITE: subsystem not initialized for item:%d
13595:8:19:782:gps_nv_efs.c:CGPS_NV_EFS_WRITE: data pointer is null
13595:8:19:782:gps_nv_efs.c:CGPS_NV_EFS_WRITE: data pointer is null
13596:8:19:789:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid File system specified %d
13596:8:19:789:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid File system specified %d
13597:8:19:796:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid Subscription ID specified %d
13597:8:19:796:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Invalid Subscription ID specified %d
13598:8:19:803:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Destination buffer size incorrect
13598:8:19:803:gps_nv_efs.c:CGPS_NV_EFS_WRITE: Destination buffer size incorrect
13599:8:19:834:gps_nv_efs.c:CGPS_NV_EFS_WRITE2: EFS write error %d, %d
13599:8:19:834:gps_nv_efs.c:CGPS_NV_EFS_WRITE2: EFS write error %d, %d
13653:8:19:1959:aries_gpsdiag.c:CGPS_INIT_CMD: Cannot initialize PDAPI!!
13654:8:19:1975:aries_gpsdiag.c:CGPS_ACTIVATE_CMD: UNKNOWN CLIENT ID!!
13655:8:19:1987:aries_gpsdiag.c:CGPS_DEACTIVATE_CMD: UNKNOWN CLIENT ID!!
13656:8:19:1999:aries_gpsdiag.c:CGPS_RELEASE_CMD: UNKNOWN CLIENT ID!!
13659:8:19:2100:aries_gpsdiag.c:CGPS_REGISTER_PD_CMD: UNKNOWN CLIENT ID!!
13660:8:19:2132:aries_gpsdiag.c:CGPS_REGISTER_PA_CMD: UNKNOWN CLIENT ID!!
13661:8:19:2164:aries_gpsdiag.c:CGPS_REGISTER_LCS_CMD: UNKNOWN CLIENT ID!!
13662:8:19:2196:aries_gpsdiag.c:CGPS_REGISTER_EXT_STAT_CMD: UNKNOWN CLIENT ID!!
13737:8:19:5293:aries_gpsdiag.c:SubsysCmdCode NOT CGPS_DIAG_PDAPI_CMD.
21340:4:21:2076:estk_gps.c:estk_gps_full_clean_up_slot: slot_id=0x%x
28673:4:46:4889:gps_common.c:TT Dal Inteface initialized for GNSS: %d
29699:8:46:2688:gps_common.c:NULL handle or failed to get count
31797:8:47:6521:gps_common.c:Invalid CN0 %d
31798:8:47:6580:gps_common.c:Unsupported PDI %d
31799:8:47:6722:gps_common.c:NF: Unexpected Measurements Src %u
37888:16:48:5187:gps_common.c:gps_MemAlloc() failed
37888:16:48:5187:gps_common.c:gps_MemAlloc() failed
39003:2:49:4662:cd_getpt.c:GPS_ASSIST_EPH: Have:0X%08X, Wish:0X%08X
39021:2:49:12135:cd_getpt.c:CD/SBAS NV: Read NV_CGPS_SBAS_USER_PREFERENCE_I Failure
39022:2:49:12158:cd_getpt.c:CD/SBAS NV: Read NV_CGPS_SBAS_ENABLED_I Failure
41023:8:49:82:gps_rom_almanac.c:gps_GetGpsRomAlmanac: NULL input pointer
41023:8:49:82:gps_rom_almanac.c:gps_GetGpsRomAlmanac: NULL input pointer
43189:2:50:546:sm_nv_efs.c:CGPS_NV_EFS_REG: Registry Get %d Default
43692:2:50:195:gm_gps_env_based_backoff.c:GM Env based back off Min : %d Max : %d
43693:2:50:318:gm_gps_env_based_backoff.c:Wont kick EbEE: too soon from fix
43694:2:50:340:gm_gps_env_based_backoff.c:Inside GPS ENV Backoff %d Backoff %d Kick EBEE %d
43989:2:50:250:lm_task.c:=LM TASK= Read NV NV_AAGPS_APP_TRACKING_GPSON_THSLD_I(sec) = %d
43990:2:50:264:lm_task.c:=LM TASK= Read NV NV_AAGPS_APP_TRACKING_GPSIDLE_THSLD_I(sec) = %d
43991:2:50:278:lm_task.c:=LM TASK= Read NV NV_AAGPS_APP_TRKG_PERIODIC_REQ_DLY_MARGIN_I(sec) = %d
43992:2:50:291:lm_task.c:=LM TASK= Read NV NV_CGPS_QOS_OVERRIDE_ACC_THRESHOLD_I(m) = %d
43993:2:50:299:lm_task.c:=LM TASK= Read NV NV_CGPS_QOS_OVERRIDE_TIME_I(s) = %d
44401:2:50:9165:tm_core.c:TM: Received event TM_PD_EVENT_GPS_BEGIN from LM
44402:2:50:9183:tm_core.c:TM: Received event TM_PD_EVENT_GPS_DONE from LM
44405:2:50:9241:tm_core.c:TM: Received event TM_PD_EVENT_GPS_IDLE from LM
44450:2:50:11366:tm_core.c:tm_core_get_gps_state] passed invalid paramaters
44451:2:50:11371:tm_core.c:tm_core entering function [tm_core_get_gps_state] source %d
44461:2:50:13306:tm_core.c:tm_core_report_gps_state_info: Rcvd response for request %d from MGP
44861:2:50:2107:tm_pdapi_iface.c:TM: Always allow Unlock(%d) from lock state (%d) for SET PDSM_PA_GPS_LOCK
44862:2:50:2232:tm_pdapi_iface.c:TM: ====== tm_core_convert_seconds_to_gps_time %d %d 
44987:2:50:4907:tm_pdapi_iface.c:TM: SetParam: PDSM_PA_GPS_MIN_WEEK_NUMBER is %u 
45026:2:50:781:tm_ruim.c:tm_ruim_abort_non_e911_agps_session: e911=%d handle=%d!
45027:2:50:788:tm_ruim.c:tm_ruim_abort_non_e911_agps_session
45100:2:50:330:tm_pdapi_iface_no_ship.c:TM: PcsStatus:%d GPS_LOCK:%d, OemCtrl:%d
45213:2:50:302:tm_l1_iface.c:GF_DEBUG: in tm_nr_l1_iface_proc_event(), event type NR5G_ML1_GPS_EVENT_CELL_MEAS_UPDATE=%d, subscriber_flag=%u, UMTS_UP_mask=%u, GM_NR5G_mask=%u
45968:2:50:3672:tm_umts_common_utils.c:TM: tm_umts_common_is_supl_ni_allowed(): GPS_LOCK:%d, OemCtrl:0x%X RetVal:%d
46055:2:50:304:tm_umts_cp_gsm_rrlp.c:CP-GSM: process_ref_time; e_agps_mode=%d
46429:2:50:1292:tm_umts_up_supl_api.c:TM Wishlist: 0x%lX (GPS), 0x%lX (GLONASS) 0x%lX (BDS) agps_mode %d callflow %d
46472:2:50:254:tm_rrlp_up.c:supl_rrlp_gps_assist_proc
46562:2:50:6627:gps_common.c:==== gnss_ConvertSecondsToGpsTime %d %d 
46563:2:50:6646:gps_common.c:==== gnss_ConvertGpsTimeToSeconds %d 
46564:2:50:1322:gps_nv_efs.c:CGPS EFS FILE RESP HANDLER Result = %d, FileOp = %d
46609:2:50:3907:sm_log.c:LOG_CGPS_PDSM_EXT_STATUS_POS_REPORT_C turned off!
47226:4:50:1145:pdapi.c:=PDSM= pdsm_pd_query_gps_state(). CDPtr %p
47591:4:50:6840:lm_mgp.c:CPI injection extern time type= GPS GPS Week = %u, GPS Ms = %u, GPS_Time_valid:%d, UTC Ms %u
47592:4:50:6855:lm_mgp.c:CPI injection extern time type= UTC GPS Week = %u, GPS Ms = %u, UTC ms = %u, GPS_Time_valid:%d
47703:4:50:10684:tm_core.c:CPI injection extern time type= GPS GPS Week = %u, GPS Ms = %u, GPS_Time_valid:%d
47704:4:50:10705:tm_core.c:CPI injection extern time type= UTC GPS Week = %u, GPS Ms = %u, UTC ms = %u, GPS_Time_valid:%d
48413:4:50:3378:tm_lpp_cp.c:TM_LPP_CP: DGPS_CORRECTIONS not supported
48420:4:50:3582:tm_lpp_cp.c:TM_LPP_CP: DGPS_CORRECTIONS (Glo) not supported
48466:4:50:11709:tm_lpp_cp.c:Privacy: Unsupported agps_mode %d
48498:4:50:7756:tm_lpp_up.c:Privacy: Unsupported agps_mode %d
48512:4:50:1214:tm_lpp_up.c:TM_LPP_UP: GPS DGPS_CORRECTIONS not supported
48517:4:50:1398:tm_lpp_up.c:TM_LPP_UP: DGPS_CORRECTIONS (Glo) not supported
48528:4:50:1866:tm_lpp_up.c:TM_LPP_UP: DGPS_CORRECTIONS (BDS) not supported
48668:4:50:3044:tm_umts_cp_gsm.c:Privacy: Unsupported agps_mode %d
48703:4:50:8325:tm_umts_cp_wcdma.c:Privacy: Unsupported agps_mode %d
48704:4:50:4331:tm_umts_cp_wcdma.c:cp_wcdma_encode_rrc_msrReport_gps_meas: Calling BlacklistFilter
48733:4:50:1430:tm_umts_up_supl.c:tm_umts_up_supl_callflow_disrupt_handler   cf_state protocol callflow op_mode agps_mode fix_reported supl_close_waiting
48763:4:50:10827:tm_umts_up_supl.c:SUPL LM gps_sess_timer = %d, SUPL CF timer= %d
48802:4:50:9381:tm_umts_up_supl.c:supl_SuplEnd_proc   cf_state protocol callflow agps_mode fix_reported supl_close_waiting
48877:4:50:1282:tm_umts_up_supl_api.c:Drop Wishlist for agps_mode %d
48933:4:50:3421:tm_rrlp_up.c:Privacy: Unsupported agps_mode %d
48988:4:50:6387:gps_common.c:Filter GNSS SvId: %u (BlackListed)
49008:4:50:996:tm_rrlp.c:tm_rrlp_mprsp_gps_meas_build: Calling BlacklistFilter
49181:8:50:1871:gnss_user.c:sm_user_handler_report_gps_eph_config: unexpected len %d (expected %d)
49198:8:50:2211:gnss_user.c:sm_user_handler_report_min_gps_week: unexpected len %d (expected %d)
49274:8:50:1232:gnss_user.c:gps_user_dispatcher_cb: Unexpected len %d (expected at least %d)
49496:8:50:1225:sm_nv_efs.c:CGPS_NV_EFS_REG: Invalid item id: %d
49498:8:50:1597:sm_nv_efs.c:CGPS_NV_EFS_SET_CACHED: Invalid sub %d
49499:8:50:1872:sm_nv_efs.c:CGPS_NV_EFS_SET_CACHED Invalid item id: %d
49512:8:50:1262:sm_nv_efs.c:CGPS_NV_EFS_GET_CAHCED: Invalid sub %d (Max=%d)
49513:8:50:1563:sm_nv_efs.c:CGPS_NV_EFS_GET_CAHCED: Invalid item id: %d
49945:8:50:186:gm_gps_env_based_backoff.c:GM POS Fix pointer NULL
49946:8:50:294:gm_gps_env_based_backoff.c:NULL Event payload recv
50155:8:50:256:lm_task.c:=LM TASK= Failed reading NV NV_AAGPS_APP_TRACKING_GPSON_THSLD_I setting default(sec) = %d
50156:8:50:270:lm_task.c:=LM TASK= Failed reading NV NV_AAGPS_APP_TRACKING_GPSIDLE_THSLD_I setting default(sec) = %d
50157:8:50:284:lm_task.c:=LM TASK= Failed reading NV NV_AAGPS_APP_TRKG_PERIODIC_REQ_DLY_MARGIN_I setting default(sec) = %d
50233:8:50:4971:lm_mgp.c:lm_translate_gnss_to_gps_assist_data: API should only be used when FEATURE_GPS_GEN7_ME_API is defined
50233:8:50:4971:lm_mgp.c:lm_translate_gnss_to_gps_assist_data: API should only be used when FEATURE_GPS_GEN7_ME_API is defined
50435:8:50:3725:tm_cm_iface.c:p_cgps_ss_info or cp_ss_info_ptr is NULL
50552:8:50:13292:tm_core.c:tm_core_report_gps_state_info: NULL argument received
50553:8:50:13302:tm_core.c:tm_core_report_gps_state_info: NULL pointer argument received
51195:8:50:4487:tm_xtra.c:=TM XTRA= p_gps_time pointer is NULL
51242:8:50:2979:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_ephemeris Error: invalid arguments
51247:8:50:3167:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_almanac Error: invalid arguments
51251:8:50:3279:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_iono_model Error: invalid arguments
51253:8:50:3355:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_utc_model Error: invalid arguments
51255:8:50:3436:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_signal_availability Error: invalid arguments
51257:8:50:3510:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_signal_specific_health Error: invalid arguments
51259:8:50:3627:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_ephemeris Error: invalid arguments
51268:8:50:3849:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_isc Error: invalid arguments
51269:8:50:3911:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_isc Error: Invalid nBytesRead (n_size=%d nBytesRead=%d)
51272:8:50:4014:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_time_convert Error: invalid arguments
51277:8:50:4201:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_health_page Error: invalid arguments
51301:8:50:5187:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_ephemeris Error: invalid arguments
51306:8:50:5397:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_almanac Error: invalid arguments
51420:8:50:10407:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_almanac Error: invalid arguments
51433:8:50:10787:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_isc Error: invalid arguments
51501:8:50:13214:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_ephemeris Error: invalid arguments
51511:8:50:13557:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_utc_model Error: invalid arguments
51534:8:50:14646:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_apc_offset Error: invalid arguments
51565:8:50:15715:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_assist_records Error: invalid arguments
51584:8:50:17106:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_apc_offset Error: invalid arguments
51585:8:50:17151:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_apc_offset: rcvd older (%u) APC data than current (%u)
51608:8:50:17974:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_gps_apc_offset Error: invalid arguments
51653:8:50:575:tm_xtra_data_handler.c:=TM XTRA= tm_xtra_update_gps_xtra_header: NULL pointers found: %p and %p
53049:8:50:7763:tm_umts_up_supl.c:SuplPosInit attempt aborted: gps_mode = %u
53056:8:50:8547:tm_umts_up_supl.c:SuplPosInit attempt aborted: gps_mode = %u
53105:8:50:6925:tm_umts_up_supl.c:posMethod mismatch supl resp:0x%x agps_mode %d pos_prtl %d
53309:8:50:463:gnss_wwan_iface.c:gnss_wwan_iface_l1_cgps_register_event_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53310:8:50:515:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_event_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53310:8:50:515:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_event_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53311:8:50:562:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_ue_pos_capability_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53311:8:50:562:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_ue_pos_capability_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53312:8:50:6996:gps_common.c:gnss_GetQtime(),QTime invalid
53313:8:50:7065:gps_common.c:Invalid MeasBlkSrc %d
53314:8:50:7158:gps_common.c:Invalid PdSignalType %d
53315:8:50:7191:gps_common.c:Invalid MaskValue=0x%08X
53316:8:50:1386:gps_nv_efs.c:could not allocate IPC
53317:8:50:1316:gps_nv_efs.c:CGPS EFS FILE RESP HANDLER Result = %d, FileOp = %d
58517:2:81:2006:tle_ptm_mgr.cpp:GPS_WK:%u, GPS_MSEC:%d, valid:%u
58517:2:81:2006:tle_ptm_mgr.cpp:GPS_WK:%u, GPS_MSEC:%d, valid:%u
60448:8:81:849:tle_nv_mgr.cpp:CGPS_NV_EFS_REG: Invalid item id: %d
66766:2:86:5968:loc_pd.c:locPd_EventCb, PDSM_PD_EVENT_GPS_BEGIN
66767:2:86:5974:loc_pd.c:locPd_EventCb, PDSM_PD_EVENT_GPS_DONE
66794:2:86:8877:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GPS_L1CA metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66795:2:86:8891:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GPS_L2C metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66796:2:86:8907:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GPS_L5 metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
67817:4:86:1108:loc_xtra.c:locXtra_ProcessDataValidityEvent: start_gps_week=%d, start_gps_minute=%d, valid_duration=%d
67817:4:86:1108:loc_xtra.c:locXtra_ProcessDataValidityEvent: start_gps_week=%d, start_gps_minute=%d, valid_duration=%d
114688:2:4606:205:cmapi_gps_user.c:TimeDiffMsec: GPS time diff %d %u
114689:2:4606:210:cmapi_gps_user.c:TimeDiffMsec: Qtime diff %d %u
114690:2:4606:214:cmapi_gps_user.c:TimeDiffMsec: GPS time and Qtime invalid
115957:8:4606:162:cmapi_mm_info_cb_user.cpp:user_handler_mm_register_cgps_event_cb: unexpected len %d (expected at least %d)
116024:8:4606:113:cmapi_services_gs_user.cpp:user_handler_gsclassmark_register_cgps_cb: unexpected len %d (expected at least %d)
116132:8:4606:465:cmapi_wcdma_user.c:cmapi_wcdma_rrc_gps_event_data_cb_params: invalid driver data argument
116133:8:4606:471:cmapi_wcdma_user.c:cmapi_wcdma_rrc_gps_event_data_cb_params: unexpected len %d (expected %d)
116136:8:4606:523:cmapi_wcdma_user.c:cmapi_wcdma_l1_gps_event_data_cb_params: invalid driver data argument
116137:8:4606:529:cmapi_wcdma_user.c:cmapi_wcdma_l1_gps_event_data_cb_params: unexpected len %d (expected %d)
116138:8:4606:198:cmapi_gps_user.c:TimeDiffMsec: Null Pointers %u %u %u
116139:8:4606:150:cmapi_gps_user.c:TimeDiffMs: Null Pointers %u %u %u
116140:8:4606:95:cmapi_gps_user.c:Qtime Diff - Null Pointers
116141:8:4606:102:cmapi_gps_user.c:Qtime Diff - Invalid inputs %u %u
4163993478:298|9a55ac6b:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_loc_client_ind_handler - Locked
4163993561:319|ad6aa378:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_loc_client_ind_handler - sessionStatus: 0x%02x, enable int: %d, valid fix: %d, mode: %d
4163993700:512|6663d035:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_loc_client_resp_cb msg id %d
4163994690:872|c46b00ab:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_start_fresh_position_req: status %d timeout=%d req=0x%04x req_err=0x%02x, enable int %d
4163994829:923|5c86c4f6:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_stop_fresh_position_req: status %d req 0x%04x req_err=0x%02x
4163994941:971|3245521d:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_start_listen_position_req: status %d req 0x%04x req_err=0x%02x
4163995055:1019|985f5405:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_stop_listen_position_req: status %d req 0x%04x req_err=0x%02x
4163995495:1342|911674e4:2:PM:I policyman_gps.c:POLICYMAN:policyman_gps_sleep_timer_expiry called
4165325849:101:2:MM:mm_lsm.c:MM:DS: SUB %d =MM= MM sent MM_CGPS_MSG_EVENT_RESET_UE_POS_INFO to LSM
4165325937:106:3:MM:mm_lsm.c:MM:DS: SUB %d =MM= mm_cgps_event_cb event not initialized by LCS
4165326098:211:3:MM:mm_lsm.c:MM:DS: SUB %d =MM= mm_cgps_event_cb event not initialized by LCS
4165477590:9545:3:MM:emm_utility.c:MM:DS: SUB %d Unable to access NV to read NV_AAGPS_MT_LRSUPPORT_I, resetting LPP
4165502754:124:3:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= unknown message from GPS module, discarding it
4165502848:128:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= received UL_GENERIC_NAS_TRANSPORT
4165502929:133:3:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= Spec version '%d',drop this message
4165503012:161:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= GPS UL_MSG_REQ received while not in LTE or NTN rrc_active %d on RAT %d
4165503131:261:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= Cache UL GENERIC NAS Message
4165503207:296:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= free the UL GENERIC NAS msg dsm items %d rrc active %d emm_substate %d
4165503325:312:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= Cache UL Generic NAS message emm_state %d
4165503414:337:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= free the UL GENERIC NAS msg dsm items %d emm_state %d
4165503515:380:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= received DL_GENERIC_NAS_TRANSPORT
4165503596:385:3:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= Spec version '%d',drop this message
4165503679:445:2:MM:emm_gps_handler.c:MM:DS: SUB %d =EMM= NAS_EMM_DL_GENERIC_NAS_TRANSPORT_IND : Success(0)/Failure(non Zero) = %d
4165503799:97:2:MM:emm_gps_lib.c:MM:DS: SUB %d =EMM= Length of Message Container = %d
4165503874:125:2:MM:emm_gps_lib.c:MM:DS: SUB %d =EMM= Length of Additional Information = %d
4165503955:211:2:MM:emm_gps_lib.c:MM:DS: SUB %d =EMM= Sending GENERIC_NAS_TRANSPORT_FAILURE_IND: Cause: %d, %d, Trans ID: 0x%x
4165504071:253:3:MM:emm_gps_lib.c:MM:DS: SUB %d =EMM= emm_msgr_send failed with cause %d
4165504149:327:2:MM:emm_gps_lib.c:MM:DS: SUB %d =EMM= UL_GENERIC_NAS_TRANSPORT succeded
4165899814:118:2:MM:mm5g_gps_handler.c:MM:DS: SUB %d =MM5G= GPS UL_MSG_REQ received with msg id %d RAT %d, container type %d, rrc_active %din mm5g_state %d, mm5g_substate %d, connection state %d
4165899998:355:2:MM:mm5g_gps_handler.c:MM:DS: SUB %d =MM5G= received DL_NAS_TRANSPORT_MSG with additional info of length %d
4165900111:421:2:MM:mm5g_gps_handler.c:MM:DS: SUB %d =MM5G= NAS_EMM_DL_GENERIC_NAS_TRANSPORT_IND status = %d
4165900209:426:2:MM:mm5g_gps_handler.c:MM:DS: SUB %d =MM5G= Wrong state to process SMS DL NAS Transport: mm5g_state=%d, update_status=%d
4165900335:96:2:MM:mm5g_gps_lib.c:MM:DS: SUB %d =MM5G= Length of Message Container=%d
4165900410:123:2:MM:mm5g_gps_lib.c:MM:DS: SUB %d =MM5G= Length of Additional Information=%d
4165900491:203:2:MM:mm5g_gps_lib.c:MM:DS: SUB %d =MM5G= Sending LPP_GENERIC_NAS_TRANSPORT_FAILURE_IND: Cause type=%d, Cause=%d, Trans ID: 0x%x
4165900623:241:2:MM:mm5g_gps_lib.c:MM:DS: SUB %d =MM5G= Status for sending NAS_EMM_GENERIC_NAS_TRANSPORT_FAILURE_IND=%d
4165900732:276:2:MM:mm5g_gps_lib.c:MM:DS: SUB %d =MM5G= LPP_UL_NAS_TRANSPORT (trans_id=%d) failed with cause: rrc_failure_cause=%d, proc_type=%d, ota_cause=%d, proc_failure=%d, local_cause=%d
4165900913:446:2:MM:mm5g_gps_lib.c:MM:DS: SUB %d =MM5G= LPP_UL_NAS_TRANSPORT succeded
4165900988:454:2:MM:mm5g_gps_lib.c:MM:DS: SUB %d =MM5G= NAS_EMM_UL_GENERIC_NAS_TRANSPORT_CNF send status=%d
4166643741:2162:2:CM:cmipcall.c:=CM=:cmemgps_proc_sip380 emerg_srv_categ %d emerg_service_urn len %d is_emerg_on_other_sub: %d
4166688412:146:2:PM:policyman_qsh.c:POLICYMAN:Received QSH INJECT_GPS_FIX mode %d
4166706252:ssscr_user_ss_pref_gps_end

```

## MSG_GNSS

```



7209:8:7:2754:gps_common.c:gnss_ConvertGpsTime2GloTime: NULL pointer!
11318:2:19:6103:gps_common.c:GNSS-LE validation failed, HEPE:%lu, threshold:%lu, quality:%d
11525:2:19:927:gnss_gdt.c:GDT: gdt_SendBegin()
11526:2:19:933:gnss_gdt.c:GDT: Invalid params
11527:2:19:1000:gnss_gdt.c:GDT: gdt_SendData()
11528:2:19:1003:gnss_gdt.c:GDT: Invalid params
11529:2:19:1014:gnss_gdt.c:GDT: Invalid params
11530:2:19:1056:gnss_gdt.c:GDT: Send buffer already locked. Cannot send until buffer unlocked
11531:2:19:1112:gnss_gdt.c:GDT: gdt_ProtectedExtendedSend()
11532:2:19:1118:gnss_gdt.c:GDT: Invalid params
11533:2:19:1203:gnss_gdt.c:GDT: gdt_SendEnd()
11534:2:19:1209:gnss_gdt.c:GDT: Invalid params
11535:2:19:1274:gnss_gdt.c:GDT: gdt_SendOpen()
11536:2:19:1280:gnss_gdt.c:GDT: Invalid params
11537:2:19:1286:gnss_gdt.c:GDT: Invalid Function Callback - Open
11538:2:19:1327:gnss_gdt.c:GDT: file descriptor = %d
11539:2:19:1376:gnss_gdt.c:GDT: gdt_SendClose()
11540:2:19:1382:gnss_gdt.c:GDT: Invalid params
11541:2:19:1388:gnss_gdt.c:GDT: Invalid Function Callback - Close
11542:2:19:1463:gnss_gdt.c:GDT: gdt_ReceiveBegin()
11543:2:19:1469:gnss_gdt.c:GDT: Invalid params
11544:2:19:1548:gnss_gdt.c:GDT: gdt_ReceiveOpen()
11545:2:19:1554:gnss_gdt.c:GDT: Invalid params
11546:2:19:1560:gnss_gdt.c:GDT: Invalid Function Callback - ReceiveOpen
11547:2:19:1603:gnss_gdt.c:GDT: file descriptor = %d
11548:2:19:1654:gnss_gdt.c:GDT: gdt_ReceiveData()
11549:2:19:1658:gnss_gdt.c:GDT: Invalid params
11550:2:19:1669:gnss_gdt.c:GDT: Invalid params
11551:2:19:1709:gnss_gdt.c:GDT: Rcv buffer already locked. 
11552:2:19:1770:gnss_gdt.c:GDT: gdt_ProtectedExtendedReceive()
11553:2:19:1777:gnss_gdt.c:GDT: Invalid params
11554:2:19:1790:gnss_gdt.c:GDT: Invalid file descriptor
11555:2:19:1905:gnss_gdt.c:GDT: Invalid GDT Service - ReceiveData
11556:2:19:1911:gnss_gdt.c:GDT: Invalid Function Callback - ReceiveData
11557:2:19:1991:gnss_gdt.c:GDT: gdt_ReceiveClose(), status:%d
11558:2:19:1997:gnss_gdt.c:GDT: Invalid params
11559:2:19:2003:gnss_gdt.c:GDT: Invalid Function Callback - ReceiveClose
11560:2:19:2096:gnss_gdt.c:GDT: gdt_ReceiveEnd()
11561:2:19:2102:gnss_gdt.c:GDT: Invalid params
11562:2:19:2194:gnss_gdt.c:GDT: gdt_BufAlloc(), requesting buffer size = %d
11563:2:19:2198:gnss_gdt.c:GDT: Invalid params
11564:2:19:2263:gnss_gdt.c:GDT: gdt_BufFree()
11565:2:19:2267:gnss_gdt.c:GDT: Invalid params
11566:2:19:2320:gnss_gdt.c:GDT: gdt_RcvdSendBeginResponse(), serviceId:%d, sessId:%lu, status:%d
11567:2:19:2335:gnss_gdt.c:GDT: Invalid GDT Service - Start
11568:2:19:2342:gnss_gdt.c:GDT: Invalid Function Callback - Start
11569:2:19:2396:gnss_gdt.c:GDT: gdt_RcvdSendEndResponse(), serviceId:%d, sessId:%lu, status:%d
11570:2:19:2418:gnss_gdt.c:GDT: Invalid GDT Service - End
11571:2:19:2424:gnss_gdt.c:GDT: Invalid Function Callback - End
11572:2:19:2487:gnss_gdt.c:GDT: gdt_RcvdSendAckResponse()
11573:2:19:2509:gnss_gdt.c:GDT: Invalid GDT Service - SendAck
11574:2:19:2515:gnss_gdt.c:GDT: Invalid Function Callback - SendAck
11575:2:19:2570:gnss_gdt.c:GDT: gdt_RcvdReceiveReadyResponse(), serviceId:%d, sessId:%lu, status:%d
11576:2:19:2583:gnss_gdt.c:GDT: Invalid GDT Service - ReceiveReady
11577:2:19:2589:gnss_gdt.c:GDT: Invalid Function Callback - ReceiveReady
11578:2:19:2633:gnss_gdt.c:GDT: gdt_RcvdReceiveCloseResponse()
11579:2:19:2645:gnss_gdt.c:GDT: gdt_RcvdReceiveCloseResponse(), serviceId:%d, sessId:%lu, status:%d
11580:2:19:2658:gnss_gdt.c:GDT: Invalid GDT Service - ReceiveClose
11581:2:19:2664:gnss_gdt.c:GDT: Invalid Function Callback - ReceiveClose
11582:2:19:2720:gnss_gdt.c:GDT: gdt_RcvdReceiveEndResponse(), serviceId:%d, sessId:%lu, status:%d
11583:2:19:2733:gnss_gdt.c:GDT: Invalid GDT Service - ReceiveEnd
11584:2:19:2739:gnss_gdt.c:GDT: Invalid Function Callback - ReceiveEnd
11585:2:19:258:gnss_gdt.c:GDT: gdt_Init()
11586:2:19:731:gnss_gdt.c:GDT: gdt_GetSessionValidity()
11587:2:19:737:gnss_gdt.c:GDT: Mismatching session ID, Existing sessID:%lu, injected sessId:%lu
11588:2:19:777:gnss_gdt.c:GDT: gdt_SetSessionIdAndValidity() Valid:%d
11589:2:19:791:gnss_gdt.c:GDT: Session ID (%d) for service (%d) is now %d (valid=1)
11590:2:19:690:gnss_gdt.c:GDT: gdt_CheckSessionId() - session (%d) vs table (%d)
11591:2:19:567:gnss_gdt.c:GDT: gdt_GetFileDescriptorAndValidity()
11592:2:19:584:gnss_gdt.c:GDT: File descriptor for service (%d) retrieved: %d, validity (%d)
11593:2:19:439:gnss_gdt.c:GDT: gdt_GetBuf()
11594:2:19:446:gnss_gdt.c:GDT: Invalid params
11595:2:19:483:gnss_gdt.c:GDT: gdt_GetLockStatus()
11596:2:19:495:gnss_gdt.c:GDT: Current lock status: %d
11597:2:19:522:gnss_gdt.c:GDT: gdt_SetLockStatus() - lock status = %u
11598:2:19:534:gnss_gdt.c:GDT: Lock status for service (%d) changed to: %d
11599:2:19:617:gnss_gdt.c:GDT: gdt_SetFileDescriptorAndValidity()
11600:2:19:632:gnss_gdt.c:GDT: File descriptor for service (%d) changed to: %d, validity (%d)
11601:2:19:340:gnss_gdt.c:GDT: gdt_RegisterBuf()
11602:2:19:353:gnss_gdt.c:GDT: Buffer registered with size (%u)
11603:2:19:354:gnss_gdt.c:GDT: Buffer registered = 0x%x
11604:2:19:384:gnss_gdt.c:GDT: gdt_UnregisterBuf()
11605:2:19:397:gnss_gdt.c:GDT: Provided buffer does not match handle or buffer still locked.
11606:2:19:400:gnss_gdt.c:p_ByteBuffer = 0x%x, gdt_BufTable.p_MemHandle = 0x%x, lock = %u
11607:2:19:405:gnss_gdt.c:GDT: Buffer unregistered
11608:2:19:314:gnss_hal.cpp:Heap usage in module:%u, usage[Bytes]:%lu, Status:%u
12320:4:19:94:gnss_lpp_ecid.c:GNSS_LPP_ECID: GET Rx-Tx measurements CMD. Sess type %d, meas_type %d, sub %d
12320:4:19:94:gnss_lpp_ecid.c:GNSS_LPP_ECID: GET Rx-Tx measurements CMD. Sess type %d, meas_type %d, sub %d
12321:4:19:108:gnss_lpp_ecid.c:GNSS_LPP_ECID: IPC msg send to WWANME, IPC sent = %u
12321:4:19:108:gnss_lpp_ecid.c:GNSS_LPP_ECID: IPC msg send to WWANME, IPC sent = %u
12322:4:19:97:gnss_nr_ecid_noship.c:NR5G ECID: GET ECID meas. Sess type %d meas_type %d sub %d success %u
12328:4:19:5788:gps_common.c:gnss_SendPdrControl: Sending PDR control flag %u from source %u
12329:4:19:5907:gps_common.c:gnss_UpdatePdrControlDb: Accepting new PDR control, input flag %u, source %u, new flag %u
12351:4:19:945:gnss_gdt.c:GDT: Cannot call SendBegin while another session is ongoing
12352:4:19:1033:gnss_gdt.c:p_TempHandle = 0x%x , cpz_GdtIface->z_GdtBuffer = 0x%x
12353:4:19:1034:gnss_gdt.c:GDT: Buffer mismatch. Incorrect buffer supplied.
12354:4:19:1040:gnss_gdt.c:GDT: Data size is greater than allocated buffer.
12355:4:19:1151:gnss_gdt.c:Failed rfs write with result = %d
12356:4:19:1158:gnss_gdt.c:rfs write successful
12357:4:19:1480:gnss_gdt.c:GDT: Cannot call ReceiveBegin while another session is ongoing
12358:4:19:1687:gnss_gdt.c:GDT: Buffer mismatch. Incorrect buffer supplied.
12359:4:19:1693:gnss_gdt.c:GDT: Receive buffer size is not the same as what was allocated.
12360:4:19:1917:gnss_gdt.c:Failed rfs read with result = %d
12361:4:19:1931:gnss_gdt.c:rfs read successful
12362:4:19:2014:gnss_gdt.c:File descriptor invalid
12363:4:19:2210:gnss_gdt.c:GDT: Buffer for this service has already been allocated.
12364:4:19:2222:gnss_gdt.c:GDT: Buffer size (%d) not allocated.
12365:4:19:3193:gnss_gdt.c:GDT: Logging of format %u is not supported
12366:4:19:3200:gnss_gdt.c:GDT: Logging is not enabled
12367:4:19:3211:gnss_gdt.c:GDT: Logging Failed Data:%p, len:%lu 
12368:4:19:3230:gnss_gdt.c:GDT: Logging of format totalParts:%u, service:%d, len:%lu 
12369:4:19:3240:gnss_gdt.c:GDT: Log allocation failed 
13499:8:19:121:gnss_nr_ecid_noship.c:NR5G ECID: Receive ECID meas. Null ptr
13507:8:19:1907:gps_common.c:NULL access in gnss_GloMeasBlkGGRfgdAdjust()
13515:8:19:2930:gps_common.c:GNSS/NAVIC: can't convert GPS time to NAVIC time, parameters invalid
13516:8:19:3050:gps_common.c:GNSS/COMMON LogPosRpt: null pointer %u %u %u
13517:8:19:3712:gps_common.c:gnss_LogPositionReport: Input NULL pointer
13518:8:19:3737:gps_common.c:Log packet allocation for GNSS Position Report Failed
13523:8:19:5305:gps_common.c:Bad GNSS type %u
13525:8:19:5376:gps_common.c:GNSS/COMMON: Null pointer access inwgs84_Ecef2LlaGet() %u %u
13526:8:19:5418:gps_common.c:GNSS/COMMON: Null pointer access ingnss_Wgs84EcefToDtmEcef() %u %u
13526:8:19:5418:gps_common.c:GNSS/COMMON: Null pointer access ingnss_Wgs84EcefToDtmEcef() %u %u
13527:8:19:5490:gps_common.c:GNSS/COMMON: Null pointer access ingnss_Ecef2LlaGet() %u %u
13527:8:19:5490:gps_common.c:GNSS/COMMON: Null pointer access ingnss_Ecef2LlaGet() %u %u
13528:8:19:5527:gps_common.c:GNSS/COMMON: Invalid Datum typein gnss_Ecef2LlaGet %u
13528:8:19:5527:gps_common.c:GNSS/COMMON: Invalid Datum typein gnss_Ecef2LlaGet %u
13529:8:19:5795:gps_common.c:gnss_SendPdrControl: Sending PDR control flag to LE failed
13530:8:19:5801:gps_common.c:gnss_SendPdrControl: Sending PDR control flag to SAML failed
13531:8:19:5831:gps_common.c:gnss_InitPdrControlDb: Input pointer NULL
13532:8:19:5873:gps_common.c:gnss_UpdatePdrControlDb: InputPr pointer NULL
13533:8:19:5880:gps_common.c:gnss_UpdatePdrControlDb: Invalid PDR control source, flag %u, source %u
13536:8:19:6349:gps_common.c:GNSSMASK: invalid mask pointer.
13542:8:19:3632:gps_common.c:GNSS/COMMON CountAvailMeas: null pointer %u %u %u %u %u
13543:8:19:3385:gps_common.c:GNSS/COMMON LogPosRpt: null pointer %u %u %u %u %u
13758:8:19:7104:aries_gpsdiag.c:gpsdiag_GNSSDiagBufferTest - response packet not allocated!
13759:8:19:172:gnss_hal.cpp:Heap overrun, Module:%u, InUseSize:%lu, thisSize:%lu
13760:8:19:237:gnss_hal.cpp:Heap overrun, Module:%u, InUseSize:%lu, thisSize:%lu
14336:16:19:285:gnss_hal.cpp:Serious error, Failed to get HAL instance
28673:4:46:4889:gps_common.c:TT Dal Inteface initialized for GNSS: %d
30727:4:47:6890:mgp_me_api.c:GNSS Nav config 1 0x%02X Des 0x%02X
30728:4:47:6900:mgp_me_api.c:GNSS Nav config Cur 0x%02X Des 0x%02X
30730:4:47:9526:mgp_me_api.c:me_BdsB2aGnssTimeOffsetPut called, p_Msg 0x%x
30731:4:47:9541:mgp_me_api.c:me_BdsB2aGnssTimeOffsetPut os_IpcSend failed
31790:8:47:5600:mc_api.c:Invalid gnss type: %d
34828:2:48:1938:os_api_gnss_socket.c:create socket (%d %d) handle %d scb %d
34829:2:48:2031:os_api_gnss_socket.c:Could not create control socket. error %d
34830:2:48:2058:os_api_gnss_socket.c:connect socket (%d) ctrl sock handle %d
34831:2:48:2132:os_api_gnss_socket.c:socket (%d) destroy message sent
34832:2:48:2221:os_api_gnss_socket.c:socket (%d) sendData %d seqNum message sent
34833:2:48:2229:os_api_gnss_socket.c:Failed to send message to socket! Error %d
34834:2:48:2386:os_api_gnss_socket.c:Client (%d) sendData %d seqNum %dmessage sent
34835:2:48:2392:os_api_gnss_socket.c:Failed to send message to socket! Error %d
34836:2:48:2522:os_api_gnss_socket.c:looked up server socket address!!! %d %d %d
34837:2:48:2584:os_api_gnss_socket.c:looked up MPSS server socket address!!! %d %d %d
34838:2:48:3111:os_api_gnss_socket.c:Starting GNSS Socket task ...!!
34838:2:48:3111:os_api_gnss_socket.c:Starting GNSS Socket task ...!!
34839:2:48:3129:os_api_gnss_socket.c:Start qPoll on %d sockFDs
34840:2:48:3167:os_api_gnss_socket.c:sockfd %d rcvd event %d
34841:2:48:3184:os_api_gnss_socket.c:sockfd %d rcvd event %d
34842:2:48:516:os_api_gnss_socket.c:Client Ctrl Block %d not found!
34843:2:48:603:os_api_gnss_socket.c:No Free sock control blocks!
34844:2:48:559:os_api_gnss_socket.c:No Free sock control blocks!
34845:2:48:404:os_api_gnss_socket.c:lookup Client socket!! Service[%d]:Instance[%d]
34846:2:48:430:os_api_gnss_socket.c:looked up Client socket address!!! %d %d %d %d
34847:2:48:447:os_api_gnss_socket.c:Saved Client socket address!!! %d %d %d %d
34848:2:48:310:os_api_gnss_socket.c:v_Initialized:%d l_clientSockHdl %d l_numSockFds %d sockFD:%d %d %d %d %d Error %d
34849:2:48:316:os_api_gnss_socket.c:MPSS Server Socket FD: %d InitError %d, ServerSockError %d
34850:2:48:2835:os_api_gnss_socket.c:Process message of size %d
34851:2:48:2742:os_api_gnss_socket.c:Process Sock Cmd %d
34852:2:48:863:os_api_gnss_socket.c:looked up service socket address!!! %d %d %d
34853:2:48:885:os_api_gnss_socket.c:SockHdl %d fd %d ctrl_fd %d Ready to connect to Svc %d!!!
34854:2:48:892:os_api_gnss_socket.c:SvcLookup Socket started operation!
34855:2:48:909:os_api_gnss_socket.c:SvcLookup socket added to qpoll array at index %d total %d
34856:2:48:1308:os_api_gnss_socket.c:Connect SockHdl %d to Svc (%d %d)
34857:2:48:1316:os_api_gnss_socket.c:Family %d addrtype %d nodeid %d portid %d
34858:2:48:2923:os_api_gnss_socket.c:handle event %x recevied for socket %d
34859:2:48:480:os_api_gnss_socket.c:MPSS client not found for MSG [%d]!
34860:2:48:1634:os_api_gnss_socket.c:No events received!!
34861:2:48:1638:os_api_gnss_socket.c:Find SCB for FD %d!!
34862:2:48:1660:os_api_gnss_socket.c:Handle Ctrl Sock Event SockHdl %d
34863:2:48:1667:os_api_gnss_socket.c:Handle Client Sock Event SockHdl %d events 0x%x
34864:2:48:358:os_api_gnss_socket.c:Socket Handle  %d not found!
34865:2:48:1443:os_api_gnss_socket.c:Rcvd Events Mask 0x%x sockfd %d
34866:2:48:1461:os_api_gnss_socket.c:Rcvd MSG Size %d expected 0%d
34867:2:48:1588:os_api_gnss_socket.c:Rcvd MSG Size %d expected 0%d
34868:2:48:1116:os_api_gnss_socket.c:handle event %x recevied for socket %d
34869:2:48:245:gnss_common_nv.c:gnss_ReadNonStdConfig, err = %d
34869:2:48:245:gnss_common_nv.c:gnss_ReadNonStdConfig, err = %d
34870:2:48:296:gnss_common_nv.c:gnss_ReadPowerDebugConfig not initialized
34870:2:48:296:gnss_common_nv.c:gnss_ReadPowerDebugConfig not initialized
34871:2:48:308:gnss_common_nv.c:gnss_ReadPowerDebugConfig not read
34871:2:48:308:gnss_common_nv.c:gnss_ReadPowerDebugConfig not read
34872:2:48:313:gnss_common_nv.c:gnss_ReadPowerDebugConfig was read properly
34872:2:48:313:gnss_common_nv.c:gnss_ReadPowerDebugConfig was read properly
34873:2:48:317:gnss_common_nv.c:gnss_ReadPowerDebugConfig MCPM Mode was set
34873:2:48:317:gnss_common_nv.c:gnss_ReadPowerDebugConfig MCPM Mode was set
34874:2:48:365:gnss_common_nv.c:gnss_ReadLocWwanDcCfgNv, err = %d
34874:2:48:365:gnss_common_nv.c:gnss_ReadLocWwanDcCfgNv, err = %d
35850:4:48:1742:os_api_gnss_socket.c:Client Control Block Index [%d]
35851:4:48:1818:os_api_gnss_socket.c:Client ID not supported
35852:4:48:2367:os_api_gnss_socket.c:Client Addr Service [%d], instance [%d]
35853:4:48:2373:os_api_gnss_socket.c:SendTo Addr Node [%d], Port [%d]
35854:4:48:2943:os_api_gnss_socket.c:Process message of size %d
35855:4:48:2961:os_api_gnss_socket.c:Process message of size %d
35856:4:48:3010:os_api_gnss_socket.c:Intended Client_ID %d
35857:4:48:3024:os_api_gnss_socket.c:Routing Message ID %d, ClientCtrlBlk index [%d], Owner thread [%d]
35858:4:48:1547:os_api_gnss_socket.c:SCB %d Waiting for Svc (%d %d). Service exit Evt rcvd for Svc (%d %d)
35859:4:48:1177:os_api_gnss_socket.c:Process message of size %d
35860:4:48:1195:os_api_gnss_socket.c:Process message of size %d
36868:8:48:635:os_api_gnss_socket.c:Could not find socket handle %d
36869:8:48:1726:os_api_gnss_socket.c:Invalid MPSS Client ID!
36870:8:48:1737:os_api_gnss_socket.c:Failed to find Client Control Block Index [%d]
36871:8:48:1788:os_api_gnss_socket.c:Invalid MPSS Client ID!
36872:8:48:1796:os_api_gnss_socket.c:Failed to get Client Ctrl block!
36873:8:48:1881:os_api_gnss_socket.c:Bad parameters %d %d
36874:8:48:1890:os_api_gnss_socket.c:No free socket controlblock!
36875:8:48:1903:os_api_gnss_socket.c:Could not create socket (%d %d) error %d
36876:8:48:1929:os_api_gnss_socket.c:Could not send IPC message to GNSS Socket Task, Error: (%d)
36876:8:48:1929:os_api_gnss_socket.c:Could not send IPC message to GNSS Socket Task, Error: (%d)
36877:8:48:2005:os_api_gnss_socket.c:Bad parameters %d %d or NULL pointer %d
36878:8:48:2011:os_api_gnss_socket.c:Bad gnssSockHdl %d
36878:8:48:2011:os_api_gnss_socket.c:Bad gnssSockHdl %d
36879:8:48:2021:os_api_gnss_socket.c:Bad socket state %d %d
36880:8:48:2050:os_api_gnss_socket.c:Could not send IPC message to GNSS Socket Task, Error: (%d)
36880:8:48:2050:os_api_gnss_socket.c:Could not send IPC message to GNSS Socket Task, Error: (%d)
36881:8:48:2103:os_api_gnss_socket.c:Bad socket handle! %d
36882:8:48:2111:os_api_gnss_socket.c:Bad socket handle! %d currently unused
36883:8:48:2128:os_api_gnss_socket.c:Could not send IPC message to GNSS Socket Task, Error: (%d)
36883:8:48:2128:os_api_gnss_socket.c:Could not send IPC message to GNSS Socket Task, Error: (%d)
36884:8:48:2194:os_api_gnss_socket.c:Null pointer! %d 0 data %d to send. sock handle %d
36885:8:48:2201:os_api_gnss_socket.c:Bad socket handle! %d
36886:8:48:2209:os_api_gnss_socket.c:Bad socket state! %d for handle %d
36887:8:48:2339:os_api_gnss_socket.c:Null pointer! %d 0 data %d to send. Client ID %d
36888:8:48:2349:os_api_gnss_socket.c:Failed to find Client Control Block Index [%d]
36889:8:48:2354:os_api_gnss_socket.c:Client Control Block Index [%d]
36890:8:48:2359:os_api_gnss_socket.c:Failed to get remote addr for Client Control Block Index [%d]
36891:8:48:2455:os_api_gnss_socket.c:Failed creating socket!! %d %d
36892:8:48:2464:os_api_gnss_socket.c:Failed creating MPSS Server socket!! %d
36893:8:48:2494:os_api_gnss_socket.c:Failed to bind socket!! %d
36894:8:48:2512:os_api_gnss_socket.c:Failed to lookup server socket!! %d
36895:8:48:2533:os_api_gnss_socket.c:Failed to connect sockets!! %d
36896:8:48:2555:os_api_gnss_socket.c:Failed to bind MPSS Server socket!! %d
36897:8:48:2574:os_api_gnss_socket.c:Failed to lookup MPSS server socket!! %d
36898:8:48:2591:os_api_gnss_socket.c:Failed to create mutex!!
36899:8:48:2603:os_api_gnss_socket.c:Error closing Socket Handles!! %d %d
36900:8:48:2612:os_api_gnss_socket.c:Error closing MPSS Socket Handles!! %d 
36901:8:48:2668:os_api_gnss_socket.c:Unexpected Thread ID Error for GNSS Socket Task %d
36901:8:48:2668:os_api_gnss_socket.c:Unexpected Thread ID Error for GNSS Socket Task %d
36902:8:48:2698:os_api_gnss_socket.c:Unexpected Thread ID Error for GNSS MPSS Server Socket Task %d
36902:8:48:2698:os_api_gnss_socket.c:Unexpected Thread ID Error for GNSS MPSS Server Socket Task %d
36903:8:48:3115:os_api_gnss_socket.c:Failed to init task!!
36904:8:48:3141:os_api_gnss_socket.c:Error returned from qPoll : %d
36905:8:48:389:os_api_gnss_socket.c:lookup Client socket NULL Ptr
36906:8:48:417:os_api_gnss_socket.c:Failed to lookup Client socket!! %d
36907:8:48:2808:os_api_gnss_socket.c:Unhandled event! 0x%x
36908:8:48:2819:os_api_gnss_socket.c:Unexpected event! 0x%x
36909:8:48:2841:os_api_gnss_socket.c:message size too large! Drop on the floor %d
36910:8:48:2860:os_api_gnss_socket.c:Unsupported message size! Drop on the floor %d
36911:8:48:2872:os_api_gnss_socket.c:Error processing Msg on internal server socket %d
36912:8:48:2737:os_api_gnss_socket.c:Null pointer
36913:8:48:714:os_api_gnss_socket.c:Null pointer
36914:8:48:722:os_api_gnss_socket.c:Bad params %d %d
36915:8:48:733:os_api_gnss_socket.c:Bad Gnss sock hdl %d %d %d
36915:8:48:733:os_api_gnss_socket.c:Bad Gnss sock hdl %d %d %d
36916:8:48:742:os_api_gnss_socket.c:Inconsistent socket state %d!!!
36917:8:48:1010:os_api_gnss_socket.c:Null pointer
36918:8:48:1020:os_api_gnss_socket.c:Bad Gnss sock hdl %d threadID %d
36918:8:48:1020:os_api_gnss_socket.c:Bad Gnss sock hdl %d threadID %d
36919:8:48:1032:os_api_gnss_socket.c:Mismatched thread IDs for socket %d threadID %d threadID %d
36920:8:48:1040:os_api_gnss_socket.c:Error closing Socket Handle!! %d
36921:8:48:1049:os_api_gnss_socket.c:Error closing Socket Handle!! %d
36922:8:48:800:os_api_gnss_socket.c:Null pointer
36923:8:48:810:os_api_gnss_socket.c:Bad params %d %d
36924:8:48:821:os_api_gnss_socket.c:Bad Gnss sock hdl %d %d
36924:8:48:821:os_api_gnss_socket.c:Bad Gnss sock hdl %d %d
36925:8:48:835:os_api_gnss_socket.c:Inconsistent socket state %d!!!
36926:8:48:854:os_api_gnss_socket.c:Failed to lookup socket!! %d
36927:8:48:917:os_api_gnss_socket.c:Could not add svclookup socket! no empty slot!
36928:8:48:1300:os_api_gnss_socket.c:Null pointer
36929:8:48:1326:os_api_gnss_socket.c:Svc connect status %d
36930:8:48:1339:os_api_gnss_socket.c:Could not add data socket! no empty slot! numSockFds = %d
36931:8:48:1351:os_api_gnss_socket.c:SockFD array at %d already occupied by sockfd %d! Overwrite!!
36932:8:48:1374:os_api_gnss_socket.c:Could not allocate IPC MSG
36933:8:48:1391:os_api_gnss_socket.c:Failed to send IPC mesage
36934:8:48:2972:os_api_gnss_socket.c:Could not allocate IPC MSG
36935:8:48:2993:os_api_gnss_socket.c:Failed to rcv dgram. Error %d
36936:8:48:3006:os_api_gnss_socket.c:Failed to find service client ID [%d], MsgID [%d]
36937:8:48:3017:os_api_gnss_socket.c:Failed to route Message ID %d, ClientCtrlBlk index [%d]
36938:8:48:3043:os_api_gnss_socket.c:Failed to send IPC mesage
36939:8:48:3049:os_api_gnss_socket.c:ThreadID [%d] out of bounds
36940:8:48:1628:os_api_gnss_socket.c:Null pointer
36941:8:48:1645:os_api_gnss_socket.c:Could not find socket handle %d
36942:8:48:1675:os_api_gnss_socket.c:scbIdx %d does not match control sockFd %d or clientSockFD %d
36943:8:48:1680:os_api_gnss_socket.c:ThreadID [%d] out of bound
36944:8:48:1449:os_api_gnss_socket.c:Unexpected event received 0x%x expected 0x%x
36945:8:48:1481:os_api_gnss_socket.c:Service ID mismatch expected (%d %d) got (%d %d)
36946:8:48:1498:os_api_gnss_socket.c:No Hangup/Svc Remove msg received! But Service up anew (%d %d)
36947:8:48:1505:os_api_gnss_socket.c:Error closing Socket Handle!! %d
36948:8:48:1524:os_api_gnss_socket.c:Could not create socket fd error: %d scb %d
36949:8:48:1569:os_api_gnss_socket.c:SCB %d Waiting for svc (%d %d). Port Removed (Node %d, Port %d). Ignore
36950:8:48:1578:os_api_gnss_socket.c:SCB %d Waiting for svc (%d %d). Unknown evt %d. Ignore
36951:8:48:1109:os_api_gnss_socket.c:Null pointer
36952:8:48:1125:os_api_gnss_socket.c:Could not allocate IPC MSG
36953:8:48:1146:os_api_gnss_socket.c:Failed to send IPC mesage
36954:8:48:1157:os_api_gnss_socket.c:Error closing Socket Handle!! %d
36955:8:48:1206:os_api_gnss_socket.c:Could not allocate IPC MSG
36956:8:48:1230:os_api_gnss_socket.c:Failed to rcv dgram. Error %d
36957:8:48:1238:os_api_gnss_socket.c:Failed to send IPC mesage
36958:8:48:1254:os_api_gnss_socket.c:Unhandled event %x recevied for socket %d
38913:2:49:1419:mgp_pe_api.c:PE: GNSS Delete GPS:%u, GLO:%u, QZSS:%u, BDS:%u, GAL:%u, SBAS:%u, NAVIC:%u
38920:2:49:1498:mgp_pe_api.c:PE: GNSS Delete Clk: 0x%8x
38921:2:49:1510:mgp_pe_api.c:PE: GNSS Delete Pos: 0x%8x
38925:2:49:1560:mgp_pe_api.c:PE: GNSS Delete Eph: BDS 0x%08x%08x
38932:2:49:1630:mgp_pe_api.c:PE: GNSS Delete Alm: BDS 0x%08x%08x
38936:2:49:1666:mgp_pe_api.c:PE: GNSS Delete XTRA: %u
38951:2:49:3502:mgp_pe_api.c:PE: Gnss Info Reporting: Received new config Mask: %08X%08lX
39020:2:49:9402:cd_getpt.c:Eph for unknown GNSS SV requested: %d
39939:4:49:1123:mgp_pe_api.c:PE: GnssUpdateInfo, Null Pointer
40961:8:49:1171:mgp_pe_api.c:PE/ASSIST: NULL in MGP_GNSS_ASSIST_DATA_EPH for constellation %u
40962:8:49:1277:mgp_pe_api.c:pe_GnssUpdateInfo: e_AssistanceType(%d) dropped because u_NumSvs(%d) larger than N_MAX_VISIBLE_GNSS_SV
40962:8:49:1277:mgp_pe_api.c:pe_GnssUpdateInfo: e_AssistanceType(%d) dropped because u_NumSvs(%d) larger than N_MAX_VISIBLE_GNSS_SV
40963:8:49:1306:mgp_pe_api.c:PE/ASSIST: NULL in MGP_GNSS_ASSIST_DATA_NAVIC_NAV_HEALTH
40964:8:49:1319:mgp_pe_api.c:PE/ASSIST: NULL in MGP_GNSS_ASSIST_DATA_NAVIC_ANAV_DC
40965:8:49:1332:mgp_pe_api.c:PE/ASSIST: NULL in MGP_GNSS_ASSIST_DATA_NAVICL1_CAPABILITY
40966:8:49:1345:mgp_pe_api.c:PE/ASSIST: NULL in MGP_GNSS_ASSIST_DATA_NAVICL1_HEALTH_GRP_DELAY
40967:8:49:1388:mgp_pe_api.c:PE: GnssDeleteInfo, Null Pointer
40984:8:49:428:nf_getpt.c:NF: nf_ClearGnssPolys, Invalid GNSS type %u
40984:8:49:428:nf_getpt.c:NF: nf_ClearGnssPolys, Invalid GNSS type %u
41009:8:49:2859:nf_getpt.c:NF: NULL Pointer Access in nf_SetClearGnssPersistentBlacklist()- %d, %d
41014:8:49:3196:sm_api.c:SM_API: Null pointer access in sm_ReportGnssFixInfo()
41045:8:49:5459:cd_getpt.c:CD: Unsupported GNSS type:%u
41046:8:49:5513:cd_getpt.c:CD: Unsupported GNSS type:%u
41086:8:49:5872:cd_getpt.c:CD: FillGnssMbPosFixInfoInPeStatus, null pointer
41984:1:50:1089:gnss_msgr_task.c:GNSS MSGR: No of attach items for pull up = %d
41984:1:50:1089:gnss_msgr_task.c:GNSS MSGR: No of attach items for pull up = %d
41985:1:50:1155:gnss_msgr_task.c:GNSS MSGR: rcvd NAS len = %d
41985:1:50:1155:gnss_msgr_task.c:GNSS MSGR: rcvd NAS len = %d
42031:1:50:1500:sm_api.c:SM_API: Report GNSS RF Status Info to SM...
42035:1:50:1719:sm_api.c:sm_ReportGnssInfo: Report GPS time to SM...
42074:1:50:3745:sm_api.c:sm_ReportGnssTimeConversionInfo
42178:1:50:2860:lm_mgp.c:GNSS fix not qualified due to unknown GPS week
42188:1:50:1381:tm_api.c:tm_handle_gnss_le_fix()
42191:1:50:1543:tm_api.c:tm_handle_gnss_qual_ind()
42213:1:50:2917:tm_api.c:TM: tm_ReportTribandGnssConfig = %d
42284:1:50:5173:tm_core.c:tm_core_populate_dgnss_indicator u_DataType %d w_RefStationId %d q_DgnssConstMask 0x%x q_DiffDataEpochTimeMsec %d
42284:1:50:5173:tm_core.c:tm_core_populate_dgnss_indicator u_DataType %d w_RefStationId %d q_DgnssConstMask 0x%x q_DiffDataEpochTimeMsec %d
42297:1:50:1923:tm_loc_processing_client.c:TM: LPC:dgnss info valid refStnId %d SrcID %d EpochMsec %d
42298:1:50:1927:tm_loc_processing_client.c:TM: LPC:dgnss info invalid
42305:1:50:3842:tm_loc_processing_client.c:LPC: GNSS fix not qualified due to unknown GPS week
42370:1:50:8307:tm_lpp_cp.c:TM_LPP_CP: PD END not sent for GNSS error cause:%d
42376:1:50:6807:tm_lpp_up.c:No GNSS MSB
42419:1:50:1073:tech_sel_gnss_tech_mgr.cpp:Foreground LPPM Enabled %d
42420:1:50:1093:tech_sel_gnss_tech_mgr.cpp:Ped Align Avail %d
42421:1:50:1607:tech_sel_gnss_tech_mgr.cpp:No OnDemand Fix request present to star GNSS fix
42421:1:50:1607:tech_sel_gnss_tech_mgr.cpp:No OnDemand Fix request present to star GNSS fix
42422:1:50:1623:tech_sel_gnss_tech_mgr.cpp:Winning Pos Req Client = %d
42558:1:50:125:tm_rrlp.c:GNSS RefTime type (%d) not handled
43008:2:50:438:gnss_user.c:DEBUG: mgp_ReceiverUpdate with RCVR CMD %d received at gnss_user
43008:2:50:438:gnss_user.c:DEBUG: mgp_ReceiverUpdate with RCVR CMD %d received at gnss_user
43009:2:50:442:gnss_user.c:DEBUG-0: mgp_ReceiverUpdate pz_MgpConfig is NULL
43010:2:50:1107:gnss_user.c:DEBUG XTRA: gnss_GetXtra3DataPtr: 0x%X
43010:2:50:1107:gnss_user.c:DEBUG XTRA: gnss_GetXtra3DataPtr: 0x%X
43011:2:50:1118:gnss_user.c:DEBUG XTRA: gnss_GetXtra3ApcOffsetDataPtr: 0x%X
43011:2:50:1118:gnss_user.c:DEBUG XTRA: gnss_GetXtra3ApcOffsetDataPtr: 0x%X
43151:2:50:540:gnss_msgr_task.c:GNSS MSGR: Got MSGR message(cnt=%d), sending NAS_DL_EVENT...
43151:2:50:540:gnss_msgr_task.c:GNSS MSGR: Got MSGR message(cnt=%d), sending NAS_DL_EVENT...
43152:2:50:344:gnss_msgr_task.c:GNSS MSGR: client successfully created
43152:2:50:344:gnss_msgr_task.c:GNSS MSGR: client successfully created
43267:2:50:7642:gm_core.c:Update GPS lock state %d. GnssLocked=%d
43278:2:50:8926:gm_core.c:GM: gnss_unavail_ind_timer_expiry 
43290:2:50:9965:gm_core.c:GM: nonqmi_gnss_unavail_ind_timer_expiry 
43337:2:50:2646:gm_core.c:GNSS position with high hepe rejected. fix_hepe %d, max_pos_unc %d 
43364:2:50:8651:gm_core.c:Reconfigured gloabl GNSS Unavail Agressive Exit to %d
43367:2:50:8654:gm_core.c:Reconfigured gloabl GNSS Pos Qos timeout to %d
43394:2:50:8490:gm_core.c:New Client GNSS Unavail Agressive Exit = %d
43398:2:50:8529:gm_core.c:Set GNSS Pos Qos timeout = %d
43399:2:50:8543:gm_core.c:New GNSS Pos Qos timeout = %d
43407:2:50:1957:gm_core.c:GM: Start gnss_nonqmi_unavail timer for %d secs
43408:2:50:1962:gm_core.c:i_request_client_position: gnss_qmi_unavail_reported ? %d gnss_qmi_unavail_timer_started ? %d
43408:2:50:1962:gm_core.c:i_request_client_position: gnss_qmi_unavail_reported ? %d gnss_qmi_unavail_timer_started ? %d
43409:2:50:1969:gm_core.c:GM: Start gnss_qmi_unavail timer for %d secs
43411:2:50:10935:gm_core.c:gm_core_handle_position_update: gnss_qmi_unavail_reported?::%d gnss_qmi_unavail_timer_started?::%d
43411:2:50:10935:gm_core.c:gm_core_handle_position_update: gnss_qmi_unavail_reported?::%d gnss_qmi_unavail_timer_started?::%d
43412:2:50:10941:gm_core.c:GM: Send GNSS avail ind  to QMI
43416:2:50:592:gm_ebee.c:Capturing timestamp for gnss fix start %ld
43417:2:50:624:gm_ebee.c:Capturing timestamp for gnss fix rcvd %ld
43448:2:50:1653:gm_ebee.c:In GNSS apptracking. Not using MoSB
43449:2:50:1754:gm_ebee.c:In GNSS apptracking. Not using Context Based Backoff
43476:2:50:231:gm_iod_based_backoff.c:IOD backoff Request a GNSS fix
43477:2:50:237:gm_iod_based_backoff.c:IOD Not used in Good GNSS Env
43478:2:50:247:gm_iod_based_backoff.c:IOD backoff not needed. Num GNSS fix fail = %d
43616:2:50:768:gm_log.c:Total Num GNSS Fixes: %d
43994:2:50:307:lm_task.c:=LM TASK= Read NV NV_GNSS_OEM_FEATURE_MASK = 0x%x
44007:2:50:1113:lm_task.c:=LM TASK= GNSS RF Config cmd rcvd from TM diag
44008:2:50:1120:lm_task.c:=LM TASK= GNSS Prx RF Config cmd rcvd from TM diag
44009:2:50:1127:lm_task.c:=LM TASK= GNSS RF Linearity Config cmd rcvd from TM diag
44097:2:50:3088:lm_mgp.c:Received FIX REPORT from MGP, Best Position = %d, gnss_fix_received_this_session = %d, refloc_rcvd_this_session = %d
44101:2:50:3133:lm_mgp.c:OFF Modem GNSS: High reliability CPI received. Report final fix
44102:2:50:3138:lm_mgp.c:OFF Modem GNSS: Medium reliability CPI received. Cache fix and report intermediate
44103:2:50:3147:lm_mgp.c:OFF Modem GNSS: Low Reliability CPI received. Report intermediate fix
44126:2:50:4535:lm_mgp.c:=LM TASK= OFF Modem GNSS: CPI received in LM from TM
44136:2:50:6512:lm_mgp.c:LPC: Received E911 fix qualification update from LPC. Updated bestfix; valid %d, gnssfixrcvd %d, reflocrcvd %d, EE_timer_started %d, EE_timer_popped %d, EE_timer_residual_value %d
44156:2:50:7089:lm_mgp.c:=LM TASK= OFF Modem GNSS: CPI to fix pos report conversion successfull
44158:2:50:3049:tm_api.c:TM: tm_ReportGnssBandsConfig: TM_API: IPC msg send to SM for GNSS Bands Config, id = %d
44158:2:50:3049:tm_api.c:TM: tm_ReportGnssBandsConfig: TM_API: IPC msg send to SM for GNSS Bands Config, id = %d
44241:2:50:4708:tm_cm_iface.c:TM: TM_CM: Got CM indication for e911 state %d, e911_state=%d srv_system=%d as_id %d, gnssCfg 0x%X
44417:2:50:10015:tm_core.c:Deleting GNSS SV blacklist BDS, mask:0X%X %X
44423:2:50:10171:tm_core.c:Deleting GNSS SV HEALTH GAL
44431:2:50:10257:tm_core.c:Deleting GNSS SV HEALTH NAVIC
44519:2:50:18184:tm_core.c:Current GNSSTime V:%d TimeFormat:%d Wk:%d TowMs:%d TuncMs:%d
44520:2:50:18258:tm_core.c:TM: GetNextLS: GnssTime V:%d Wk=%u TowMs=%u CurLs=%u NextLsEvent V:%d Wk: %u TowMs=%u NextLs V:%d Ls:%u
44524:2:50:18548:tm_core.c:NAVIC to GPS Time Offset Param: Gnss-Id = %d
44532:2:50:19371:tm_core.c:E911 Stats CPI: accepted = %d, fused with GNSS = %d, validity = %d
44545:2:50:20113:tm_core.c:tm_core_RequestGnssBandsCfg: Request Gnss Bands Config from MGP
44545:2:50:20113:tm_core.c:tm_core_RequestGnssBandsCfg: Request Gnss Bands Config from MGP
44546:2:50:20197:tm_core.c:tm_core_HandleGnssBandsConfig: Send Primary GNSS Signal=%d, Gnss Bands Config=%d to LocMW. MB enabled = %d
44546:2:50:20197:tm_core.c:tm_core_HandleGnssBandsConfig: Send Primary GNSS Signal=%d, Gnss Bands Config=%d to LocMW. MB enabled = %d
44546:2:50:20197:tm_core.c:tm_core_HandleGnssBandsConfig: Send Primary GNSS Signal=%d, Gnss Bands Config=%d to LocMW. MB enabled = %d
44648:2:50:5868:tm_loc_processing_client.c:TM: LPC:Received aiding info is %d GNSS type, not expected
44675:2:50:6896:tm_loc_processing_client.c:TM: LPC: Sent E911 fix qualification to LM: BestfixValid %d, gnssrcvd %d, reflocrcvd %d, EE_timer_started %d, EE_timer_popped %d, EE_timer_residual_value %d
44808:2:50:2055:tm_loc_processing_client.c:TM: LPC:tm_core_xlate_prms_to_ext_raw_clock_meas:MSA mode, No GNSS measurement report
44896:2:50:2733:tm_pdapi_iface.c:TM: GetParam: PDSM_PA_GNSS_MEAS_REPORT_CONFIG MeasRpt=%llu
44898:2:50:2751:tm_pdapi_iface.c:Getting GNSS Persistent Blacklist SV Mask
44899:2:50:2763:tm_pdapi_iface.c:Getting GNSS Constellation Configuration
44910:2:50:2886:tm_pdapi_iface.c:TM: Getting PDSM_PA_TRIBAND_GNSS_CONFIG. ClientId=%d
44933:2:50:3772:tm_pdapi_iface.c:TM: PersistentBL GnssType:Set:Clear GPS:0x%X:0x%X GLO:0x%X:0x%X BDS:0x%X:0x%X QZSS:0x%X:0x%X
44934:2:50:3778:tm_pdapi_iface.c:TM: PersistentBL GnssType:Set:Clear GAL:0x%X:0x%X NAVIC:0x%X:0x%X SBAS:0x%X:0x%X
44938:2:50:3896:tm_pdapi_iface.c:tm_core_handle_set_param: Received GNSS_CONFIG Meas:%d,SVPoly:%d ReqEph:%d ReqSvPoly:%d
44979:2:50:4790:tm_pdapi_iface.c:TM: SetParam: PDSM_PA_GNSS_REPORTING_CONFIG value 0x%X
44994:2:50:5013:tm_pdapi_iface.c:TM: SetParam: PDSM_PA_TRIBAND_GNSS_CONFIG value %d
45029:2:50:899:tm_ruim.c:GNSS MCFG_REFRESH got type=%d index=%d slot=%d
45030:2:50:957:tm_ruim.c:GNSS MCFG_REFRESH registration success
45093:2:50:505:tm_lte_nas_msgr_iface.c:MSGR message NAS_MM5G_GNSS_CIPHER_KEY_DATA_IND cipering set count=%d
45107:2:50:622:tm_decode_xtra3_navic_assist_data.c:=TM XTRA= tm_xtra3_navic_assist_allow_download() XTRA No Gnss session
45336:2:50:501:gfc_qmi_internal.c:Set Gnss Pos QosSessTimeout %d
45347:2:50:2919:gfc_qmi_internal.c:GFC_QMI: Setting GNSS unavail ind timeout as %d from NV
45351:2:50:2970:gfc_qmi_internal.c:GFC_QMI: Setting GNSS Pos Qos Sess timeout as %d from NV
45400:2:50:2391:tm_lpp.c:TM_LPP: Build LS msg from %u GNSS system meas report. LS choice 0x%02X.
45542:2:50:1370:tm_lpp_e.c:Position Source: WIFI = %d, CellId=%d, CPI=%d, GNSS=%d 
45555:2:50:9149:tm_lpp_cp.c:Not expecting GNSS MSB
45559:2:50:9216:tm_lpp_cp.c:Run to RLI/GNSS-Auxi timeout for e911
45583:2:50:11103:tm_lpp_cp.c:Gnss_Auxi timer id %lu expired
45600:2:50:5284:tm_lpp_cp.c:Stop GNSS_Auxi timer id %lu
45601:2:50:5321:tm_lpp_cp.c:OTDOA respond before timeout. Stop GNSS_Auxi timer id %lu
45602:2:50:5345:tm_lpp_cp.c:MultiRTT respond before timeout. Stop GNSS_Auxi timer id %lu
45613:2:50:5232:tm_lpp_cp.c:Transition to new state 0x%X to fetch non-gnss tech
45614:2:50:5240:tm_lpp_cp.c:No non-gnss tech to fetch
45620:2:50:2810:tm_lpp_cp.c:TM_LPP_CP:GNSS Assist processing
45621:2:50:2933:tm_lpp_cp.c:Proc common AD GNSS RefTime %d RefLoc %d Iono %d
45622:2:50:3054:tm_lpp_cp.c:TM_LPP_CP: RefTime posted to TmCore. Type %d, GNSS ID %d
45646:2:50:6333:tm_lpp_cp.c:rli tech 0x%X, 0x%X (total), lpp_flags 0x%X, ecid_req 0x%X, gnss_req 0x%X, AGNSS %d, QoS present %d m_req_flags 0x%X
45646:2:50:6333:tm_lpp_cp.c:rli tech 0x%X, 0x%X (total), lpp_flags 0x%X, ecid_req 0x%X, gnss_req 0x%X, AGNSS %d, QoS present %d m_req_flags 0x%X
45653:2:50:4282:tm_lpp_cp.c:Position already obtained, no action for LPP GNSS MSB
45710:2:50:6836:tm_lpp_up.c:Run to RLI/GNSS-Auxi timeout for e911
45734:2:50:700:tm_lpp_up.c:TM_LPP_UP:GNSS Assist processing
45738:2:50:1081:tm_lpp_up.c:TM_LPP_UP: Invalid gnss id for IONO AD
45755:2:50:1934:tm_lpp_up.c:A-GNSS MSA more AD to come. Run-to-timeout.
45756:2:50:1942:tm_lpp_up.c:A-GNSS session with no RLI. Use default session timeout.
45759:2:50:3044:tm_lpp_up.c:rli tech 0x%X, 0x%X (total), lpp_flags 0x%X, gnss_req 0x%X, QoS present %d, QoS timeout %d Delay Adjusted %d
45772:2:50:5076:tm_lpp_up.c:GNSS timer id %lu %lu msec
45782:2:50:5668:tm_lpp_up.c:Stop GNSS_Auxi timer id %lu
45827:2:50:411:tech_sel_rule_wifi_assist_helper.cpp:Calculated GNSS strength %d Prev GNSS strength %d
45827:2:50:411:tech_sel_rule_wifi_assist_helper.cpp:Calculated GNSS strength %d Prev GNSS strength %d
45828:2:50:1044:tech_sel_rule_wifi_assist_helper.cpp:GNSS Strength Event: GNSS Strength %d
45828:2:50:1044:tech_sel_rule_wifi_assist_helper.cpp:GNSS Strength Event: GNSS Strength %d
45839:2:50:268:tech_sel_rule_15.cpp:TechSel Rule %d: Session is M2 but GNSS in Non-Ped mode. Stop LPPM. Continue MSB
45851:2:50:162:tech_sel_gnss_tech_mgr.cpp:Ped Ind reported %d
45852:2:50:194:tech_sel_gnss_tech_mgr.cpp:GNSS Qual Ind reported %d
45852:2:50:194:tech_sel_gnss_tech_mgr.cpp:GNSS Qual Ind reported %d
45853:2:50:224:tech_sel_gnss_tech_mgr.cpp:Ped Dev Ctx reported %d
45854:2:50:253:tech_sel_gnss_tech_mgr.cpp:Ped align avail reported as  %d
45855:2:50:283:tech_sel_gnss_tech_mgr.cpp:Ped sensor assistance avail reported as  %d
45856:2:50:324:tech_sel_gnss_tech_mgr.cpp:LPPM Status: Possible %d, MgpSubMode %d TBM %u
45857:2:50:365:tech_sel_gnss_tech_mgr.cpp:LPPM Disengaged
45858:2:50:376:tech_sel_gnss_tech_mgr.cpp:LPPM Engaged. Power Mode %d, TBM %u
45859:2:50:745:tech_sel_gnss_tech_mgr.cpp:LPPM started: Mode M%d TBM %u
45860:2:50:800:tech_sel_gnss_tech_mgr.cpp:Engine Not in LPPM, cannot modify TBM/Power Mode
45861:2:50:812:tech_sel_gnss_tech_mgr.cpp:Modified LPPM: Current M%d TBM %u, New M%d TBM %u
45862:2:50:867:tech_sel_gnss_tech_mgr.cpp:SDP Sensors Allowed %d
46126:2:50:3702:tm_umts_cp_wcdma.c:Glo PE : %d, AGNSS Mode %d, PosData %3u, PE %3u
46144:2:50:5592:tm_umts_cp_wcdma.c:Choosen A-GNSS position method %d
46237:2:50:2820:tm_umts_up_supl.c:SUPL: Intermediate fix received. Do not cache non-gnss fix
46243:2:50:3360:tm_umts_up_supl.c:SUPL: GNSS meas. ignored - not expecting
46346:2:50:7022:tm_umts_up_supl.c:AGNSS posmethod but Ver2 SUPL_RESP missing. Default to GPS+GLO!!
46355:2:50:9628:tm_umts_up_supl.c:AGNSS error reported. Clean up Session
46562:2:50:6627:gps_common.c:==== gnss_ConvertSecondsToGpsTime %d %d 
46563:2:50:6646:gps_common.c:==== gnss_ConvertGpsTimeToSeconds %d 
46593:2:50:493:gnss_diag_buf.c:tx_permission_cb = %d
46594:2:50:1141:gnss_diag_buf.c:gnss_dm_log_commit 0x%x log_code 0x%x log_status %d b_gnss_diag_buf_conf %d err_code %d
46594:2:50:1141:gnss_diag_buf.c:gnss_dm_log_commit 0x%x log_code 0x%x log_status %d b_gnss_diag_buf_conf %d err_code %d
46594:2:50:1141:gnss_diag_buf.c:gnss_dm_log_commit 0x%x log_code 0x%x log_status %d b_gnss_diag_buf_conf %d err_code %d
46595:2:50:1176:gnss_diag_buf.c:gnss_dm_log_commit b_un_initialized = %d buffer = 0x%x init_over = %d log in dbuff %d
46595:2:50:1176:gnss_diag_buf.c:gnss_dm_log_commit b_un_initialized = %d buffer = 0x%x init_over = %d log in dbuff %d
46596:2:50:1200:gnss_diag_buf.c:gnss_dm_log_commit log 0x%x to diag buffer!!
46596:2:50:1200:gnss_diag_buf.c:gnss_dm_log_commit log 0x%x to diag buffer!!
46597:2:50:1275:gnss_diag_buf.c:Set Tx Mode to STREAMING for buffer %d!!
46598:2:50:1335:gnss_diag_buf.c:Set Tx Mode to CIRCULAR for buffer %d!!
46599:2:50:1402:gnss_diag_buf.c:Invalid buffer ID %d!!
46600:2:50:1407:gnss_diag_buf.c:Invalid buffer cmd %d!!
46601:2:50:818:gnss_diag_buf.c:gnss_dm_log_search log_list_sz %d logs %d %d!
46601:2:50:818:gnss_diag_buf.c:gnss_dm_log_search log_list_sz %d logs %d %d!
46607:2:50:2829:sm_log.c:LOG_GNSS_PDSM_POSITION_REPORT_CALLBACK_C turned off!
46610:2:50:4212:sm_log.c:LOG_GNSS_PDSM_EXT_STATUS_MEAS_REPORT_C turned off!
47188:4:50:647:gnss_msgr_task.c:GNSS MSGR: Starting GNSS MSGR task (ThreadID=%d)...
47188:4:50:647:gnss_msgr_task.c:GNSS MSGR: Starting GNSS MSGR task (ThreadID=%d)...
47188:4:50:647:gnss_msgr_task.c:GNSS MSGR: Starting GNSS MSGR task (ThreadID=%d)...
47189:4:50:694:gnss_msgr_task.c:GNSS MSGR: Registering for NAS MSGR messages
47189:4:50:694:gnss_msgr_task.c:GNSS MSGR: Registering for NAS MSGR messages
47190:4:50:809:gnss_msgr_task.c:GNSS MSGR: Deregistering for NAS MSGR messages
47190:4:50:809:gnss_msgr_task.c:GNSS MSGR: Deregistering for NAS MSGR messages
47191:4:50:925:gnss_msgr_task.c:GNSS MSGR: NAS_EMM_UL_GENERIC_NAS_TRANSPORT_REQ payload (DSM size=%d)
47191:4:50:925:gnss_msgr_task.c:GNSS MSGR: NAS_EMM_UL_GENERIC_NAS_TRANSPORT_REQ payload (DSM size=%d)
47192:4:50:1002:gnss_msgr_task.c:GNSS MSGR: sent NAS msg with size=%d trans_id=0x%x
47192:4:50:1002:gnss_msgr_task.c:GNSS MSGR: sent NAS msg with size=%d trans_id=0x%x
47193:4:50:1070:gnss_msgr_task.c:GNSS MSGR: No NAS message available!!
47193:4:50:1070:gnss_msgr_task.c:GNSS MSGR: No NAS message available!!
47194:4:50:1084:gnss_msgr_task.c:GNSS MSGR: received DL_GENERIC_NAS_TRANSPORT msg
47194:4:50:1084:gnss_msgr_task.c:GNSS MSGR: received DL_GENERIC_NAS_TRANSPORT msg
47195:4:50:1120:gnss_msgr_task.c:GNSS MSGR: Length of LPP Message = %d
47195:4:50:1120:gnss_msgr_task.c:GNSS MSGR: Length of LPP Message = %d
47196:4:50:1168:gnss_msgr_task.c:GNSS MSGR: received NAS_EMM_UL_GENERIC_NAS_TRANSPORT_CNF msg TrasID=0x%x
47196:4:50:1168:gnss_msgr_task.c:GNSS MSGR: received NAS_EMM_UL_GENERIC_NAS_TRANSPORT_CNF msg TrasID=0x%x
47197:4:50:1173:gnss_msgr_task.c:GNSS MSGR: received NAS_EMM_GENERIC_NAS_TRANSPORT_FAILURE_IND msg TrasID=0x%x Failure=%d
47197:4:50:1173:gnss_msgr_task.c:GNSS MSGR: received NAS_EMM_GENERIC_NAS_TRANSPORT_FAILURE_IND msg TrasID=0x%x Failure=%d
47198:4:50:397:gnss_msgr_task.c:GNSS MSGR: TASK OFFLINE received
47198:4:50:397:gnss_msgr_task.c:GNSS MSGR: TASK OFFLINE received
47199:4:50:416:gnss_msgr_task.c:GNSS MSGR: TASK STOP received
47199:4:50:416:gnss_msgr_task.c:GNSS MSGR: TASK STOP received
47297:4:50:1735:sm_api.c:SM_API: failed when translating gnss time to internal gps time 
47351:4:50:6145:gm_core.c:Pos fix Hepe = %d, Max GNSS hepe accepted %d 
47352:4:50:6177:gm_core.c:GNSS position fix Type %d
47374:4:50:3813:gm_core.c:GNSS is Locked. GF Id=%d Type=%d Init with StartTime=0x%X
47391:4:50:1856:gm_core.c:GNSS Position Already requested to client
47426:4:50:3541:gm_motion_sensor_wifi_cdh.c:Gnss Fix velocity = %d, numSvs = %d
47427:4:50:3582:gm_motion_sensor_wifi_cdh.c:BL AP. GNSSFix shows motion V(%d) numSvs(%d)
47504:4:50:3003:lm_tm.c:GNSS Engine Monitor timer %d timedout
47516:4:50:2048:lm_diag.c:=LM TASK= TSG Mode %u, GNSS %u, Doppler %u
47550:4:50:4735:lm_mgp.c:=LM TASK= GNSS RF status info received from MGP.
47551:4:50:4743:lm_mgp.c:=LM TASK= GNSS RF status info PGA Gain %d BP1 AmpI %d AmpQ %d
47556:4:50:4944:lm_mgp.c:=LM TASK= nominal mgp_GnssUpdateInfo
47561:4:50:5681:lm_mgp.c:Handle SET request for GNSS Engine Monitor Reporting = %d
47562:4:50:5706:lm_mgp.c:Handle GET request for GNSS Engine Monitor Reporting. Config value = %d
47581:4:50:914:lm_mgp.c:=LM TASK= Fix not CPI or GNSS, ignored for session best
47631:4:50:3266:tm_cm_iface.c:Reject New DDsubID %d for AGNSS because e911 call on sub %d
47632:4:50:3282:tm_cm_iface.c:Set New DDsubID %d for NI AGNSS prev_data_sub %d
47642:4:50:4849:tm_cm_iface.c:TM: TextToEmerg: GPS constellation not enabled. gnssCfg = 0x%X
47684:4:50:9561:tm_core.c:TM: PDSM_EXT_STATUS_REPORT_ME_METRICS GNSS_EnMask = %d, Tick = 0x%X%08X B5PgaGnDbV = %d
47689:4:50:9875:tm_core.c:Deleting GNSS SV blacklist GPS
47690:4:50:9941:tm_core.c:Deleting GNSS SV blacklist GLO, mask:0X%X
47691:4:50:9948:tm_core.c:Deleting GNSS SV Health GLO
47692:4:50:10022:tm_core.c:Deleting GNSS SV HEALTH BDS
47693:4:50:10102:tm_core.c:Deleting GNSS SV blacklist QZSS, mask:0X%X
47694:4:50:10164:tm_core.c:Deleting GNSS SV blacklist GAL, mask:0X%X %X
47695:4:50:10250:tm_core.c:Deleting GNSS SV blacklist NAVIC, mask : %X
47778:4:50:20176:tm_core.c:TM: tm_core_HandleGnssBandsConfig: Received Config: Primary=%d L1:0x%x, MB=0x%x, Aux=0x%x
47788:4:50:5130:tm_core.c:v_IsDgnssUsed: %d u_DgnssConstUsage %x
47788:4:50:5130:tm_core.c:v_IsDgnssUsed: %d u_DgnssConstUsage %x
47820:4:50:2106:tm_diag.c:GNSS Nav config 13 (Rfdev_fmt) 0x%2x, 0x%2x, 0x%2x 
47821:4:50:2108:tm_diag.c:GNSS Nav config 13 (MGP_fmt), 0x%x, 0x%x, 0x%x 
47822:4:50:2126:tm_diag.c:GNSS Nav config 13 rsp (Rfdev_fmt) 0x%x, 0x%x
47855:4:50:3267:tm_loc_processing_client.c:TM: LPC:tm_core Late PDSM_EXT_STATUS_GNSS_MEASUREMENT
47857:4:50:1118:tm_loc_processing_client.c:TM: LPC:tm_core_xlate_gnss_prms_to_pd_gnss_meas received NULL pointer
47857:4:50:1118:tm_loc_processing_client.c:TM: LPC:tm_core_xlate_gnss_prms_to_pd_gnss_meas received NULL pointer
47858:4:50:1256:tm_loc_processing_client.c:TM: tm_core_xlate_gnss_prms_to_pd_gnss_meas Invalid MeasBlKSrc %d
47858:4:50:1256:tm_loc_processing_client.c:TM: tm_core_xlate_gnss_prms_to_pd_gnss_meas Invalid MeasBlKSrc %d
47915:4:50:4218:tm_pdapi_iface.c:XTRA-T GNSS_NV_EFS_TLE_XTRAT_CLIENT_TOKEN could not be set
47916:4:50:4252:tm_pdapi_iface.c:XTRA-T GNSS_NV_EFS_TLE_XTRAT_USER_SESSION_CONTROL could not be set
47945:4:50:902:tm_prtl_iface.c:Sending GNSS (%d) Unhealhty SV info to LM 0x%x
47981:4:50:2745:tm_util.c:New GnssConfig = 0x%X
47992:4:50:346:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: registration for POLICYMAN_CFG_COUNTRY_CHANGE_IND message SUCCESSFUL
47993:4:50:360:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: %d Msgs registered!
47999:4:50:206:tm_pdapi_iface_no_ship.c:gnss_meas_rep_qmi_out_control %d
48239:4:50:394:tm_broadcast_ad_cdfw_source.c:tm_broadcast_ad_cdfw_handle_source_status: QDGNSS_READY_TO_ACCEPT_DATA
48240:4:50:402:tm_broadcast_ad_cdfw_source.c:tm_broadcast_ad_cdfw_handle_source_status: QDGNSS_DATA_SOURCE_NOT_SUPPORTED
48241:4:50:411:tm_broadcast_ad_cdfw_source.c:tm_broadcast_ad_cdfw_handle_source_status: QDGNSS_DATA_FORMAT_NOT_SUPPORTED
48242:4:50:420:tm_broadcast_ad_cdfw_source.c:tm_broadcast_ad_cdfw_handle_source_status: QDGNSS_OTHER_SOURCE_IN_USE
48243:4:50:429:tm_broadcast_ad_cdfw_source.c:tm_broadcast_ad_cdfw_handle_source_status: QDGNSS_MESSAGE_PARSE_ERROR
48244:4:50:436:tm_broadcast_ad_cdfw_source.c:tm_broadcast_ad_cdfw_handle_source_status: QDGNSS_STOP_SOURCE_INJECT
48293:4:50:2461:tm_lpp.c:tm_lpp_prov_loc_gnss_meas_build: Calling BlacklistFilter
48294:4:50:2524:tm_lpp.c:TM_LPP: Can't provide all LS requested measurements. Fill GNSS-Error.
48295:4:50:2630:tm_lpp.c:tm_lpp_prov_loc_gnss_meas_build: Calling BlacklistFilter
48327:4:50:1356:tm_lpp_common_utils.c:GNSS position transferred
48329:4:50:1442:tm_lpp_common_utils.c:GNSS meas transferred
48381:4:50:563:tm_lpp_cp.c:Stop GNSS_Auxi timer id %lu, ndx %u
48384:4:50:7295:tm_lpp_cp.c:No tx item found gnss pos
48386:4:50:7165:tm_lpp_cp.c:No tx item found gnss pos
48394:4:50:7238:tm_lpp_cp.c:No tx item found gnss meas
48396:4:50:7348:tm_lpp_cp.c:No tx item found gnss meas
48403:4:50:3034:tm_lpp_cp.c:Unsupported GNSS Reftime Id : %d
48408:4:50:3217:tm_lpp_cp.c:Processing r9 elt. %d gnss_id %d n %d
48439:4:50:6122:tm_lpp_cp.c:GNSS MSB loc req nullify. NV control off.
48440:4:50:6133:tm_lpp_cp.c:GNSS MSA meas req nullify. NV control off.
48486:4:50:6799:tm_lpp_up.c:GNSS MSB is not requested
48487:4:50:6816:tm_lpp_up.c:GNSS MSB position is already valid
48488:4:50:6824:tm_lpp_up.c:Handle GNSS final position %d. RLI tech mask 0x%lX. E911 %d
48495:4:50:7450:tm_lpp_up.c:No tx item found gnss meas
48504:4:50:915:tm_lpp_up.c:Unsupported GNSS RefTime Id : %d
48539:4:50:5654:tm_lpp_up.c:GNSS timer id %lu expired
48549:4:50:4890:tm_lpp_up.c:Stop GNSS_Auxi timer id %lu
48556:4:50:842:tm_pdapi_client.c:PDAPIcmd DataSource %d not supported. Defaulting to GNSS
48562:4:50:3579:tm_pdapi_client.c:TM_PDAPI: PDSM_EXT_STATUS_GNSS_MEASUREMENT Seq[%u][%u] NumSvs=%u FC=%u PdSig=%d NHz=%d
48567:4:50:3611:tm_pdapi_client.c:TM_PDAPI: GNSS Engine Monitor Status Report Type
48572:4:50:3636:tm_pdapi_client.c:TM: TM_PDAPI: GNSS Time Conv Param Report Type
48580:4:50:3685:tm_pdapi_client.c:TM: TM_PDAPI: Sending PDSM_EXT_STATUS_EVENT_GNSS_BANDS_SUPPORTED_CFG
48584:4:50:1354:tm_pdapi_client.c:TM: _RLM: getCfg=0x%X NumGnss=%d
48585:4:50:1368:tm_pdapi_client.c:TM: _RLM: NewCfg=0x%X UsableGnssConfig UsableNumGnss=%d
48585:4:50:1368:tm_pdapi_client.c:TM: _RLM: NewCfg=0x%X UsableGnssConfig UsableNumGnss=%d
48594:4:50:385:rlm_api.cpp:TM: _RLM: SetGnssConfig=0x%X UsableGnssConfig UsableNumGnss=%d
48594:4:50:385:rlm_api.cpp:TM: _RLM: SetGnssConfig=0x%X UsableGnssConfig UsableNumGnss=%d
48594:4:50:385:rlm_api.cpp:TM: _RLM: SetGnssConfig=0x%X UsableGnssConfig UsableNumGnss=%d
48596:4:50:527:rlm_api.cpp:TM: _RLM: RL ENABLED. GnssConfig=0x%X NumGnss=%d
48596:4:50:527:rlm_api.cpp:TM: _RLM: RL ENABLED. GnssConfig=0x%X NumGnss=%d
48601:4:50:593:rlm_api.cpp:TM: _RLM: RL ENABLED. GnssConfig=0x%X NumGnss=%d
48601:4:50:593:rlm_api.cpp:TM: _RLM: RL ENABLED. GnssConfig=0x%X NumGnss=%d
48602:4:50:436:tech_sel_gnss_tech_mgr.cpp:GNSS LE Req %d
48602:4:50:436:tech_sel_gnss_tech_mgr.cpp:GNSS LE Req %d
48603:4:50:440:tech_sel_gnss_tech_mgr.cpp:Registered for LE reporting
48604:4:50:465:tech_sel_gnss_tech_mgr.cpp:GNSS Fix Start
48604:4:50:465:tech_sel_gnss_tech_mgr.cpp:GNSS Fix Start
48605:4:50:488:tech_sel_gnss_tech_mgr.cpp:E911 Start. Stop LPPM
48606:4:50:521:tech_sel_gnss_tech_mgr.cpp:Abort Reason %d Internal Session %d On Demand Sess Running %d
48607:4:50:2003:tech_sel_gnss_tech_mgr.cpp:Fix Retry timer started for %d msec
48608:4:50:2104:tech_sel_gnss_tech_mgr.cpp:Session took %d msecs
48609:4:50:2109:tech_sel_gnss_tech_mgr.cpp:Power consumed is %u
48610:4:50:2178:tech_sel_gnss_tech_mgr.cpp:Start Sensor LE Num Sensor LE Req %d IsSensorLeOn %d
48611:4:50:2223:tech_sel_gnss_tech_mgr.cpp:Stop Sensor LE Num Sensor LE Req %d IsSensorLeOn %d
48612:4:50:2293:tech_sel_gnss_tech_mgr.cpp:Sensor Le Status Error %d State %d
48613:4:50:2337:tech_sel_gnss_tech_mgr.cpp:Sensor Le Rel Disp Status %d
48794:4:50:11723:tm_umts_up_supl.c:SLP Supports gnss post tech mask 0x%x GPS(0x1) GLO (0x20) BDS (0x40)
48988:4:50:6387:gps_common.c:Filter GNSS SvId: %u (BlackListed)
49010:4:50:1347:tm_rrlp.c:GPS meas. absent, GNSS code phase assigned from FC to GLONASS TOD
49152:8:50:1268:gnss_user.c:sm_user_handler_set_nav_config_resp_ind: unexpected len %d (expected %d)
49153:8:50:1289:gnss_user.c:sm_user_handler_get_nav_config_resp_ind: unexpected len %d (expected %d)
49154:8:50:1310:gnss_user.c:sm_user_handler_get_cradle_mount_config: unexpected len %d (expected %d)
49155:8:50:1332:gnss_user.c:sm_user_handler_set_cradle_mount_config: unexpected len %d (expected %d)
49156:8:50:1354:gnss_user.c:sm_user_handler_set_cradle_mount_config: unexpected len %d (expected %d)
49157:8:50:1376:gnss_user.c:sm_user_handler_get_sensor_control_config: unexpected len %d (expected %d)
49158:8:50:1398:gnss_user.c:sm_user_handler_sensor_perform_control_config: unexpected len %d (expected %d)
49159:8:50:1420:gnss_user.c:sm_user_handler_set_sensor_external_power_config: unexpected len %d (expected %d)
49160:8:50:1442:gnss_user.c:sm_user_handler_set_sensor_control_config: unexpected len %d (expected %d)
49161:8:50:1464:gnss_user.c:sm_user_handler_set_sensor_performance_control_config: unexpected len %d (expected %d)
49162:8:50:1486:gnss_user.c:sm_user_handler_set_spi_status_config: unexpected len %d (expected %d)
49163:8:50:1508:gnss_user.c:sm_user_handler_report_event: unexpected len %d (expected %d)
49164:8:50:1528:gnss_user.c:sm_user_handler_is_NR5G_pos_supported: unexpected len %d (expected %d)
49165:8:50:1548:gnss_user.c:sm_user_handler_report_leap_sec: unexpected len %d (expected %d)
49166:8:50:1568:gnss_user.c:sm_user_handler_report_inconsistency_data: unexpected len %d (expected %d)
49167:8:50:1588:gnss_user.c:sm_user_handler_update_mgp_info_to_rlm: unexpected len %d (expected %d)
49168:8:50:1608:gnss_user.c:sm_user_handler_rpt_triband_gnss_config: unexpected len %d (expected %d)
49168:8:50:1608:gnss_user.c:sm_user_handler_rpt_triband_gnss_config: unexpected len %d (expected %d)
49169:8:50:1628:gnss_user.c:sm_user_handler_rpt_mgp_lpp_status: unexpected len %d (expected %d)
49170:8:50:1649:gnss_user.c:sm_user_handler_report_ped_status: unexpected len %d (expected %d)
49171:8:50:1669:gnss_user.c:sm_user_handler_report_inj_done: unexpected len %d (expected %d)
49172:8:50:1689:gnss_user.c:sm_user_handler_report_xo_offset_info: unexpected len %d (expected %d)
49173:8:50:1709:gnss_user.c:sm_user_handler_report_ephemeris: unexpected len %d (expected %d)
49174:8:50:1729:gnss_user.c:sm_user_handler_report_iq_test_cap: unexpected len %d (expected %d)
49175:8:50:1749:gnss_user.c:sm_user_handler_report_qmi_event: unexpected len %d (expected %d)
49176:8:50:1770:gnss_user.c:sm_user_handler_report_latency: unexpected len %d (expected %d)
49177:8:50:1790:gnss_user.c:sm_user_handler_report_wbiq_info: unexpected len %d (expected %d)
49178:8:50:1810:gnss_user.c:sm_user_handler_report_nbiq_info: unexpected len %d (expected %d)
49179:8:50:1830:gnss_user.c:sm_user_handler_report_l5_blnk_duration_info: unexpected len %d (expected %d)
49180:8:50:1850:gnss_user.c:sm_user_handler_report_rcvr_state_change: unexpected len %d (expected %d)
49181:8:50:1871:gnss_user.c:sm_user_handler_report_gps_eph_config: unexpected len %d (expected %d)
49182:8:50:1891:gnss_user.c:sm_user_handler_report_persist_sv_mask_to_fltr: unexpected len %d (expected %d)
49183:8:50:1911:gnss_user.c:sm_user_handler_report_qual_ind: unexpected len %d (expected %d)
49184:8:50:1931:gnss_user.c:sm_user_handler_report_lfm_config_update: unexpected len %d (expected %d)
49185:8:50:1951:gnss_user.c:sm_user_handler_report_sensor_assist_avail_ind: unexpected len %d (expected %d)
49186:8:50:1971:gnss_user.c:sm_user_handler_report_fix: unexpected len %d (expected %d)
49187:8:50:1991:gnss_user.c:sm_user_handler_init_nav_pos: unexpected len %d (expected %d)
49188:8:50:2011:gnss_user.c:sm_user_handler_report_nmea_for_debug: unexpected len %d (expected %d)
49189:8:50:2031:gnss_user.c:sm_user_handler_report_debug_nmea: unexpected len %d (expected %d)
49190:8:50:2051:gnss_user.c:sm_user_handler_report_apc_offset: unexpected len %d (expected %d)
49191:8:50:2071:gnss_user.c:sm_user_handler_report_ped_allign_avail_ind: unexpected len %d (expected %d)
49192:8:50:2091:gnss_user.c:sm_user_handler_report_pos_resp_goodness: unexpected len %d (expected %d)
49193:8:50:2111:gnss_user.c:sm_user_handler_report_power_consumption: unexpected len %d (expected %d)
49194:8:50:2131:gnss_user.c:sm_user_handler_report_pfa_test_presc_dwell: unexpected len %d (expected %d)
49195:8:50:2151:gnss_user.c:sm_user_handler_report_me_metrics: unexpected len %d (expected %d)
49196:8:50:2171:gnss_user.c:sm_user_handler_report_engine_error_recovery: unexpected len %d (expected %d)
49197:8:50:2191:gnss_user.c:sm_user_handler_report_inconsistency_test_status: unexpected len %d (expected %d)
49198:8:50:2211:gnss_user.c:sm_user_handler_report_min_gps_week: unexpected len %d (expected %d)
49199:8:50:2231:gnss_user.c:sm_user_handler_update_cpe_timing_control: unexpected len %d (expected %d)
49200:8:50:2251:gnss_user.c:sm_user_handler_report_dpo_status: unexpected len %d (expected %d)
49201:8:50:2271:gnss_user.c:sm_user_handler_report_multiband_config: unexpected len %d (expected %d)
49202:8:50:2291:gnss_user.c:sm_user_handler_report_xtra_data: unexpected len %d (expected %d)
49203:8:50:2311:gnss_user.c:sm_user_handler_report_gnss_assist_data_status: unexpected len %d (expected %d)
49203:8:50:2311:gnss_user.c:sm_user_handler_report_gnss_assist_data_status: unexpected len %d (expected %d)
49204:8:50:2331:gnss_user.c:sm_user_handler_report_sv_poly_gnss: unexpected len %d (expected %d)
49204:8:50:2331:gnss_user.c:sm_user_handler_report_sv_poly_gnss: unexpected len %d (expected %d)
49205:8:50:2353:gnss_user.c:sm_user_handler_report_poly: unexpected len %d (expected %d)
49206:8:50:2373:gnss_user.c:sm_user_handler_sub_frame_put: unexpected len %d (expected %d)
49207:8:50:2393:gnss_user.c:gnss_qdi_rpt_sv_gal_page_put_params: unexpected len %d (expected %d)
49207:8:50:2393:gnss_user.c:gnss_qdi_rpt_sv_gal_page_put_params: unexpected len %d (expected %d)
49208:8:50:2413:gnss_user.c:sm_user_handler_DC_report: unexpected len %d (expected %d)
49209:8:50:2433:gnss_user.c:sm_user_handler_report_glo_eph: unexpected len %d (expected %d)
49210:8:50:2453:gnss_user.c:sm_user_handler_report_bds_eph: unexpected len %d (expected %d)
49211:8:50:2473:gnss_user.c:sm_user_handler_report_gal_eph: unexpected len %d (expected %d)
49212:8:50:2493:gnss_user.c:sm_user_handler_report_const_config: unexpected len %d (expected %d)
49213:8:50:2513:gnss_user.c:sm_user_handler_glo_string_put: unexpected len %d (expected %d)
49214:8:50:2533:gnss_user.c:sm_user_handler_report_min_sv_elevation: unexpected len %d (expected %d)
49215:8:50:2553:gnss_user.c:sm_user_handler_report_pe_mag_cal_status: unexpected len %d (expected %d)
49216:8:50:2573:gnss_user.c:sm_user_handler_report_val_len_nmea_debug: unexpected len %d (expected %d)
49217:8:50:2593:gnss_user.c:sm_user_handler_report_debug_data: unexpected len %d (expected %d)
49218:8:50:2613:gnss_user.c:sm_user_handler_report_gnss_time: unexpected len %d (expected %d)
49218:8:50:2613:gnss_user.c:sm_user_handler_report_gnss_time: unexpected len %d (expected %d)
49219:8:50:2633:gnss_user.c:sm_user_handler_report_meas_info: unexpected len %d (expected %d)
49220:8:50:2657:gnss_user.c:sm_user_handler_report_unprop_gnss_fix_info: unexpected len %d (expected %d)
49220:8:50:2657:gnss_user.c:sm_user_handler_report_unprop_gnss_fix_info: unexpected len %d (expected %d)
49221:8:50:2677:gnss_user.c:sm_user_handler_ped_dev_context: unexpected len %d (expected %d)
49222:8:50:2697:gnss_user.c:sm_user_handler_report_gnss_persist_sv_mask: unexpected len %d (expected %d)
49222:8:50:2697:gnss_user.c:sm_user_handler_report_gnss_persist_sv_mask: unexpected len %d (expected %d)
49223:8:50:2717:gnss_user.c:sm_user_handler_reg_api_notify: unexpected len %d (expected %d)
49224:8:50:2737:gnss_user.c:sm_user_handler_report_dr_sync_config: unexpected len %d (expected %d)
49225:8:50:2757:gnss_user.c:sm_user_handler_store_ftcal_data: unexpected len %d (expected %d)
49226:8:50:2777:gnss_user.c:sm_user_handler_report_dr_sync_pulse_detector: unexpected len %d (expected %d)
49227:8:50:2797:gnss_user.c:sm_user_handler_report_lte_ecid_rcvr_meas: unexpected len %d (expected %d)
49228:8:50:2817:gnss_user.c:sm_user_handler_report_nr_ecid_rcvr_meas: unexpected len %d (expected %d)
49229:8:50:2837:gnss_user.c:sm_user_handler_store_ftcal_ppm_data: unexpected len %d (expected %d)
49230:8:50:2859:gnss_user.c:sm_user_handler_report_supported_bands: unexpected len %d (expected %d)
49231:8:50:2880:gnss_user.c:sm_user_handler_report_tle_delete_time: unexpected len %d (expected %d)
49232:8:50:2900:gnss_user.c:sm_user_handler_update_slow_clk_time: unexpected len %d (expected %d)
49233:8:50:2920:gnss_user.c:sm_user_handler_delete_position: unexpected len %d (expected %d)
49234:8:50:2940:gnss_user.c:sm_user_handler_tle_inject_position: unexpected len %d (expected %d)
49235:8:50:2960:gnss_user.c:sm_user_handler_lsmp_prem_srvs_ind: unexpected len %d (expected %d)
49236:8:50:2980:gnss_user.c:sm_user_handler_lsmp_get_params: unexpected len %d (expected %d)
49237:8:50:3000:gnss_user.c:sm_user_handler_pf_power_state: unexpected len %d (expected %d)
49238:8:50:3020:gnss_user.c:sm_user_handler_report_diag_iq_test_cap: unexpected len %d (expected %d)
49239:8:50:3040:gnss_user.c:sm_user_handler_report_rf_dev_trk_cap: unexpected len %d (expected %d)
49240:8:50:3080:gnss_user.c:sm_user_handler_report_lte_otdoa_rcv_meas: unexpected len %d (expected %d)
49241:8:50:3101:gnss_user.c:sm_user_handler_wwan_qecid_rcv_meas: unexpected len %d (expected %d)
49242:8:50:3122:gnss_user.c:sm_user_handler_read_qmi_srv_config: unexpected len %d (expected %d)
49243:8:50:3060:gnss_user.c:sm_user_handle_loc_qmi_shim_broadcast_ind: unexpected len %d (expected %d)
49244:8:50:3142:gnss_user.c:sm_user_handle_lm_mgp_update_info: unexpected len %d (expected %d)
49245:8:50:3162:gnss_user.c:sm_user_handle_sm_self_test_results: unexpected len %d (expected %d)
49246:8:50:3182:gnss_user.c:sm_user_handle_sm_rpt_oem_japper_info: unexpected len %d (expected %d)
49247:8:50:3202:gnss_user.c:sm_user_handle_lm_sess_req_start: unexpected len %d (expected %d)
49248:8:50:3222:gnss_user.c:sm_user_handle_pdapi_client_init: unexpected len %d (expected %d)
49249:8:50:3242:gnss_user.c:sm_user_handle_pdapi_client_activate: unexpected len %d (expected %d)
49250:8:50:3262:gnss_user.c:sm_user_handle_pdapi_client_release: unexpected len %d (expected %d)
49251:8:50:3283:gnss_user.c:sm_user_handle_sm_reset_loc_service_begin: unexpected len %d (expected %d)
49252:8:50:3303:gnss_user.c:sm_user_handle_tle_delete_data: unexpected len %d (expected %d)
49253:8:50:3322:gnss_user.c:sm_user_handle_gdt_protected_extended_send: unexpected len %d (expected %d)
49254:8:50:3341:gnss_user.c:sm_user_handle_gdt_protected_extended_recv: unexpected len %d (expected %d)
49255:8:50:3362:gnss_user.c:sm_user_handle_inject_time_sync_data_ind_proxy: unexpected len %d (expected %d)
49256:8:50:3382:gnss_user.c:sm_user_handle_inject_sensor_data_ind_proxy: unexpected len %d (expected %d)
49257:8:50:3402:gnss_user.c:sm_user_handle_inject_motion_data_ind_proxy: unexpected len %d (expected %d)
49258:8:50:3421:gnss_user.c:sm_user_handle_inject_ped_rpt_data_ind_proxy: unexpected len %d (expected %d)
49259:8:50:3440:gnss_user.c:sm_user_handle_inject_veh_sensor_data_ind_proxy: unexpected len %d (expected %d)
49260:8:50:3554:gnss_user.c:sm_user_handle_gts_report_time: unexpected len %d (expected %d)
49261:8:50:3574:gnss_user.c:sm_user_handle_loc_gf_sensor_amd_cb: unexpected len %d (expected %d)
49262:8:50:3596:gnss_user.c:sm_user_handle_loc_gf_sensor_smd_cb: unexpected len %d (expected %d)
49263:8:50:3618:gnss_user.c:sm_user_handle_loc_gf_sensor_rmd_cb: unexpected len %d (expected %d)
49264:8:50:3640:gnss_user.c:sm_user_handle_loc_gf_sensor_ped_cb: unexpected len %d (expected %d)
49265:8:50:3662:gnss_user.c:sm_user_handle_loc_gf_sensor_cmd_db_cb: unexpected len %d (expected %d)
49266:8:50:3684:gnss_user.c:sm_user_handle_tm_slim_notify_data_cb: unexpected len %d (expected %d)
49267:8:50:3459:gnss_user.c:sm_user_handle_loc_slim_send_event_time_sync_needed_ind: unexpected len %d (expected %d)
49268:8:50:3478:gnss_user.c:sm_user_handle_loc_slim_send_event_sensor_streaming_readiness_ind: unexpected len %d (expected %d)
49269:8:50:3497:gnss_user.c:sm_user_handle_loc_slim_send_event_motion_data_control_ind: unexpected len %d (expected %d)
49270:8:50:3516:gnss_user.c:sm_user_handle_loc_slim_send_event_pedometer_control_ind: unexpected len %d (expected %d)
49271:8:50:3535:gnss_user.c:sm_user_handle_loc_slim_send_event_vehicle_sensor_injection_ind: unexpected len %d (expected %d)
49272:8:50:3706:gnss_user.c:sm_user_handle_jamming_report: unexpected len %d (expected %d)
49273:8:50:1226:gnss_user.c:sm_user_dispatcher_cb: Invalid driver data argument
49274:8:50:1232:gnss_user.c:gps_user_dispatcher_cb: Unexpected len %d (expected at least %d)
49275:8:50:1240:gnss_user.c:sm_user_dispatcher_cb: Invalid method index %d, maximum expected %d
49276:8:50:1246:gnss_user.c:sm_user_dispatcher_cb: No handler is registered for index %d
49277:8:50:4577:aries_os_api.c:GNSS MSGR: Failed to create GNSS MSGR client error=%d
49277:8:50:4577:aries_os_api.c:GNSS MSGR: Failed to create GNSS MSGR client error=%d
49278:8:50:4597:aries_os_api.c:GNSS MSGR: Failed to add rex_q for GNSS MSGR error=%d
49278:8:50:4597:aries_os_api.c:GNSS MSGR: Failed to add rex_q for GNSS MSGR error=%d
49279:8:50:4609:aries_os_api.c:GNSS MSGR: Failed to add rex_q for GNSS MSGR error=%d
49279:8:50:4609:aries_os_api.c:GNSS MSGR: Failed to add rex_q for GNSS MSGR error=%d
49382:8:50:566:gnss_msgr_task.c:GNSS MSGR: NULL pointer rcvd from os_IpcReceive()
49382:8:50:566:gnss_msgr_task.c:GNSS MSGR: NULL pointer rcvd from os_IpcReceive()
49383:8:50:596:gnss_msgr_task.c:GNSS MSGR: Unknown IPC MSG ID
49383:8:50:596:gnss_msgr_task.c:GNSS MSGR: Unknown IPC MSG ID
49384:8:50:684:gnss_msgr_task.c:GNSS MSGR: Can't registering for NAS MSGR messages since GNSS client not created!
49384:8:50:684:gnss_msgr_task.c:GNSS MSGR: Can't registering for NAS MSGR messages since GNSS client not created!
49384:8:50:684:gnss_msgr_task.c:GNSS MSGR: Can't registering for NAS MSGR messages since GNSS client not created!
49385:8:50:690:gnss_msgr_task.c:GNSS MSGR: NAS MSGR messages already registered!
49385:8:50:690:gnss_msgr_task.c:GNSS MSGR: NAS MSGR messages already registered!
49386:8:50:712:gnss_msgr_task.c:GNSS MSGR: Failed to register NAS DL message error=%d, variant=%d
49386:8:50:712:gnss_msgr_task.c:GNSS MSGR: Failed to register NAS DL message error=%d, variant=%d
49387:8:50:731:gnss_msgr_task.c:GNSS MSGR: Failed to register NAS UL CNF message error=%d, variant=%d
49387:8:50:731:gnss_msgr_task.c:GNSS MSGR: Failed to register NAS UL CNF message error=%d, variant=%d
49388:8:50:755:gnss_msgr_task.c:GNSS MSGR: Failed to register NAS failure IND message error=%d, variant=%d
49388:8:50:755:gnss_msgr_task.c:GNSS MSGR: Failed to register NAS failure IND message error=%d, variant=%d
49389:8:50:799:gnss_msgr_task.c:GNSS MSGR: Can't deregistering for NAS MSGR messages since GNSS client not created!
49389:8:50:799:gnss_msgr_task.c:GNSS MSGR: Can't deregistering for NAS MSGR messages since GNSS client not created!
49389:8:50:799:gnss_msgr_task.c:GNSS MSGR: Can't deregistering for NAS MSGR messages since GNSS client not created!
49390:8:50:805:gnss_msgr_task.c:GNSS MSGR: NAS MSGR messages not registered!
49390:8:50:805:gnss_msgr_task.c:GNSS MSGR: NAS MSGR messages not registered!
49391:8:50:825:gnss_msgr_task.c:GNSS MSGR: Failed to deregister NAS DL message error=%d, variant=%d
49391:8:50:825:gnss_msgr_task.c:GNSS MSGR: Failed to deregister NAS DL message error=%d, variant=%d
49392:8:50:835:gnss_msgr_task.c:GNSS MSGR: Failed to deregister NAS UL CNF message error=%d, variant=%d
49392:8:50:835:gnss_msgr_task.c:GNSS MSGR: Failed to deregister NAS UL CNF message error=%d, variant=%d
49393:8:50:845:gnss_msgr_task.c:GNSS MSGR: Failed to deregister NAS failure IND message error=%d, variant=%d
49393:8:50:845:gnss_msgr_task.c:GNSS MSGR: Failed to deregister NAS failure IND message error=%d, variant=%d
49394:8:50:895:gnss_msgr_task.c:GNSS MSGR: Can't send NAS MSGR message since GNSS client not created!
49394:8:50:895:gnss_msgr_task.c:GNSS MSGR: Can't send NAS MSGR message since GNSS client not created!
49394:8:50:895:gnss_msgr_task.c:GNSS MSGR: Can't send NAS MSGR message since GNSS client not created!
49395:8:50:901:gnss_msgr_task.c:GNSS MSGR: invalid NAS MSGR message(size=%d), can't send!
49395:8:50:901:gnss_msgr_task.c:GNSS MSGR: invalid NAS MSGR message(size=%d), can't send!
49396:8:50:907:gnss_msgr_task.c:GNSS MSGR: NULL ptr, NAS ptr = 0x%x
49396:8:50:907:gnss_msgr_task.c:GNSS MSGR: NULL ptr, NAS ptr = 0x%x
49397:8:50:921:gnss_msgr_task.c:GNSS MSGR: NULL MSGR attachment ptr! LPP=0x%x NASID=0x%x
49397:8:50:921:gnss_msgr_task.c:GNSS MSGR: NULL MSGR attachment ptr! LPP=0x%x NASID=0x%x
49398:8:50:932:gnss_msgr_task.c:GNSS MSGR: Only %d bytes pushed to DSM 0x%x (%d payload)
49398:8:50:932:gnss_msgr_task.c:GNSS MSGR: Only %d bytes pushed to DSM 0x%x (%d payload)
49399:8:50:947:gnss_msgr_task.c:GNSS MSGR: Only %d bytes pushed to DSM 0x%x (%d payload)
49399:8:50:947:gnss_msgr_task.c:GNSS MSGR: Only %d bytes pushed to DSM 0x%x (%d payload)
49400:8:50:982:gnss_msgr_task.c:GNSS MSGR: Failed to send NAS msg with size=%d transid=0x%x
49400:8:50:982:gnss_msgr_task.c:GNSS MSGR: Failed to send NAS msg with size=%d transid=0x%x
49401:8:50:1053:gnss_msgr_task.c:GNSS MSGR: Can't send NAS MSGR message since GNSS client not created!
49401:8:50:1053:gnss_msgr_task.c:GNSS MSGR: Can't send NAS MSGR message since GNSS client not created!
49401:8:50:1053:gnss_msgr_task.c:GNSS MSGR: Can't send NAS MSGR message since GNSS client not created!
49402:8:50:1059:gnss_msgr_task.c:GNSS MSGR: invalid parameter (buf_size=%d)!
49402:8:50:1059:gnss_msgr_task.c:GNSS MSGR: invalid parameter (buf_size=%d)!
49403:8:50:1096:gnss_msgr_task.c:GNSS MSGR: wrong attachments! num=%d LPP=0x%x NASID=0x%x
49403:8:50:1096:gnss_msgr_task.c:GNSS MSGR: wrong attachments! num=%d LPP=0x%x NASID=0x%x
49404:8:50:1106:gnss_msgr_task.c:GNSS MSGR: NULL DSM ptr! LPP=0x%x NASID=0x%x!
49404:8:50:1106:gnss_msgr_task.c:GNSS MSGR: NULL DSM ptr! LPP=0x%x NASID=0x%x!
49405:8:50:1115:gnss_msgr_task.c:GNSS MSGR: buf size %d too small (LPP msg size %d)
49405:8:50:1115:gnss_msgr_task.c:GNSS MSGR: buf size %d too small (LPP msg size %d)
49406:8:50:1126:gnss_msgr_task.c:GNSS MSGR: LPP DSM 0x%x only %d bytes pulled (length=%d)
49406:8:50:1126:gnss_msgr_task.c:GNSS MSGR: LPP DSM 0x%x only %d bytes pulled (length=%d)
49407:8:50:1140:gnss_msgr_task.c:GNSS MSGR: received wrong size %d for NAS ID for LPP
49407:8:50:1140:gnss_msgr_task.c:GNSS MSGR: received wrong size %d for NAS ID for LPP
49408:8:50:1150:gnss_msgr_task.c:GNSS MSGR: NASID DSM 0x%x only %d bytes pulled (length=%d)
49408:8:50:1150:gnss_msgr_task.c:GNSS MSGR: NASID DSM 0x%x only %d bytes pulled (length=%d)
49409:8:50:1180:gnss_msgr_task.c:GNSS MSGR: received unknown message from NAS module, discarding it
49409:8:50:1180:gnss_msgr_task.c:GNSS MSGR: received unknown message from NAS module, discarding it
49410:8:50:457:gnss_msgr_task.c:GNSS MSGR: Failed to send IPC mesage
49410:8:50:457:gnss_msgr_task.c:GNSS MSGR: Failed to send IPC mesage
49411:8:50:464:gnss_msgr_task.c:GNSS MSGR: Failed to create IPC mesage
49411:8:50:464:gnss_msgr_task.c:GNSS MSGR: Failed to create IPC mesage
49412:8:50:348:gnss_msgr_task.c:GNSS MSGR: client creation failed!
49412:8:50:348:gnss_msgr_task.c:GNSS MSGR: client creation failed!
49413:8:50:280:gnss_msgr_task.c:GNSS MSGR: Failed to get client ID
49413:8:50:280:gnss_msgr_task.c:GNSS MSGR: Failed to get client ID
49414:8:50:287:gnss_msgr_task.c:GNSS MSGR: Failed to create GNSS MSGR client error=%d
49414:8:50:287:gnss_msgr_task.c:GNSS MSGR: Failed to create GNSS MSGR client error=%d
49414:8:50:287:gnss_msgr_task.c:GNSS MSGR: Failed to create GNSS MSGR client error=%d
49415:8:50:303:gnss_msgr_task.c:GNSS MSGR: Failed to add rex_q for GNSS MSGR error=%d
49415:8:50:303:gnss_msgr_task.c:GNSS MSGR: Failed to add rex_q for GNSS MSGR error=%d
49415:8:50:303:gnss_msgr_task.c:GNSS MSGR: Failed to add rex_q for GNSS MSGR error=%d
49416:8:50:308:gnss_msgr_task.c:GNSS MSGR: Failed to delete GNSS MSGR client error=%d
49416:8:50:308:gnss_msgr_task.c:GNSS MSGR: Failed to delete GNSS MSGR client error=%d
49416:8:50:308:gnss_msgr_task.c:GNSS MSGR: Failed to delete GNSS MSGR client error=%d
49417:8:50:506:gnss_msgr_task.c:GNSS MSGR: Failed to send IPC mesage
49417:8:50:506:gnss_msgr_task.c:GNSS MSGR: Failed to send IPC mesage
49418:8:50:513:gnss_msgr_task.c:GNSS MSGR: Failed to create IPC mesage
49418:8:50:513:gnss_msgr_task.c:GNSS MSGR: Failed to create IPC mesage
49466:8:50:552:sm_api.c:SM_API: Null pointer access in sm_ReportGnssFixInfo()
49467:8:50:632:sm_api.c:SM_API: Null pointer access in sm_ReportGnssMeasInfo()
49472:8:50:785:sm_api.c:SM_API: Null pointer access in sm_ReportGnssMeasInfoNHz()
49479:8:50:1039:sm_api.c:SM_API: Null pointer access in sm_ReportGnssFixInfo()
49480:8:50:1153:sm_api.c:SM_API: translation from gnss assist data to internal assist data failed \n
49486:8:50:3741:sm_api.c:sm_ReportGnssTimeConversionInfo NULL Pointer
49494:8:50:476:sm_api.c:Memory allocation failed for gnss_MeasStructType in sm_clean_meas_rpt 
49563:8:50:694:sm_util.c:sm_GnssMeas_memscpy() NULL Ptr %d %d
49767:8:50:2433:gm_api.c:Ipc creation failed while sending GNSS Engine State update
49768:8:50:2448:gm_api.c:Failed to send GNSS Engine State update IPC to SM_GM
49828:8:50:3524:gm_motion_sensor_wifi_cdh.c:Cant process NULL GNSS fix
50133:8:50:3025:lm_tm.c:=LM TASK= Unknown GNSS Monitor type received %u
50134:8:50:3070:lm_tm.c:=LM TASK= Unknown GNSS Monitor type received %u
50158:8:50:326:lm_task.c:=LM TASK= NV item has out of limits value = %d, hence setting default NV NV_GNSS_LM_HEPE_THRESHOLD = %d 
50159:8:50:341:lm_task.c:mgp_GnssGetConstellationConfig FAILED
50186:8:50:2076:lm_diag.c:=LM TASK= LM received NULL GNSS RF Cmd from TM diag
50187:8:50:2119:lm_diag.c:=LM TASK= LM received NULL GNSS Prx RF Cmd from TM diag
50222:8:50:3061:lm_mgp.c:Received GNSS LE fix
50233:8:50:4971:lm_mgp.c:lm_translate_gnss_to_gps_assist_data: API should only be used when FEATURE_GPS_GEN7_ME_API is defined
50315:8:50:1385:tm_api.c:Null pointer in handling GNSS LE Fix
50362:8:50:3038:tm_api.c:pz_SupportedGnssBands is NULL
50363:8:50:3053:tm_api.c:IPC msg for GNSS Bands Config failed
50621:8:50:20168:tm_core.c:TM: pz_SupportedGnssBands is NULL
50630:8:50:13229:tm_core.c:tm_core: NULL p_gnss_fix_rpt? %d, ReqType %d
50810:8:50:3594:tm_loc_processing_client.c:TM: Couldn't allocate memory in heap for gnss_MeasStructType in lpc_filter_meas_rpt
50970:8:50:894:tm_ruim.c:GNSS MCFG_REFRESH got NULL ptr!
50973:8:50:961:tm_ruim.c:GNSS MCFG_REFRESH registration fail!
51022:8:50:2414:tm_util.c:TM: Could not allocated memory for gnss_MeasStructType type in tm_util_clean_qzss_sbas_meas_rpt 
51028:8:50:2742:tm_util.c:mgp_GnssGetConstellationConfig FAILED
51035:8:50:3227:tm_util.c:Unknown/Unsupported Mgp/Gnss Signal Type!
51036:8:50:3280:tm_util.c:Unknown/Unsupported PDSM/Gnss Signal Type!
51048:8:50:222:tm_lte_nas_msgr_iface.c:GNSS MSGR: Failed to init tm_msgr
51049:8:50:239:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register LTE TLB Info IND message
51050:8:50:249:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register Emergency Mode Status IND message
51051:8:50:261:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register NR 3GPP TLB Info IND message
51052:8:50:273:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register Emergency SMS sent IND  message
51053:8:50:284:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register FTM IND message
51054:8:50:295:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register NAS CIPHER KEY DATA IND message
51055:8:50:305:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register NR5g RRC POS_SIB IND message
51056:8:50:342:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: Failed to register POLICYMAN_CFG_COUNTRY_CHANGE_IND message
51057:8:50:354:tm_lte_nas_msgr_iface.c:TM: GNSS MSGR: No Msgs registered! Deleting client!!
51218:8:50:540:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_decode_header Error: invalid u_LenFitGnss [%d]\n
51221:8:50:776:tm_decode_xtra3_data.c:=TM XTRA= tm_xtra3_update_gnss_xtra_header Error: invalid arguments
51468:8:50:12153:tm_decode_xtra3_data.c:=TM XTRA= GAL FNAV clock corr Week [%u] too much ahead of the header GNSS week number [%u]
51693:8:50:1378:tm_diag_pfa_test.c:tm_diag_PfaTestPrescDwellResultsHandler: Unsupported GNSS type %d
51862:8:50:378:tm_broadcast_ad_nr_rrc.c:drop unsupported gnss_id %d for possib
51898:8:50:214:tm_broadcast_ad_nr_rrc.c:Unknown gnss_id %d
51931:8:50:1802:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_RTK_ReferenceStationInfo_r15
51937:8:50:1952:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_RTK_CommonObservationInfo_r15
51940:8:50:1595:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_correction_points_r16
51946:8:50:2095:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_RTK_Observations_r15
51949:8:50:2157:tm_broadcast_ad_lpp.c:p_dgnss_meas is NULL or meas_index %d is out of bound
51955:8:50:160:tm_broadcast_ad_lpp.c:Unknown GNSS ID!! %d
51957:8:50:459:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_orbit_corr_r15
51962:8:50:284:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_clk_corr_r15
51967:8:50:670:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_code_bias_r15
51972:8:50:834:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_ura_r16
51977:8:50:1420:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_phase_bias_r16
51982:8:50:983:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_stec_corr_r16
51987:8:50:1144:tm_broadcast_ad_lpp.c:TM_BROADCAST_LPP: Malloc failed for p_gnss_ssr_grid_corr_r16
52073:8:50:2529:tm_lpp.c:TM_LPP: ProvLocInfo GNSS-Error build buffer alloc failure
52074:8:50:2595:tm_lpp.c:TM_LPP: ProvLocInfo GNSS Meas build buffer alloc failure
52147:8:50:6079:tm_lpp.c:Bad GNSS-to-id in BDS TimeModel %d
52196:8:50:650:tm_lpp_common_utils.c:Disallowed AGNSS request combination 0x%X(raw) 0x%X
52197:8:50:681:tm_lpp_common_utils.c:AGNSS MSB request pending 0x%X
52198:8:50:701:tm_lpp_common_utils.c:AGNSS MSA request pending 0x%X
52205:8:50:1360:tm_lpp_common_utils.c:Can't copy GNSS position
52207:8:50:1446:tm_lpp_common_utils.c:Can't copy GNSS meas
52353:8:50:11099:tm_lpp_cp.c:Gnss_Auxi timer null
52382:8:50:2322:tm_lpp_cp.c:AA Data not for Glonass. gnss_SignalID %d
52384:8:50:2355:tm_lpp_cp.c:could not make local copy of GNSS AA data size %d bytes
52403:8:50:4418:tm_lpp_cp.c:GNSS method null
52415:8:50:5653:tm_lpp_cp.c:GNSS Aux Timer not allocated
52528:8:50:1524:tm_lpp_up.c:Dynamic mem alloc failed for GNSS AA data size %d bytes
52529:8:50:1581:tm_lpp_up.c:Dynamic mem alloc failed. Gnss auxi info size %d bytes
52534:8:50:1881:tm_lpp_up.c:TM_LPP_UP: GNSS ID %lu not supported for Genereic AD
52550:8:50:2505:tm_lpp_up.c:GNSS method null
52563:8:50:5063:tm_lpp_up.c:GNSS Aux Timer was not allocated
52570:8:50:5650:tm_lpp_up.c:Gnss_Auxi timer null
52703:8:50:118:tech_sel_rules_engine.cpp:GNSS TechMgr/Wifi TechMgr/Event Manager instance in NULL
52755:8:50:590:tech_sel_rule_wifi_assist_helper.cpp:NULL pointer in GNSS calculation
52761:8:50:1040:tech_sel_rule_wifi_assist_helper.cpp:Could not create instance of Gnss Strength event
52785:8:50:239:tech_sel_rule_pdr.cpp:Invalid GNSS Qual %d or Ped Dev Ctx %d
52807:8:50:136:tech_sel_gnss_tech_mgr.cpp:Null Event Manager Instance found
52808:8:50:318:tech_sel_gnss_tech_mgr.cpp:Null pointer passed in Lppm status ind
52809:8:50:352:tech_sel_gnss_tech_mgr.cpp:Null pointer in handling Gnss Engine State Change Ind
52809:8:50:352:tech_sel_gnss_tech_mgr.cpp:Null pointer in handling Gnss Engine State Change Ind
52810:8:50:428:tech_sel_gnss_tech_mgr.cpp:Null pointer in handling Session Start
52811:8:50:513:tech_sel_gnss_tech_mgr.cpp:Null pointer in handling Session Stop
52812:8:50:724:tech_sel_gnss_tech_mgr.cpp:Already in LPPM. Current Mode %d TBM %d. Use Modify LPPM for change
52813:8:50:731:tech_sel_gnss_tech_mgr.cpp:Failed to put engine in LPPM mode
52814:8:50:806:tech_sel_gnss_tech_mgr.cpp:Failed to modify LPPM Mode/TBM
52815:8:50:1202:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Ped Change event
52816:8:50:1208:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52817:8:50:1245:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Gnss Qual Change event
52817:8:50:1245:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Gnss Qual Change event
52818:8:50:1251:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52819:8:50:1288:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Rel Dev Pos Change event
52820:8:50:1294:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52821:8:50:1329:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Tbm status event
52822:8:50:1335:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52823:8:50:1373:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Tbm status event
52824:8:50:1379:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52825:8:50:1414:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Session Start event
52826:8:50:1420:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52827:8:50:1456:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Fix Start event
52828:8:50:1462:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52829:8:50:1499:tech_sel_gnss_tech_mgr.cpp:Could not create instance of E911 Start event
52830:8:50:1505:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52831:8:50:1542:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Session Stop event
52832:8:50:1548:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52833:8:50:1584:tech_sel_gnss_tech_mgr.cpp:Could not create instance of Ped Assist Avail event
52834:8:50:1590:tech_sel_gnss_tech_mgr.cpp:Could not get Event Manager Instance
52835:8:50:1675:tech_sel_gnss_tech_mgr.cpp:Failed to start GNSS fix
52835:8:50:1675:tech_sel_gnss_tech_mgr.cpp:Failed to start GNSS fix
52836:8:50:1698:tech_sel_gnss_tech_mgr.cpp:Started GNSS fix
52836:8:50:1698:tech_sel_gnss_tech_mgr.cpp:Started GNSS fix
52837:8:50:1725:tech_sel_gnss_tech_mgr.cpp:Max Requests received for On Demand Gnss Fixes
52837:8:50:1725:tech_sel_gnss_tech_mgr.cpp:Max Requests received for On Demand Gnss Fixes
52838:8:50:1779:tech_sel_gnss_tech_mgr.cpp:Max Requests received for On Demand Gnss Fixes
52838:8:50:1779:tech_sel_gnss_tech_mgr.cpp:Max Requests received for On Demand Gnss Fixes
52839:8:50:1825:tech_sel_gnss_tech_mgr.cpp:Max Requests received for unsolicited Gnss Fixes
52839:8:50:1825:tech_sel_gnss_tech_mgr.cpp:Max Requests received for unsolicited Gnss Fixes
52840:8:50:1856:tech_sel_gnss_tech_mgr.cpp:Max Requests received for Gnss Le Fixes
52840:8:50:1856:tech_sel_gnss_tech_mgr.cpp:Max Requests received for Gnss Le Fixes
52841:8:50:1891:tech_sel_gnss_tech_mgr.cpp:Nothing to deregister for Gnss Le Fixes
52841:8:50:1891:tech_sel_gnss_tech_mgr.cpp:Nothing to deregister for Gnss Le Fixes
52842:8:50:1929:tech_sel_gnss_tech_mgr.cpp:Null pointer found in event handler
52843:8:50:1965:tech_sel_gnss_tech_mgr.cpp:Null pointer found in Pos fix event
52844:8:50:2160:tech_sel_gnss_tech_mgr.cpp:Duplicate Sensor Le Start Request
52845:8:50:2166:tech_sel_gnss_tech_mgr.cpp:Max number of Sensor LE requests already being handled
52846:8:50:2209:tech_sel_gnss_tech_mgr.cpp:Wrong Request Id passed in Stop Sensor LE request
52847:8:50:2215:tech_sel_gnss_tech_mgr.cpp:Sensor Le Request is already stopped
52848:8:50:2253:tech_sel_gnss_tech_mgr.cpp:Wrong Request Id passed in Get Sensor LE request
52849:8:50:2259:tech_sel_gnss_tech_mgr.cpp:Wrong Request Id passed in Get Sensor LE request
52850:8:50:2289:tech_sel_gnss_tech_mgr.cpp:Null pointer passed while handling Sensor Le Status
52851:8:50:2333:tech_sel_gnss_tech_mgr.cpp:Null pointer passed while handling Sensor Le Rel Disp
53084:8:50:13695:tm_umts_up_supl.c:u_agnss_error_reported = TRUE
53214:8:50:1179:tm_rrlp_utils.c:Unexpected NULL pointer p_gnss_glonass_sat_elem !!
53222:8:50:138:tm_rrlp_utils.c:Bad GNSS-to-id in BDS TimeModel %d
53308:8:50:297:gnss_wwan_iface.c:gnss_wwan_iface_rrcmeas_get_cell_plmn_info_sub: Call failed. NO OEMPD support for sub id %u
53308:8:50:297:gnss_wwan_iface.c:gnss_wwan_iface_rrcmeas_get_cell_plmn_info_sub: Call failed. NO OEMPD support for sub id %u
53309:8:50:463:gnss_wwan_iface.c:gnss_wwan_iface_l1_cgps_register_event_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53309:8:50:463:gnss_wwan_iface.c:gnss_wwan_iface_l1_cgps_register_event_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53310:8:50:515:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_event_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53310:8:50:515:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_event_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53311:8:50:562:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_ue_pos_capability_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53311:8:50:562:gnss_wwan_iface.c:gnss_wwan_iface_rrcgps_register_cgps_ue_pos_capability_cb_sub: Call failed. WCDMA handle is NULL for sub id %u
53312:8:50:6996:gps_common.c:gnss_GetQtime(),QTime invalid
53325:8:50:7070:aries_gpsdiag.c:gpsdiag_GNSSDiagBufferTest - p_Req is NULL
53326:8:50:7087:aries_gpsdiag.c:gpsdiag_GNSSDiagBufferTest - Unknown command %d
53327:8:50:516:gnss_diag_buf.c:Unknown DiagBuffer!!
53328:8:50:1251:gnss_diag_buf.c:Reset time too small %dms. Increase to 10ms!
53329:8:50:1269:gnss_diag_buf.c:Could not set Tx Mode to STREAMING for buffer %d!! Error %d!
53330:8:50:1329:gnss_diag_buf.c:Could not set Tx Mode to CIRCULAR for buffer %d!! Error %d!
53331:8:50:373:gnss_diag_buf.c:Null ptr 0x%x
53332:8:50:380:gnss_diag_buf.c:invalid buff size %d
53333:8:50:389:gnss_diag_buf.c:Buffer already allocated 0x%x size %d
53334:8:50:453:gnss_diag_buf.c:Error code %d DalErr %d
53335:8:50:321:gnss_diag_buf.c:Could not allocate buffer of size %d! try half the size
53336:8:50:329:gnss_diag_buf.c:Could not allocate buffer of size %d! give up!
53337:8:50:604:gnss_diag_buf.c:Could not create diagBuffer. Error %d
53338:8:50:611:gnss_diag_buf.c:Could not create mutex for diagBuffer
53339:8:50:620:gnss_diag_buf.c:Could not register CB!!
53344:8:50:149:tech_sel_log.cpp:Tech Sel Gnss Qual Change Log allocation failure
53346:8:50:227:tech_sel_log.cpp:Tech Sel Gnss Strength Change Log allocation failure
56353:8:80:2363:sdp_qmi_loc.c:[QMILOC] sdp_qmiloc_gnss_state_change_notification_handler: error code: %u
58410:2:81:1802:tle_dbm_mgr.cpp:Skip this cell because only upload cells with GNSS
58411:2:81:1809:tle_dbm_mgr.cpp:Skip this cell because GnssCont:%u is less than %u
58594:2:81:257:tle_wwan_tdp_upload_conditions.cpp:Position source is not GNSS.
58740:2:81:2322:tle_xta_mgr.cpp:Logging of LOG_GNSS_GTP_TDP_CONFIG_C in not enabled
59395:4:81:257:tle_task.cpp:GNSS Debug: Creating tle_task
60679:8:81:114:tle_xta_mgr.cpp:Failed to read GNSS_NV_EFS_TLE_XTRAT_USER_SESSION_CONTROL, from NV EFS
60680:8:81:121:tle_xta_mgr.cpp:Failed to read GNSS_NV_EFS_TLE_XTRAT_FEATURE_CONTROL, from NV EFS
63488:4:85:393:gts_api.c:PutTimeEst: GNSS const  type %u
64516:8:85:398:gts_api.c:PutTimeEst: GNSS type mismatch %u %u %u
64519:8:85:533:gts_api.c:PutTimeEst: Invalid GNSS type %u
65546:1:86:2185:loc_pa.c:locPa_DeleteAssistData: GnssData QmiLocMask=0x%llx, PdapiMask=0x%llx, RetVal=%d
65548:1:86:3632:loc_pa.c:locPa_DeleteGnssServiceData: RetVal=%d
65561:1:86:8310:loc_pd.c:locPd_HandleSvMeasReport:dgnssSvMeasurement_valid %d measStatus 0x%x 
65562:1:86:7766:loc_pd.c:locPd_HandleSvMeasReport:dgnss corr src %d src ID %d ref Stn ID %d
65567:1:86:1208:loc_ni.c:locNi_XlateSuplReqFromPdapiToQmi: supl extensions,trigger type = %d,supported Networks = %u, gnss type = %u\n
66560:2:86:3243:loc_qmi_shim.c:locQmiReadGnssOemLockCtrlNv: NV Read succeeded for LOCK_CTRL_FOR_OEM
66561:2:86:3280:loc_qmi_shim.c:locQmiReadGnssOemLockCtrlNv NV value = 0x%x, HwRead=%d, PosRequest=%d, Notification=%d, Priviledged = %d
66661:2:86:1822:loc_pa.c:Received gnss_sv_id: %lu, gnss_sv_id_min: %u, gnss_sv_id_max: %u
66661:2:86:1822:loc_pa.c:Received gnss_sv_id: %lu, gnss_sv_id_min: %u, gnss_sv_id_max: %u
66661:2:86:1822:loc_pa.c:Received gnss_sv_id: %lu, gnss_sv_id_min: %u, gnss_sv_id_max: %u
66757:2:86:2790:loc_pd.c:locPd_GetReport interm_pos_info DGNSS src valid %d
66759:2:86:2176:loc_pd.c:Null pz_DgnssCorrSrc. Set DGNSS info false!
66759:2:86:2176:loc_pd.c:Null pz_DgnssCorrSrc. Set DGNSS info false!
66760:2:86:2243:loc_pd.c: locPd_PopulateDgnssCorrInfo %d %d 0x%x
66781:2:86:5873:loc_pd.c:locPd_EncodeGetAvailableGnssPos Calling Secure API for Encryption
66794:2:86:8877:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GPS_L1CA metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66795:2:86:8891:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GPS_L2C metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66796:2:86:8907:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GPS_L5 metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66797:2:86:8922:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GLO_G1 metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66798:2:86:8938:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GLO_G2 metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66799:2:86:8954:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_BDS_B1I metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66800:2:86:8969:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_BDS_B2A metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66801:2:86:8984:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GAL_E1 metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66802:2:86:8999:loc_pd.c:locPd_ExtEventCb, PDSM_GNSS_SIG_TYPE_GAL_E5A metrices bpAmpI = %d, bpAmpQ = %d, jammerPwrDb = %d
66805:2:86:7457:loc_pd.c:locPd_HandleGnssBandsCfg:  Primary Signal: PdsmType=%x QmiType=%x
66806:2:86:7461:loc_pd.c:locPd_HandleGnssBandsCfg:  gnssSupportedSignals: PdsmType=%x QmiType=%x
66806:2:86:7461:loc_pd.c:locPd_HandleGnssBandsCfg:  gnssSupportedSignals: PdsmType=%x QmiType=%x
66949:2:86:3259:loc_client.c:AFW client registers for Event QMI_LOC_EVENT_MASK_GNSS_BANDS_SUPPORTED_V02, Request Supported Gnss Bands Config, Client =%d
66949:2:86:3259:loc_client.c:AFW client registers for Event QMI_LOC_EVENT_MASK_GNSS_BANDS_SUPPORTED_V02, Request Supported Gnss Bands Config, Client =%d
66950:2:86:3265:loc_client.c:AFW Client has previously for event QMI_LOC_EVENT_MASK_GNSS_BANDS_SUPPORTED_V02, Client=%d
66956:2:86:3012:loc_client.c:Send a Request to SM to get GNSS Bands Cfg from MGP 
67013:2:86:803:loc_constrained_tunc_client.c:TUNC CONSTRAINT: Query GNSS Energy Consumed!!
67599:4:86:7226:loc_qmi_shim.c:Received Notification from PCS. GnssState=%d
67600:4:86:7232:loc_qmi_shim.c:GnssPeripheralState changed from %d ==> %d
67609:4:86:3208:loc_qmi_shim.c:FeatureNotSupported. GnssCtrl=0x%X EnabledByPcs=%d FeatureSupported=%d
67699:4:86:2878:loc_pa.c:locPa_SetGnssConstellRptCfg: RetVal=%d, MeasRptCfg=%d, SvPolyCfg=%d RptEph=%d RptPoly=%d
67720:4:86:4312:loc_pa.c:locPa_SetTribandGnssConfig: QmiConfig=%d PdsmConfid=%d RetVal=%u
67760:4:86:8770:loc_pd.c:locPd_ExtEventCb PDSM_EXT_STATUS_EVENT_GNSS_MEASUREMENT Seq[%u]/[%u] NumSvs=%u FC=%u PdSig=%d NHz=%d
67765:4:86:9130:loc_pd.c:locPd_ExtEventCb PDSM_EXT_STATUS_EVENT_GNSS_BANDS_SUPPORTED_CFG
67847:4:86:2467:loc_client.c:QMI_LOC_GNSS_BANDS_SUPPORTED MSG ID = 0x00EA for Event= 0X%08x not sent, supported only for AFW client. Client Type:
67889:4:86:939:loc_qteecom_svc.c:loc_QteecomIPFMFeatureSet() eDGNSS feature enable: %d:%d
68612:8:86:3247:loc_qmi_shim.c:locQmiReadGnssOemLockCtrlNv: NV Read failed for LOCK_CTRL_FOR_OEM
68697:8:86:4215:loc_qmi_shim.c:locStartBatchingReq: GNSS Disabled by PCS
68702:8:86:4415:loc_qmi_shim.c:locStartDbtReq: GNSS Disabled by PCS
68712:8:86:4667:loc_qmi_shim.c:locStartOTBReq: GNSS Disabled by PCS
69011:8:86:917:loc_pa.c:locPa_ProcessQmiRequest QMI_LOC_SET_GNSS_CONSTELL_REPORT_CONFIG NULL p_QmiLocMsgData
69016:8:86:978:loc_pa.c:locPa_ProcessQmiRequest QMI_LOC_DELETE_GNSS_SERVICE_DATA_REQ NULL p_QmiLocMsgData
69075:8:86:2831:loc_pa.c:locPa_SetGnssConstellRptCfg: NULL parameters
69076:8:86:2839:loc_pa.c:locPa_SetGnssConstellRptCfg: Invalid configurations received
69089:8:86:3510:loc_pa.c:locPa_XlateDeleteGnssServiceData, 0x%x, 0x%x
69090:8:86:3519:loc_pa.c:locPa_XlateDeleteGnssServiceData, invalid deleteAllFlag %d
69119:8:86:4283:loc_pa.c:locPa_SetTribandGnssConfig, l_ClientHandle = %d
69120:8:86:4289:loc_pa.c:locPa_SetTribandGnssConfig: NULL Pointer received for Set
69121:8:86:4295:loc_pa.c:locPa_SetTribandGnssConfig: No Config provided
69185:8:86:1215:loc_pd.c:locPd_GetSvReport, NULL pz_GnssSvInfoIndMsg
69212:8:86:5594:loc_pd.c:locPd_EncodeGetAvailableGnssPos Memory Allocation 2 failed
69214:8:86:7496:loc_pd.c:Invalid Gnss System %lu. Default to GPS
69226:8:86:7548:loc_pd.c:locPd_PopulateGnssMeasHeader Invalid Args %p %p
69232:8:86:7434:loc_pd.c:locPd_HandleGnssBandsCfg NULL ExtStatus
69233:8:86:7441:loc_pd.c:locPd_HandleGnssBandsCfg NULL pz_GnssSupportedBandsInd
69233:8:86:7441:loc_pd.c:locPd_HandleGnssBandsCfg NULL pz_GnssSupportedBandsInd
69333:8:86:555:loc_mgp_iface.c:locMgp_HandleEphemerisReport: Invalid GNSS Type %d
69342:8:86:163:loc_mgp_iface.c:fillEphReportingTime: NULL Input Ptr, QMI=%d GNSS=%d, Or Invalid Time
69351:8:86:633:loc_mgp_iface.c:Invalid GNSS System %d. Default to GPS
69441:8:86:4453:loc_client.c:IsPosRequestAllowed:FORCED_DISABLED:q_GnssCtrlConfig=0x%X
69459:8:86:3271:loc_client.c:Event Registration Failed. Only AFW client can register for Event=QMI_LOC_EVENT_MASK_GNSS_BANDS_SUPPORTED_V02, ClientType=%d, Client=%d
69525:8:86:857:loc_conv_locEng_qmiLoc.c:convertLocEngPosToAvailGnssPos: NULL param error, pLocEngPosition = %p, pAvailablePosition = %p
69526:8:86:945:loc_conv_locEng_qmiLoc.c:convert_pos_report_to_gnss: Unexpected status %d\n
69527:8:86:1024:loc_conv_locEng_qmiLoc.c:convert_pos_report_to_gnss: Confidence  valid %d  %d\n
69610:8:86:807:loc_constrained_tunc_client.c:loc_mw_query_gnss_energy_consumed NULL ptr!
69613:8:86:863:loc_constrained_tunc_client.c:loc_query_gnss_energy_consumed:NULL pointer!
69660:8:86:544:aon_gmproxy.cpp:xlatePositionRpt NULL p_gnss_fix_rpt
10625182:88:3:RFC_ID=10:dtr_iu_rx_adc_tag.c:IU_TRACE:Time 0x%x, IU_ID 0x%x, sub6_rx_gnss_tag - snapshot wb mask Chain 0x%x, Chain 0x%x , layer_mask 0x%x
10625183:192:3:RFC_ID=10:dtr_iu_rx_adc_tag.c:IU_TRACE:Time 0x%x, IU_ID 0x%x, sub6_rx_gnss_tag - snapshot wb_bmask 0X%x,result_idx 0x%x
4163987544:921|844f8da5:2:PM:I policyman_context.c:POLICYMAN:gnss sequence status: %d, index %d
4164708165:64378:2:CM:qmi_nas.c:=QMI=:HST_INFO_IND: LTE high_speed_flag %d, gnss_speed_hst_state %d, sub %d
4164708262:64399:2:CM:qmi_nas.c:=QMI=:HST_INFO_IND: NR5G high_speed_flag %d, gnss_speed_hst_state %d, sub %d
4165340358:2463:2:MM:emm_esm_handler.c:MM:DS: SUB %d =EMM= Restored values: Backoff Timer for GNSS initial value = %d, Backoff Timer for GNSS current value = %d, Backoff timer value Ue for rej 78 = %d NTN backoff threshold distance = %d
4165340358:2463:2:MM:emm_esm_handler.c:MM:DS: SUB %d =EMM= Restored values: Backoff Timer for GNSS initial value = %d, Backoff Timer for GNSS current value = %d, Backoff timer value Ue for rej 78 = %d NTN backoff threshold distance = %d
4165379484:9700:2:MM:emm_rrc_handler.c:MM:DS: SUB %d =EMM= NTN backoff timer for GNSS fail reset value : %d
4165489229:13911:2:MM:emm_utility.c:MM:DS: SUB %d =EMM= NTN backoff_timer_val 0x%x backoff_timer_val_for_gnss_failure 0x%x geo_distance_between_backoff_loc_and_curr_loc 0x%x
4165728420:5093:2:MM:mm5g_registration_handler.c:MM:DS: SUB %d =MM5G= Dropping unexpected Gnss Ciphering Key received from NW
4165728535:5102:2:MM:mm5g_registration_handler.c:MM:DS: SUB %d =MM5G= Dropping unexpected Gnss Ciphering Key received from NW
4165837729:9033:2:MM:mm5g_multimode_handler.c:MM:DS: SUB %d =MM5G= GNSS msg id - 0x%x received
4165903070:137:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Update current registered PLMN with invalid TAC for ciphering_key[%d]
4165903190:147:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Sending NAS_MM5G_GNSS_CIPHER_KEY_DATA_IND status = %u
4165903190:147:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Sending NAS_MM5G_GNSS_CIPHER_KEY_DATA_IND status = %u
4165903294:151:3:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Failed to send NAS_MM5G_GNSS_CIPHER_KEY_DATA_IND
4165903294:151:3:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Failed to send NAS_MM5G_GNSS_CIPHER_KEY_DATA_IND
4165903393:270:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_add_gnss_cipher_key_info PLMN (%u - %u), TAC %u %u %u ciphering_key_set_id %u remaining_validity_time %u
4165903393:270:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_add_gnss_cipher_key_info PLMN (%u - %u), TAC %u %u %u ciphering_key_set_id %u remaining_validity_time %u
4165903559:272:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_add_gnss_cipher_key_info 
4165903559:272:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_add_gnss_cipher_key_info 
4165903646:336:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_is_gnss_cipher_key_info_for_tai_expired expired %u entry_exists %u
4165903646:336:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_is_gnss_cipher_key_info_for_tai_expired expired %u entry_exists %u
4165903774:391:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - Find gnss cipher entry PLMN (%u - %u), TAC %u %u %u is_next_expiry %u
4165903774:391:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - Find gnss cipher entry PLMN (%u - %u), TAC %u %u %u is_next_expiry %u
4165903900:405:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - Found entry for cipher info
4165903984:445:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - Cleanup chipher key info list
4165904070:501:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_delete_gnss_cipher_key_info_for_tai
4165904070:501:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_delete_gnss_cipher_key_info_for_tai
4165904167:553:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_delete_gnss_cipher_key_info_for_plmn
4165904167:553:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_delete_gnss_cipher_key_info_for_plmn
4165904265:654:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_start_gnss_cipher_key_validity_timer timer %u timer_status %u
4165904265:654:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_start_gnss_cipher_key_validity_timer timer %u timer_status %u
4165904388:716:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= offset from current time %d
4165904466:827:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Registraiton to fetch gnss ciphering key will be done later when feasible
4165904466:827:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Registraiton to fetch gnss ciphering key will be done later when feasible
4165904590:846:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Registraiton to fetch gnss ciphering key will be done later when feasible
4165904590:846:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Registraiton to fetch gnss ciphering key will be done later when feasible
4165904714:850:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Attempt to start Registraiton to fetch gnss ciphering key
4165904714:850:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Attempt to start Registraiton to fetch gnss ciphering key
4165904822:892:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= GNSS msg START CIPHER KEY FETCH, current_ciphekey_fetch_state %u
4165904822:892:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= GNSS msg START CIPHER KEY FETCH, current_ciphekey_fetch_state %u
4165904937:896:3:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Received START CIPHER KEY FETCH before STOP CIPHER KEY FETCH
4165905048:925:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= GNSS msg STOP CIPHER KEY FETCH, current_ciphekey_fetch_state %u
4165905048:925:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= GNSS msg STOP CIPHER KEY FETCH, current_ciphekey_fetch_state %u
4165905162:929:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= Received STOP CIPHER KEY FETCH before START CIPHER KEY FETCH
4165905273:996:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_clear_gnss_cipher_key_list 
4165905273:996:2:MM:mm5g_gnss_handler.c:MM:DS: SUB %d =MM5G= DBG - mm5g_clear_gnss_cipher_key_list 
4166086466:8448:2:REG:reg_sim.c:REG:DS: SUB %d =REG DBG= reg_sim_update_ntn_fplmn_list_based_on_gnss longitude 0x%x latitude 0x%x altitude 0x%x
4166086599:8471:2:REG:reg_sim.c:REG:DS: SUB %d =REG DBG= reg_sim_update_ntn_fplmn_list_based_on_gnss longitude 0x%x latitude 0x%x altitude 0x%x
4166086732:8478:2:REG:reg_sim.c:REG:DS: SUB %d =REG DBG= reg_sim_update_ntn_fplmn_list_based_on_gnss removing plmn 0x%x 0x%x 0x%x



```








## LOG_GPS



```


/* 20 Position & speed info read from GPS receiver */
#define LOG_GPS_C                                      (0x14 + LOG_1X_BASE_C)

/* 54 IS-801 forward link message */
#define LOG_GPS_FWD_MSG_C                              (0x36 + LOG_1X_BASE_C)

/* 55 IS-801 reverse link message */
#define LOG_GPS_REV_MSG_C                              (0x37 + LOG_1X_BASE_C)

/* 56 GPS search session statistics */
#define LOG_GPS_STATS_MSG_C                            (0x38 + LOG_1X_BASE_C)

/* 57 GPS search results */
#define LOG_GPS_SRCH_PEAKS_MSG_C                       (0x39 + LOG_1X_BASE_C)


/* 150 GPS Visit Parameters */
#define LOG_GPS_VISIT_PARAMETERS_C                     (0x96 + LOG_1X_BASE_C)

/* 151 GPS Measurement */
#define LOG_GPS_MEASUREMENT_C                          (0x97 + LOG_1X_BASE_C)



/* 202 gpsOne fatpath information */
#define LOG_GPS_FATPATH_INFO_C                         (0xCA + LOG_1X_BASE_C)


/* 218 */
#define LOG_GPS_BIT_EDGE_RESULTS_C                     (0xDA + LOG_1X_BASE_C)

/* 221 */
#define LOG_GPS_SINGLE_PEAK_SRCH_RESULTS_C             (0xDD + LOG_1X_BASE_C)



/* 225 */
#define LOG_GPS_SCHEDULER_TRACE_C                      (0xE1 + LOG_1X_BASE_C)


/* GPS demodulation tracking header info */
#define LOG_GPS_DEMOD_TRACKING_HEADER_C                 (0x250 + LOG_1X_BASE_C)

/* GPS demodulation tracking results */
#define LOG_GPS_DEMOD_TRACKING_C                        (0x251 + LOG_1X_BASE_C)

/* GPS bit edge logs from demod tracking */
#define LOG_GPS_DEMOD_BIT_EDGE_C                        (0x252 + LOG_1X_BASE_C)

/* GPS demodulation soft decisions */
#define LOG_GPS_DEMOD_SOFT_DECISIONS_C                  (0x253 + LOG_1X_BASE_C)

/* GPS post-processed demod tracking results */
#define LOG_GPS_DEMOD_TRACKING_POST_PROC_C              (0x254 + LOG_1X_BASE_C)

/* GPS subframe log */
#define LOG_GPS_DEMOD_SUBFRAME_C                        (0x255 + LOG_1X_BASE_C)


/* 739 GPS PE Position Report log */
#define LOG_GPS_PE_POSITION_REPORT_C                    (0x2E3 + LOG_1X_BASE_C)

/* 740 GPS PE Position Report Extended log */
#define LOG_GPS_PE_POSITION_REPORT_EXT_C                (0x2E4 + LOG_1X_BASE_C)

/* 741 log */
#define LOG_MDDI_HOST_STATS_C                           (0x2E5 + LOG_1X_BASE_C)

/* GPS Decoded Ephemeris */
#define LOG_GPS_DECODED_EPHEMERIS_C                     (0x2E6 + LOG_1X_BASE_C)

/* GPS Decoded Almanac */
#define LOG_GPS_DECODED_ALMANAC_C                       (0x2E7 + LOG_1X_BASE_C)



/* GPS Position Engine Info */
#define LOG_GPS_POSITION_ENGINE_INFO_C                  (0x2E9 + LOG_1X_BASE_C)


/* GPS search processed peak results and their associated search parameters */
#define LOG_GPS_PROCESSED_PEAK_C                        (0x352 + LOG_1X_BASE_C)


/* Internal - GPS PE Position Report Part 3 */
#define LOG_GPS_PE_POSITION_REPORT_PART3_C              (0x359 + LOG_1X_BASE_C)


/* GPS Spectral Information */
#define LOG_GPS_SPECTRAL_INFO_C                         (0x36D + LOG_1X_BASE_C)



/* GPS Prescribed Dwell Result */
#define LOG_GPS_PRESCRIBED_DWELL_RESULT_C               (0x374 + LOG_1X_BASE_C)



/* GPS DCME Srch Job Completed */
#define LOG_GPS_DCME_SRCH_JOB_COMPLETED_C               (0x3CF + LOG_1X_BASE_C)







```



## LOG_GNSS

```


/* GNSS Position Report */
#define LOG_GNSS_POSITION_REPORT_C                      (0x476 + LOG_1X_BASE_C)

/* GNSS GPS Measurement Report */
#define LOG_GNSS_GPS_MEASUREMENT_REPORT_C               (0x477 + LOG_1X_BASE_C)

/* GNSS Clock Report */
#define LOG_GNSS_CLOCK_REPORT_C                         (0x478 + LOG_1X_BASE_C)

/* GNSS Demod Soft Decision */
#define LOG_GNSS_DEMOD_SOFT_DECISIONS_C                 (0x479 + LOG_1X_BASE_C)

/* GNSS ME 5MS IQ sum */
#define LOG_GNSS_ME_5MS_IQ_SUMS_C                       (0x47A + LOG_1X_BASE_C)

/* GNSS CD DB report */
#define LOG_GNSS_CD_DB_REPORT_C                         (0x47B + LOG_1X_BASE_C)

/* GNSS PE WLS position report */
#define LOG_GNSS_PE_WLS_POSITION_REPORT_C               (0x47C + LOG_1X_BASE_C)

/* GNSS PE KF position report */
#define LOG_GNSS_PE_KF_POSITION_REPORT_C                (0x47D + LOG_1X_BASE_C)

/* GNSS PRX RF HW status report */
#define LOG_GNSS_PRX_RF_HW_STATUS_REPORT_C              (0x47E + LOG_1X_BASE_C)

/* GNSS DRX RF HW status report */
#define LOG_GNSS_DRX_RF_HW_STATUS_REPORT_C              (0x47F + LOG_1X_BASE_C)

/* GNSS Glonass Measurement report */
#define LOG_GNSS_GLONASS_MEASUREMENT_REPORT_C           (0x480 + LOG_1X_BASE_C)

/* GNSS GPS HBW RXD measurement */
#define LOG_GNSS_GPS_HBW_RXD_MEASUREMENT_C              (0x481 + LOG_1X_BASE_C)

/* GNSS PDSM position report callback */
#define LOG_GNSS_PDSM_POSITION_REPORT_CALLBACK_C        (0x482 + LOG_1X_BASE_C)

/* ISense Request String  */
#define LOG_ISENSE_REQUEST_STR_C                        (0x483 + LOG_1X_BASE_C)

/* ISense Response String */
#define LOG_ISENSE_RESPONSE_STR_C                       (0x484 + LOG_1X_BASE_C)

/* Bluetooth SOC General Log Packet*/
#define LOG_BT_SOC_GENERAL_C                            (0x485 + LOG_1X_BASE_C)

/* QCRil Call Flow */
#define LOG_QCRIL_CALL_FLOW_C                           (0x486 + LOG_1X_BASE_C)

/* CGPS Wideband FFT stats */
#define LOG_CGPS_WB_FFT_STATS_C                         (0x487 + LOG_1X_BASE_C)

/* CGPS Slow Clock Calibration Report*/
#define LOG_CGPS_SLOW_CLOCK_CALIB_REPORT_C              (0x488 + LOG_1X_BASE_C)

/* SNS GPS TIMESTAMP */
#define LOG_SNS_GPS_TIMESTAMP_C                         (0x489 + LOG_1X_BASE_C)

/* GNSS Search Strategy Task Allocation */
#define LOG_GNSS_SEARCH_STRATEGY_TASK_ALLOCATION_C      (0x48A + LOG_1X_BASE_C)

/* RF MC STM state */
#define LOG_1XHDR_MC_STATE_C                            (0x48B + LOG_1X_BASE_C)

/* Record in the Sparse Network DB */
#define LOG_CGPS_SNDB_RECORD_C                          (0x48C + LOG_1X_BASE_C)

/* Record removed from the DB */
#define LOG_CGPS_SNDB_REMOVE_C                          (0x48D + LOG_1X_BASE_C)

/* CGPS Reserved */
#define LOG_GNSS_CC_PERFORMANCE_STATS_C                 (0x48E + LOG_1X_BASE_C)

/* GNSS PDSM Set Paramerters */
#define LOG_GNSS_PDSM_SET_PARAMETERS_C                  (0x48F + LOG_1X_BASE_C)

/* GNSS PDSM PD Event Callback */
#define LOG_GNSS_PDSM_PD_EVENT_CALLBACK_C               (0x490 + LOG_1X_BASE_C)

/* GNSS PDSM PA Event Callback */
#define LOG_GNSS_PDSM_PA_EVENT_CALLBACK_C               (0x491 + LOG_1X_BASE_C)

/* CGPS Reserved */
#define LOG_CGPS_RESERVED2_C                            (0x492 + LOG_1X_BASE_C)

/* CGPS Reserved */
#define LOG_CGPS_RESERVED3_C                            (0x493 + LOG_1X_BASE_C)

/* GNSS PDSM EXT Status MEAS Report */
#define LOG_GNSS_PDSM_EXT_STATUS_MEAS_REPORT_C          (0x494 + LOG_1X_BASE_C)

/* GNSS SM Error */
#define LOG_GNSS_SM_ERROR_C                             (0x495 + LOG_1X_BASE_C)


#define LOG_GNSS_SBAS_REPORT_C                         ((0x4C7) + LOG_1X_BASE_C)


#define LOG_GNSS_OEMDRE_MEASUREMENT_REPORT_C           ((0x4DE) + LOG_1X_BASE_C)

#define LOG_GNSS_OEMDRE_POSITION_REPORT_C              ((0x4E0) + LOG_1X_BASE_C)

#define LOG_GNSS_OEMDRE_SVPOLY_REPORT_C                ((0x4E1) + LOG_1X_BASE_C)

#define LOG_GNSS_OEMDRSYNC_C                           ((0x4E2) + LOG_1X_BASE_C)



#define LOG_GNSS_PDSM_PPM_SESSION_BEGIN_C              ((0x4E5) + LOG_1X_BASE_C)

#define LOG_GNSS_PDSM_PPM_SESSION_PPM_SUSPEND_C        ((0x4E6) + LOG_1X_BASE_C)

#define LOG_GNSS_PDSM_PPM_REPORT_THROTTLED_C           ((0x4E7) + LOG_1X_BASE_C)

#define LOG_GNSS_PDSM_PPM_REPORT_FIRED_C               ((0x4E8) + LOG_1X_BASE_C)

#define LOG_GNSS_PDSM_PPM_SESSION_END_C                ((0x4E9) + LOG_1X_BASE_C)



#define LOG_GNSS_SENSOR_STREAMING_READY_STATUS         ((0x50D) + LOG_1X_BASE_C)

#define LOG_GNSS_TIME_SYNC_REQ                         ((0x50E) + LOG_1X_BASE_C)

#define LOG_GNSS_INJECT_TIME_SYNC_DATA                 ((0x50F) + LOG_1X_BASE_C)

#define LOG_GNSS_INJECT_SENSOR_DATA                    ((0x510) + LOG_1X_BASE_C)

#define LOG_GNSS_GET_SENSOR_CONFIG_RESPONSE            ((0x511) + LOG_1X_BASE_C)

#define LOG_GNSS_SET_SENSOR_CONFIG                     ((0x512) + LOG_1X_BASE_C)

#define LOG_GNSS_PE_HEADING_FILTER                     ((0x513) + LOG_1X_BASE_C)

#define LOG_GNSS_PE_NHC                                ((0x514) + LOG_1X_BASE_C)

#define LOG_GNSS_PE_CRD                                ((0x515) + LOG_1X_BASE_C)

/* The below two logs are the same. This is done so we don't break any build that
   is using the older name  (LOG_GNSS_PE_RESERVED) */
#define LOG_GNSS_PE_RESERVED                           ((0x516) + LOG_1X_BASE_C)

#define LOG_GNSS_CONFIGURATION_STATE_C                 ((0x516) + LOG_1X_BASE_C)


#define LOG_GNSS_YAW_GYRO_CALIBRATION                  ((0x519) + LOG_1X_BASE_C)


#define LOG_GNSS_BP_AMP_INFO_C                         ((0x520) + LOG_1X_BASE_C)


#define LOG_GNSS_BROADBAND_JAMMER_INFO_C       ((0x526) + LOG_1X_BASE_C)


#define LOG_GNSS_QUIPS_POSITION_REPORT_C ((0x55D) + LOG_1X_BASE_C)
#define LOG_GNSS_QUIPS_AP_MEAS_BLOCKS_C  ((0x55E) + LOG_1X_BASE_C)
#define LOG_GNSS_FAST_TCAL_C        ((0x567) + LOG_1X_BASE_C)
#define LOG_GNSS_QUIPS_IN_USE_AP_DATABASE_C     ((0x56F) + LOG_1X_BASE_C)
#define LOG_GNSS_PE_EVENTS_C                    ((0x587) + LOG_1X_BASE_C)
#define LOG_GNSS_ISAGNAV_SDP_EVENTS_C           ((0x589) + LOG_1X_BASE_C)



/* Aliasing instead of replacing to keep backwards compatibility */
#define LOG_GNSS_SAP_SDP_EVENTS_C               (LOG_GNSS_ISAGNAV_SDP_EVENTS_C)


#define LOG_GNSS_QUIPS_AP_MEAS_BLOCKS_C  ((0x55E) + LOG_1X_BASE_C)


#define LOG_GNSS_FAST_TCAL_C        ((0x567) + LOG_1X_BASE_C)

#define LOG_GNSS_QUIPS_IN_USE_AP_DATABASE_C     ((0x56F) + LOG_1X_BASE_C)
#define LOG_GNSS_PE_EVENTS_C                    ((0x587) + LOG_1X_BASE_C)
#define LOG_GNSS_ISAGNAV_SDP_EVENTS_C           ((0x589) + LOG_1X_BASE_C)
/* Aliasing instead of replacing to keep backwards compatibility */


#define LOG_GNSS_SAP_SDP_EVENTS_C               (LOG_GNSS_ISAGNAV_SDP_EVENTS_C)



#define LOG_GNSS_WWAN_WLAN_IMD_JAMMER_STATUS_C  ((0x596) + LOG_1X_BASE_C)
#define LOG_GNSS_PDSM_BEST_AVAIL_POS_INFO_C             ((0x5B1) + LOG_1X_BASE_C)
#define LOG_GNSS_PDSM_EXT_STATUS_BEST_AVAIL_POS_INFO_C  ((0x5B2) + LOG_1X_BASE_C)
#define LOG_GNSS_SPECTRUM_ANALYZER_SCAN_PARAMS_C           ((0x634) + LOG_1X_BASE_C)
#define LOG_GNSS_SPECTRUM_ANALYZER_JAMMER_MEASUREMENTS_C   ((0x635) + LOG_1X_BASE_C)
#define LOG_GNSS_SPECTRUM_ANALYZER_NOTCH_ASSIGNMENT_C      ((0x636) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_PQME1_C                                ((0x63D) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_PQME2_C                                ((0x63E) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_PQME3_C                                ((0x63F) + LOG_1X_BASE_C)
#define LOG_GNSS_BDS_MEASUREMENT_REPORT_C                  ((0x756) + LOG_1X_BASE_C)
#define LOG_GNSS_MBM_C                                     ((0x806) + LOG_1X_BASE_C)
#define LOG_GNSS_PLE_UPDATE_C                              ((0x807) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_MOTION_DETECTION_REPORT_C         ((0x833) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_POSITION_REPORT_C                 ((0x837) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_DPO_STATUS_C                            ((0x838) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_TIME_SYNC_C                           ((0x839) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_WWAN_MOT_CLASS_C                  ((0x883) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_EVENT_C                               ((0x885) + LOG_1X_BASE_C)
#define LOG_GNSS_GAL_MEASUREMENT_REPORT_C                   ((0x886) + LOG_1X_BASE_C)
#define LOG_GNSS_OEMCPE_MEASUREMENT_REPORT_C                ((0x88E) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_MOTION_WIFI_STATE_C               ((0x899) + LOG_1X_BASE_C)
#define LOG_GNSS_GTP_TDP_CONFIG_C                           ((0x8A3) + LOG_1X_BASE_C)
#define LOG_GNSS_TLM_TDP_SCANLIST_C                         ((0x8A4) + LOG_1X_BASE_C)
#define LOG_GNSS_GTS_INPUT_REPORT_C                         ((0x8AB) + LOG_1X_BASE_C)
#define LOG_GNSS_GTS_TIME_UPDATE_C                          ((0x8AC) + LOG_1X_BASE_C)
#define LOG_GNSS_GTS_DB_DUMP_C                              ((0x8AD) + LOG_1X_BASE_C)
#define LOG_GNSS_GTS_EVENTS_C                               ((0x8AE) + LOG_1X_BASE_C)
#define LOG_GNSS_DATA_BLOB_C                                ((0x8AF) + LOG_1X_BASE_C)
#define LOG_GNSS_CD_LPPM_STATE_TRANSITION_C                 ((0x8B0) + LOG_1X_BASE_C)
#define LOG_GNSS_CD_LPPM_ENGAGE_REPORT_C                    ((0x8B1) + LOG_1X_BASE_C)
#define LOG_GNSS_MC_CONFIG_C                                ((0x8B2) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_ADD_REPORT_C                      ((0x8B3) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_DELETE_REPORT_C                   ((0x8B4) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_PURGE_REPORT_C                    ((0x8B5) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_BREACH_REPORT_C                   ((0x8B6) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_LPPM_STATUS_C                           ((0x8BF) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_PQME5_C                                 ((0x8C5) + LOG_1X_BASE_C)
#define LOG_GNSS_LOCMW_ALS_IOD_DATA_C                       ((0x8C8) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_POSITION_REQUEST_C                ((0x8DF) + LOG_1X_BASE_C)
#define LOG_GNSS_GF_ALS_IOD_SENSOR_STATE_C                  ((0x8EB) + LOG_1X_BASE_C)
#define LOG_GNSS_COG_REFERENCE_C                            ((0x8EC) + LOG_1X_BASE_C)
#define LOG_GNSS_GIT_IPC_REPORT_C                           ((0x8ED) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_QMI_EVT_C                             ((0x8EE) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_QMI_MSG_C                             ((0x8EF) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_STATS_C                           ((0x8F0) + LOG_1X_BASE_C)
#define LOG_GNSS_GEOFENCE_EBEE_INFO_C                       ((0x8F4) + LOG_1X_BASE_C)
#define LOG_GNSS_QZSS_SBAS_MEASUREMENT_REPORT_C             ((0x8F5) + LOG_1X_BASE_C)
#define LOG_GNSS_WIFI_COV_LOC_EST_C                         ((0x8F6) + LOG_1X_BASE_C)
#define LOG_GNSS_UIMAGE_TRANSITION_INFO_C                   ((0x8F9) + LOG_1X_BASE_C)
#define LOG_GNSS_APDR_DIAG_MSG_C                            ((0x8FA) + LOG_1X_BASE_C)
#define LOG_GNSS_DGNSS_MESSAGES_C                           ((0x902) + LOG_1X_BASE_C)
#define LOG_GNSS_WLM_SCAN_INFO_C                            ((0x916) + LOG_1X_BASE_C)
#define LOG_GNSS_INJECTED_XTRA_DATA_C                       ((0x91F) + LOG_1X_BASE_C)
#define LOG_GNSS_TLE_TDP_MEAS_BLK_C                         ((0x92E) + LOG_1X_BASE_C)
#define LOG_GNSS_MC_F3_GENERIC_C                            ((0x950) + LOG_1X_BASE_C)
#define LOG_GNSS_MC_MEM_ALLOC_STATUS_C                      ((0x953) + LOG_1X_BASE_C)
#define LOG_GNSS_SM_GERA_CORE_STATUS_C                      ((0x97E) + LOG_1X_BASE_C)
#define LOG_GNSS_TECH_SEL_RULE_ENG_DECISION_C               ((0x982) + LOG_1X_BASE_C)
#define LOG_GNSS_TECH_SEL_PED_CHANGE_IND_C                  ((0x983) + LOG_1X_BASE_C)
#define LOG_GNSS_TECH_SEL_GNSS_QUAL_CHANGE_IND_C            ((0x984) + LOG_1X_BASE_C)
#define LOG_GNSS_TECH_SEL_REL_DEV_POS_CHANGE_IND_C          ((0x985) + LOG_1X_BASE_C)
#define LOG_GNSS_TECH_SEL_GNSS_STRENGTH_CHANGE_IND_C        ((0x986) + LOG_1X_BASE_C)
#define LOG_GNSS_TECH_SEL_WIFI_STRENGTH_CHANGE_IND_C        ((0x987) + LOG_1X_BASE_C)
#define LOG_GNSS_PE_DETECTORS_C                             ((0x993) + LOG_1X_BASE_C)
#define LOG_GNSS_TECH_SEL_LPPM_REQ_C                        ((0x99D) + LOG_1X_BASE_C)
#define LOG_GNSS_EXT_LPPM_ENGAGE_REPORT_C                   ((0x9A9) + LOG_1X_BASE_C)
#define LOG_GNSS_BP_JAMMER_MEASUREMENT_C                    ((0x9BA) + LOG_1X_BASE_C)
#define LOG_GNSS_REF_OSC_UPDATE_C                           ((0x9BB) + LOG_1X_BASE_C)
#define LOG_GNSS_SHALLOW_KNOWN_SCAN_C                       ((0x9BC) + LOG_1X_BASE_C)
#define LOG_GNSS_FUSION_CSM_WIFI_CS_PUSH_GNSS_FIX_DATA_C    ((0x9BD) + LOG_1X_BASE_C)
#define LOG_GNSS_FUSION_CSM_WIFI_CS_PUSH_WIFI_SCAN_DATA_C   ((0x9BE) + LOG_1X_BASE_C)
#define LOG_GNSS_FUSION_CSM_WIFI_CS_PUSH_WWAN_CELL_DATA_C   ((0x9BF) + LOG_1X_BASE_C)
#define LOG_GNSS_FUSION_CSM_WIFI_CS_EVENT_C                 ((0x9C0) + LOG_1X_BASE_C)
#define LOG_GNSS_CLOCK_VOTING_C                             ((0x9C3) + LOG_1X_BASE_C)
#define LOG_GNSS_GLONASS_L2_MEASUREMENT_REPORT_C            ((0x9C4) + LOG_1X_BASE_C)
#define LOG_GNSS_GPS_L2_MEASUREMENT_REPORT_C                ((0x9CF) + LOG_1X_BASE_C)
#define LOG_GNSS_RGS_TABLE_LOG_C                            ((0x9D2) + LOG_1X_BASE_C)
#define LOG_GNSS_NBIQ_FREQUENCY_C	                        ((0x9D4) + LOG_1X_BASE_C)
#define LOG_GNSS_WLAN_CRITICAL_DEMOD_C                      ((0x9D5) + LOG_1X_BASE_C)
#define LOG_GNSS_SLE_UPDATE_C                               ((0x9DC) + LOG_1X_BASE_C)
#define LOG_GNSS_SLE_REL_DISP_C                             ((0x9DD) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_STATE_C                               ((0x9E4) + LOG_1X_BASE_C)
#define LOG_GNSS_SAML_INJECT_SENSOR_DATA_C                  ((0x9E5) + LOG_1X_BASE_C)
#define LOG_GNSS_GPS_L5_MEASUREMENT_REPORT_C                ((0x9EB) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_QMI_SNS_CLIENT_API_REQUEST_C          ((0x9F0) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_QMI_SNS_CLIENT_API_RESPONSE_C         ((0x9F1) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_QMI_SNS_CLIENT_API_INDICATION_C       ((0x9F2) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_SSC_SNSPB_SUID_EVENT_C                ((0x9F3) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_SSC_SNSPB_SENSOR_DATA_EVENT_C         ((0x9F4) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_SSC_SNSPB_AMD_RMD_EVENT_C             ((0x9F5) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_SSC_SNSPB_PEDO_EVENT_C                ((0x9F6) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_SSC_SNSPB_DB_EVENT_C                  ((0x9F7) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_NAV_E911_CONCURRENCY_STATUC_C           ((0x9F8) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_MULTIBAND_IQ_SUMS_C                     ((0x9F9) + LOG_1X_BASE_C)
#define LOG_GNSS_AFC_FREQ_LOG_C                             ((0x9FB) + LOG_1X_BASE_C)
#define LOG_GNSS_ABS_SIG_POWER_LOG_C                        ((0x9FD) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_CLOCK_VOTING_STATUS_C                     ((0xC4A) + LOG_1X_BASE_C)
#define LOG_GNSS_SM_EXPERIMENTAL_1_C                          ((0xC4B) + LOG_1X_BASE_C)
#define LOG_GNSS_SM_EXPERIMENTAL_2_C                          ((0xC4C) + LOG_1X_BASE_C)
#define LOG_GNSS_SM_EXPERIMENTAL_3_C                          ((0xC4D) + LOG_1X_BASE_C)
#define LOG_GNSS_SM_EXPERIMENTAL_4_C                          ((0xC4E) + LOG_1X_BASE_C)
#define LOG_GNSS_SM_EXPERIMENTAL_5_C                          ((0xC4F) + LOG_1X_BASE_C)
#define LOG_GNSS_TILT_MAG_CAL_EPOCH_STATS_C                   ((0xC51) + LOG_1X_BASE_C)
#define LOG_GNSS_TILT_MAG_CAL_LONG_TERM_STATS_C               ((0xC52) + LOG_1X_BASE_C)
#define LOG_GNSS_CONSTELLATION_CONTROL_C                      ((0xC53) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_ENGINE_MSG_C                              ((0xC54) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_ENGINE_VERSION_C                          ((0xC55) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_CONTROL_CMD_C                             ((0xC56) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_SUB_INFO_C                                ((0xC57) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_ERR_LOG_INFO_C                            ((0xC58) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_CONNECTION_STATUS_C                       ((0xC59) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_DEVICE_ID_C                               ((0xC5A) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_PVT_RPEPORT_C                             ((0xC5B) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_SV_POLY_REPORT_C                          ((0xC5C) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_SV_INFO_REPORT_C                          ((0xC5D) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_MEAS_REPORT_C                             ((0xC5E) + LOG_1X_BASE_C)
#define LOG_GNSS_BDS_B2_MEASUREMENT_REPORT_C                  ((0xC5F) + LOG_1X_BASE_C)
#define LOG_GNSS_GAL_E5A_MEASUREMENT_REPORT_C                 ((0xC60) + LOG_1X_BASE_C)
#define LOG_GNSS_DPO_IQ_SUMS_C                                ((0xC62) + LOG_1X_BASE_C)
#define	LOG_GNSS_PE_GMS_DATA_REPORT_C                         ((0xC65) + LOG_1X_BASE_C)
#define	LOG_GNSS_ISTB_UPDATE_REPORT_C                         ((0xC66) + LOG_1X_BASE_C)
#define LOG_GNSS_CC_CCP_STATUS_C                              ((0xC67) + LOG_1X_BASE_C)
#define LOG_GNSS_CC_DCP_STATUS_C                              ((0xC68) + LOG_1X_BASE_C)
#define LOG_GNSS_IPM_ALGO_STATE_C                             ((0xC69) + LOG_1X_BASE_C)
#define LOG_GNSS_EH_AGGREGATOR_STATE_AND_OUTPUT_C             ((0xC6A) + LOG_1X_BASE_C)
#define LOG_GNSS_QDR3_SNAPSHOT_C                              ((0xC78) + LOG_1X_BASE_C)
#define LOG_GNSS_QDR2_SNAPSHOT_C                              ((0xC79) + LOG_1X_BASE_C)
#define LOG_GNSS_DR_QDR_CORE_C                                ((0xC7A) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_EPH_REPORT_C                              ((0xC7B) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_PQME_C                                    ((0xC7C) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_RTX_CORR_DATA_C                           ((0xC86) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_IONO_DATA                                 ((0xC8B) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_GLO_ADDITIONAL_DATA                       ((0xC8C) + LOG_1X_BASE_C)
#define LOG_GNSS_CLIENT_API_LOCATION_REPORT_C                 ((0xC8F) + LOG_1X_BASE_C)
#define LOG_GNSS_CLIENT_API_SV_REPORT_C                       ((0xC90) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_NAVIC_VITERBI_DATA_C                      ((0xC91) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_NAVIC_PREAMBLE_SEARCH_DATA_C              ((0xC92) + LOG_1X_BASE_C)
#define LOG_GNSS_NAVIC_MEASUREMENT_REPORT_C                  ((0xC93) + LOG_1X_BASE_C)
#define LOG_GNSS_CC_DCM_LOAD                                  ((0xC95) + LOG_1X_BASE_C)
#define LOG_GNSS_QZSS_L1S_MEASUREMENT_REPORT_C                ((0xC97) + LOG_1X_BASE_C)
#define LOG_GNSS_SIRF_STAR_OSP_PKT_C                          ((0xC99) + LOG_1X_BASE_C)
#define LOG_GNSS_SLL_DIAG_PKT_C                               ((0xC9A) + LOG_1X_BASE_C)
#define LOG_GNSS_CLIENT_API_NMEA_REPORT_C                     ((0xCB2) + LOG_1X_BASE_C)
#define LOG_GNSS_GPS_L1_MEASUREMENT_REPORT_C                  ((0xCB5) + LOG_1X_BASE_C)
#define LOG_GNSS_PE_DGNSS_DATA_REPORT_C                       ((0xCB8) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_REPORT_STATUS_RESP_C                      ((0xCBC) + LOG_1X_BASE_C)
#define LOG_GNSS_GPM_VOTE_STATS_REPORT_C                      ((0xCBD) + LOG_1X_BASE_C)
#define LOG_GNSS_QSOCKET_MESSAGE_HEADER_C                     ((0xCBE) + LOG_1X_BASE_C)
#define LOG_GNSS_QSOCKET_MESSAGE_HEADER_AND_PAYLOAD_C         ((0xCBF) + LOG_1X_BASE_C)
#define LOG_GNSS_QSOCKET_MSG_HEADER_ONLY_C                    ((0xCC1) + LOG_1X_BASE_C)
#define LOG_GNSS_QSOCKET_MSG_HEADER_AND_BODY_C                ((0xCC2) + LOG_1X_BASE_C)
#define LOG_GNSS_SLIM_SENSOR_DATA_C                           ((0xCC3) + LOG_1X_BASE_C)
#define LOG_GNSS_BDS_B1C_MEASUREMENT_REPORT_C                 ((0xCC6) + LOG_1X_BASE_C)
#define LOG_GNSS_CLIENT_API_SV_POLY_REPORT_C                  ((0xCC7) + LOG_1X_BASE_C)
#define LOG_GNSS_GPS_QZSS_SBAS_L1_CX4_MEASUREMENT_REPORT_C    ((0xCC9) + LOG_1X_BASE_C)
#define LOG_GNSS_GPS_QZSS_SBAS_L1_CX2_MEASUREMENT_REPORT_C    ((0xCCB) + LOG_1X_BASE_C)
#define LOG_GNSS_GPS_L1_CX2_MEASUREMENT_REPORT_C              ((0xCCC) + LOG_1X_BASE_C)
#define LOG_GNSS_VPE_SENSOR_DATA_C                            ((0xCCD) + LOG_1X_BASE_C)
#define LOG_GNSS_VPE_MSR_DATA_C                               ((0xCCE) + LOG_1X_BASE_C)
#define LOG_GNSS_VPE_TIME_SYNC_DATA_C                         ((0xCCF) + LOG_1X_BASE_C)
#define LOG_GNSS_GILE_QDR_AID_REPORT_C                        ((0xCD4) + LOG_1X_BASE_C)
#define LOG_GNSS_PE_AUX_MEAS_SELECT_INFO_C                    ((0xCD5) + LOG_1X_BASE_C)
#define LOG_GNSS_PE_ENV_BEARING_VALIDATION_INFO_C             ((0xCD6) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_FACTORY_INFO                              ((0xCD7) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_QWES_DATA_INFO                            ((0xCDA) + LOG_1X_BASE_C)
#define LOG_GNSS_EP_SAP_INS_PARAMS_REPORT_C                   ((0xCDB) + LOG_1X_BASE_C)
#define LOG_GNSS_GAL_E5B_MEASUREMENT_REPORT_C                 ((0xCE0) + LOG_1X_BASE_C)
#define LOG_GNSS_GPS_QZSS_SBAS_L5_MEASUREMENT_REPORT_C        ((0xCE1) + LOG_1X_BASE_C)
#define LOG_GNSS_LATENCY_REPORT_C                             ((0xCE8) + LOG_1X_BASE_C)
#define LOG_GNSS_QZSS_SBAS_L2C_MEASUREMENT_REPORT_C           ((0xCE9) + LOG_1X_BASE_C)
#define LOG_GNSS_APC_OFFSET_C                                 ((0xD05) + LOG_1X_BASE_C)
#define LOG_GNSS_VPE_FAILURE_C                                ((0xD07) + LOG_1X_BASE_C)
#define LOG_GNSS_MEASUREMENT_REPORT_COUNT_C                   ((0xD1F) + LOG_1X_BASE_C)
#define LOG_GNSS_CP_STATIC_ODDS_C                             ((0xD20) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_MODES_C                                   ((0xD21) + LOG_1X_BASE_C)
#define LOG_GNSS_POWER_STATUS_REPORT_C                        ((0xD22) + LOG_1X_BASE_C)
#define LOG_GNSS_POWER_PROFILING_REPORT_C                     ((0xD23) + LOG_1X_BASE_C)
#define LOG_GNSS_MC_JAMMER_STATUS_C                           ((0xD24) + LOG_1X_BASE_C)
#define LOG_GNSS_CLIENT_API_CMD_C                             ((0xD25) + LOG_1X_BASE_C)
#define LOG_GNSS_ME_XIMEM_Status_C                            ((0xD2A) + LOG_1X_BASE_C)
#define LOG_GNSS_IZAT_FLP_REPORT_C                            ((0xD4C) + LOG_1X_BASE_C)
#define LOG_GNSS_TUNNEL_STATUS_C                              ((0xD5E) + LOG_1X_BASE_C)
#define LOG_GNSS_LOC_NR5G_RTT_ASSISTANCE_DATA_C               ((0xDA8) + LOG_1X_BASE_C)
#define LOG_GNSS_LOC_NR5G_RTT_PRS_OCCASION_MEASUREMENTS_C     ((0xDA9) + LOG_1X_BASE_C)
#define LOG_GNSS_LOC_NR5G_RTT_PRS_TOA_MEASUREMENTS_C          ((0xDAA) + LOG_1X_BASE_C)
#define LOG_GNSS_LOC_NR5G_RTT_PRS_RSTD_MEASUREMENTS_C         ((0xDAB) + LOG_1X_BASE_C)





```





### LOG [ GNSS PDSM PD Event Callback ]

```

16:34:51.559474	[0x1490]	GNSS PDSM PD Event Callback
Version = 0
q_ClientId = 3491
u_EventType = SESSION_BEGIN
u_EventData = 0
u_Reserved = { 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0 }





```

### LOG [ GNSS PDSM Position Report Callback  ]

```

16:44:04.896859	[0x1482]	GNSS PDSM Position Report Callback
Version = 0
q_ClientId = 3491
lat {
   latitude = 41.3940382
}
long {
longitude = -86.61594272
}
q_TimeStamp = 1414082663
u_LocUncAng = 8
u_LocUncA = 5
u_LocUncP = 5
u_OptFieldMask = PDSM_PD_ALTITUDE_VALID | PDSM_PD_VELOCITY_VALID | PDSM_PD_UTCOFFSET_VALID | PDSM_PD_UNCERTAINTY_VALID | PDSM_PD_EXT_LOC_VALID | 0x20
x_Altitude = 177 m
w_Heading = 766
w_VelocityHor = 136
u_FixType = Other
s_VelocityVer = 0
u_LocUncertaintyV = 5
u_PositionMethod = Standalone
u_PosSource = GPS | New | GLO
u_EndSessionCause = 1
u_UncEllConfidence = 39
s_utcOffset = 18
q_ExtLoclat = 987700162
q_ExtLoclon = -1033368466
w_ExtLocUncA = 2 m
w_ExtLocUncP = 2 m
q_VelHoriz = 3406 cm/s
q_VelVert = 22 cm/s
q_TimeStampTowMs = 60263000
w_TimeStampGpsWeek = 2338
u_PosReportedToNetwork = 0
w_VelUncHoriz = 7
w_VelUncVert = 7
q_Hdop = 5
q_Pdop = 8
q_Vdop = 6
u_UtcHour = 5
u_UtcMin = 0
w_UtcSec = 0
q_UtcDate = 9
q_LocUncHorizontal = 32779280 m
u_TimeMask = BDS (0x8) | 0x40
q_GloOffset = 50437515 ms
q_Reserved = { 117440512, 0 }




```


### LOG [ GNSS GPS Measurement Report ]
```

16:44:04.830089	[0x1477]	GNSS GPS Measurement Report
Version = 0
V0 {
   F Count = 54807286
   GPS Week Number = 2338
   GPS Milliseconds = 60262429
   Time Bias = 0.018348
   ClkTimeUncMs = 2.16663e-005
   Clock Frequency Bias = -2.005613
   Clock Frequency Uncertainty = 2.83922
   Number Of SVs = 9
   SV[0] {
      SV =   2
      ObsState =      TRK
      Tot = 190
      Gd = 190
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 32.2
      Latency = -20
      Pre = 20
      Post = 47
      Ms =  60262349
      SubMs = 0.46073633
      TUnc = 0.000102
      Speed = -158.14
      SpdUnc = 0.3
      MeasStat {
         MeasStat = 0x082400FF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  301.9
      ElvDeg =   24.2
      CarrPhase =           0.000000
      FnSpd = -158.030
      FnSpdU = 0.0375
      CSlip = 42
   }
   SV[1] {
      SV =  10
      ObsState =      TRK
      Tot = 181
      Gd = 181
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 34.2
      Latency = -30
      Pre = 20
      Post = 47
      Ms =  60262359
      SubMs = 0.53514260
      TUnc = 0.000077
      Speed =  319.36
      SpdUnc = 0.3
      MeasStat {
         MeasStat = 0x0824007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  137.0
      ElvDeg =   60.6
      CarrPhase =           0.000000
      FnSpd =  319.485
      FnSpdU = 0.0509
      CSlip = 248
   }
   SV[2] {
      SV =  12
      ObsState =      TRK
      Tot = 173
      Gd = 173
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 34.0
      Latency = -19
      Pre = 20
      Post = 47
      Ms =  60262350
      SubMs = 0.34758475
      TUnc = 0.000077
      Speed =  120.58
      SpdUnc = 0.3
      MeasStat {
         MeasStat = 0x0824007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =   58.6
      ElvDeg =   21.0
      CarrPhase =           0.000000
      FnSpd =  120.555
      FnSpdU = 0.0344
      CSlip = 216
   }
   SV[3] {
      SV =  21
      ObsState =      TRK
      Tot = 40
      Gd =  40
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 28.4
      Latency = -18
      Pre = 20
      Post = 47
      Ms =  60262351
      SubMs = 0.06402376
      TUnc = 0.000156
      Speed =   75.22
      SpdUnc = 0.4
      MeasStat {
         MeasStat = 0x0824007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  282.2
      ElvDeg =   31.0
      CarrPhase =           0.000000
      FnSpd =   75.028
      FnSpdU = 0.0428
      CSlip = 2
   }
   SV[4] {
      SV =  23
      ObsState =      TRK
      Tot = 108
      Gd = 108
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 33.1
      Latency = -19
      Pre = 20
      Post = 47
      Ms =  60262350
      SubMs = 0.23050952
      TUnc = 0.000088
      Speed =  678.58
      SpdUnc = 0.3
      MeasStat {
         MeasStat = 0x082400BF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  136.6
      ElvDeg =   18.9
      CarrPhase =           0.000000
      FnSpd =  678.846
      FnSpdU = 0.0352
      CSlip = 46
   }
   SV[5] {
      SV =  25
      ObsState =      TRK
      Tot = 69
      Gd =  69
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 29.9
      Latency = -14
      Pre = 20
      Post = 47
      Ms =  60262355
      SubMs = 0.28551623
      TUnc = 0.000136
      Speed = -266.27
      SpdUnc = 0.4
      MeasStat {
         MeasStat = 0x0824007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  101.3
      ElvDeg =   33.2
      CarrPhase =           0.000000
      FnSpd = -266.289
      FnSpdU = 0.0354
      CSlip = 102
   }
   SV[6] {
      SV =  28
      ObsState =      TRK
      Tot = 181
      Gd = 181
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 30.0
      Latency = -10
      Pre = 20
      Post = 47
      Ms =  60262359
      SubMs = 0.55989397
      TUnc = 0.000136
      Speed = -327.36
      SpdUnc = 0.4
      MeasStat {
         MeasStat = 0x0824007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  210.9
      ElvDeg =   63.3
      CarrPhase =           0.000000
      FnSpd = -327.275
      FnSpdU = 0.0245
      CSlip = 85
   }
   SV[7] {
      SV =  31
      ObsState =      TRK
      Tot = 180
      Gd = 180
      PrtyEM = 0x0000
      FiltN = 1
      CNo = 37.5
      Latency = -17
      Pre = 20
      Post = 47
      Ms =  60262352
      SubMs = 0.93225902
      TUnc = 0.000044
      Speed = -687.26
      SpdUnc = 0.2
      MeasStat {
         MeasStat = 0x082400FF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  216.5
      ElvDeg =   27.6
      CarrPhase =           0.000000
      FnSpd = -687.258
      FnSpdU = 0.0297
      CSlip = 35
   }
   SV[8] {
      SV =  32
      ObsState =      TRK
      Tot = 167
      Gd = 167
      PrtyEM = 0x0001
      FiltN = 1
      CNo = 30.2
      Latency = -30
      Pre = 20
      Post = 47
      Ms =  60262359
      SubMs = 0.55589926
      TUnc = 0.000136
      Speed =   47.67
      SpdUnc = 0.4
      MeasStat {
         MeasStat = 0x082400BF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =   16.7
      ElvDeg =   71.0
      CarrPhase =           0.000000
      FnSpd =   47.909
      FnSpdU = 0.0545
      CSlip = 142
   }
}


```



### LOG [ GNSS Glonass Measurement Report ]

```
16:44:04.830218	[0x1480]	GNSS Glonass Measurement Report
Version = 0
V0 {
   F Count = 54807286
   GLONASS Cycle Number 4 Years = 8
   GLONASS Number Of Days In 4 Year = 301
   GLONASS Milliseconds = 71044429
   Time Bias = 0.022154
   ClkTimeUncMs = 2.28654e-005
   Clock Frequency Bias = -2.005613
   Clock Frequency Uncertainty = 2.83922
   Number Of SVs = 9
   SV[0] {
      SV =  66
      FrqIdx = -4
      ObsState =         TRK
      Tot = 181
      Gd = 181
      HammEM = 0x00
      FiltN = 1
      CNo = 28.9
      Latency = -27
      Pre = 20
      Post = 47
      Ms =  71044362
      SubMs = 0.51569736
      TUnc = 0.000156
      Speed =  104.39
      SpdUnc = 0.4
      MeasStat {
         MeasStat = 0x082700FF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =   44.4
      ElvDeg =   57.9
      CarrPhase =           0.000000
      FnSpd =  104.430
      FnSpdU = 0.0561
      CSlip = 138
   }
   SV[1] {
      SV =  82
      FrqIdx = -3
      ObsState =         TRK
      Tot = 167
      Gd = 167
      HammEM = 0x00
      FiltN = 1
      CNo = 27.4
      Latency = -14
      Pre = 20
      Post = 47
      Ms =  71044355
      SubMs = 0.55050749
      TUnc = 0.000180
      Speed =  354.87
      SpdUnc = 0.5
      MeasStat {
         MeasStat = 0x082700FF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  274.2
      ElvDeg =   26.7
      CarrPhase =           0.000000
      FnSpd =  355.231
      FnSpdU = 0.6083
      CSlip = 55
   }
   SV[2] {
      SV =  77
      FrqIdx = -2
      ObsState =         TRK
      Tot = 95
      Gd =  94
      HammEM = 0x00
      FiltN = 1
      CNo = 26.9
      Latency = -10
      Pre = 20
      Post = 47
      Ms =  71044359
      SubMs = 0.21609499
      TUnc = 0.000208
      Speed = -651.41
      SpdUnc = 0.5
      MeasStat {
         MeasStat = 0x0827007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  158.6
      ElvDeg =   39.6
      CarrPhase =           0.000000
      FnSpd = -651.716
      FnSpdU = 0.0584
      CSlip = 65
   }
   SV[3] {
      SV =  76
      FrqIdx = -1
      ObsState =         TRK
      Tot = 181
      Gd = 181
      HammEM = 0x00
      FiltN = 1
      CNo = 32.9
      Latency = -27
      Pre = 20
      Post = 47
      Ms =  71044362
      SubMs = 0.24932428
      TUnc = 0.000088
      Speed =   -4.33
      SpdUnc = 0.3
      MeasStat {
         MeasStat = 0x082700FF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =   84.7
      ElvDeg =   55.4
      CarrPhase =           0.000000
      FnSpd =   -4.182
      FnSpdU = 0.0479
      CSlip = 191
   }
   SV[4] {
      SV =  75
      FrqIdx = 0
      ObsState =         TRK
      Tot = 169
      Gd = 169
      HammEM = 0x00
      FiltN = 1
      CNo = 36.1
      Latency = -18
      Pre = 20
      Post = 47
      Ms =  71044351
      SubMs = 0.59172118
      TUnc = 0.000050
      Speed =  697.68
      SpdUnc = 0.2
      MeasStat {
         MeasStat = 0x0827007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =   27.6
      ElvDeg =   14.9
      CarrPhase =           0.000000
      FnSpd =  697.651
      FnSpdU = 0.0316
      CSlip = 38
   }
   SV[5] {
      SV =  65
      FrqIdx = 1
      ObsState =         TRK
      Tot = 108
      Gd = 108
      HammEM = 0x01
      FiltN = 1
      CNo = 18.3
      Latency = -16
      Pre = 20
      Post = 47
      Ms =  71044353
      SubMs = 0.53092366
      TUnc = 0.000647
      Speed =  540.43
      SpdUnc = 1.0
      MeasStat {
         MeasStat = 0x0823001F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =   93.7
      ElvDeg =   20.2
      CarrPhase =           0.000000
      FnSpd =    0.000
      FnSpdU = 0.0000
      CSlip = 239
   }
   SV[6] {
      SV =  83
      FrqIdx = 3
      ObsState =         TRK
      Tot = 8
      Gd =   8
      HammEM = 0x01
      FiltN = 1
      CNo = 21.6
      Latency = -16
      Pre = 20
      Post = 47
      Ms =  71044353
      SubMs = 0.43387124
      TUnc = 0.000423
      Speed = -190.62
      SpdUnc = 0.8
      MeasStat {
         MeasStat = 0x0823001F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  319.0
      ElvDeg =   20.8
      CarrPhase =           0.000000
      FnSpd =    0.000
      FnSpdU = 0.0000
      CSlip = 96
   }
   SV[7] {
      SV =  81
      FrqIdx = 4
      ObsState =         TRK
      Tot = 170
      Gd = 170
      HammEM = 0x00
      FiltN = 1
      CNo = 35.3
      Latency = -19
      Pre = 20
      Post = 47
      Ms =  71044350
      SubMs = 0.71181178
      TUnc = 0.000058
      Speed =  718.84
      SpdUnc = 0.3
      MeasStat {
         MeasStat = 0x0827007F
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  227.6
      ElvDeg =   11.3
      CarrPhase =           0.000000
      FnSpd =  718.670
      FnSpdU = 0.0337
      CSlip = 75
   }
   SV[8] {
      SV =  67
      FrqIdx = 5
      ObsState =         TRK
      Tot = 106
      Gd = 106
      HammEM = 0x00
      FiltN = 1
      CNo = 32.8
      Latency = -10
      Pre = 20
      Post = 47
      Ms =  71044359
      SubMs = 0.10273521
      TUnc = 0.000088
      Speed = -450.22
      SpdUnc = 0.3
      MeasStat {
         MeasStat = 0x082700FF
      }
      MiscStat {
         MpVal = 0
         DirVal = 1
      }
      MP =   0
      AziDeg =  316.4
      ElvDeg =   39.9
      CarrPhase =           0.000000
      FnSpd = -450.195
      FnSpdU = 0.0224
      CSlip = 157
   }
}

```


### LOG [ GNSS_Position_Report ]

```

16:44:47.532790	[0x1476]	GNSS Position Report
Version = 24
V24 {
   F Count = 0
   Position Source = Internal Database
   PosVelFlag_IntDB {
      Pos Vel Flag = 0
      Raw Value = 0x0000
   }
   PosVelFlag2_IntDB {
      Pos Vel Flag = 0
      Raw Value = 0x0000000000000000
   }
   PosVelFlag3_IntDB {
      Pos Vel Flag = 0
      Raw Value = 0x0000000000000000
   }
   u_FixMode = 0
   Failure Code = 0
   Fix Events {
      Fix Events = 0
      Raw Value = 0x0000
   }
   GPS Week Number = 65535
   GPS Milliseconds = 0
   GLONASS Cycle Number 4 Years = 255
   GLONASS Number Of Days In 4 Year = 65535
   GLONASS Milliseconds = 0
   Number Of Positions = 8
   Final Position Latitude = 0.7224637866
   Latitude = 41.39412582 degree
   Final Position Longitude = -1.5123760700
   Longitude = -86.65276585 degree
   Final Position Altitude = 0
   Heading = -999
   Heading Uncertainty In Radians = 1.8138
   East Velocity = 0
   North Velocity = 0
   Vertical Velocity = 0
   East Velocity Uncertainty = 14.4338
   North Velocity Uncertainty = 14.4338
   Vertical Velocity Uncertainty = 14.4338
   Clock Bias = 0
   Clock Bias Uncertainty = 70263.9
   Inter GNSS TB {
      GPS To GLONASS Time Bias = 0
      GPS To GLONASS Time Bias Uncertainty = 599585
      Filtered GPS To GLONASS Time Bias = 0
      Filtered GPS To GLONASS Time Bias Uncertainty = 599585
      GPS To Beidou Time Bias (m) = 0
      GPS To Beidou Time Bias Unc (m) = 599585
      Filtered GPS To Beidou Time Bias (m) = 0
      Filtered GPS To Beidou Time Bias Unc (m) = 599585
      Beidou To Glonass Time Bias (m) = 0
      Beidou To Glonass Time Bias Unc (m) = 599585
      Filtered Beidou To Glonass Time Bias (m) = 0
      Filtered Beidou To Glonass Time Bias Unc (m) = 599585
      GPS to Gal Time Bias (m) = 0
      GPS to Gal Time Bias Uncertainty (m) = 599585
      Filtered GPS to Gal Time Bias (m) = 0
      Filtered GPS to Gal Time Bias Unc(m) = 599585
      Gal to Glo Time Bias (m) = 0
      Gal to Glo Time Bias Unc (m) = 599585
      Filtered Gal to Glo Time Bias (m) = 0
      Filtered Gal to Glo Time Bias Unc (m) = 599585
      Gal to Bds Time Bias (m) = 0
      Gal to Bds Time Bias Unc (m) = 599585
      Filtered Gal to Bds Time Bias (m) = 0
      Filtered Gal to Bds Time Bias Unc(m) = 599585
      GPS To Navic Time Bias(m) = 0
      GPS To Navic Time Bias Uncertainty(m) = 599585
      Filtered GPS To Navic Time Bias(m) = 0
      Filtered GPS To Navic Time Bias Uncertainty(m) = 599585
      GLO To Navic Time Bias(m) = 0
      GLO To Navic Time Bias Uncertainty(m) = 599585
      Filtered GLO To Navic Time Bias(m) = 0
      Filtered GLO To Navic Time Bias Uncertainty(m) = 599585
      BDS To Navic Time Bias(m) = 0
      BDSTo Navic Time Bias Uncertainty(m) = 599585
      Filtered BDS To Navic Time Bias(m) = 0
      Filtered BDS To Navic Time Bias Uncertainty(m) = 599585
      GAL To Navic Time Bias(m) = 0
      GAL To Navic Time Bias Uncertainty(m) = 599585
      Filtered GAL Navic IRNSS Time Bias(m) = 0
      Filtered GAL To Navic Time Bias Uncertainty(m) = 599585
      GPS L5 To GLO LX Time Bias(m) = 0
      GPS L5 To GLO LX Time Bias Uncertainty(m) = 599585
      Filtered  GPS L5 To GLO LX Time Bias(m) = 0
      Filtered GPS L5 To GLO LX Time Bias  Uncertainty(m) = 599585
      GPS L5 To BDS B2A Time Bias(m) = 0
      GPS L5 To BDS B2A Time Bias Uncertainty(m) = 599585
      Filtered  GPS L5 To BDS B2A Time Bias(m) = 0
      Filtered GPS L5 To BDS B2A Time Bias  Uncertainty(m) = 599585
      BDS B2A To GLO LX Time Bias(m) = 0
      BDS B2A To GLO LX Time Bias Uncertainty(m) = 599585
      Filtered  BDS B2A To GLO LX Time Bias(m) = 0
      Filtered BDS B2A To GLO LX Time Bias  Uncertainty(m) = 599585
      GPS L5 To GAL E5A Time Bias(m) = 0
      GPS L5 To GAL E5A Time Bias Uncertainty(m) = 599585
      Filtered  GPS L5 To GAL E5A Time Bias(m = 0
      Filtered GPS L5 To GAL E5A Time Bias  Uncertainty(m) = 599585
      GAL E5A To  GLO LX Time Bias(m) = 0
      GAL E5A To  GLO LX Time Bias Uncertainty(m) = 599585
      Filtered GAL E5A To GLO LX Time Bias(m) = 0
      Filtered GAL E5A To GLO LX Time Bias  Uncertainty(m) = 599585
      GAL E5A To  BDS B2A Time Bias(m) = 0
      GAL E5A To  BDS B2A Time Bias Uncertainty(m) = 599585
      Filtered GAL E5A To  BDS B2A Time Bias(m) = 0
      Filtered GAL E5A To  BDS B2A Time Bias  Uncertainty(m) = 599585
      GPS L5 To NAVIC Time Bias(m) = 0
      GPS L5 To NAVIC  Time Bias Uncertainty(m) = 599585
      Filtered  GPS L5 To NAVIC  Time Bias(m) = 0
      Filtered GPS L5 To NAVIC  Time Bias  Uncertainty(m) = 599585
      GLO LX To  NAVIC Time Bias(m) = 0
      GLO LX To  NAVIC Time Bias Uncertainty(m) = 599585
      Filtered GLO LX To NAVIC Time Bias(m) = 0
      Filtered GLO LX To NAVIC Time Bias(m) = 599585
      BDS B2A To NAVIC Time Bias(m) = 0
      BDS B2A To NAVIC Time Bias Uncertainty(m) = 599585
      Filtered  BDS B2A To NAVIC Time Bias(m) = 0
      Filtered BDS B2A To NAVIC Time Bias  Uncertainty(m) = 599585
      GAL E5A To  NAVIC Time Bias(m) = 0
      GAL E5A To  NAVIC Time Bias Uncertainty(m) = 599585
      Filtered GAL E5A To NAVIC Time Bias(m) = 0
      Filtered GAL E5A To NAVIC Time Bias  Uncertainty(m) = 599585
   }
   SFT Offset = 0
   SFT Offset Uncertainty = 302400
   Clock Drift = 0
   Clock Drift Uncertainty = 1000
   Filtered Altitude = 0
   Filtered Altitude Uncertainty = 0
   Raw Altitude = 0
   Raw Altitude Uncertainty = 0
   Position Dilution Of Precision = 0
   Horizontal Dilution Of Precision = 0
   Vertical Dilution Of Precision = 0
   Time Position Dilution of Precision = 0
   Geometrical Position Dilution of Precision = 0
   Ellipse Confidence = 39
   Ellipse Angle = 0
   Ellipse SemiMajor Axis = 8591.16
   Ellipse SemiMinor Axis = 8591.16
   Vertical Position Uncertainty = 6000
   Position Reliability Horizontal = Medium
   Position Reliability Vertical = Medium
   GNSS Only Heading = -999
   GNSS Only Heading Uncertainty In Radians = 1.8138
   Sensor Data Usage Mask {
      Sensor Data Usage Mask = 0
      Raw Value = 0x00000000
   }
   Sensor Aiding Mask {
      Sensor Aiding Mask = 0
      Raw Value = 0x00000000
   }
   Tech Contributions = { 
      65535, 0, 0, 0, 0, 0, 0, 0, 
      0, 0, 0, 0, 0, 0
   }
   SBAS SVs {
      u_NumSbasSvs = 0
      w_SbasSvId = { 0, 0, 0 }
   }
   u_DgnssSrc = 0
   u_DgnssUsage = 0
   w_DgnssRefStationId = 0
   q_DgnssSrcId = 0
   q_DifferentialDateAgeMsec = 0
   GPS L1 Used SVs = 0
   GLO G1 Used SVs = 0
   BDS B1 Used SVs = 0
   GAL E1 Used SVs = 0
   QZSS L1 Used SVs = 0
   GPS L2 Used SVs = 0
   GPS L5 Used SVs = 0
   GLO G2 Used SVs = 0
   BDS B2 Used SVs = 0
   GAL E5a Used SVs = 0
   QZSS L2 Used SVs = 0
   QZSS L5 Used SVs = 0
   BDS B2a Used SVs = 0
   NAVIC L5 Used SVs = 0
   BDS B1C Used SVs = 0
   BDS B2B Used SVs = 0
   GAL E5b Used SVs = 0
   GPS L1 Total SVs = 0
   GLO G1 Total SVs = 0
   BDS B1 Total Svs = 0
   GAL E1 Total SVs = 0
   QZSS L1 Total SVs = 0
   GPS L2 Total SVs = 0
   GPS L5 Total SVs = 0
   GLO G2 Total SVs = 0
   BDS B2 Total SVs = 0
   GAL E5a Total SVs = 0
   QZSS L2 Total SVs = 0
   QZSS L5 Total SVs = 0
   BDS B2a Total SVs = 0
   NAVIC L5 Total Svs = 0
   BDS B1C Total SVs = 0
   BDS B2B Total SVs = 0
   GAL E5b Total SVs = 0
   Integrity Risk Level = 0.000000
   ProtectionAlongTrack = -1.000000
   ProtectionCrossTrack = -1.000000
   ProtectionVertical = -1.000000
   e_OperatingMode = GNSS_L1_L5_MODE_CLK_REF_L1
   ConformityIndex = -1
}




```


### LOG [ GNSS Inject Sensor Data ]

```
16:45:44.063043	[0x1510]	GNSS Inject Sensor Data
Version = 3
v3 {
   u_SensorLogged = ACCELEROMETER (1)
   w_TimeGpsWeekOfFirstSample = 2338
   dw_TimeGpsMsecOfFirstSample = 60362068 msec
   t_LocalTimeOfFirstSampleMsec = 54882431
   t_LocalTimeOfDataReceiveMsec = 54882688
   u_Flags {
      u_Flags = 128
      SignReversal = DO_NOT_REVERSE (0)
      SyncMethod = RTT_TIME_SYNC_BETWEEN_MODEM_TIME_AND_SENSOR_SYSTEM_TIME (0)
      TimeSource = UNIDENTIFIED_OR_UNKNOWN_SOURCE (0)
      GpsTimeValidity = VALID (1)
   }
   u_NumSamples = 25
   axis[0] {
      w_SampleTimeOffset = 0
      f_xAxisSample = 0.946735
      f_yAxisSample = 9.83931
      f_zAxisSample = 0.675849
   }
   axis[1] {
      w_SampleTimeOffset = 10
      f_xAxisSample = 0.91561
      f_yAxisSample = 9.43948
      f_zAxisSample = -0.135785
   }
   axis[2] {
      w_SampleTimeOffset = 20
      f_xAxisSample = 0.855756
      f_yAxisSample = 9.72918
      f_zAxisSample = -0.0400173
   }
   axis[3] {
      w_SampleTimeOffset = 30
      f_xAxisSample = 1.35854
      f_yAxisSample = 9.99015
      f_zAxisSample = -0.140574
   }
   axis[4] {
      w_SampleTimeOffset = 41
      f_xAxisSample = 1.8278
      f_yAxisSample = 10.2008
      f_zAxisSample = -0.389571
   }
   axis[5] {
      w_SampleTimeOffset = 51
      f_xAxisSample = 1.665
      f_yAxisSample = 10.2248
      f_zAxisSample = -0.435061
   }
   axis[6] {
      w_SampleTimeOffset = 61
      f_xAxisSample = 1.27474
      f_yAxisSample = 9.76988
      f_zAxisSample = -0.262678
   }
   axis[7] {
      w_SampleTimeOffset = 71
      f_xAxisSample = 1.0425
      f_yAxisSample = 9.32695
      f_zAxisSample = 0.278412
   }
   axis[8] {
      w_SampleTimeOffset = 81
      f_xAxisSample = 0.989831
      f_yAxisSample = 9.23598
      f_zAxisSample = -0.0807187
   }
   axis[9] {
      w_SampleTimeOffset = 91
      f_xAxisSample = 1.12391
      f_yAxisSample = 9.45385
      f_zAxisSample = -0.569136
   }
   axis[10] {
      w_SampleTimeOffset = 102
      f_xAxisSample = 1.21728
      f_yAxisSample = 9.74594
      f_zAxisSample = 0.00307836
   }
   axis[11] {
      w_SampleTimeOffset = 112
      f_xAxisSample = 1.3729
      f_yAxisSample = 10.22
      f_zAxisSample = -0.200429
   }
   axis[12] {
      w_SampleTimeOffset = 122
      f_xAxisSample = 1.45191
      f_yAxisSample = 10.3589
      f_zAxisSample = -1.11501
   }
   axis[13] {
      w_SampleTimeOffset = 132
      f_xAxisSample = 1.14545
      f_yAxisSample = 10.0308
      f_zAxisSample = 0.132365
   }
   axis[14] {
      w_SampleTimeOffset = 142
      f_xAxisSample = 1.10236
      f_yAxisSample = 9.50413
      f_zAxisSample = 0.592052
   }
   axis[15] {
      w_SampleTimeOffset = 153
      f_xAxisSample = 1.11672
      f_yAxisSample = 9.52567
      f_zAxisSample = -0.442243
   }
   axis[16] {
      w_SampleTimeOffset = 163
      f_xAxisSample = 0.951524
      f_yAxisSample = 9.71721
      f_zAxisSample = -0.267466
   }
   axis[17] {
      w_SampleTimeOffset = 173
      f_xAxisSample = 1.18376
      f_yAxisSample = 10.0237
      f_zAxisSample = 0.362209
   }
   axis[18] {
      w_SampleTimeOffset = 183
      f_xAxisSample = 1.4567
      f_yAxisSample = 10.3828
      f_zAxisSample = -0.456608
   }
   axis[19] {
      w_SampleTimeOffset = 193
      f_xAxisSample = 1.03532
      f_yAxisSample = 10.6845
      f_zAxisSample = -0.238736
   }
   axis[20] {
      w_SampleTimeOffset = 203
      f_xAxisSample = 1.06405
      f_yAxisSample = 10.4786
      f_zAxisSample = 0.52262
   }
   axis[21] {
      w_SampleTimeOffset = 213
      f_xAxisSample = 1.23643
      f_yAxisSample = 10.2224
      f_zAxisSample = -0.336898
   }
   axis[22] {
      w_SampleTimeOffset = 223
      f_xAxisSample = 1.02096
      f_yAxisSample = 9.9255
      f_zAxisSample = -0.820527
   }
   axis[23] {
      w_SampleTimeOffset = 233
      f_xAxisSample = 0.901245
      f_yAxisSample = 9.56398
      f_zAxisSample = -0.025652
   }
   axis[24] {
      w_SampleTimeOffset = 243
      f_xAxisSample = 0.994619
      f_yAxisSample = 9.38202
      f_zAxisSample = -0.00171004
   }
}

```

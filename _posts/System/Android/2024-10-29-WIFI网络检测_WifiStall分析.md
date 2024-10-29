---
layout: post
title: Wifi-Stall检测_Monitor
category: Wifi
tags: Wifi Stall
keywords: Wifi Stall
typora-root-url: ..\..\..\
typora-copy-images-to: ..\..\..\public\zimage
---

## 简介
 * TOC
 {:toc}

##  Wifi Stall 

```
 WIfi Stall  === Wifi 失速  联网速度下降问题

```



### /proc//net/snmp_网络统计信息

```

cat /proc//net/snmp
Ip: Forwarding DefaultTTL InReceives InHdrErrors InAddrErrors ForwDatagrams InUnknownProtos InDiscards InDelivers OutRequests OutDiscards OutNoRoutes ReasmTimeout ReasmReqds ReasmOKs ReasmFails FragOKs FragFails FragCreates OutTransmits
Ip: 2 64 14873 0 6 0 0 0 14867 35916 0 624 0 0 0 0 0 0 0 35803
Icmp: InMsgs InErrors InCsumErrors InDestUnreachs InTimeExcds InParmProbs InSrcQuenchs InRedirects InEchos InEchoReps InTimestamps InTimestampReps InAddrMasks InAddrMaskReps OutMsgs OutErrors OutRateLimitGlobal OutRateLimitHost OutDestUnreachs OutTimeExcds OutParmProbs OutSrcQuenchs OutRedirects OutEchos OutEchoReps OutTimestamps OutTimestampReps OutAddrMasks OutAddrMaskReps
Icmp: 7 0 0 7 0 0 0 0 0 0 0 0 0 0 378 0 0 0 18 0 0 0 0 360 0 0 0 0 0
IcmpMsg: InType3 OutType3 OutType8
IcmpMsg: 7 18 360
Tcp: RtoAlgorithm RtoMin RtoMax MaxConn ActiveOpens PassiveOpens AttemptFails EstabResets CurrEstab InSegs OutSegs RetransSegs InErrs OutRsts InCsumErrors
Tcp: 1 200 120000 -1 3922 0 3046 185 33 13660 11468 23291 1 230 0
Udp: InDatagrams NoPorts InErrors OutDatagrams RcvbufErrors SndbufErrors InCsumErrors IgnoredMulti MemErrors
Udp: 792 18 3 1107 3 0 0 319 0
UdpLite: InDatagrams NoPorts InErrors OutDatagrams RcvbufErrors SndbufErrors InCsumErrors IgnoredMulti MemErrors
UdpLite: 0 0 0 0 0 0 0 0 0

```



```

TCP重传率 =  重传分片数(RetransSegs_23291) / TCP发送的分片数(OutSegs _11468)  ===  202.2%

retrans = [(RetransSegs(当前重传数据)－last RetransSegs(之前的重传数据)) ／ (OutSegs－last OutSegs) ]  * 100 

```



| Tcp: | RtoAlgorithm | RtoMin | RtoMax | MaxConn | ActiveOpens 240秒内新增主动TCP连接数 | PassiveOpens 240秒内被动接受到的TCP连接数 | AttemptFails 240秒内连接失败个数 | EstabResets | CurrEstab当前TCP连接数量ESTABLED状态 | InSegs 240秒内接受 的TCP报文 | OutSegs 240秒内发送的报文 | RetransSegs 240秒内重传的报文 | InErrs (校验失败) 240秒内TCP入包错误 | OutRsts 240秒内TCP发包错误 | InCsumErrors |
| ---- | ------------ | ------ | ------ | ------- | ------------------------------------ | ----------------------------------------- | -------------------------------- | ----------- | ------------------------------------ | ---------------------------- | ------------------------- | ----------------------------- | ------------------------------------ | -------------------------- | ------------ |
| Tcp: | 1            | 200    | 120000 | -1      | 3922                                 | 0                                         | 3046                             | 185         | 33                                   | 13660                        | 11468                     | 23291                         | 1                                    | 230                        | 0            |



| Udp: | InDatagrams 240秒内UDP 接收数据报 | NoPorts 未知端口接收数据包 | InErrors  (校验失败,收包缓冲区满) 等其他原因导致 UDP无法发送到应用层的包 | OutDatagrams 240秒内UDP 发送数据报 | RcvbufErrors 接收缓冲区溢出的包量 | SndbufErrors 发送缓冲区溢出的包量 | InCsumErrors | IgnoredMulti | MemErrors |
| ---- | --------------------------------- | -------------------------- | ------------------------------------------------------------ | ---------------------------------- | --------------------------------- | --------------------------------- | ------------ | ------------ | --------- |
| Udp: | 792                               | 18                         | 3                                                            | 1107                               | 3                                 | 0                                 | 0            | 319          | 0         |






### /packages/modules/NetworkStack/src/com/android/server/connectivity/NetworkMonitor.java

```
gedit /packages/modules/NetworkStack/src/com/android/server/connectivity/NetworkMonitor.java

```



### /packages/modules/NetworkStack/src/android/net/util/DataStallUtils.java

```
gedit packages/modules/NetworkStack/src/android/net/util/DataStallUtils.java

```




##  Wifi Monitor 

```
Wifi Monitor  ===  检测Wifi 连通性 无网等情况问题



```

 

//xx img src="/public/zimage/system/android/10_anr/22.png"  />

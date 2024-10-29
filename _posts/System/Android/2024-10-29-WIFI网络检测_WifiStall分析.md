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



//  InSegs段
mTcpData.receivedCount = Integer.parseInt(tcpDataParts[inSegsIndex]);

//  OutSegs段
mTcpData.sentCount = Integer.parseInt(tcpDataParts[outSegsIndex]);

// RetransSegs段
mTcpData.retransmitCount = Integer.parseInt(tcpDataParts[retransSegsIndex]);

// InErrs段 
mTcpData.lostCount = Integer.parseInt(tcpDataParts[inErrsIndex]);
            
            
            

//set the threshold to 5 to avoid wrong calculation
//mLatestFailPercentageFromSnmp = ((mTcpData.sentCount > 10) ? (((mTcpData.retransmitCount + mTcpData.lostCount) / mTcpData.sentCount) * 100):0);
//when only set sentCount > 10 within 15 second as the condition, sometime sentCount only < 10 in live broadcast when disconnect network in Ap side
//Maybe try to add TX + Retransmit + lost > 20 as the condition


// public static final int DEFAULT_TCP_PACKETS_FAIL_PERCENTAGE = 80;
// 重传率计算 大于 80%  那么 就判定为  WIFI STALL 
    mLatestFailPercentageFromSnmp  = ((mTcpData.retransmitCount + mTcpData.lostCount) / mTcpData.sentCount) * 100;
    
```



### /packages/modules/NetworkStack/src/android/net/util/DataStallUtils.java

```
gedit packages/modules/NetworkStack/src/android/net/util/DataStallUtils.java



```




##  Wifi Monitor 

```
Wifi Monitor  ===  检测Wifi 连通性 无网等情况问题





```

### NetCapability && Transport

https://blog.csdn.net/liuning1985622/article/details/138542838

```

    public interface NetCapability { 
        NET_CAPABILITY_MMS;指示这是一个能够访问运营商的 MMSC 以发送和接收彩信的网络。
        NET_CAPABILITY_SUPL;指示这是一个能够访问运营商的 SUPL 服务器的网络用于检索 GPS 信息。
        NET_CAPABILITY_DUN;表示这是一个能够访问运营商的 DUN 或网络共享网关的网络。
        NET_CAPABILITY_FOTA;表示这是一个能够访问运营商FOTA门户的网络用于无线更新。
        NET_CAPABILITY_IMS;指示这是一个能够访问运营商的 IMS 服务器的网络用于网络注册和信令。
        NET_CAPABILITY_CBS;指示这是一个能够访问运营商的 CBS 服务器的网络用于运营商特定的服务。
        NET_CAPABILITY_WIFI_P2P;指示这是一个能够到达 Wi-Fi 直接对等方的网络。
        NET_CAPABILITY_IA;指示这是一个能够访问运营商的初始连接服务器的网络。
        NET_CAPABILITY_RCS;指示这是一个能够访问运营商的 RCS 服务器用于富通信服务的网络。
        NET_CAPABILITY_XCAP;表示这是一个能够访问运营商的 XCAP 服务器用于配置和控制的网络。
        NET_CAPABILITY_EIMS;表示这是一个能够访问运营商的紧急 IMS 服务器或其他服务的网络用于在紧急呼叫期间发出网络信令。
        NET_CAPABILITY_NOT_METERED;指示此网络不按流量计费。
        NET_CAPABILITY_INTERNET;指示此网络应该能够访问互联网。
        NET_CAPABILITY_NOT_RESTRICTED;指示此网络可用于常规用途。
        NET_CAPABILITY_TRUSTED;指示用户已指示对此网络的隐式信任。
        NET_CAPABILITY_NOT_VPN;指示此网络不是 VPN。
        NET_CAPABILITY_VALIDATED;指示已成功验证此网络上的连接。
        NET_CAPABILITY_CAPTIVE_PORTAL;指示上次探测此网络时发现此网络具有强制网络门户。
        NET_CAPABILITY_NOT_ROAMING;指示此网络未漫游。
        NET_CAPABILITY_FOREGROUND;指示此网络可供应用使用而不是在后台保持以便于快速网络切换的网络。
        NET_CAPABILITY_NOT_CONGESTED;指示此网络未拥塞。
        NET_CAPABILITY_NOT_SUSPENDED;指示此网络当前未挂起。
        NET_CAPABILITY_OEM_PAID;指示通过此网络的流量由 OEM 支付。例如系统应用可以使用此网络上传遥测数据。
        NET_CAPABILITY_MCX;指示这是一个能够访问运营商的关键任务服务器的网络。
        NET_CAPABILITY_PARTIAL_CONNECTIVITY;指示此网络经过测试仅提供部分连接。
        NET_CAPABILITY_TEMPORARILY_NOT_METERED;指示此网络暂时不按流量计费。
        NET_CAPABILITY_OEM_PRIVATE;指示此网络是 OEM 专用的仅供 OEM 使用。
        NET_CAPABILITY_VEHICLE_INTERNAL;表示这是一个内部车辆网络用于与其他汽车系统通信。
        NET_CAPABILITY_NOT_VCN_MANAGED;指示此网络未包含在虚拟运营商网络 VCN 中。
        NET_CAPABILITY_ENTERPRISE;指示此网络供企业使用。
        NET_CAPABILITY_VSIM;表示此网络能够访问运营商的虚拟 Sim 服务。
        NET_CAPABILITY_BIP;指示此网络能够支持承载独立 Protol。
        NET_CAPABILITY_HEAD_UNIT;指示此网络已连接到汽车音响主机。
        NET_CAPABILITY_MMTEL;指示此网络能够支持 MMTEL多媒体电话服务。
        NET_CAPABILITY_PRIORITIZE_LATENCY;指示此网络应该能够确定互联网延迟的优先级。
        NET_CAPABILITY_PRIORITIZE_BANDWIDTH;指示此网络应能够确定互联网带宽的优先级。
    }
    public interface Transport { 
        TRANSPORT_CELLULAR;指示此网络使用手机网络传输。
        TRANSPORT_WIFI;指示此网络使用 Wi-Fi 传输。
        TRANSPORT_BLUETOOTH;指示此网络使用蓝牙传输。
        TRANSPORT_ETHERNET;指示此网络使用以太网传输。
        TRANSPORT_VPN;指示此网络使用 VPN 传输。
        TRANSPORT_WIFI_AWARE;指示此网络使用 Wi-Fi 感知传输。
        TRANSPORT_LOWPAN;指示此网络使用 LoWPAN 传输。
        TRANSPORT_TEST;
        TRANSPORT_USB;指示此网络使用 USB 传输。
    }
    
```

### windows_nslookup_查询本地DNS服务器

```

C:\Users\> nslookup www.baidu.com
默认DNS服务器:  internal-xx-dns.xxxx.com
Address:  10.10.8.8

名称:    www.a.shifen.com
Addresses:  2408:873d:22:1a01:0:ff:b087:eecc
          2408:873d:22:18ac:0:ff:b021:1393
          153.3.238.102
          153.3.238.110
Aliases:  www.baidu.com

_______________________________________________

C:\Users\>nslookup     【 连接WIFI网络的情况 】
默认DNS服务器:  internal-xxx-dns.xxx.com
Address:  10.10.8.8
_______________________________________________

C:\Users\>nslookup     【 关闭WIFI网络的情况 】
*** 默认DNS服务器不可用
默认服务器:  UnKnown
Address:  127.0.0.1

_______________________________________________

C:\Users\>nslookup     【 PC连接手机热点网络 】

nslookup www.baidu.com
服务器:  UnKnown              【默认私网DNS服务器异常】        
Address:  192.168.109.94
_______________________________________________

C:\Users\> nslookup www.baidu.com
服务器:  UnKnown
Address:  192.168.109.94

DNS request timed out.   【本地DNS服务器请求超时】        
    timeout was 2 seconds.
DNS request timed out.
    timeout was 2 seconds.
非权威应答:
名称:    www.a.shifen.com
Addresses:  240e:e9:6002:15a:0:ff:b05c:1278
          240e:e9:6002:15c:0:ff:b015:146f
          180.101.50.188
          180.101.50.242
Aliases:  www.baidu.com



```


###  WIFI 无网Log 追踪


#### UI层无网提示
```
gedit ./frameworks/base/packages/SettingsLib/res/values-zh-rCN/strings.xml

<string name="wifi_limited_connection">"网络连接受限"</string>
<string name="wifi_status_no_internet">"无法访问互联网"</string>
<string name="wifi_status_sign_in_required">"必须登录"</string>
<string name="private_dns_broken">"无法访问私人 DNS 服务器"</string>




gedit ./frameworks/opt/net/wifi/libs/WifiTrackerLib/res/values-zh-rCN/strings.xml

<string name="wifi_connected_low_quality">"质量不佳"</string>
<string name="wifitrackerlib_wifi_connected_cannot_provide_internet" >"已连接到设备，但无法提供互联网连接。"</string>


```

```

gedit ./frameworks/base/packages/SettingsLib/src/com/android/settingslib/wifi/WifiStatusTracker.java

########## WifiStatusTracker.java

══════════════════ // 注册网络状态监听回调 mNetworkCallback  mConnectivityManager.registerNetworkCallback(mNetworkRequest, mNetworkCallback, mHandler);  

   private final NetworkCallback mNetworkCallback = new NetworkCallback(NetworkCallback.FLAG_INCLUDE_LOCATION_INFO) {

        public void onCapabilitiesChanged(
             .....
______   updateStatusLabel(); // 监听网络能力Capabilities变化 更新显示
        }
        
        public void onLost(Network network) {
______      updateStatusLabel(); // 监听网络断开 更新显示
        }
           
    }
══════════════════     mCacheListener =
                new WifiNetworkScoreCache.CacheListener(mHandler) {
                    @Override
                    public void networkCacheUpdated(List<ScoredNetwork> updatedNetworks) {
_____监听网络估分更新回调_更新显示_____  updateStatusLabel();
                        mMainThreadHandler.post(() -> postResults());
                    }
                };         



  /** Refresh the status label on Locale changed. */
══════════════════  public void refreshLocale() {
_____本地配置更新时_更新显示_____ updateStatusLabel();
        mMainThreadHandler.post(() -> postResults());
    }
    





════════更新显示【updateStatusLabel】═════════   private void updateStatusLabel() {

        NetworkCapabilities networkCapabilities;
        // 是否是默认使用的网络
        isDefaultNetwork = mDefaultNetworkCapabilities != null
                && connectionIsWifi(mDefaultNetworkCapabilities);
                
        if (isDefaultNetwork) {
            // Wifi is connected and the default network.
_______获取当前NetworkCapabilities________ networkCapabilities = mDefaultNetworkCapabilities;
        } else {
_______获取系统NetworkCapabilities________ networkCapabilities = mConnectivityManager.getNetworkCapabilities(
                    mWifiManager.getCurrentNetwork());
        }
        isCaptivePortal = false;  // 是否是CaptivePort网络
        if (networkCapabilities != null) {
            if (networkCapabilities.hasCapability(NET_CAPABILITY_CAPTIVE_PORTAL)) {
                statusLabel = mContext.getString(R.string.wifi_status_sign_in_required);
                isCaptivePortal = true;  // 必须登录
                return;
            } else if (networkCapabilities.hasCapability(NET_CAPABILITY_PARTIAL_CONNECTIVITY)) {
  //  NET_CAPABILITY_PARTIAL_CONNECTIVITY;指示此网络经过测试仅提供部分连接。

                statusLabel = mContext.getString(R.string.wifi_limited_connection);  // 网络连接受限
                return;
            } else if (!networkCapabilities.hasCapability(NET_CAPABILITY_VALIDATED)) {
  //  NET_CAPABILITY_VALIDATED 指示已成功验证此网络上的连接 
                final String mode = Settings.Global.getString(mContext.getContentResolver(),
                        Settings.Global.PRIVATE_DNS_MODE);
       if (networkCapabilities.isPrivateDnsBroken()) {// 默认私网DNS异常 无法正确解析内部域名 直接ping外部的IP地址是正常的，ping DNS服务器地址也是正常的 使用nslookup进行域名解析测试的时候，默认私网DNS服务器返回 request timed out错误
                    statusLabel = mContext.getString(R.string.private_dns_broken);
                } else {
                    statusLabel = mContext.getString(R.string.wifi_status_no_internet);  // 无法访问互联网
                }
                return;
            } else if (!isDefaultNetwork && mDefaultNetworkCapabilities != null
                    && mDefaultNetworkCapabilities.hasTransport(TRANSPORT_CELLULAR)) {
 // TRANSPORT_CELLULAR 指示此网络使用手机网络传输
                statusLabel = mContext.getString(
 // 质量不佳 com.android.wifitrackerlib.R.string.wifi_connected_low_quality); 
                return;
            }
        }
}


```


#### NetworkCapabilities的更新

```


```




### /packages/modules/NetworkStack/src/com/android/server/connectivity/NetworkMonitor.java


https://blog.csdn.net/weixin_43278325/article/details/131230059


 ```
Wi-Fi连接成功后需要做连通性的一个校验，如果校验成功，则表示网络畅通，反之就是网络受阻.


ValidatedState 状态表示网络已成功验证，或用户希望“按原样”验证，或不满足默认的NetworkRequest，因此已跳过验证。

MaybeNotifyState 状态表示可能已通知用户需要登录。此状态注意在退出状态时清除通知。

CaptivePortalState 状态表示检测到 CaptivePortal，并向用户显示登录通知

EvaluatingState 状态表示正在评估网络的 Internet 连接性，或者用户已指出该网络是不需要的。

WaitingForNextProbe 状态表示评估探测失败，状态从ProbingState转换。这确保状态机仅在探测进行时处于ProbingState，而不是在等待执行下一个探测时，这允许ProbingState延迟大多数消息，直到探测完成。
```





 <img src="/public/zimage/system/android/wifi_monitor/wifi_monitor.jpg"  />

```


```



```

现在再回到开头的那个问题，Setting那边调用下来的 CMD_LAUNCH_CAPTIVE_PORTAL_APP 
 会在 MaybeNotifyState 状态里处理，

 当 DefaultState 里面处理了网络连接事件 CMD_NETWORK_CONNECTED就会转到 EvaluatingState 状态里面，然后自己给自己发送 CMD_REEVALUATE来进行网络评估，
 如果是需要评估的就转到 ProbingState 里面对URL进行判断，结果会随着CMD_PROBE_COMPLETE消息带过来，

 1.如果是protal网络的，就需要启动CaptivePortalLoginActivity去sign-in，用户sign-in结束，再进行一次校验，
 2.如果是非protal网络，就直接对URL连通性进行检测成功后都转到 EvaluatingBandwidthState 和 EvaluatingPrivateDnsState 状态机里面，
   然后再给自己发送消息CMD_EVALUATE_PRIVATE_DNS来处理一些事物，验证都ok的话最终转到ValidatedState状态机里，
   至此，整个流程大致完毕。

```

 <img src="/public/zimage/system/android/wifi_monitor/wifi_monitor.jpg"  />




###  WifiEntry.signIn() 追踪

####  NetworkProviderSettings.java

```
######/packages/apps/Settings/src/com/android/settings/network/NetworkProviderSettings.java


══════════════════      protected void updateWifiEntryPreferences() {


       final WifiEntry connectedEntry = mWifiPickerTracker.getConnectedWifiEntry();

 final ConnectedWifiEntryPreference pref = createConnectedWifiEntryPreference(connectedEntry);
 connectedWifiPreferenceCategory.addPreference(pref);
 //  已连接网络的点击事件处理
    pref.setOnPreferenceClickListener(preference -> {
______检测是否可登录并登录_______     if (connectedEntry.canSignIn()) {
            connectedEntry.signIn(null /* callback */);
        } else {
          // 打开WIFI详情开关
          launchNetworkDetailsFragment(pref);
        }
        return true;
    });


}

```





```

gedit ./packages/modules/NetworkStack/src/com/android/server/connectivity/NetworkMonitor.java


══════════════════ public NetworkMonitor(){  // NetworkMonitor 的构造函数创建了状态机
      // CHECKSTYLE:OFF IndentationCheck
        addState(mDefaultState);
        addState(mMaybeNotifyState, mDefaultState);
            addState(mEvaluatingState, mMaybeNotifyState);
                addState(mProbingState, mEvaluatingState);
                addState(mWaitingForNextProbeState, mEvaluatingState);
            addState(mCaptivePortalState, mMaybeNotifyState);
        addState(mEvaluatingPrivateDnsState, mDefaultState);
            addState(mStartingPrivateDnsEvaluation, mEvaluatingPrivateDnsState);
            addState(mResolvingPrivateDnsState, mEvaluatingPrivateDnsState);
            addState(mProbingForPrivateDnsState, mEvaluatingPrivateDnsState);
        addState(mEvaluatingBandwidthState, mDefaultState);
        addState(mValidatedState, mDefaultState);
        setInitialState(mDefaultState);
        // CHECKSTYLE:ON IndentationCheck






══════════════════ private class ProbingState extends State {
        private Thread mThread;

        @Override
        public void enter() {
_____________ mThread = new Thread(() -> sendMessage(obtainMessage(CMD_PROBE_COMPLETE, token, 0,
                    isCaptivePortal(deps, httpsUrls, httpUrls, fallbackUrl))));
            mThread.start();
        }
}


══════════════════ private CaptivePortalProbeResult isCaptivePortal(ValidationProperties properties,
            URL[] httpsUrls, URL[] httpUrls, URL fallbackUrl) {
            

      final CaptivePortalProbeResult result;
        if (pacUrl != null) {
            result = sendDnsAndHttpProbes(null, pacUrl, ValidationProbeEvent.PROBE_PAC);
            reportHttpProbeResult(NETWORK_VALIDATION_PROBE_HTTP, result);
        } else if (mUseHttps && httpsUrls.length == 1 && httpUrls.length == 1) {
            // Probe results are reported inside sendHttpAndHttpsParallelWithFallbackProbes.
            // 【 进行 Http HTTPS 请求】
_____________     result = sendHttpAndHttpsParallelWithFallbackProbes(properties, proxyInfo,
                    httpsUrls[0], httpUrls[0], fallbackUrl);
        } else if (mUseHttps) {
            // Support result aggregation from multiple Urls.
            result = sendMultiParallelHttpAndHttpsProbes(properties, proxyInfo, httpsUrls,
                    httpUrls);
        } else {
            result = sendDnsAndHttpProbes(proxyInfo, httpUrls[0], ValidationProbeEvent.PROBE_HTTP);
            reportHttpProbeResult(NETWORK_VALIDATION_PROBE_HTTP, result);
        }
            
 }




══════════════════  private CaptivePortalProbeResult sendHttpAndHttpsParallelWithFallbackProbes(
            ValidationProperties properties, ProxyInfo proxy, URL httpsUrl, URL httpUrl,
            URL fallbackUrl) {
..............
        try {
            httpsProbe.start();
            httpProbe.start();
            latch.await(PROBE_TIMEOUT_MS, TimeUnit.MILLISECONDS);
        } catch (InterruptedException e) {
            validationLog("Error: probes wait interrupted!");
            return CaptivePortalProbeResult.failed(CaptivePortalProbeResult.PROBE_UNKNOWN);
        }
..............
        final CaptivePortalProbeResult httpsResult = httpsProbe.result();
        final CaptivePortalProbeResult httpResult = httpProbe.result();

        // Look for a conclusive probe result first.
        if (isConclusiveResult(httpResult, capportApiUrl)) {
_____________     reportProbeResult(httpProbe.result());
            return httpResult;
        }

        if (isConclusiveResult(httpsResult, capportApiUrl)) {
            reportProbeResult(httpsProbe.result());
            return httpsResult;
        }
 // Otherwise wait until http and https probes completes and use their results.   等待 httpProbe 完成 

        try {
            httpProbe.join();
_____________   reportProbeResult(httpProbe.result());

            if (httpProbe.result().isPortal()) {
                return httpProbe.result();
            }
    
            httpsProbe.join();
_____________   reportHttpProbeResult(NETWORK_VALIDATION_PROBE_HTTPS, httpsProbe.result());

            if (httpsProbe.result().isFailed() && httpProbe.result().isSuccessful()) {
                return CaptivePortalProbeResult.PARTIAL;
            }
            return httpsProbe.result();
        } catch (InterruptedException e) {
            validationLog("Error: http or https probe wait interrupted!");
            return CaptivePortalProbeResult.failed(CaptivePortalProbeResult.PROBE_UNKNOWN);
        }

}





══════════════════  private void reportProbeResult(@NonNull CaptivePortalProbeResult res) {
        if (res instanceof CapportApiProbeResult) {
            maybeReportCaptivePortalData(((CapportApiProbeResult) res).getCaptivePortalData());
        }

        // This is not a if-else case since partial connectivity will concluded from both HTTP and
        // HTTPS probe. Both HTTP and HTTPS result should be reported.
        if (res.isConcludedFromHttps()) {
            reportHttpProbeResult(NETWORK_VALIDATION_PROBE_HTTPS, res);
        }
    
        if (res.isConcludedFromHttp()) {
            reportHttpProbeResult(NETWORK_VALIDATION_PROBE_HTTP, res);
        }
    }


```




```
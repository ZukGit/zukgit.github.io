---
layout: post
title: adb_shell_cmd命令
category: 系统
tags: Android
keywords: cmd android
typora-root-url: ..\..\..\
typora-copy-images-to: ..\..\..\public\zimage
---
---
---
## 简介
* TOC
{:toc}



## A


## B


## C


## D


## E


## F


## G


## H


## I


## J


## K


## L


## M


## N


## O


## P



## Q


## R


## S


## T


## U


## V


## W

### Wifi 

```
adb shell cmd wifi     ## 打印 help信息

Wi-Fi (wifi) commands:
  help or -h
    Print this help text.
  get-country-code
    Gets country code as a two-letter string
  set-wifi-enabled enabled|disabled
    Enables/disables Wifi on this device.
  set-scan-always-available enabled|disabled
    Sets whether scanning should be available even when wifi is off.
  list-scan-results
    Lists the latest scan results
  start-scan
    Start a new scan
  list-networks
    Lists the saved networks
  forget-network <networkId>
    Remove the network mentioned by <networkId>
        - Use list-networks to retrieve <networkId> for the network
  status
    Current wifi status
  set-verbose-logging enabled|disabled
    Set the verbose logging enabled or disabled
  is-verbose-logging
    Check whether verbose logging enabled or disabled
  start-restricting-auto-join-to-subscription-id subId
    temporarily disable all wifi networks except merged carrier networks with the given subId
  stop-restricting-auto-join-to-subscription-id
    Undo the effects of start-restricting-auto-join-to-subscription-id
  add-suggestion <ssid> open|owe|wpa2|wpa3 [<passphrase>] [-u] [-o] [-p] [-m]  [-s] [-d] [-b <bssid>] [-e] [-i] [-a <carrierId>] [-c <subscriptionId>]
    Add a network suggestion with provided params
    Use 'network-suggestions-set-user-approved com.android.shell yes' to approve suggestions added via shell (Needs root access)
    <ssid> - SSID of the network
    open|owe|wpa2|wpa3 - Security type of the network.
        - Use 'open' or 'owe' for networks with no passphrase
           - 'open' - Open networks (Most prevalent)
           - 'owe' - Enhanced open networks
        - Use 'wpa2' or 'wpa3' for networks with passphrase
           - 'wpa2' - WPA-2 PSK networks (Most prevalent)
           - 'wpa3' - WPA-3 PSK networks
    -u - Mark the suggestion untrusted.
    -o - Mark the suggestion oem paid.
    -p - Mark the suggestion oem private.
    -m - Mark the suggestion metered.
    -h - Mark the network hidden.
    -s - Share the suggestion with user.
    -d - Mark the suggestion autojoin disabled.
    -b <bssid> - Set specific BSSID.
    -r - Enable non_persistent randomization (disabled by default)
    -a - Mark the suggestion carrier merged
    -c <carrierId> - set carrier Id
    -i <subscriptionId> - set subscription Id, if -a is used, this must be set
  remove-suggestion <ssid> [-l]
    Remove a network suggestion with provided SSID of the network
    -l - Remove suggestion with lingering, if not set will disconnect immediately
  remove-all-suggestions
    Removes all suggestions added via shell
  list-suggestions
    Lists the suggested networks added via shell
  set-coex-cell-channels [lte|nr <bandNumber 1-261> <downlinkFreqKhz or UNKNOWN: -1> <downlinkBandwidthKhz or UNKNOWN: 0> <uplinkFreqKhz or UNKNOWN: -1> <uplinkBandwidthKhz or UNKNOWN: 0>] ...
    Sets a list of zero or more cell channels to use for coex calculations. Actual device reported cell channels will be ignored until reset-coex-cell-channels is called.
  reset-coex-cell-channels
    Removes all cell channels set in set-coex-cell-channels and returns to listening on actual device reported cell channels
  get-coex-cell-channels
    Prints the cell channels being used for coex.
  set-connected-score <score>
    Set connected wifi network score (to choose between LTE & Wifi for default route).
    This turns off the active connected scorer (default or external).
    Only works while connected to a wifi network. This score will stay in effect until you call reset-connected-score or the device disconnects from the current network.
    <score> - Integer score should be in the range of 0 - 60
  reset-connected-score
    Turns on the default connected scorer.
    Note: Will clear any external scorer set.
  start-softap <ssid> (open|wpa2|wpa3|wpa3_transition|owe|owe_transition) <passphrase> [-b 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6] [-x] [-f <int> [<int>]]
    Start softap with provided params
    Note that the shell command doesn't activate internet tethering. In some devices, internet sharing is possible when Wi-Fi STA is also enabled and isassociated to another AP with internet access.
    <ssid> - SSID of the network
    open|wpa2|wpa3|wpa3_transition|owe|owe_transition - Security type of the network.
        - Use 'open', 'owe', 'owe_transition' for networks with no passphrase
        - Use 'wpa2', 'wpa3', 'wpa3_transition' for networks with passphrase
    -b 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6 - select the preferred bands.
        - Use '2' to select 2.4GHz band as the preferred band
        - Use '5' to select 5GHz band as the preferred band
        - Use '6' to select 6GHz band as the preferred band
        - Use 'any' to indicate no band preference
        - Use 'bridged' to indicate bridged AP which enables APs on both 2.4G + 5G
        - Use 'bridged_2_5' to indicate bridged AP which enables APs on both 2.4G + 5G
        - Use 'bridged_2_6' to indicate bridged AP which enables APs on both 2.4G + 6G
        - Use 'bridged_5_6' to indicate bridged AP which enables APs on both 5G + 6G
    Note: If the band option is not provided, 2.4GHz is the preferred band.
          The exact channel is auto-selected by FW unless overridden by force-softap-channel command or '-f <int> <int>' option
    -f <int> <int> - force exact channel frequency for operation channel
    Note: -f <int> <int> - must be the last option
          For example:
          Use '-f 2412' to enable single Soft Ap on 2412
          Use '-f 2412 5745' to enable bridged dual Soft Ap on 2412 and 5745
    -x - Specifies the SSID as hex digits instead of plain text (T and above)
  stop-softap
    Stop softap (hotspot)
  pmksa-flush <networkId>
        - Flush the local PMKSA cache associated with the network id. Use list-networks to retrieve <networkId> for the network
  reload-resources
    Reset the WiFi resources cache which will cause them to be reloaded next time they are accessed. Necessary if overlays are manually modified.
  launch-dialog-simple [-t <title>] [-m <message>] [-l <url> <url_start> <url_end>] [-y <positive_button_text>] [-n <negative_button_text>] [-x <neutral_button_text>] [-c <timeout_millis>]
    Launches a simple dialog and waits up to 15 seconds to print the response.
    -t - Title
    -m - Message
    -l - URL of the message, with the start and end index inside the message
    -y - Positive Button Text
    -n - Negative Button Text
    -x - Neutral Button Text
    -c - Optional timeout in milliseconds
    -s - Use the legacy dialog implementation on the system process
  launch-dialog-p2p-invitation-sent <device_name> <pin> [-i <display_id>]
    Launches a P2P Invitation Sent dialog.
    <device_name> - Name of the device the invitation was sent to
    <pin> - PIN for the invited device to input
  launch-dialog-p2p-invitation-received <device_name> [-p] [-d <pin>] [-i <display_id>] [-c <timeout_millis>]
    Launches a P2P Invitation Received dialog and waits up to 15 seconds to print the response.
    <device_name> - Name of the device sending the invitation
    -p - Show PIN input
    -d - Display PIN <pin>
    -i - Display ID
    -c - Optional timeout in milliseconds
  query-interface <uid> <package_name> STA|AP|AWARE|DIRECT [-new]
    Query whether the specified could be created for the specified UID and package name, and if so - what other interfaces would be destroyed
    -new - query for a new interfaces (otherwise an existing interface is ok
  interface-priority-interactive-mode enable|disable|default
    Enable or disable asking the user when there's an interface priority conflict, |default| implies using the device default behavior.
  set-one-shot-screen-on-delay-ms <delayMs>
    set the delay for the next screen-on connectivity scan in milliseconds.
  set-network-selection-config <enabled|disabled> <enabled|disabled> -a <associated_network_selection_override>
    set whether sufficiency check is enabled for screen off case (first arg), and screen on case (second arg)
    -a - set as one of the int WifiNetworkSelectionConfig.ASSOCIATED_NETWORK_SELECTION_OVERRIDE_ values:
      0 - no override
      1 - override to enabled
      2 - override to disabled
  set-ipreach-disconnect enabled|disabled
    Sets whether CMD_IP_REACHABILITY_LOST events should trigger disconnects.
  get-ipreach-disconnect
    Gets setting of CMD_IP_REACHABILITY_LOST events triggering disconnects.
  take-bugreport
    take bugreport through betterBug. If it failed, take bugreport through bugreport manager.
	

```

#### 打开热点
```

-f 参数 AndroidT 开始有效 
start-softap <ssid> (open|wpa2|wpa3|wpa3_transition|owe|owe_transition) <passphrase> [-b 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6] [-x] [-f <int> [<int>]]
start-softap <ssid> (open|wpa2|wpa3|wpa3_transition|owe|owe_transition) <passphrase> [-b 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6] [-x] [-f <int> [<int>]]



adb shell cmd wifi start-softap   321 wpa2 87654321 -b 5    ## 【 打开一个 ssid=321 的 wpa2密码87654321的 5G频段的热点】
 

adb shell cmd wifi start-softap   321 open -b 2      ## 【 打开一个 ssid=321 的 open的 2G频段的热点】

adb shell cmd wifi start-softap   321 open -b 5      ## 【 打开一个 ssid=321 的 open的 5G频段的热点】

adb shell cmd wifi start-softap   321 open -b 6      ## 【 打开一个 ssid=321 的 open的 6G频段的热点】

adb shell cmd wifi start-softap   Wifi_2412 open -b 2  -f 2412 ## 【 打开一个 ssid= Wifi_2412 的 open的指定频段 2412 频段的热点】


adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180   ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180 频段的热点】

adb shell cmd wifi start-softap   Wifi_5200 open -b 5  -f 5200  ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180 频段的热点】





```

#### 设置查看热点频段
```

adb root && adb shell cmd wifi force-softap-channel enabled 5745

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤ 
channel: 149 band: 2
2G freq: [2412, 2417, 2422, 2427, 2432, 2437, 2442, 2447, 2452, 2457, 2462, 2467, 2472]
5G freq: [5180, 5200, 5220, 5240, 5745, 5765, 5785, 5805, 5825]
5G DFS: [5260, 5280, 5300, 5320]
6G freq: []
60G freq: []
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧



```



### Wifi 



## X


## Y


## Z





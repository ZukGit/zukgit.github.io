---
layout: post
title: adb_shell_cmd_服务ShellCommand命令
category: 系统
tags: adb Android ShellCommand 
keywords: adb cmd android shellcommand
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

### adb shell cmd bluetooth_manager

```

adb shell cmd location  bluetooth_manager -h 

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Bluetooth Manager Commands:
  help or -h
    Print this help text.
  enable
    Enable Bluetooth on this device.
  enableBle
    Call enableBle to activate ble only mode on this device.
  disable
    Disable Bluetooth on this device.
  disableBle
    revoke the call to enableBle. No-op if enableBle wasn't call before
  wait-for-state:<STATE>
    Wait until the adapter state is <STATE>. <STATE> can be one of STATE_OFF | STATE_ON
    Note: This command can timeout and failed
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧


```


####  adb shell cmd bluetooth_manager enable  打开蓝牙
```

adb shell cmd bluetooth_manager enable

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
BluetoothShellCommand: Exec enable
enable: Success
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


####  adb shell cmd bluetooth_manager  disable 关闭蓝牙
```

adb shell cmd bluetooth_manager enable

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
BluetoothShellCommand: Exec disable
disable: Success
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


####  adb shell cmd bluetooth_manager  enableBle  打开BLE低功耗蓝牙
```

adb shell cmd bluetooth_manager  enableBle 

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
uetoothShellCommand: Exec enableBle
enableBle: Success

╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```



####  adb shell cmd bluetooth_manager  disableBle  关闭BLE低功耗蓝牙
```

 adb shell cmd bluetooth_manager  disableBle 

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
BluetoothShellCommand: Exec disableBle
disableBle: Success


╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```



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

### Location(GPS)

```

 adb shell cmd location  -h 
 
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Location service commands:
  help or -h
    Print this help text.
  is-location-enabled [--user <USER_ID>]
    Gets the master location switch enabled state. If no user is specified,
    the current user is assumed.
  set-location-enabled true|false [--user <USER_ID>]
    Sets the master location switch enabled state. If no user is specified,
    the current user is assumed.
  providers
    The providers command is followed by a subcommand, as listed below:

    add-test-provider <PROVIDER> [--requiresNetwork] [--requiresSatellite]
      [--requiresCell] [--hasMonetaryCost] [--supportsAltitude]
      [--supportsSpeed] [--supportsBearing]
      [--powerRequirement <POWER_REQUIREMENT>]
      [--extraAttributionTags <TAG>,<TAG>,...]
      Add the given test provider. Requires MOCK_LOCATION permissions which
      can be enabled by running "adb shell appops set <uid>
      android:mock_location allow". There are optional flags that can be
      used to configure the provider properties and additional arguments. If
      no flags are included, then default values will be used.
    remove-test-provider <PROVIDER>
      Remove the given test provider.
    set-test-provider-enabled <PROVIDER> true|false
      Sets the given test provider enabled state.
    set-test-provider-location <PROVIDER> --location <LATITUDE>,<LONGITUDE>
      [--accuracy <ACCURACY>] [--time <TIME>]
      Set location for given test provider. Accuracy and time are optional.
    send-extra-command <PROVIDER> <COMMAND>
      Sends the given extra command to the given provider.

      Common commands that may be supported by the gps provider, depending on
      hardware and software configurations:
        delete_aiding_data - requests deletion of any predictive aiding data
        force_time_injection - requests NTP time injection
        force_psds_injection - requests predictive aiding data injection
        request_power_stats - requests GNSS power stats update

╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧



```




####  adb shell cmd location is-location-enabled 查看GPS开关是否打开
```

adb shell cmd location is-location-enabled 

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
false
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧


adb shell cmd location  set-location-enabled  true && adb shell cmd location is-location-enabled 

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
true
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧


```


####  adb shell cmd location set-location-enabled true 打开GPS开关
```

adb shell cmd location set-location-enabled true|false

adb shell cmd location set-location-enabled true
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


#### location providers send-extra-command gps force_time_injection 更新GPS时间
```
// 更新GPS时间
adb shell cmd location providers send-extra-command gps force_time_injection

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


#### location providers send-extra-command gps delete_aiding_data  删除GPS辅助数据
```
// 删除GPS辅助 数据
adb shell cmd location providers send-extra-command gps force_time_injection

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```



#### location providers send-extra-command gps force_psds_injection  注入GPS辅助数据

```
// 注入GPS辅助数据
adb shell cmd location providers send-extra-command gps force_time_injection

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```




#### location providers send-extra-command gps request_power_stats  请求卫星状态

```
// 请求卫星状态
adb shell cmd location providers send-extra-command gps request_power_stats

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```



## M



###  adb shell motsettings list global

```


adb  root &&  adb shell motsettings get global channel_id                // 查看值
skyline

adb  root &&  adb shell motsettings put global channel_id skyline        // 设置值



adb shell motsettings list global    // 查看 motsettings 下的 global 表的所有内容
adb shell settings list global      // 查看 settings 下的 global 表的所有内容

```


### adb shell dumpsys device_config

```

adb shell dumpsys device_config        // 查看各种系统配置  flags 谷歌flags


adb shell dumpsys device_config   | grep location
    

location/android.location.flags.disable_stationary_throttling=true
location/android.location.flags.enable_location_bypass=true
location/android.location.flags.enable_ni_supl_message_injection_by_carrier_config_bugfix=true
location/android.location.flags.fix_is_in_emergency_anr=true
location/android.location.flags.fix_service_watcher=true
location/android.location.flags.geoid_heights_via_altitude_hal=true
location/android.location.flags.gnss_api_measurement_request_work_source=true
location/android.location.flags.gnss_api_navic_l1=true
location/android.location.flags.gnss_configuration_from_resource=true
location/android.location.flags.location_bypass=true
location/android.location.flags.location_validation=true
location/android.location.flags.new_geocoder=true
location/android.location.flags.population_density_provider=true
location/android.location.flags.release_supl_connection_on_timeout=true
location/android.location.flags.replace_future_elapsed_realtime_jni=true
location/android.location.flags.subscriptions_changed_listener_thread=true
location/android.location.flags.use_legacy_ntp_time=true


```




## N


### adb shell cmd nfc 

```

 adb shell cmd nfc
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
NFC (Near-field communication) commands:
  help or -h
    Print this help text.
  status
    Gets status of UWB stack
  enable-nfc
    Toggle NFC on
  disable-nfc [persist]
    Toggle NFC off (optionally make it persistent)
  set-observe-mode enable|disable
    Enable or disable observe mode.
  set-reader-mode enable-polling|disable-polling
    Enable or reader mode polling
  set-controller-always-on enable|disable
    Enable or disable controller always on
  set-discovery-tech poll-mask|listen-mask
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```


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
adb shell cmd wifi -h    ## 打印 help信息

Wi-Fi (wifi) commands:
  help or -h
    Print this help text.
  get-country-code
    Gets country code as a two-letter string
  set-wifi-enabled enabled|disabled
    Enables/disables Wifi on this device.
  set-scan-always-available enabled|disabled
    Sets whether scanning should be available even when wifi is off.
  connect-network <ssid> open|owe|wpa2|wpa3|wep [<passphrase>] [-x] [-m] [-d] [-b <bssid>] [-r auto|none|persistent|non_persistent]
    Connect to a network with provided params and add to saved networks list
    <ssid> - SSID of the network
    open|owe|wpa2|wpa3|wep - Security type of the network.
        - Use 'open' or 'owe' for networks with no passphrase
           - 'open' - Open networks (Most prevalent)
           - 'owe' - Enhanced open networks
        - Use 'wpa2' or 'wpa3' or 'wep' for networks with passphrase
           - 'wpa2' - WPA-2 PSK networks (Most prevalent)
           - 'wpa3' - WPA-3 PSK networks
           - 'wep'  - WEP network. Passphrase should be bytes in hex or encoded into String using UTF-8
    -x - Specifies the SSID as hex digits instead of plain text
    -m - Mark the network metered.
    -d - Mark the network autojoin disabled.
    -h - Mark the network hidden.
    -p - Mark the network private (not shared).
    -b <bssid> - Set specific BSSID.
    -r auto|none|persistent|non_persistent - MAC randomization scheme for the network
  add-network <ssid> open|owe|wpa2|wpa3|wep [<passphrase>] [-x] [-m] [-d] [-b <bssid>] [-r auto|none|persistent|non_persistent]
    Add/update saved network with provided params
    <ssid> - SSID of the network
    open|owe|wpa2|wpa3|wep - Security type of the network.
        - Use 'open' or 'owe' for networks with no passphrase
           - 'open' - Open networks (Most prevalent)
           - 'owe' - Enhanced open networks
        - Use 'wpa2' or 'wpa3' for networks with passphrase
           - 'wpa2' - WPA-2 PSK networks (Most prevalent)
           - 'wpa3' - WPA-3 PSK networks
           - 'wep'  - WEP network. Passphrase should be bytes in hex or encoded into String using UTF-8
    -x - Specifies the SSID as hex digits instead of plain text
    -m - Mark the network metered.
    -d - Mark the network autojoin disabled.
    -h - Mark the network hidden.
    -p - Mark the network private (not shared).
    -b <bssid> - Set specific BSSID.
    -r auto|none|persistent|non_persistent - MAC randomization scheme for the network
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
  set-verbose-logging enabled|disabled [-l <verbose log level>]
    Set the verbose logging enabled or disabled with log level
      -l - verbose logging level
           0 - verbose logging disabled
           1 - verbose logging enabled
           2 - verbose logging Show key mode
           3 - verbose logging only for Wi-Fi Aware feature
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
  launch-dialog-p2p-invitation-sent <device_name> [-d <pin>] [-i <display_id>]
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
  get-allowed-channel [-b 1|6|7|8|15|16|31]
    get allowed channels in each operation mode from wifiManager if available. Otherwise, it returns from wifiScanner.
    -b - set the band in which channels are allowed
       '1'  - band 2.4 GHz
       '6'  - band 5 GHz with DFS channels
       '7'  - band 2.4 and 5 GHz with DFS channels
       '8'  - band 6 GHz
       '15' - band 2.4, 5, and 6 GHz with DFS channels
       '16' - band 60 GHz
       '31' - band 2.4, 5, 6 and 60 GHz with DFS channels
  get-cached-scan-data
    Gets scan data cached by the firmware
  force-overlay-config-value bool|integer|string|integer-array|string-array <overlayName> enabled|disabled <configValue>
    Force overlay to a specified value.
    bool|integer|string|integer-array|string-array - specified the type of the overlay
    <overlayName> - name of the overlay whose value is overridden.
    enabled|disabled: enable the override or disable it and revert to using the built-in value.
    <configValue> - override value of the overlay.Must match the overlay type
  get-overlay-config-values
    Get current overlay value in resource cache.
  get-softap-supported-features
    Gets softap supported features. Will print 'wifi_softap_acs_supported'
    and/or 'wifi_softap_wpa3_sae_supported',
    and/or 'wifi_softap_bridged_ap_supported',
    and/or 'wifi_softap_bridged_ap_with_sta_supported',
    each on a separate line.
  set-poll-rssi-interval-msecs <int> [<int>]
    Sets the interval between RSSI polls to the specified value(s), in milliseconds.
    When only one value is specified, set the interval to that value. When two values are specified, set the regular (short) interval to the first value, and set the long interval to the second value. Note that the enabling/disabling of auto adjustment between the two intervals is handled by the respective flags. If the auto adjustment is disabled, it is equivalent to only specifying the first value, and then setting the interval to that value
  get-poll-rssi-interval-msecs
    Gets current interval between RSSI polls, in milliseconds.
  force-hi-perf-mode enabled|disabled
    Sets whether hi-perf mode is forced or left for normal operation.
  force-low-latency-mode enabled|disabled
    Sets whether low latency mode is forced or left for normal operation.
  network-suggestions-set-user-approved <package name> yes|no
    Sets whether network suggestions from the app is approved or not.
  network-suggestions-has-user-approved <package name>
    Queries whether network suggestions from the app is approved or not.
  imsi-protection-exemption-set-user-approved-for-carrier <carrier id> yes|no
    Sets whether Imsi protection exemption for carrier is approved or not
  imsi-protection-exemption-has-user-approved-for-carrier <carrier id>
    Queries whether Imsi protection exemption for carrier is approved or not
  imsi-protection-exemption-clear-user-approved-for-carrier <carrier id>
    Clear the user choice on Imsi protection exemption for carrier
  network-requests-remove-user-approved-access-points <package name>
    Removes all user approved network requests for the app.
  clear-user-disabled-networks
    Clears the user disabled networks list.
  send-link-probe
    Manually triggers a link probe.
  start-softap <ssid> (open|wpa2|wpa3|wpa3_transition|owe|owe_transition) <passphrase> [-b 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6] [-x] [-w 20|40|80|160|320] [-f <int> [<int>]]
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
    -w 20|40|80|160|320 - select the maximum channel bandwidth (MHz)
  stop-softap
    Stop softap (hotspot)
  force-softap-band enabled <int> | disabled
    Forces soft AP band to 2|5|6
  force-softap-channel enabled <int> | disabled [-w <maxBandwidth>]
    Sets whether soft AP channel is forced to <int> MHz [-w <maxBandwidth>]
        -w 0|20|40|80|160|320 - select the maximum channel bandwidth (MHz)
         Note: If the bandwidth option is not provided or set to 0, framework will set the maximum bandwidth to auto, allowing HAL to select the bandwidth
    or left for normal   operation.
  force-country-code enabled <two-letter code> | disabled 
    Sets country code to <two-letter code> or left for normal value
    or '00' for forcing to world mode country code
  set-wifi-watchdog enabled|disabled
    Sets whether wifi watchdog should trigger recovery
  get-wifi-watchdog
    Gets setting of wifi watchdog trigger recovery.
  settings-reset
    Initiates wifi settings reset
  allow-root-to-get-local-only-cmm enabled|disabled
    sets whether the shell running as root could use the local-only secondary STA
  add-request [-g] [-i] [-n] [-s] <ssid> open|owe|wpa2|wpa3 [<passphrase>] [-b <bssid>] [-d <band=2|5|6|60>]
    Add a network request with provided params
    Use 'network-requests-set-user-approved android yes' to pre-approve requests added via rooted shell (Not persisted)
    -g - Marks the following SSID as a glob pattern
    <ssid> - SSID of the network, or glob pattern if -g is present
    open|owe|wpa2|wpa3 - Security type of the network.
        - Use 'open' or 'owe' for networks with no passphrase
           - 'open' - Open networks (Most prevalent)
           - 'owe' - Enhanced open networks
        - Use 'wpa2' or 'wpa3' for networks with passphrase
           - 'wpa2' - WPA-2 PSK networks (Most prevalent)
           - 'wpa3' - WPA-3 PSK networks
    -b <bssid> - Set specific BSSID.
    -i Set internet capability.
    -d Specify the band of access point: 2, 5, 6, or 60
    -s No SSID provided, to be chosen by network selection.
    -n - Prevent auto-selection of BSSID and force it to be null so that the request matches all BSSIDs.
  remove-request <ssid>
    Remove a network request with provided SSID of the network
  remove-all-requests
    Removes all active requests added via shell
  list-requests
    Lists the requested networks added via shell
  network-requests-set-user-approved <package name> yes|no
    Sets whether network requests from the app is approved or not.
    Note: Only 1 such app can be approved from the shell at a time
  network-requests-has-user-approved <package name>
    Queries whether network requests from the app is approved or not.
    Note: This only returns whether the app was set via the 'network-requests-set-user-approved' shell command
  list-all-suggestions
    Lists all suggested networks on this device
  list-suggestions-from-app <package name>
    Lists the suggested networks from the app
  set-emergency-callback-mode enabled|disabled
    Sets whether Emergency Callback Mode (ECBM) is enabled.
    Equivalent to receiving the TelephonyManager.ACTION_EMERGENCY_CALLBACK_MODE_CHANGED broadcast.
  set-emergency-call-state enabled|disabled
    Sets whether we are in the middle of an emergency call.
Equivalent to receiving the TelephonyManager.ACTION_EMERGENCY_CALL_STATE_CHANGED broadcast.
  set-emergency-scan-request enabled|disabled
    Sets whether there is a emergency scan request in progress.
  network-suggestions-set-as-carrier-provider <packageName> yes|no
    Set the <packageName> work as carrier provider or not.
  is-network-suggestions-set-as-carrier-provider <packageName>
    Queries whether the <packageName> is working as carrier provider or not.
  remove-app-from-suggestion_database <packageName>
    Remove <packageName> from the suggestion database, all suggestions and user approval will be deleted, it is the same as uninstalling this app.
  trigger-recovery
    Trigger Wi-Fi subsystem restart.
  start-faking-scans
    Start faking scan results into the framework (configured with 'add-fake-scan'), stop with 'stop-faking-scans'.
  stop-faking-scans
    Stop faking scan results - started with 'start-faking-scans'.
  add-fake-scan [-x] <ssid> <bssid> <capabilities> <frequency> <dbm>
    Add a fake scan result to be used when enabled via `start-faking-scans'.
    Example WPA2: add-fake-scan fakeWpa2 80:01:02:03:04:05 "[WPA2-PSK-CCMP][RSN-PSK-CCMP][ESS]" 2412 -55
    Example WPA3: add-fake-scan fakeWpa3 80:01:02:03:04:06 "[RSN-SAE+FT/SAE-CCMP][ESS]" 2412 -55
    Example Open: add-fake-scan fakeOpen 80:01:02:03:04:07 "[ESS]" 2412 -55
    Example OWE: add-fake-scan fakeOwe 80:01:02:03:04:08 "[RSN-OWE-CCMP]" 2412 -55
    Example WPA2/WPA3 transition mode: add-fake-scan fakeWpa2t3 80:01:02:03:04:09 "[WPA2-PSK-CCMP][RSN-PSK+SAE-CCMP][ESS][MFPC]" 2412 -55
    Example Open/OWE transition mode: add-fake-scan fakeOpenOwe 80:01:02:03:04:0A "[RSN-OWE_TRANSITION-CCMP][ESS]" 2412 -55
    Example Passpoint: add-fake-scan fakePasspoint 80:01:02:03:04:0B "[WPA2-EAP/SHA1-CCMP][RSN-EAP/SHA1-CCMP][ESS][MFPR][MFPC][PASSPOINT]" 2412 -55
    -x - Specifies the SSID as hex digits instead of plain text
  reset-fake-scans
    Resets all fake scan results added by 'add-fake-scan'.
  enable-scanning enabled|disabled [-h]
    Sets whether all scanning should be enabled or disabled
    -h - Enable scanning for hidden networks.
  set-passpoint-enabled enabled|disabled
    Sets whether Passpoint should be enabled or disabled
  start-lohs <ssid> (open|wpa2|wpa3|wpa3_transition|owe|owe_transition) <passphrase> [-b 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6] [-x] [-w 20|40|80|160|320] [-f <int> [<int>]])
    Start local only softap (hotspot) with provided params
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
          Use '-f 2412 5745' to enable bridged dual lohs on 2412 and 5745
    -x - Specifies the SSID as hex digits instead of plain text (T and above)
    -w 20|40|80|160|320 - select the maximum bandwidth (MHz)
  stop-softap
    Stop softap (hotspot)
    Note: If the band option is not provided, 2.4GHz is the preferred band.
  stop-lohs
    Stop local only softap (hotspot)
  set-multi-internet-mode 0|1|2
    Sets Multi Internet use case mode. 0-disabled 1-dbs 2-multi ap
  set-pno-request <ssid> [-f <frequency>]
    Requests to include a non-quoted UTF-8 SSID in PNO scans
  clear-pno-request
    Clear the PNO scan request.
  set-pno-scan enabled|disabled
    Set the PNO scan enabled or disabled.
  start-dpp-enrollee-responder [-i <info>] [-c <curve>]
    Start DPP Enrollee responder mode.
    -i - Device Info to be used in DPP Bootstrapping URI
    -c - Cryptography Curve integer 1:p256v1, 2:s384r1, etc
  start-dpp-configurator-initiator <networkId> <netRole> <enrolleeURI>
    Start DPP Configurator Initiator mode.
    netRole - 0: STA, 1: AP
    enrolleeURI - Bootstrapping URI received from Enrollee
  stop-dpp
    Stop DPP session.
  set-ssid-charset <locale_language> <charset_name>
    Sets the SSID translation charset for the given locale language.
    Example: set-ssid-charset zh GBK
  clear-ssid-charsets
    Clears the SSID translation charsets set in set-ssid-charset.
  get-last-caller-info api_type
    Get the last caller information for a WifiManager.ApiType
  trigger-afc-location-update <longitude> <latitude> <height>
    Passes in longitude, latitude, and height values as arguments of type double for a fake location update to trigger framework logic to query the AFC server. The longitude and latitude pair is in decimal degrees and the height is the altitude in meters. The server URL needs to have been previously set with the configure-afc-server shell command.
    Example: trigger-afc-location-update 37.425056 -122.984157 3.043
  set-afc-channel-allowance -e <secs_until_expiry> [-f <low_freq>,<high_freq>,<psd>:...|none] [-c <operating_class>,<channel_cfi>,<max_eirp>:...|none]
    Sets the allowed AFC channels and frequencies.
    -e - Seconds until the availability expires.
    -f - Colon-separated list of available frequency info.
      Note: each frequency should contain 3 comma separated values, where the first is the low frequency (MHz), the second the high frequency (MHz), the third the max PSD (dBm per MHz). To set an empty frequency list, enter "none" in place of the list of allowed frequencies.
    -c - Colon-separated list of available channel info.
      Note: each channel should contain 3 comma separated values, where the first is the global operating class, the second the channel CFI, the third the max EIRP in dBm. To set an empty channel list, enter "none" in place of the list of allowed channels.
    Example: set-afc-channel-allowance -e 30 -c none -f 5925,6020,23:6020,6050,1
  configure-afc-server <url> [-r [<request property key> <request property value>] ...]
    Sets the server URL and request properties for the HTTP request which the AFC Client will query.
    -r - HTTP header fields that come in pairs of key and value which are added to the HTTP request. Must be an even number of arguments. If there is no -r option provided or no arguments provided after the -r option, then set the request properties to none in the request.
    Example: configure-afc-server https://testURL -r key1 value1 key2 value2
  set-ssid-roaming-mode <ssid> none|normal|aggressive [-x]
    Sets the roaming mode for the given SSID.
    -x - Specifies the SSID as hex digits instead of plain text.
    Example: set-ssid-roaming-mode test_ssid aggressive


```

#### adb shell cmd wifi start-softap 打开热点
```


  start-softap <ssid> (open|wpa2|wpa3|wpa3_transition|owe|owe_transition) <passphrase> [-b 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6] [-x] [-w 20|40|80|160|320] [-f <int> [<int>]]
  start-softap <ssid>【1.热点名称】 (open|wpa2|wpa3|wpa3_transition|owe|owe_transition)【2.认证方式】 <passphrase>【3.密码】 [-b【4. band频段】 2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6] [-x]【5.指定SSID为十六进制int值而非text】 [-w【6.bandwidth 信道带宽】 20|40|80|160|320] [-f【7.指定channel frequency信道频率下_可多选DBS 必须最后输入】 <int> [<int>]]
        


adb shell cmd wifi start-softap   321 wpa2 87654321 -b 5    ## 【 打开一个 ssid=321 的 wpa2密码87654321的 5G频段的热点】
 


-b 参数列表  【band频段】  【2|5|6|any|bridged|bridged_2_5|bridged_2_6|bridged_5_6】
        - Use '2' to select 2.4GHz band as the preferred band
        - Use '5' to select 5GHz band as the preferred band
        - Use '6' to select 6GHz band as the preferred band
        - Use 'any' to indicate no band preference
        - Use 'bridged' to indicate bridged AP which enables APs on both 2.4G + 5G
        - Use 'bridged_2_5' to indicate bridged AP which enables APs on both 2.4G + 5G
        - Use 'bridged_2_6' to indicate bridged AP which enables APs on both 2.4G + 6G
        - Use 'bridged_5_6' to indicate bridged AP which enables APs on both 5G + 6G
	
adb shell cmd wifi start-softap   321 open -b 2      ## 【 打开一个 ssid=321 的 open的 2G频段的热点】

adb shell cmd wifi start-softap   321 open -b 5      ## 【 打开一个 ssid=321 的 open的 5G频段的热点】

adb shell cmd wifi start-softap   321 open -b 6      ## 【 打开一个 ssid=321 的 open的 6G频段的热点】


adb shell cmd wifi start-softap   Wifi_2412 open -b 2  -f 2412 ## 【 打开一个 ssid= Wifi_2412 的 open的指定频段 2412 频段的热点】


adb shell cmd wifi start-softap   Wifi_5200 open -b 5  -f 5200  ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180 频段的热点】
adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180   ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180 频段 的热点】

-w 参数列表   【bandwidth 信道带宽】  【20|40|80|160|320】
case 0:  apMaxBandWidth = SoftApInfo.CHANNEL_WIDTH_AUTO;  break;
case 20: apMaxBandWidth = SoftApInfo.CHANNEL_WIDTH_20MHZ; break;
case 40: apMaxBandWidth = SoftApInfo.CHANNEL_WIDTH_40MHZ; break;
case 80: apMaxBandWidth = SoftApInfo.CHANNEL_WIDTH_80MHZ; break;
case 160:apMaxBandWidth = SoftApInfo.CHANNEL_WIDTH_160MHZ;break;
case 320:apMaxBandWidth = SoftApInfo.CHANNEL_WIDTH_320MHZ;break;

adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180 -w 0      ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180  频段 WIDTH_AUTO自动带宽 的热点】
adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180 -w 20     ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180  频段 WIDTH_20MHZ   带宽 的热点】
adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180 -w 40     ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180  频段 WIDTH_40MHZ   带宽 的热点】
adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180 -w 80     ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180  频段 WIDTH_80MHZ   带宽 的热点】
adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180 -w 160    ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180  频段 WIDTH_160MHZ  带宽 的热点】
adb shell cmd wifi start-softap   Wifi_5180 open -b 5  -f 5180 -w 320    ## 【 打开一个 ssid=Wifi_5180 的 open的指定频段 5180  频段 WIDTH_320MHZ  带宽 的热点】







```

#### adb shell cmd wifi force-softap-channel 设置热点频段
```

adb root && adb shell cmd wifi force-softap-channel enabled 5745


force-softap-channel enabled <int> | disabled [-w <maxBandwidth>]
    1.<int>   当前的频率channel
    2.-w 0|20|40|80|160|320 - select the maximum channel bandwidth (MHz)
        Note: If the bandwidth option is not provided or set to 0, framework will set the maximum bandwidth to auto, allowing HAL to select the bandwidth

adb root && adb shell cmd wifi force-softap-channel enabled 5745 -w 80
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤ 
channel: 149 band: 2 maximum channel bandwidth: 80
2G freq: [2412, 2417, 2422, 2427, 2432, 2437, 2442, 2447, 2452, 2457, 2462, 2467, 2472]
5G freq: [5745, 5765, 5785, 5805, 5825]
5G DFS: [5180, 5200, 5220, 5240, 5260, 5280, 5300, 5320]
6G freq: []
60G freq: []
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧



```



####  adb shell cmd wifi get-allowed-channel 获取可用频率

```
 adb shell cmd wifi get-allowed-channel [-b 1|6|7|8|15|16|31]
 
     -b - set the band in which channels are allowed
       '1'  - band 2.4 GHz
       '6'  - band 5 GHz with DFS channels
       '7'  - band 2.4 and 5 GHz with DFS channels
       '8'  - band 6 GHz
       '15' - band 2.4, 5, and 6 GHz with DFS channels
       '16' - band 60 GHz
       '31' - band 2.4, 5, 6 and 60 GHz with DFS channels
 

 adb shell cmd wifi get-allowed-channel 
 
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤ 
Allowed ch in STA mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472 5180 5200 5220 5240 5260 5280 5300 5320 5745 5765 5785 5805 5825
Allowed ch in SAP mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472 5745 5765 5785 5805 5825
Allowed ch in WiFi-Direct GC mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472 5180 5200 5220 5240 5260 5280 5300 5320 5745 5765 5785 5805 5825
Allowed ch in WiFi-Direct GO mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472 5745 5765 5785 5805 5825
Allowed ch in WiFi-Aware mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472 5745 5765 5785 5805 5825
Allowed ch in TDLS mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472 5180 5200 5220 5240 5260 5280 5300 5320 5745 5765 5785 5805 5825
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

 adb shell cmd wifi get-allowed-channel  -b 1
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Allowed ch in STA mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472
Allowed ch in SAP mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472
Allowed ch in WiFi-Direct GC mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472
Allowed ch in WiFi-Direct GO mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472
Allowed ch in WiFi-Aware mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472
Allowed ch in TDLS mode:
2412 2417 2422 2427 2432 2437 2442 2447 2452 2457 2462 2467 2472
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


####  adb shell cmd wifi get-country-code 获取当前国家码
```
 adb shell cmd wifi get-country-code
Wifi Country Code = CN

```





####  adb shell cmd wifi force-country-code enabled JP 更新国家码
```

adb shell cmd wifi force-country-code enabled JP 
 

adb shell cmd wifi get-country-code
Wifi Country Code = JP 


```


####  adb shell cmd wifi set-scan-always-available disabled 关闭wifi扫描 
```
Wifi Scaning 开关 ,wifi off 情况下 仍然支持扫描的feature开关
对应 Settings >  Location > Location Services > Wi-Fi Scanning [on|off]

adb shell cmd wifi set-scan-always-available enabled
adb shell cmd wifi set-scan-always-available disabled



```


####  adb shell cmd wifi set-wifi-enabled enabled 打开WIFI模块
```
adb shell cmd wifi set-wifi-enabled enabled|disabled

adb shell cmd wifi set-wifi-enabled enabled 
adb shell cmd wifi set-wifi-enabled disabled 

// 打开Wifi  触发扫描   打印扫描结果

adb shell cmd wifi set-wifi-enabled enabled  && adb shell cmd wifi  start-scan && adb shell cmd wifi    list-scan-results

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤

    BSSID              Frequency      RSSI           Age(sec)     SSID                                 Flags
  6c:cd:d6:f0:fd:cb       2457    -61(0:-62/1:-67)     3.300    NETGEAR11                         [WPA2-PSK-CCMP][RSN-PSK-CCMP][ESS][WPS]
  c2:a7:31:c6:fe:fd       6115    -65(0:-66/1:-71)     0.710    TP-Link_FF00_6G                   [RSN-SAE-CCMP][ESS][MFPR][MFPC]
  ba:a7:31:c6:fe:fc       5240    -50(0:-53/1:-53)     2.619    TP-Link_FF00                      [WPA2-PSK-CCMP][RSN-PSK-CCMP][ESS][WPS]
  aa:bb:aa:cc:ss:65       5300    -52(0:-61/1:-53)     2.348    xxx-internet                      [ESS]
  cd:aa:ss:dd:54:81       2437    -66(0:-72/1:-67)     3.622    xxx-enterwifi                     [WPA2-EAP/SHA1-CCMP][RSN-EAP/SHA1-CCMP][ESS]
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```


####  adb shell cmd wifi status 查看当前wifi状态
```

adb shell cmd wifi status  打印WIFI模块状态-未连接网络

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Wifi is enabled
Wifi scanning is only available when wifi is enabled
==== ClientModeManager instance: ConcreteClientModeManager{id=408299759 iface=wlan0 role=ROLE_CLIENT_PRIMARY} ====
Wifi is not connected  当前没有连接网络
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```

```

adb shell cmd wifi status  打印WIFI模块状态-已连接网络


╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Wifi is enabled
Wifi scanning is only available when wifi is enabled
==== ClientModeManager instance: ConcreteClientModeManager{id=408299759 iface=wlan0 role=ROLE_CLIENT_PRIMARY} ====
Wifi is connected to "CMCC_2.4G"
WifiInfo: SSID: "CMCC_2.4G", BSSID: 16:d8:64:f4:d5:19, MAC: 9a:01:93:78:be:46, IP: /192.168.1.106, Security type: 2, Supplicant state: COMPLETED, Wi-Fi standard: 11ax, RSSI: -26, Link speed: 516Mbps, Tx Link speed: 516Mbps, Max Supported Tx Link speed: 573Mbps, Rx Link speed: 458Mbps, Max Supported Rx Link speed: 573Mbps, Frequency: 2437MHz, Net ID: 0, Metered hint: false, score: 60, isUsable: true, CarrierMerged: false, SubscriptionId: -1, IsPrimary: 1, Trusted: true, Restricted: false, Ephemeral: false, OEM paid: false, OEM private: false, OSU AP: false, FQDN: <none>, Provider friendly name: <none>, Requesting package name: <none>"CMCC_2.4G"wpa2-pskMLO Information: , Is TID-To-Link negotiation supported by the AP: false, AP MLD Address: <none>, AP MLO Link Id: <none>, AP MLO Affiliated links: <none>, Vendor Data: <none>
successfulTxPackets: 1083
successfulTxPacketsPerSecond: 6.42797061153898
retriedTxPackets: 436
retriedTxPacketsPerSecond: 4.1902650236307615
lostTxPackets: 0
lostTxPacketsPerSecond: 0.0
successfulRxPackets: 5604
successfulRxPacketsPerSecond: 0.03681659457804908
NetworkCapabilities: [ Transports: WIFI Capabilities: NOT_METERED&INTERNET&NOT_RESTRICTED&TRUSTED&NOT_VPN&NOT_ROAMING&FOREGROUND&NOT_CONGESTED&NOT_SUSPENDED&PARTIAL_CONNECTIVITY&NOT_VCN_MANAGED&NOT_BANDWIDTH_CONSTRAINED LinkUpBandwidth>=12000Kbps LinkDnBandwidth>=29790Kbps TransportInfo: <SSID: <unknown ssid>, BSSID: 02:00:00:00:00:00, MAC: 02:00:00:00:00:00, IP: /192.168.1.106, Security type: 2, Supplicant state: COMPLETED, Wi-Fi standard: 11ax, RSSI: -27, Link speed: 413Mbps, Tx Link speed: 413Mbps, Max Supported Tx Link speed: 573Mbps, Rx Link speed: 516Mbps, Max Supported Rx Link speed: 573Mbps, Frequency: 2437MHz, Net ID: -1, Metered hint: false, score: 60, isUsable: true, CarrierMerged: false, SubscriptionId: -1, IsPrimary: 1, Trusted: true, Restricted: false, Ephemeral: false, OEM paid: false, OEM private: false, OSU AP: false, FQDN: <none>, Provider friendly name: <none>, Requesting package name: <none><none>MLO Information: , Is TID-To-Link negotiation supported by the AP: false, AP MLD Address: <none>, AP MLO Link Id: <none>, AP MLO Affiliated links: <none>, Vendor Data: <none>> SignalStrength: -27 AdminUids: [1000] SSID: "CMCC_2.4G" UnderlyingNetworks: Null]
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```


####  adb shell cmd wifi is-verbose-logging  查看wifi详情开关是否打开
```

 adb shell cmd wifi is-verbose-logging
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
enabled
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```

####  adb shell cmd wifi   set-verbose-logging enabled  打开wifi详情开关
```

adb shell cmd wifi set-verbose-logging enabled|disabled

adb shell cmd wifi set-verbose-logging enabled
adb shell cmd wifi set-verbose-logging disabled

adb shell cmd wifi set-verbose-logging enabled
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤

╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


####  adb shell cmd wifi list-suggestions 打开当前收到的连接网络建议
 
```

 adb shell cmd wifi list-suggestions 

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
No suggestions on this device
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


####  adb shell cmd wifi   get-coex-cell-channels

```

adb shell cmd wifi   get-coex-cell-channels

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Cell channels: []
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```




####  adb shell cmd wifi  set-one-shot-screen-on-delay-ms 1000 设置亮屏扫描延迟时间

```

// 每次亮屏 都会触发扫描事件
// 设置亮屏扫描延迟时间
adb shell cmd wifi  set-one-shot-screen-on-delay-ms 1000 

    set-one-shot-screen-on-delay-ms <delayMs>
         set the delay for the next screen-on connectivity scan in milliseconds.

adb shell cmd wifi  set-one-shot-screen-on-delay-ms 1000 
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤

╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```



####  adb shell cmd wifi set-ipreach-disconnect disabled 开关CMD_IP_REACHABILITY_LOST事件触发断连
```
adb shell cmd wifi  set-ipreach-disconnect enabled      // CMD_IP_REACHABILITY_LOST事件发生保持连接
adb shell cmd wifi  set-ipreach-disconnect disabled     // CMD_IP_REACHABILITY_LOST事件发生不断开连接

adb shell cmd wifi  set-ipreach-disconnect enabled|disabled
    Sets whether CMD_IP_REACHABILITY_LOST events should trigger disconnects.


adb shell cmd wifi  set-ipreach-disconnect disabled
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤

╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```

####  adb shell cmd wifi   get-ipreach-disconnect

```

  adb shell cmd wifi   get-ipreach-disconnect       // 查看 开关CMD_IP_REACHABILITY_LOST事件触发断连设置的值

  adb shell cmd wifi get-ipreach-disconnect
    Gets setting of CMD_IP_REACHABILITY_LOST events triggering disconnects.

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
IPREACH_DISCONNECT state is false        // 当前feature是关闭的
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧



adb shell cmd wifi  set-ipreach-disconnect enabled  &&  adb shell cmd wifi get-ipreach-disconnect

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
IPREACH_DISCONNECT state is true      // 当前feature是打开的
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```



####  adb shell cmd wifi  get-overlay-config-values 查看WIFI的配置文件
```

adb shell cmd wifi  get-overlay-config-values

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Dump of WifiResourceCache
WifiResourceCache - resource value Begin ----
Resource Name: config_wifiDisableFirmwareRoamingInIdleMode, value: false
Resource Name: config_wifiFrameworkSoftApShutDownIdleInstanceInBridgedModeTimeoutMillisecond, value: 300000
Resource Name: config_wifi6ghzSupport, value: false
Resource Name: config_wifiSoftapPassphraseAsciiEncodableCheck, value: false
Resource Name: config_wifiSoftapHeSuBeamformeeSupported, value: true
Resource Name: config_wifiSaeUpgradeOffloadEnabled, value: false
Resource Name: config_wifiSaeUpgradeEnabled, value: true
Resource Name: config_wifi60ghzSupport, value: false
Resource Name: config_wifiFrameworkSoftApDisableBridgedModeShutdownIdleInstanceWhenCharging, value: false
Resource Name: config_wifiWepDeprecated, value: false
Resource Name: config_wifiInterfaceAddedSelfRecoveryEnabled, value: false
Resource Name: config_wifiLowConnectedScoreThresholdToTriggerScanForMbb, value: 55
Resource Name: config_wifiPollRssiLongIntervalMilliseconds, value: 6000
Resource Name: config_wifiWpaPersonalDeprecated, value: false
Resource Name: config_wifiFlushAnqpCacheOnWifiToggleOffEvent, value: true
Resource Name: config_wifiSoftap24ghzSupported, value: true
Resource Name: config_wifiSaeH2eSupported, value: true
Resource Name: config_wifiSoftapOweTransitionSupported, value: false
Resource Name: config_wifiSoftapIeee80211axSupported, value: true
Resource Name: config_wifi_p2p_mac_randomization_supported, value: true
Resource Name: config_wifiSoftap2gChannelList, value: 1-11
Resource Name: config_wifiAdjustPollRssiIntervalEnabled, value: false
Resource Name: config_wifiAfcServerUrlsForCountry, value: []
Resource Name: config_wifiConfigurationWifiRunnerThresholdInMs, value: 100
Resource Name: config_wifiSoftapAutoAppendLowerBandsToBandConfigurationEnabled, value: true
Resource Name: config_wifi_softap_ieee80211ac_supported, value: true
Resource Name: config_wifiDriverWorldModeCountryCode, value: 00
Resource Name: config_wifiSoftap5gChannelList, value: 36-161
Resource Name: config_wifiWepAllowedControlSupported, value: true
Resource Name: config_wifiAllowInsecureEnterpriseConfigurationsForSettingsAndSUW, value: true
Resource Name: config_wifi_turn_off_during_emergency_call, value: false
Resource Name: config_wifiSoftapHeSuBeamformerSupported, value: true
Resource Name: config_wifi_softap_acs_supported, value: true
Resource Name: config_wifiEapFailureConfig, value: [1915, 16385, 0, 3, 1]
Resource Name: config_wifiSoftap5ghzSupported, value: true
Resource Name: config_wifiClientModeImplNumLogRecs, value: 100
Resource Name: config_wifiSoftapMacAddressCustomizationSupported, value: true
Resource Name: config_wifi_revert_country_code_on_cellular_loss, value: true
Resource Name: config_wifi_connected_mac_randomization_supported, value: true
Resource Name: config_wifi_softap_sae_supported, value: true
Resource Name: config_wifiStaWithBridgedSoftApConcurrencySupported, value: false
Resource Name: config_wifiFrameworkSoftApShutDownTimeoutMilliseconds, value: 600000
Resource Name: config_wifiAfcSupported, value: false
Resource Name: config_wifiPollRssiIntervalMilliseconds, value: 3000
Resource Name: config_wifiHardwareSoftapMaxClientCount, value: 10
Resource Name: config_wifiSaveFactoryMacToWifiConfigStore, value: true
Resource Name: config_wifi_ap_mac_randomization_supported, value: true
Resource Name: config_wifi24ghzSupport, value: true
Resource Name: config_wifiBridgedSoftApSupported, value: false
Resource Name: config_wifiForceDisableMacRandomizationSsidPrefixList, value: []
Resource Name: config_wifiOweUpgradeEnabled, value: true
Resource Name: config_wifiSoftapAcsIncludeDfs, value: false
Resource Name: config_wifiSoftapHeMuBeamformerSupported, value: false
Resource Name: config_wifiVerboseLoggingAlwaysOnLevel, value: 0
Resource Name: config_wifiSofapClientForceDisconnectSupported, value: true
Resource Name: config_wifiSoftapIeee80211beSupported, value: false
Resource Name: config_wifi5ghzSupport, value: true
Resource Name: config_wifiDriverSupportedNl80211RegChangedEvent, value: false
Resource Name: config_wifiSoftapHeTwtSupported, value: false
Resource Name: config_wifiStaDynamicCountryCodeUpdateSupported, value: false
Resource Name: config_wifiScanHiddenNetworksScanOnlyMode, value: false
Resource Name: config_wifiSoftapOweSupported, value: false
Resource Name: config_wifiD2dAllowedControlSupportedWhenInfraStaDisabled, value: false
WifiResourceCache - resource value End ----
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


####  adb shell cmd wifi  force-overlay-config-value xxx 更改配置文件的值
```

adb shell cmd wifi  force-overlay-config-value bool|integer|string|integer-array|string-array <overlayName> enabled|disabled <configValue>


force-overlay-config-value bool|integer|string|integer-array|string-array <overlayName> enabled|disabled <configValue>
    Force overlay to a specified value.
    bool|integer|string|integer-array|string-array - specified the type of the overlay
    <overlayName> - name of the overlay whose value is overridden.
    enabled|disabled: enable the override or disable it and revert to using the built-in value.
    <configValue> - override value of the overlay.Must match the overlay type

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤

╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```


####  adb shell cmd wifi get-softap-supported-features 获取热点支持的Feature

```

adb shell cmd wifi get-softap-supported-features


adb shell cmd wifi get-softap-supported-features
    Gets softap supported features. Will print 'wifi_softap_acs_supported'
    and/or 'wifi_softap_wpa3_sae_supported',
    and/or 'wifi_softap_bridged_ap_supported',
    and/or 'wifi_softap_bridged_ap_with_sta_supported',
    each on a separate line

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
wifi_softap_acs_supported
wifi_softap_wpa3_sae_supported
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```



####  adb shell cmd wifi  send-link-probe 触发一个link probe
```

 adb shell cmd wifi  send-link-probe     //   Manually triggers a link probe.

╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
Link probe succeeded after 31 ms
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```

####  adb shell cmd wifi get-wifi-watchdog
```

  set-wifi-watchdog enabled|disabled
    Sets whether wifi watchdog should trigger recovery
  get-wifi-watchdog
    Gets setting of wifi watchdog trigger recovery.

adb shell cmd wifi set-wifi-watchdog enabled
adb shell cmd wifi set-wifi-watchdog disabled
adb shell cmd wifi get-wifi-watchdog

adb shell cmd wifi set-wifi-watchdog enabled && adb shell cmd wifi get-wifi-watchdog
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
wifi watchdog state is true
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧



adb shell cmd wifi set-wifi-watchdog disabled && adb shell cmd wifi get-wifi-watchdog
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
wifi watchdog state is false
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧

```



####  adb shell cmd wifi 
```

 adb shell cmd wifi 
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```

####  adb shell cmd wifi 
```

 adb shell cmd wifi 
╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤╤
╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧╧
```



## X




## Y


## Z





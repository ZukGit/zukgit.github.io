---
layout: post
title: adb_shell_dumpsys打印系统服务
category: 系统
tags: Android
keywords: dumpsys android
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

### package


```
adb shell dumpsys package > package.txt
```

**描述**

```
打印关于包相关的信息包括
1.libraries
2.features
3.resolvers
4.activity
5.service
6.receiver
7.content
8.permission
9.preferred
10.packages
11.queries
12.shared-users
13.providers
14.messages
15.verifiers
16.domain-verifier
17.installs
18.frozen
19.volumes
20.dexopt
21.compiler-stats
22.changes
23.service-permissions
24.known-packages
25.timeouts

```



**dump文件**

```
/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
void dump(FileDescriptor fd, PrintWriter pw, String[] args)
```

**命令集合**

```
 adb shell dumpsys package  -f --checkin --all-components
```





**提示信息**

```
adb shell dumpsys package -h

Package manager dump options:
  [-h] [-f] [--checkin] [--all-components] [cmd] ...
    --checkin: dump for a checkin    【adb shell dumpsys package --checkin】
    -f: print details of intent filters
    -h: print this help
    --all-components: include all component names in package dump
  cmd may be one of:
    apex: list active APEXes and APEX session state
    l[ibraries]: list known shared libraries
    f[eatures]: list device features
    k[eysets]: print known keysets
    r[esolvers] [activity|service|receiver|content]: dump intent resolvers
    perm[issions]: dump permissions
    permission [name ...]: dump declaration and use of given permission
    pref[erred]: print preferred package settings
    preferred-xml [--full]: print preferred package settings as xml
    prov[iders]: dump content providers
    p[ackages]: dump installed packages
    q[ueries]: dump app queryability calculations
    s[hared-users]: dump shared user IDs
    m[essages]: print collected runtime messages
    v[erifiers]: print package verifier info
    d[omain-preferred-apps]: print domains preferred apps
    i[ntent-filter-verifiers]|ifv: print intent filter verifier info
    version: print database version info
    write: write current settings now
    installs: details about install sessions
    
【检测给定APP是否有指定权限__打电话】
【 adb shell dumpsys package  -f --checkin --all-components   check-permission   android.permission.CALL_PRIVILEGED  com.google.android.permissioncontroller
】
    check-permission <permission> <package> [<user>]: does pkg hold perm? 
    
    dexopt: dump dexopt state
    compiler-stats: dump compiler statistics
    service-permissions: dump permissions required by services
    <package.name>: info about given package
```





### Log层次

#### Database versions:
#### Verifiers:
#### Intent Filter Verifier:
#### Libraries:
#### Features:
#### Activity Resolver Table:
#### Receiver Resolver Table:
#### Service Resolver Table:
#### Provider Resolver Table:
#### Preferred Activities User 0:
#### App verification status:
#### App linkages for user 0:
#### Permissions:
#### AppOp Permissions:
#### Registered ContentProviders:
#### ContentProvider Authorities:
#### Key Set Manager:
#### Packages:
#### Queries:
#### Shared users:
#### Package Changes:
#### Frozen packages:
#### Loaded volumes:
#### Service permissions:
#### Dexopt state:
#### Compiler stats:
#### Settings parse messages:
#### Active install sessions:
#### Finalized install sessions:
#### Historical install sessions:
#### Legacy install sessions:
#### APEX session state:
#### Active APEX packages:
#### Inactive APEX packages:
#### Factory APEX packages:




### 辅助类

#### DumpState.java

```
/frameworks/base/services/core/java/com/android/server/pm/DumpState.java

public final class DumpState {
    private int mTypes;   // DUMP_xx 比特位集合 

    private int mOptions; // OPTION_xx 比特位集合 

    private boolean mTitlePrinted;
    private boolean mFullPreferred;
    private boolean mCheckIn;
    private boolean mBrief;

    private String mTargetPackageName;   // 

    private SharedUserSetting mSharedUser;
	
    public static final int DUMP_LIBS = 1 << 0;
    public static final int DUMP_FEATURES = 1 << 1;
    public static final int DUMP_ACTIVITY_RESOLVERS = 1 << 2;
    public static final int DUMP_SERVICE_RESOLVERS = 1 << 3;
    public static final int DUMP_RECEIVER_RESOLVERS = 1 << 4;
    public static final int DUMP_CONTENT_RESOLVERS = 1 << 5;
    public static final int DUMP_PERMISSIONS = 1 << 6;
    public static final int DUMP_PACKAGES = 1 << 7;
    public static final int DUMP_SHARED_USERS = 1 << 8;
    public static final int DUMP_MESSAGES = 1 << 9;
    public static final int DUMP_PROVIDERS = 1 << 10;
    public static final int DUMP_VERIFIERS = 1 << 11;
    public static final int DUMP_PREFERRED = 1 << 12;
    public static final int DUMP_PREFERRED_XML = 1 << 13;
    public static final int DUMP_KEYSETS = 1 << 14;
    public static final int DUMP_VERSION = 1 << 15;
    public static final int DUMP_INSTALLS = 1 << 16;
    public static final int DUMP_DOMAIN_VERIFIER = 1 << 17;
    public static final int DUMP_DOMAIN_PREFERRED = 1 << 18;
    public static final int DUMP_FROZEN = 1 << 19;
    public static final int DUMP_DEXOPT = 1 << 20;
    public static final int DUMP_COMPILER_STATS = 1 << 21;
    public static final int DUMP_CHANGES = 1 << 22;
    public static final int DUMP_VOLUMES = 1 << 23;
    public static final int DUMP_SERVICE_PERMISSIONS = 1 << 24;
    public static final int DUMP_APEX = 1 << 25;
    public static final int DUMP_QUERIES = 1 << 26;
    public static final int DUMP_KNOWN_PACKAGES = 1 << 27;
    public static final int DUMP_PER_UID_READ_TIMEOUTS = 1 << 28;
    public static final int DUMP_SNAPSHOT_STATISTICS = 1 << 29;

    public static final int OPTION_SHOW_FILTERS = 1 << 0;
    public static final int OPTION_DUMP_ALL_COMPONENTS = 1 << 1;
    public static final int OPTION_SKIP_PERMISSIONS = 1 << 2;

	}
	

```

#### SharedUserSetting

```
frameworks/base/services/core/java/com/android/server/pm/SharedUserSetting.java
/**
 * Settings data for a particular shared user ID we know about.
 */
public final class SharedUserSetting extends SettingBase {
    final String name;

    int userId;

    // flags that are associated with this uid, regardless of any package flags
    int uidFlags;
    int uidPrivateFlags;

    // The lowest targetSdkVersion of all apps in the sharedUserSetting, used to assign seinfo so
    // that all apps within the sharedUser run in the same selinux context.
    int seInfoTargetSdkVersion;

    final ArraySet<PackageSetting> packages;

    final PackageSignatures signatures = new PackageSignatures();
    Boolean signaturesChanged;

    final ArrayMap<String, ParsedProcess> processes;

    /**
     * Snapshot support.
     */
    private final SnapshotCache<SharedUserSetting> mSnapshot;
    }
```



## Q


## R


## S


## T


## U


## V


## W


## X


## Y


## Z








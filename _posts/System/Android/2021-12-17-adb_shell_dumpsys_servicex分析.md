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



**dump文件**

```
/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
void dump(FileDescriptor fd, PrintWriter pw, String[] args)
```

```
adb shell dumpsys package > package.txt
```

**描述**

#### Database versions:  【HeadOneTag】 
```
打印关于包相关的信息包括

Database versions:  【HeadOneTag】  
  Internal:
    sdkVersion=30 databaseVersion=3 
    fingerprint=apple/apple_cmcc/apple:11/RRK3X/mp1tc2spV131:userdebug/intcfg,test-keys 
  External:
    sdkVersion=30 databaseVersion=3 
    fingerprint=apple/apple_cmcc/apple:11/RRK3X.10/mp1tc2spV16:userdebug/intcfg,test-keys 

【PackageManagerService->
final Settings mSettings-> 
mSettings.dumpVersionLPr(new IndentingPrintWriter(pw, "  "));】
/frameworks/base/services/core/java/com/android/server/pm/Settings.java

   void dumpVersionLPr(IndentingPrintWriter pw) {
        pw.increaseIndent();
        for (int i= 0; i < mVersion.size(); i++) {
            final String volumeUuid = mVersion.keyAt(i);
            final VersionInfo ver = mVersion.valueAt(i);
            if (Objects.equals(StorageManager.UUID_PRIVATE_INTERNAL, volumeUuid)) {
                pw.println("Internal:");
            } else if (Objects.equals(StorageManager.UUID_PRIMARY_PHYSICAL, volumeUuid)) {
                pw.println("External:");
            } else {
                pw.println("UUID " + volumeUuid + ":");
            }
            pw.increaseIndent();
            pw.printPair("sdkVersion", ver.sdkVersion);
            pw.printPair("databaseVersion", ver.databaseVersion);
            pw.println();
            pw.printPair("fingerprint", ver.fingerprint);
            pw.println();
            pw.decreaseIndent();
        }
        pw.decreaseIndent();
    }
    
```


#### Verifiers:  【HeadOneTag】
```
  
Verifiers:  【HeadOneTag】
  Required: com.android.vending (uid=10215)
  
  【PackageManagerService->
   final @Nullable String mRequiredVerifierPackage;】
/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
pw.println("Verifiers:");
pw.print("  Required: ");
pw.print(requiredVerifierPackage);
pw.print(" (uid=");
pw.print(getPackageUid(requiredVerifierPackage,MATCH_DEBUG_TRIAGED_MISSING,UserHandle.USER_SYSTEM));pw.println(")");
      
  
```


####  Intent Filter Verifier:  【HeadOneTag】

```

Intent Filter Verifier:  【HeadOneTag】
  Using: com.google.android.gms (uid=10187)
  
  
```

#### Libraries:  【HeadOneTag】

```
Libraries:  【HeadOneTag】
  com.mediatek.ims.rcsua ->  (jar) /system/framework/com.mediatek.ims.rcsua.jar
  com.vzw.apnlib ->  (jar) /product/app/VzwAPNLibWs/VzwAPNLibWs.apk

【PackageManagerService->
// Currently known shared libraries.
final WatchedArrayMap<String, WatchedLongSparseArray<SharedLibraryInfo>> mSharedLibraries = new WatchedArrayMap<>();】


/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java

                    final int numSharedLibraries = mSharedLibraries.size();
                    for (int index = 0; index < numSharedLibraries; index++) {
                        final String libName = mSharedLibraries.keyAt(index);
                        final WatchedLongSparseArray<SharedLibraryInfo> versionedLib =
                                mSharedLibraries.get(libName);
                        if (versionedLib == null) {
                            continue;
                        }
                        final int versionCount = versionedLib.size();
                        for (int i = 0; i < versionCount; i++) {
                            SharedLibraryInfo libraryInfo = versionedLib.valueAt(i);
                            if (!checkin) {
                                if (!printedHeader) {
                                    if (dumpState.onTitlePrinted()) {
                                        pw.println();
                                    }
                                    pw.println("Libraries:");
                                    printedHeader = true;
                                }
                                pw.print("  ");
                            } else {
                                pw.print("lib,");
                            }
                            pw.print(libraryInfo.getName());
                            if (libraryInfo.isStatic()) {
                                pw.print(" version=" + libraryInfo.getLongVersion());
                            }
                            if (!checkin) {
                                pw.print(" -> ");
                            }
                            if (libraryInfo.getPath() != null) {
                                if (libraryInfo.isNative()) {
                                    pw.print(" (so) ");
                                } else {
                                    pw.print(" (jar) ");
                                }
                                pw.print(libraryInfo.getPath());
                            } else {
                                pw.print(" (apk) ");
                                pw.print(libraryInfo.getPackageName());
                            }
                            pw.println();
                        }
                    }
                    break;
                    
```



#### Features:  【HeadOneTag】

```
Features:  【HeadOneTag】
  android.hardware.sensor.proximity
  android.hardware.sensor.accelerometer
  android.hardware.telephony
  android.software.file_based_encryption
  
  
【PackageManagerService->
 final ArrayMap<String, FeatureInfo> mAvailableFeatures;】
  
/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
              if (!checkin) {
                pw.println("Features:");
            }

            synchronized (mAvailableFeatures) {
                for (FeatureInfo feat : mAvailableFeatures.values()) {
                    if (checkin) {
                        pw.print("feat,");
                        pw.print(feat.name);
                        pw.print(",");
                        pw.println(feat.version);
                    } else {
                        pw.print("  ");
                        pw.print(feat.name);
                        if (feat.version > 0) {
                            pw.print(" version=");
                            pw.print(feat.version);
                        }
                        pw.println();
                    }
                }
            }
```



#### Activity Resolver Table:  【HeadOneTag】

```
Activity Resolver Table:  【HeadOneTag】
  Full MIME Types:
  Base MIME Types:
  Wild MIME Types:
  Schemes:
  Non-Data Actions:
  MIME Typed Actions:
~~~~~~~~~~~~~~~~~~~~~~~~~~~
----------------- 
Activity Resolver Table:
  Full MIME Types:
      application/pkix-cert:
        8e2adf4 com.android.certinstaller/.CertInstallerMain
      x-mixmedia/*:
        1dc4927 com.android.bluetooth/.opp.BluetoothOppLauncherActivity
      vnd.android.cursor.dir/raw_contact:
        bf068fa com.google.android.contacts/com.android.contacts.activities.PeopleActivity
        6a69614 com.google.android.contacts/com.android.contacts.activities.CompactContactEditorActivity
        51facb2 com.google.android.contacts/com.google.android.apps.contacts.editorlite.ContactsEditorlite
  Base MIME Types:
              vnd.android.document:
        5dd28e7 com.google.android.apps.nbu.files/.gateway.storagevolume.StorageVolumeActivity
        e639d0 com.google.android.documentsui/com.android.documentsui.files.FilesActivity
        e639d0 com.google.android.documentsui/com.android.documentsui.files.FilesActivity
        496d79c com.android.settings/.Settings$PublicVolumeSettingsActivity
        
----------------- 
  Wild MIME Types:
        *:
        dc4374e com.android.nfc/.BeamShareActivity
        680bf62 com.google.android.gm/.AutoSendActivity
        
        text:
        f36b732 com.google.android.apps.nbu.files/.gateway.share.ShareIntentGatewayHandler
        f36b732 com.google.android.apps.nbu.files/.gateway.share.ShareIntentGatewayHandler
      audio:
        fbbc496 com.google.android.apps.youtube.music/.audiopreview.AudioPreviewPlayerActivity
        40c3078 com.netease.cloudmusic/.activity.RedirectActivity
        
-----------------    
  Schemes:
    Schemes:
      satispay:
        74d16f5 com.satispay.customer/.appdata.satispayintent.SchemeActivity2
        642a7df com.satispay.customer/.payment.PayStoreActivity
      assistant-handoff:
        ef37743 com.google.android.googlequicksearchbox/com.google.android.apps.gsa.assistant.handoff.AssistantHandoffActivity
        13faffd com.google.android.googlequicksearchbox/com.google.android.apps.gsa.assistant.handoff.BrowserReturnActivity
      mobiledataplan:
        c9b40b9 com.google.android.gms/.mobiledataplan.ui.MobileDataPlanSettingsActivity
      qwallet101881483:
        7ab2bc4 com.tencent.tmgp.pkbydr/.CallbackActivity
-----------------  
    Non-Data Actions: 【不需要数据的Action 】
      android.settings.FINGERPRINT_SETUP:
        69de535 com.android.settings/.biometrics.fingerprint.SetupFingerprintEnrollIntroduction
      .preference.TalkBackSelectorMenuPreferencesActivity:
        86eb3aa com.google.android.marvin.talkback/com.google.android.accessibility.talkback.preference.TalkBackSelectorMenuPreferencesActivity
      com.google.android.gms.autofill.ACTION_SETTINGS:
        d5cd522 com.google.android.gms/.autofill.ui.AutofillSettingsActivity
      android.net.wifi.PICK_WIFI_NETWORK:
        1e7cc17 com.google.android.setupwizard/.network.WifiActivity
        64d45da com.android.settings/.wifi.WifiPickerActivity
-----------------   
  MIME Typed Actions: 【需要携带Data的Action】
      com.android.camera.action.REVIEW:
        2b4e934 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
        2b4e934 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
      com.google.android.apps.photos.editor.edit:
        795ccff com.google.android.apps.photos/.photoeditor.fragments.editor3.PhotoEditorActivity
      android.service.wallpaper.CROP_AND_SET_WALLPAPER:
        e37a225 com.google.android.googlequicksearchbox/com.android.launcher3.WallpaperCropActivity
        e38a31c com.google.android.apps.wallpaper/com.android.wallpaper.picker.StandalonePreviewActivity
      android.btopp.intent.action.OPEN:
        1dc4927 com.android.bluetooth/.opp.BluetoothOppLauncherActivity 

```



```

【PackageManagerService->
 private final ComponentResolver mComponentResolver->
 dumpActivityResolvers(pw, dumpState, packageName)】

 
/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
      if (!checkin && dumpState.isDumping(DumpState.DUMP_ACTIVITY_RESOLVERS)) {
            synchronized (mLock) {
                mComponentResolver.dumpActivityResolvers(pw, dumpState, packageName);
            }
        }
        
 frameworks/base/services/core/java/com/android/server/pm/ComponentResolver.java
 
     void dumpActivityResolvers(PrintWriter pw, DumpState dumpState, String packageName) {
        if (mActivities.dump(pw, dumpState.getTitlePrinted() ? "\nActivity Resolver Table:"
                : "Activity Resolver Table:", "  ", packageName,
                dumpState.isOptionEnabled(DumpState.OPTION_SHOW_FILTERS), true)) {
            dumpState.setTitlePrinted(true);
        }
    }
    
    

【 ComponentResolver->
private final ActivityIntentResolver mActivities->
IntentResolver.dump(pw, dumpState, packageName)
】
 
  frameworks/base/services/core/java/com/android/server/pm/ComponentResolver.java.ActivityIntentResolver.java
  
private static class ActivityIntentResolver  extends MimeGroupsAwareIntentResolver<Pair<ParsedActivity, ParsedIntentInfo>, ResolveInfo> {}



/frameworks/base/services/core/java/com/android/server/IntentResolver.java
ArrayMap<String, F[]> mTypeToFilter = new ArrayMap<String, F[]>();
ArrayMap<String, F[]> mBaseTypeToFilter = new ArrayMap<String, F[]>();
ArrayMap<String, F[]> mWildTypeToFilter = new ArrayMap<String, F[]>();
ArrayMap<String, F[]> mSchemeToFilter = new ArrayMap<String, F[]>();
ArrayMap<String, F[]> mActionToFilter = new ArrayMap<String, F[]>();
ArrayMap<String, F[]> mTypedActionToFilter = new ArrayMap<String, F[]>();

    public boolean dump(PrintWriter out, String title, String prefix, String packageName,
            boolean printFilter, boolean collapseDuplicates) {
        String innerPrefix = prefix + "  ";
        String sepPrefix = "\n" + prefix;
        String curPrefix = title + "\n" + prefix;
        if (dumpMap(out, curPrefix, "Full MIME Types:", innerPrefix,
                mTypeToFilter, packageName, printFilter, collapseDuplicates)) {
            curPrefix = sepPrefix;
        }
        if (dumpMap(out, curPrefix, "Base MIME Types:", innerPrefix,
                mBaseTypeToFilter, packageName, printFilter, collapseDuplicates)) {
            curPrefix = sepPrefix;
        }
        if (dumpMap(out, curPrefix, "Wild MIME Types:", innerPrefix,
                mWildTypeToFilter, packageName, printFilter, collapseDuplicates)) {
            curPrefix = sepPrefix;
        }
        if (dumpMap(out, curPrefix, "Schemes:", innerPrefix,
                mSchemeToFilter, packageName, printFilter, collapseDuplicates)) {
            curPrefix = sepPrefix;
        }
        if (dumpMap(out, curPrefix, "Non-Data Actions:", innerPrefix,
                mActionToFilter, packageName, printFilter, collapseDuplicates)) {
            curPrefix = sepPrefix;
        }
        if (dumpMap(out, curPrefix, "MIME Typed Actions:", innerPrefix,
                mTypedActionToFilter, packageName, printFilter, collapseDuplicates)) {
            curPrefix = sepPrefix;
        }
        return curPrefix == sepPrefix;
    }
 


/frameworks/base/services/core/java/com/android/server/IntentResolver.java
 
    boolean dumpMap(PrintWriter out, String titlePrefix, String title,
            String prefix, ArrayMap<String, F[]> map, String packageName,
            boolean printFilter, boolean collapseDuplicates) {
        final String eprefix = prefix + "  ";
        final String fprefix = prefix + "    ";
        final ArrayMap<Object, MutableInt> found = new ArrayMap<>();
        boolean printedSomething = false;
        Printer printer = null;
        for (int mapi=0; mapi<map.size(); mapi++) {
            F[] a = map.valueAt(mapi);
            final int N = a.length;
            boolean printedHeader = false;
            F filter;
            if (collapseDuplicates && !printFilter) {
                found.clear();
                for (int i=0; i<N && (filter=a[i]) != null; i++) {
                    if (packageName != null && !isPackageForFilter(packageName, filter)) {
                        continue;
                    }
                    Object label = filterToLabel(filter);
                    int index = found.indexOfKey(label);
                    if (index < 0) {
                        found.put(label, new MutableInt(1));
                    } else {
                        found.valueAt(index).value++;
                    }
                }
                for (int i=0; i<found.size(); i++) {
                    if (title != null) {
                        out.print(titlePrefix); out.println(title);
                        title = null;
                    }
                    if (!printedHeader) {
                        out.print(eprefix); out.print(map.keyAt(mapi)); out.println(":");
                        printedHeader = true;
                    }
                    printedSomething = true;
                    dumpFilterLabel(out, fprefix, found.keyAt(i), found.valueAt(i).value);  【打印点】
                }
            } else {
                for (int i=0; i<N && (filter=a[i]) != null; i++) {
                    if (packageName != null && !isPackageForFilter(packageName, filter)) {
                        continue;
                    }
                    if (title != null) {
                        out.print(titlePrefix); out.println(title);
                        title = null;
                    }
                    if (!printedHeader) {
                       【打印前缀: application/pkix-cert: 】 // application/pkix-cert:
                        out.print(eprefix); out.print(map.keyAt(mapi)); out.println(":");
                        printedHeader = true;
                    }
                    printedSomething = true;
                    dumpFilter(out, fprefix, filter); 【打印点】
                    if (printFilter) {
                        if (printer == null) {
                            printer = new PrintWriterPrinter(out);
                        }
                        getIntentFilter(filter).dump(printer, fprefix + "  ");
                    }
                }
            }
        }
        return printedSomething;
    }
    
    
    
/frameworks/base/services/core/java/com/android/server/pm/ComponentResolver.java
            protected void dumpFilterLabel(PrintWriter out, String prefix, Object label, int count) {
            @SuppressWarnings("unchecked") final Pair<ParsedService, ParsedIntentInfo> pair =
                    (Pair<ParsedService, ParsedIntentInfo>) label;
            out.print(prefix);      
            out.print(Integer.toHexString(System.identityHashCode(pair.first))); 【打印id】
            out.print(' ');
      
            //    8e2adf4【打印id】  com.android.certinstaller/.CertInstallerMain【打印名称】
            ComponentName.printShortString(out, pair.first.getPackageName(),
                    pair.first.getClassName()); 【打印名称】
            if (count > 1) {
                out.print(" ("); out.print(count); out.print(" filters)");
            }
            out.println();
        }
        
```



#### Receiver Resolver Table:  【HeadOneTag】



```
Receiver Resolver Table:  【HeadOneTag】   同Activity Resolver Table打印的数组不同而已
Receiver Resolver Table:  【HeadOneTag】
  Full MIME Types:
  Base MIME Types:
  Wild MIME Types:
  Schemes:
  Non-Data Actions:
  MIME Typed Actions:
~~~~~~~~~~~~~~~~~~~~~~~~~~~


Full MIME Types:
      application/gmail-ls:
        f3ce491 com.google.android.gm/.widget.GmailWidgetProvider
        9a6d582 com.google.android.gm/.widget.GoogleMailWidgetProvider
        43b799c com.google.android.gm/.GmailReceiver
------------------------
  Base MIME Types:
      vnd.android.cursor.dir:
        fbed03a com.google.android.dialer/com.android.voicemail.impl.sync.VoicemailProviderChangeReceiver
        2a4a6c9 com.google.android.dialer/com.android.dialer.app.calllog.CallLogReceiver
      vnd.android.cursor.item:
        2a4a6c9 com.google.android.dialer/com.android.dialer.app.calllog.CallLogReceiver
        6aeef2e com.google.android.dialer/com.android.voicemail.impl.FetchVoicemailReceiver_Receiver
------------------------
  Wild MIME Types:
      *:
        b8166e4 android/com.android.server.updates.ConversationActionsInstallReceiver
        3431dc6 android/com.android.server.updates.CertPinInstallReceiver
------------------------
  Schemes:
      android_secret_code:
        574ad1f com.motorola.demo/.SecretCodeReceiver
------------------------
  Non-Data Actions:
      com.google.android.youtube.music.pendingintent.controller_widget_request_data:
        3ff8c5d com.google.android.apps.youtube.music/.player.widget.PendingIntentReceiver
        
```

#### Service Resolver Table: 【HeadOneTag】

```
Service Resolver Table: 【HeadOneTag】   同Activity Resolver Table打印的数组不同而已
Service Resolver Table:  【HeadOneTag】
  Full MIME Types:
  Base MIME Types:
  Wild MIME Types:
  Schemes:
  Non-Data Actions:
  MIME Typed Actions:
~~~~~~~~~~~~~~~~~~~~~~~~~~~

  Full MIME Types:
      */*:
        cde7c8d com.motorola.bug2go/.service.bugReport.BugReportService
      image/*:
        1877c24 com.motorola.iqdataupload/.services.UpdateDBService

  Wild MIME Types:
      *:
        cde7c8d com.motorola.bug2go/.service.bugReport.BugReportService
      image:
        1877c24 com.motorola.iqdataupload/.services.UpdateDBService
        
  Schemes:
      internal_signin:
        76603db com.google.android.gms/.chimera.GmsInternalBoundBrokerService
        
  Non-Data Actions:
      android.telecom.InCallService:
        ab04861 com.google.android.googlequicksearchbox/com.google.android.apps.gsa.staticplugins.opa.morris2.framework.datasources.phone.LocalInCallServiceImpl
        772b8bc com.google.android.dialer/com.android.incallui.InCallServiceImpl
        
        
        
  MIME Typed Actions:
      com.motorola.iqdataUpload.services.UpdateDBService:
        1877c24 com.motorola.iqdataupload/.services.UpdateDBService
      motorola.intent.action.BUG2GO.BUGREPORT.START:
        cde7c8d com.motorola.bug2go/.service.bugReport.BugReportService

```



#### Provider Resolver Table:  【HeadOneTag】

```

Provider Resolver Table:【HeadOneTag】   同Activity Resolver Table打印的数组不同而已

  Full MIME Types:
  Base MIME Types:
  Wild MIME Types:
  Schemes:
  Non-Data Actions:
  MIME Typed Actions:
  
  Schemes:
      android-app:
        26a102d com.google.android.apps.wellbeing/.slices.impl.WellbeingSliceProvider
        28cbc9f com.google.android.apps.docs/.drive.slices.DriveSliceProvider

  Non-Data Actions:
      android.content.action.SETTINGS_HOMEPAGE_DATA:
        40a99b2 com.android.vending/com.google.android.finsky.slice.PhoneskyContextualCardProvider
        c0eba65 com.google.android.gms/.nearby.discovery.fastpair.slice.FastPairContextualCardProvider
        
```



#### Domain verification status:【HeadOneTag】

```
Domain verification status:【HeadOneTag】
  com.facebook.appmanager:
    ID: 73f29e83-4b46-4b65-99b5-5060a13ae702
    Signatures: [B9:21:09:B5:90:95:95:5C:CA:AF:5E:CB:CF:0F:15:3E:C9:64:0A:46:58:C1:01:66:7A:D9:6B:7E:49:9A:40:92]
    Domain verification state:
      www.instagram.com: legacy_failure
      mbasic.facebook.com: legacy_failure
      m.facebook.com: legacy_failure
      mtouch.facebook.com: legacy_failure
      mobile.facebook.com: legacy_failure
      www.facebook.com: legacy_failure
    User all:
      Verification link handling allowed: true
      Selection state:
        Disabled:
          www.instagram.com
          mbasic.facebook.com
          m.facebook.com
          mtouch.facebook.com
          mobile.facebook.com
          www.facebook.com
          

  com.google.android.apps.maps:
    ID: 1936fa0a-155e-4396-81c6-bd838a1a148a
    Signatures: [F0:FD:6C:5B:41:0F:25:CB:25:C3:B5:33:46:C8:97:2F:AE:30:F8:EE:74:11:DF:91:04:80:AD:6B:2D:60:DB:83]
    Invalid autoVerify domains:
      gmm-settings
    Domain verification state:
      mapy.google.pl: system_configured
      www.google.com.ag: system_configured
      www.google.com.ai: system_configured
```



```

 【 ComponentResolver->
DomainVerificationManagerInternal mDomainVerificationManager ->
printState(writer, packageName,UserHandle.USER_ALL, mSettings::getPackageLPr);
】


/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
【app 域名认证管理】
    private final DomainVerificationManagerInternal mDomainVerificationManager;


                case DumpState.DUMP_DOMAIN_PREFERRED:
                {
                    final android.util.IndentingPrintWriter writer =
                            new android.util.IndentingPrintWriter(pw);
                    if (dumpState.onTitlePrinted()) pw.println();

                    writer.println("Domain verification status:");
                    writer.increaseIndent();
                    try {
                        mDomainVerificationManager.printState(writer, packageName,
                                UserHandle.USER_ALL, mSettings::getPackageLPr);
                    } catch (PackageManager.NameNotFoundException e) {
                        pw.println("Failure printing domain verification information");
                        Slog.e(TAG, "Failure printing domain verification information", e);
                    }
                    writer.decreaseIndent();
                    break;
                }


frameworks/base/services/core/java/com/android/server/pm/verify/domain/DomainVerificationService.java

   private final DomainVerificationDebug mDebug;
   
    @Override
    public void printState(@NonNull IndentingPrintWriter writer, @Nullable String packageName,
            @Nullable @UserIdInt Integer userId,
            @NonNull Function<String, PackageSetting> pkgSettingFunction)
            throws NameNotFoundException {
        synchronized (mLock) {
            mDebug.printState(writer, packageName, userId, pkgSettingFunction, mAttachedPkgStates);
        }
    }
    
    
    
【 ComponentResolver->
DomainVerificationManagerInternal mDomainVerificationManager ->
printState(writer, packageName,UserHandle.USER_ALL, mSettings::getPackageLPr)->
 DomainVerificationDebug mDebug; -> printState(xxx)
】


/frameworks/base/services/core/java/com/android/server/pm/verify/domain/DomainVerificationDebug.java


boolean printState(){
SparseArray<DomainVerificationInternalUserState> userStates = pkgState.getUserStates();       
 for (int index = 0; index < size; index++) {
      DomainVerificationInternalUserState userState = userStates.valueAt(index);
          printState(writer, pkgState, userState.getUserId(), userState, reusedSet,
                allWebDomains, wasHeaderPrinted);
     }
}



    boolean printState(@NonNull IndentingPrintWriter writer,
            @NonNull DomainVerificationPkgState pkgState, @NonNull AndroidPackage pkg,
            @NonNull ArrayMap<String, Integer> reusedMap, boolean wasHeaderPrinted) {
        reusedMap.clear();
        reusedMap.putAll(pkgState.getStateMap());

        ArraySet<String> declaredDomains = mCollector.collectValidAutoVerifyDomains(pkg);
        int declaredSize = declaredDomains.size();
        for (int declaredIndex = 0; declaredIndex < declaredSize; declaredIndex++) {
            String domain = declaredDomains.valueAt(declaredIndex);
            reusedMap.putIfAbsent(domain, DomainVerificationState.STATE_NO_RESPONSE);
        }

        boolean printedHeader = false;

        if (!reusedMap.isEmpty()) {
            if (!wasHeaderPrinted) {
                Signature[] signatures = pkg.getSigningDetails().signatures;
                String signaturesDigest = signatures == null ? null : Arrays.toString(
                        PackageUtils.computeSignaturesSha256Digests(
                                pkg.getSigningDetails().signatures, ":"));

                writer.println(pkgState.getPackageName() + ":");  【包名】
                writer.increaseIndent();
                writer.println("ID: " + pkgState.getId()); 【包id】
                writer.println("Signatures: " + signaturesDigest);【包签名】
                writer.decreaseIndent();
                printedHeader = true;
            }

            writer.increaseIndent();
            final ArraySet<String> invalidDomains = mCollector.collectInvalidAutoVerifyDomains(pkg);
            if (!invalidDomains.isEmpty()) {
                writer.println("Invalid autoVerify domains:");  【无效域名】
                writer.increaseIndent();
                int size = invalidDomains.size();
                for (int index = 0; index < size; index++) {
                    writer.println(invalidDomains.valueAt(index));
                }
                writer.decreaseIndent();
            }

            writer.println("Domain verification state:");  
            writer.increaseIndent();
            int stateSize = reusedMap.size();
            for (int stateIndex = 0; stateIndex < stateSize; stateIndex++) {
                String domain = reusedMap.keyAt(stateIndex);
                Integer state = reusedMap.valueAt(stateIndex);
                writer.print(domain);   【有效域名】
                writer.print(": ");
                writer.println(DomainVerificationState.stateToDebugString(state));
            }
            writer.decreaseIndent();
            writer.decreaseIndent();
        }

        return printedHeader;
    }
    
    
    
    
    
     boolean printState(@NonNull IndentingPrintWriter writer,
            @NonNull DomainVerificationPkgState pkgState, @UserIdInt int userId,
            @Nullable DomainVerificationInternalUserState userState,
            @NonNull ArraySet<String> reusedSet, @NonNull ArraySet<String> allWebDomains,
            boolean wasHeaderPrinted) {
        reusedSet.clear();
        reusedSet.addAll(allWebDomains);
        if (userState != null) {
            reusedSet.removeAll(userState.getEnabledHosts());
        }

        boolean printedHeader = false;

        ArraySet<String> enabledHosts = userState == null ? null : userState.getEnabledHosts();
        int enabledSize = CollectionUtils.size(enabledHosts);
        int disabledSize = reusedSet.size();
        if (enabledSize > 0 || disabledSize > 0) {
            if (!wasHeaderPrinted) {
                writer.println(pkgState.getPackageName() + " " + pkgState.getId() + ":");
                printedHeader = true;
            }

            boolean isLinkHandlingAllowed = userState == null || userState.isLinkHandlingAllowed();

            writer.increaseIndent();
            writer.print("User ");
            writer.print(userId == UserHandle.USER_ALL ? "all" : userId); 【User all:】
            writer.println(":");
            writer.increaseIndent();
            writer.print("Verification link handling allowed: ");
            writer.println(isLinkHandlingAllowed);
            writer.println("Selection state:");
            writer.increaseIndent();

            if (enabledSize > 0) {
                writer.println("Enabled:");  
                writer.increaseIndent();
                for (int enabledIndex = 0; enabledIndex < enabledSize; enabledIndex++) {
                    //noinspection ConstantConditions
                    writer.println(enabledHosts.valueAt(enabledIndex));
                }
                writer.decreaseIndent();
            }

            if (disabledSize > 0) {
                writer.println("Disabled:"); 【 Disable的域名domain】
                writer.increaseIndent();
                for (int disabledIndex = 0; disabledIndex < disabledSize; disabledIndex++) {
                    writer.println(reusedSet.valueAt(disabledIndex)); 
                }
                writer.decreaseIndent();
            }

            writer.decreaseIndent();
            writer.decreaseIndent();
            writer.decreaseIndent();
        }

        return printedHeader;
    }
    

```





#### Preferred Activities User 0: 【HeadOneTag】

```
Preferred Activities User 0: 【HeadOneTag】 【MIME文件的默认打开应用】
// 同 Activity Resolver Table:  【HeadOneTag】
  Full MIME Types:
  Base MIME Types:
  Wild MIME Types:
  Schemes:
  Non-Data Actions:
  MIME Typed Actions:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Preferred Activities User 0:
  Full MIME Types:
      video/3gpp:
        349b2d1 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
         mMatch=0x600000 mAlways=true
          Selected from:
            com.google.android.apps.nbu.files/.gateway.preview.PreviewActivity
            com.google.android.apps.photos/.pager.HostPhotoPagerActivity
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          StaticType: "video/3gpp"

      video/mp4:
        81cf3 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
         mMatch=0x600000 mAlways=true
          Selected from:
            com.google.android.apps.nbu.files/.gateway.preview.PreviewActivity
            com.google.android.apps.photos/.pager.HostPhotoPagerActivity
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          StaticType: "video/mp4"
---------------------    
  Base MIME Types:
      image:
        b255c81 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
         mMatch=0x600000 mAlways=true
          Selected from:
            com.google.android.apps.nbu.files/.gateway.preview.PreviewActivity
            com.google.android.apps.photos/.pager.HostPhotoPagerActivity
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          StaticType: "image/vnd.djvu"
        ddde126 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
---------------------    
  Schemes:
      sms:
        5a20655 com.google.android.apps.messaging/.ui.conversation.LaunchConversationActivity
         mMatch=0x200000 mAlways=true
          Selected from:
            com.google.android.apps.messaging/.ui.conversation.LaunchConversationActivity
          Action: "android.intent.action.SENDTO"
          Category: "android.intent.category.DEFAULT"
          Scheme: "sms"
      tel:
        cea2837 com.google.android.dialer/com.android.dialer.main.impl.MainActivity
         mMatch=0x200000 mAlways=true
          Selected from:
            com.google.android.dialer/com.android.dialer.main.impl.MainActivity
          Action: "android.intent.action.DIAL"
          Category: "android.intent.category.DEFAULT"
          Scheme: "tel"
---------------------    
Non-Data Actions:
      android.intent.action.DIAL:
        814703f com.google.android.dialer/com.android.dialer.main.impl.MainActivity
         mMatch=0x100000 mAlways=true
          Selected from:
            com.google.android.dialer/com.android.dialer.main.impl.MainActivity
          Action: "android.intent.action.DIAL"
          Category: "android.intent.category.DEFAULT"
---------------------   
MIME Typed Actions:
      android.intent.action.VIEW:
        b255c81 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
         mMatch=0x600000 mAlways=true
          Selected from:
            com.google.android.apps.nbu.files/.gateway.preview.PreviewActivity
            com.google.android.apps.photos/.pager.HostPhotoPagerActivity
          Action: "android.intent.action.VIEW"
          Category: "android.intent.category.DEFAULT"
          StaticType: "image/vnd.djvu"
        ddde126 com.google.android.apps.photos/.pager.HostPhotoPagerActivity
```



```
// 同 Activity Resolver Table:  【HeadOneTag】
frameworks/base/services/core/java/com/android/server/pm/ComponentResolver.java.ActivityIntentResolver.java
                case DumpState.DUMP_PREFERRED:
                    mSettings.dumpPreferred(pw, dumpState, packageName);
                    break;


/frameworks/base/services/core/java/com/android/server/pm/Settings.java

   // The user's preferred activities associated with particular intent filters.
 private final WatchedSparseArray<PreferredIntentResolver>
            mPreferredActivities = new WatchedSparseArray<>(); 
            
            
    void dumpPreferred(PrintWriter pw, DumpState dumpState, String packageName) {
        for (int i = 0; i < mPreferredActivities.size(); i++) {
            PreferredIntentResolver pir = mPreferredActivities.valueAt(i);
            int user = mPreferredActivities.keyAt(i);
            //  同 Activity Resolver Table:  【HeadOneTag】
            if (pir.dump(pw,
                         dumpState.getTitlePrinted()
                         ? "\nPreferred Activities User " + user + ":"
                         : "Preferred Activities User " + user + ":", "  ",
                         packageName, true, false)) {
                dumpState.setTitlePrinted(true);
            }
        }
    }
    


```



#### Permissions:【HeadOneTag】静态权限

```
Permissions:【HeadOneTag】

  Permission [com.google.android.gms.auth.api.phone.permission.SEND] (b0884fb):
    sourcePackage=com.google.android.gms
    uid=10158 gids=[] type=0 prot=signature
    perm=PermissionInfo{ca71583 com.google.android.gms.auth.api.phone.permission.SEND}


  Permission [com.google.android.vending.verifier.ACCESS_VERIFIER] (12b2848):
    sourcePackage=com.android.vending
    uid=10150 gids=[] type=0 prot=signature
    perm=PermissionInfo{1504619 com.google.android.vending.verifier.ACCESS_VERIFIER}
    
```

```

【ComponentResolver-> dump()
Settings.java-> dumpPermissions()  
LegacyPermissionSettings->dumpPermissions()
LegacyPermission->dump()
】



        

frameworks/base/services/core/java/com/android/server/pm/ComponentResolver.java
      if (!checkin && dumpState.isDumping(DumpState.DUMP_PERMISSIONS)) {
            mSettings.dumpPermissions(pw, packageName, permissionNames, dumpState);
        }

/frameworks/base/services/core/java/com/android/server/pm/Settings.java
    void dumpPermissions(PrintWriter pw, String packageName, ArraySet<String> permissionNames,
            DumpState dumpState) {
        LegacyPermissionSettings.dumpPermissions(pw, packageName, permissionNames,
                mPermissionDataProvider.getLegacyPermissions(),
                mPermissionDataProvider.getAllAppOpPermissionPackages(), true, dumpState);
    }
    
    


/frameworks/base/services/core/java/com/android/server/pm/permission/LegacyPermissionSettings.java

    public static void dumpPermissions(@NonNull PrintWriter pw, @Nullable String packageName,
            @Nullable ArraySet<String> permissionNames, @NonNull List<LegacyPermission> permissions,
            @NonNull Map<String, Set<String>> appOpPermissionPackages,
            boolean externalStorageEnforced, @NonNull DumpState dumpState) {
        boolean printedSomething = false;
        final int permissionsSize = permissions.size();
        for (int i = 0; i < permissionsSize; i++) {
            final LegacyPermission permission = permissions.get(i);
            // 【 permission.dump 】 
            printedSomething = permission.dump(pw, packageName, permissionNames,
                    externalStorageEnforced, printedSomething, dumpState);
        }
        if (packageName == null && permissionNames == null) {
            boolean firstEntry = true;
            for (final Map.Entry<String, Set<String>> entry : appOpPermissionPackages.entrySet()) {
                if (firstEntry) {
                    firstEntry = false;
                    if (dumpState.onTitlePrinted()) {
                        pw.println();
                    }
                    pw.println("AppOp Permissions:");
                }
                pw.print("  AppOp Permission ");
                pw.print(entry.getKey());
                pw.println(":");
                for (final String appOpPackageName : entry.getValue()) {
                    pw.print("    ");
                    pw.println(appOpPackageName);
                }
            }
        }
    }
    
    
 frameworks/base/services/core/java/com/android/server/pm/permission/LegacyPermission.java

    public boolean dump(@NonNull PrintWriter pw, @NonNull String packageName,
            @NonNull Set<String> permissionNames, boolean readEnforced, boolean printedSomething,
            @NonNull DumpState dumpState) {
        if (packageName != null && !packageName.equals(mPermissionInfo.packageName)) {
            return false;
        }
        if (permissionNames != null && !permissionNames.contains(mPermissionInfo.name)) {
            return false;
        }
        if (!printedSomething) {
            if (dumpState.onTitlePrinted()) {
                pw.println();
            }
            pw.println("Permissions:");
        }
        pw.print("  Permission ["); pw.print(mPermissionInfo.name); pw.print("] (");
        pw.print(Integer.toHexString(System.identityHashCode(this)));
        pw.println("):");
        pw.print("    sourcePackage="); pw.println(mPermissionInfo.packageName);
        pw.print("    uid="); pw.print(mUid);
        pw.print(" gids="); pw.print(Arrays.toString(mGids));
        pw.print(" type="); pw.print(mType);
        pw.print(" prot=");
        pw.println(PermissionInfo.protectionToString(mPermissionInfo.protectionLevel));
        if (mPermissionInfo != null) {
            pw.print("    perm="); pw.println(mPermissionInfo);
            if ((mPermissionInfo.flags & PermissionInfo.FLAG_INSTALLED) == 0
                    || (mPermissionInfo.flags & PermissionInfo.FLAG_REMOVED) != 0) {
                pw.print("    flags=0x"); pw.println(Integer.toHexString(mPermissionInfo.flags));
            }
        }
        if (Objects.equals(mPermissionInfo.name,
                android.Manifest.permission.READ_EXTERNAL_STORAGE)) {
            pw.print("    enforced=");
            pw.println(readEnforced);
        }
        return true;
    }
    
    


```



#### AppOp Permissions:  【HeadOneTag】动态权限

```
AppOp Permissions:  【HeadOneTag】 用户动态同意的权限
AppOp Permissions:
  AppOp Permission android.permission.WRITE_SETTINGS:
    com.google.android.networkstack.tethering
  AppOp Permission android.permission.MANAGE_EXTERNAL_STORAGE:
    com.android.externalstorage
    com.android.providers.downloads
  AppOp Permission android.permission.SYSTEM_ALERT_WINDOW:
    com.google.android.youtube
  AppOp Permission android.permission.INSTANT_APP_FOREGROUND_SERVICE:
    com.android.shell
  AppOp Permission android.permission.PACKAGE_USAGE_STATS:
    com.android.vending
  AppOp Permission android.permission.SCHEDULE_EXACT_ALARM:
    com.google.android.googlequicksearchbox
    com.google.android.apps.messaging
  AppOp Permission android.permission.LOADER_USAGE_STATS:
    com.android.vending
  AppOp Permission android.permission.ACCESS_NOTIFICATIONS:
    com.android.settings
  AppOp Permission android.permission.INTERACT_ACROSS_PROFILES:
    com.google.android.googlequicksearchbox
  AppOp Permission android.permission.REQUEST_INSTALL_PACKAGES:
    com.android.nfc
```



```
【ComponentResolver-> dump()   
Settings.java-> dumpPermissions()  
LegacyPermissionSettings->dumpPermissions()
LegacyPermission->dump()
->appOpPermissionPackages.toString()
】

/frameworks/base/services/core/java/com/android/server/pm/permission/LegacyPermissionSettings.java

    public static void dumpPermissions(@NonNull PrintWriter pw, @Nullable String packageName...){
    
            for (final Map.Entry<String, Set<String>> entry : appOpPermissionPackages.entrySet()) {
                if (firstEntry) {
                    firstEntry = false;
                    if (dumpState.onTitlePrinted()) {
                        pw.println();
                    }
                    pw.println("AppOp Permissions:");
                }
                pw.print("  AppOp Permission ");
                pw.print(entry.getKey());  【动态授权的权限】
                pw.println(":");
                for (final String appOpPackageName : entry.getValue()) {
                    pw.print("    ");
                    pw.println(appOpPackageName);   【得到授权的app名称】
                }
            }
        }
        
```



#### Registered ContentProviders:  【HeadOneTag】

```
Registered ContentProviders:  【HeadOneTag】 手机注册的 ContentProviders
  com.google.android.inputmethod.latin/androidx.lifecycle.ProcessLifecycleOwnerInitializer:
com.google.android.gms/com.google.android.location.reporting.service.utils.ReportingContentProvider:
  com.android.settings/.homepage.contextualcards.SettingsContextualCardProvider:
  com.motorola.ccc.devicemanagement/.RedbuttonContentProvider:
```





```
【PackageManagerService-> dump()   
mComponentResolver-> dumpContentProviders()  

】


/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
 private final ComponentResolver mComponentResolver;
 
if (!checkin && dumpState.isDumping(DumpState.DUMP_PROVIDERS)) {
            synchronized (mLock) {
                mComponentResolver.dumpContentProviders(pw, dumpState, packageName);
            }
        }
        
        
/frameworks/base/services/core/java/com/android/server/pm/ComponentResolver.java
    void dumpContentProviders(PrintWriter pw, DumpState dumpState, String packageName) {
        boolean printedSomething = false;
        for (ParsedProvider p : mProviders.mProviders.values()) {
            if (packageName != null && !packageName.equals(p.getPackageName())) {
                continue;
            }
            if (!printedSomething) {
                if (dumpState.onTitlePrinted()) {
                    pw.println();
                }
                pw.println("Registered ContentProviders:");
                printedSomething = true;
            }
            pw.print("  "); //  【打印权限的包名  以及  权限的名称 】
            ComponentName.printShortString(pw, p.getPackageName(), p.getName());
            pw.println(":");
            pw.print("    ");
            pw.println(p.toString());
        }
        printedSomething = false;
        for (Map.Entry<String, ParsedProvider> entry :
                mProvidersByAuthority.entrySet()) {
            ParsedProvider p = entry.getValue();
            if (packageName != null && !packageName.equals(p.getPackageName())) {
                continue;
            }
            if (!printedSomething) {
                if (dumpState.onTitlePrinted()) {
                    pw.println();
                }
                pw.println("ContentProvider Authorities:");
                printedSomething = true;
            }
            pw.print("  ["); pw.print(entry.getKey()); pw.println("]:");
            pw.print("    "); pw.println(p.toString());

            AndroidPackage pkg = sPackageManagerInternal.getPackage(p.getPackageName());

            if (pkg != null) {
                pw.print("      applicationInfo="); pw.println(pkg.toAppInfoWithoutState());
            }
        }
    }
    
```



####  ContentProvider Authorities: 【HeadOneTag】

```
ContentProvider Authorities: 【HeadOneTag】
  [com.google.android.contacts.assistant]:
    Provider{6ec77e3 com.google.android.contacts/com.google.android.apps.contacts.common.provider.ContentNotifyStubProvider}
      applicationInfo=ApplicationInfo{92a2405 com.google.android.contacts}
  [com.google.android.gms.chimera.container.sharedmoduleprovider]:
    Provider{e229e7c com.google.android.gms/.chimera.container.SharedModuleProvider}
      applicationInfo=ApplicationInfo{ec90c5a com.google.android.gms}
```



```

【PackageManagerService-> dump()   
mComponentResolver-> dumpContentProviders()  

】


/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
 private final ComponentResolver mComponentResolver;
 
if (!checkin && dumpState.isDumping(DumpState.DUMP_PROVIDERS)) {
            synchronized (mLock) {
                mComponentResolver.dumpContentProviders(pw, dumpState, packageName);
            }
        }
        
        
/frameworks/base/services/core/java/com/android/server/pm/ComponentResolver.java
    void dumpContentProviders(PrintWriter pw, DumpState dumpState, String packageName) {
        boolean printedSomething = false;
        for (ParsedProvider p : mProviders.mProviders.values()) {
            if (packageName != null && !packageName.equals(p.getPackageName())) {
                continue;
            }
            if (!printedSomething) {
                if (dumpState.onTitlePrinted()) {
                    pw.println();
                }
                pw.println("Registered ContentProviders:");
                printedSomething = true;
            }
            pw.print("  "); //  【打印权限的包名  以及  权限的名称 】
            ComponentName.printShortString(pw, p.getPackageName(), p.getName());
            pw.println(":");
            pw.print("    ");
            pw.println(p.toString());
        }
        printedSomething = false;
        for (Map.Entry<String, ParsedProvider> entry :
                mProvidersByAuthority.entrySet()) {
            ParsedProvider p = entry.getValue();
            if (packageName != null && !packageName.equals(p.getPackageName())) {
                continue;
            }
            if (!printedSomething) {
                if (dumpState.onTitlePrinted()) {
                    pw.println();
                }
                pw.println("ContentProvider Authorities:");
                printedSomething = true;
            }
             //   【授权权限名称】
            pw.print("  ["); pw.print(entry.getKey()); pw.println("]:");
            pw.print("    "); pw.println(p.toString());  // 已授权应用名称

            AndroidPackage pkg = sPackageManagerInternal.getPackage(p.getPackageName());

            if (pkg != null) {
                pw.print("      applicationInfo="); pw.println(pkg.toAppInfoWithoutState());
            }
        }
    }
    


```



#### Key Set Manager: 【HeadOneTag】

```
Key Set Manager: 【HeadOneTag】
  [com.android.internal.display.cutout.emulation.noCutout]
      Signing KeySets: 1
  [com.google.android.networkstack.tethering]
      Signing KeySets: 5
  [com.motorola.mobiledesktop.core]
      Signing KeySets: 2
  [com.mediatek.ims]
      Signing KeySets: 2

```



```

【PackageManagerService-> dump()   
KeySetManagerService-> dumpLPr()  

】


/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java
        if (!checkin && dumpState.isDumping(DumpState.DUMP_KEYSETS)) {
            synchronized (mLock) {
                mSettings.getKeySetManagerService().dumpLPr(pw, packageName, dumpState);
            }
        }
        



/frameworks/base/services/core/java/com/android/server/pm/KeySetManagerService.java


 public void dumpLPr(PrintWriter pw, String packageName,
                        DumpState dumpState) {
        boolean printedHeader = false;
        for (ArrayMap.Entry<String, PackageSetting> e : mPackages.entrySet()) {
            String keySetPackage = e.getKey();
            if (packageName != null && !packageName.equals(keySetPackage)) {
                continue;
            }
            if (!printedHeader) {
                if (dumpState.onTitlePrinted())
                    pw.println();
                pw.println("Key Set Manager:");
                printedHeader = true;
            }
            PackageSetting pkg = e.getValue();
            pw.print("  ["); pw.print(keySetPackage); pw.println("]");
            if (pkg.keySetData != null) {
                boolean printedLabel = false;
                for (ArrayMap.Entry<String, Long> entry : pkg.keySetData.getAliases().entrySet()) {
                    if (!printedLabel) {
                        pw.print("      KeySets Aliases: ");
                        printedLabel = true;
                    } else {
                        pw.print(", ");
                    }
                    pw.print(entry.getKey());
                    pw.print('=');
                    pw.print(Long.toString(entry.getValue()));
                }
                if (printedLabel) {
                    pw.println("");
                }
                printedLabel = false;
                if (pkg.keySetData.isUsingDefinedKeySets()) {
                    ArrayMap<String, Long> definedKeySets = pkg.keySetData.getAliases();
                    final int dksSize = definedKeySets.size();
                    for (int i = 0; i < dksSize; i++) {
                        if (!printedLabel) {
                            pw.print("      Defined KeySets: ");
                            printedLabel = true;
                        } else {
                            pw.print(", ");
                        }
                        pw.print(Long.toString(definedKeySets.valueAt(i)));
                    }
                }
                if (printedLabel) {
                    pw.println("");
                }
                printedLabel = false;
/* KeySet containing all signing keys - superset of the others */
// /frameworks/base/services/core/java/com/android/server/pm/PackageKeySetData.java
 //  private long mProperSigningKeySet;   用于覆盖升级APP? 
    
                final long signingKeySet = pkg.keySetData.getProperSigningKeySet();
                pw.print("      Signing KeySets: ");
                pw.print(Long.toString(signingKeySet));    【 signingKeySet 什么意思 】
                pw.println("");
                if (pkg.keySetData.isUsingUpgradeKeySets()) {
                    for (long keySetId : pkg.keySetData.getUpgradeKeySets()) {
                        if (!printedLabel) {
                            pw.print("      Upgrade KeySets: ");
                            printedLabel = true;
                        } else {
                            pw.print(", ");
                        }
                        pw.print(Long.toString(keySetId));
                    }
                }
                if (printedLabel) {
                    pw.println("");
                }
            }
        }
    }



```



####  Packages: 【HeadOneTag】 

```
Packages: 【HeadOneTag】  安装包信息
  Package [com.google.android.youtube] (b70ed40):
    userId=10198
    pkg=Package{6deb479 com.google.android.youtube}
    codePath=/product/app/YouTube
    resourcePath=/product/app/YouTube
    legacyNativeLibraryDir=/product/app/YouTube/lib
    extractNativeLibs=false
    primaryCpuAbi=arm64-v8a
    secondaryCpuAbi=null
    cpuAbiOverride=null
    versionCode=1516625344 minSdk=21 targetSdk=30
    minExtensionVersions=[]
    versionName=15.50.35
    usesNonSdkApi=false
    splits=[base]
    apkSigningVersion=3
    applicationInfo=PackageImpl{6deb479 com.google.android.youtube}
    flags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ALLOW_BACKUP KILL_AFTER_RESTORE RESTORE_ANY_VERSION LARGE_HEAP ]
    privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION ALLOW_AUDIO_PLAYBACK_CAPTURE PRIVATE_FLAG_REQUEST_LEGACY_EXTERNAL_STORAGE HAS_DOMAIN_URLS PARTIALLY_DIRECT_BOOT_AWARE PRODUCT PRIVATE_FLAG_ALLOW_NATIVE_HEAP_POINTER_TAGGING ]
    forceQueryable=false
    queriesIntents=[Intent { act=android.support.customtabs.action.CustomTabsService }, Intent { act=android.support.customtabs.action.CustomTabsService }, Intent { act=android.support.customtabs.action.CustomTabsService }, Intent { act=android.support.customtabs.action.CustomTabsService }, Intent { act=android.intent.action.SEND dat=content://*/* typ=text/plain }, Intent { act=android.intent.action.VIEW cat=[android.intent.category.BROWSABLE] dat=http://www.example.com/... }, Intent { act=android.support.customtabs.action.CustomTabsService }]
    dataDir=/data/user/0/com.google.android.youtube
    supportsScreens=[small, medium, large, xlarge, resizeable, anyDensity]
    usesOptionalLibraries:
      org.apache.http.legacy
      androidx.window.extensions
      androidx.window.sidecar
    usesLibraryFiles:
      /system/framework/org.apache.http.legacy.jar
    timeStamp=2009-01-01 08:00:00
    firstInstallTime=2009-01-01 08:00:00
    lastUpdateTime=2009-01-01 08:00:00
    signatures=PackageSignatures{665e5be version:3, signatures:[7d3bce25], past signatures:[]}
    installPermissionsFixed=true
    pkgFlags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ALLOW_BACKUP KILL_AFTER_RESTORE RESTORE_ANY_VERSION LARGE_HEAP ]
    declared permissions:
      com.google.android.youtube.permission.C2D_MESSAGE: prot=signature, INSTALLED
    install permissions:
      com.google.android.c2dm.permission.RECEIVE: granted=true
      android.permission.USE_CREDENTIALS: granted=true
      com.google.android.providers.gsf.permission.READ_GSERVICES: granted=true
      com.google.android.youtube.permission.C2D_MESSAGE: granted=true
      android.permission.MANAGE_ACCOUNTS: granted=true
      android.permission.NFC: granted=true
      android.permission.FOREGROUND_SERVICE: granted=true
      android.permission.RECEIVE_BOOT_COMPLETED: granted=true
      com.google.android.gms.permission.AD_ID_NOTIFICATION: granted=true
      android.permission.INTERNET: granted=true
      android.permission.GET_PACKAGE_SIZE: granted=true
      android.permission.ACCESS_NETWORK_STATE: granted=true
      android.permission.USE_FINGERPRINT: granted=true
      android.permission.VIBRATE: granted=true
      android.permission.ACCESS_WIFI_STATE: granted=true
      android.permission.USE_BIOMETRIC: granted=true
      android.permission.WAKE_LOCK: granted=true
    User 0: ceDataInode=4653 installed=true hidden=false suspended=false distractionFlags=0 stopped=false notLaunched=false enabled=0 instant=false virtual=false
      overlay paths:
        /product/overlay/NavigationBarModeGestural/NavigationBarModeGesturalOverlay.apk
        /data/resource-cache/com.android.systemui-neutral-oEk0.frro
        /data/resource-cache/com.android.systemui-accent-Z3Gg.frro
      legacy overlay paths:
        /product/overlay/NavigationBarModeGestural/NavigationBarModeGesturalOverlay.apk
      gids=[3003]
      runtime permissions:
        android.permission.ACCESS_FINE_LOCATION: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.READ_EXTERNAL_STORAGE: granted=false, flags=[ REVOKE_WHEN_REQUESTED|USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.ACCESS_COARSE_LOCATION: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.READ_PHONE_STATE: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.CAMERA: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.GET_ACCOUNTS: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.WRITE_EXTERNAL_STORAGE: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED|RESTRICTION_UPGRADE_EXEMPT]
        android.permission.RECORD_AUDIO: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.READ_CONTACTS: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
      enabledComponents:
        com.google.android.youtube.ManageNetworkUsageActivity
        androidx.work.impl.background.systemalarm.RescheduleReceiver
        androidx.work.impl.background.systemjob.SystemJobService
        
```



```

【PackageManagerService-> dump()   
KeySetManagerService-> dumpPackagesLPr()  

】


/frameworks/base/services/core/java/com/android/server/pm/PackageManagerService.java

        if (dumpState.isDumping(DumpState.DUMP_PACKAGES)) {
            synchronized (mLock) {
                mSettings.dumpPackagesLPr(pw, packageName, permissionNames, dumpState, checkin);
            }
        }
        
        
frameworks/base/services/core/java/com/android/server/pm/Settings.java


    void dumpPackagesLPr(PrintWriter pw, String packageName, ArraySet<String> permissionNames, DumpState dumpState, boolean checkin) {
        final SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        final Date date = new Date();
        boolean printedSomething = false;
        final boolean dumpAllComponents =
                dumpState.isOptionEnabled(DumpState.OPTION_DUMP_ALL_COMPONENTS);
        List<UserInfo> users = getAllUsers(UserManagerService.getInstance());
        for (final PackageSetting ps : mPackages.values()) {
            if (packageName != null && !packageName.equals(ps.realName)
                    && !packageName.equals(ps.name)) {
                continue;
            }
            final LegacyPermissionState permissionsState =
                    mPermissionDataProvider.getLegacyPermissionState(ps.appId);
            if (permissionNames != null
                    && !permissionsState.hasPermissionState(permissionNames)) {
                continue;
            }

            if (!checkin && packageName != null) {
                dumpState.setSharedUser(ps.sharedUser);
            }

            if (!checkin && !printedSomething) {
                if (dumpState.onTitlePrinted())
                    pw.println();
                pw.println("Packages:");  //【打印安装的包名】
                printedSomething = true;
            }
            dumpPackageLPr(pw, "  ", checkin ? "pkg" : null, permissionNames, ps, permissionsState,
                    sdf, date, users, packageName != null, dumpAllComponents);
        }

        printedSomething = false;
        if (mRenamedPackages.size() > 0 && permissionNames == null) {
            for (final Map.Entry<String, String> e : mRenamedPackages.entrySet()) {
                if (packageName != null && !packageName.equals(e.getKey())
                        && !packageName.equals(e.getValue())) {
                    continue;
                }
                if (!checkin) {
                    if (!printedSomething) {
                        if (dumpState.onTitlePrinted())
                            pw.println();
                        pw.println("Renamed packages:");
                        printedSomething = true;
                    }
                    pw.print("  ");
                } else {
                    pw.print("ren,");
                }
                pw.print(e.getKey());
                pw.print(checkin ? " -> " : ",");
                pw.println(e.getValue());
            }
        }

        printedSomething = false;
        if (mDisabledSysPackages.size() > 0 && permissionNames == null) {
            for (final PackageSetting ps : mDisabledSysPackages.values()) {
                if (packageName != null && !packageName.equals(ps.realName)
                        && !packageName.equals(ps.name)) {
                    continue;
                }
                if (!checkin && !printedSomething) {
                    if (dumpState.onTitlePrinted())
                        pw.println();
                    pw.println("Hidden system packages:");
                    printedSomething = true;
                }
                final LegacyPermissionState permissionsState =
                        mPermissionDataProvider.getLegacyPermissionState(ps.appId);
                dumpPackageLPr(pw, "  ", checkin ? "dis" : null, permissionNames, ps,
                        permissionsState, sdf, date, users, packageName != null, dumpAllComponents);
            }
        }
    }
    
```

```

 void dumpPackageLPr(PrintWriter pw, String prefix, String checkinTag,
            ArraySet<String> permissionNames, PackageSetting ps,
            LegacyPermissionState permissionsState, SimpleDateFormat sdf, Date date,
            List<UserInfo> users, boolean dumpAll, boolean dumpAllComponents) {
        AndroidPackage pkg = ps.pkg;
        if (checkinTag != null) {
            pw.print(checkinTag);
            pw.print(",");
            pw.print(ps.realName != null ? ps.realName : ps.name);
            pw.print(",");
            pw.print(ps.appId);
            pw.print(",");
            pw.print(ps.versionCode);
            pw.print(",");
            pw.print(ps.firstInstallTime);
            pw.print(",");
            pw.print(ps.lastUpdateTime);
            pw.print(",");
            pw.print(ps.installSource.installerPackageName != null
                    ? ps.installSource.installerPackageName : "?");
            pw.print(ps.installSource.installerAttributionTag != null
                    ? "(" + ps.installSource.installerAttributionTag + ")" : "");
            pw.println();
            if (pkg != null) {
                pw.print(checkinTag); pw.print("-"); pw.print("splt,");
                pw.print("base,");
                pw.println(pkg.getBaseRevisionCode());
                if (pkg.getSplitNames() != null) {
                    int[] splitRevisionCodes = pkg.getSplitRevisionCodes();
                    for (int i = 0; i < pkg.getSplitNames().length; i++) {
                        pw.print(checkinTag); pw.print("-"); pw.print("splt,");
                        pw.print(pkg.getSplitNames()[i]); pw.print(",");
                        pw.println(splitRevisionCodes[i]);
                    }
                }
            }
            for (UserInfo user : users) {
                pw.print(checkinTag);
                pw.print("-");
                pw.print("usr");
                pw.print(",");
                pw.print(user.id);
                pw.print(",");
                pw.print(ps.getInstalled(user.id) ? "I" : "i");
                pw.print(ps.getHidden(user.id) ? "B" : "b");
                pw.print(ps.getSuspended(user.id) ? "SU" : "su");
                pw.print(ps.getStopped(user.id) ? "S" : "s");
                pw.print(ps.getNotLaunched(user.id) ? "l" : "L");
                pw.print(ps.getInstantApp(user.id) ? "IA" : "ia");
                pw.print(ps.getVirtulalPreload(user.id) ? "VPI" : "vpi");
                String harmfulAppWarning = ps.getHarmfulAppWarning(user.id);
                pw.print(harmfulAppWarning != null ? "HA" : "ha");
                pw.print(",");
                pw.print(ps.getEnabled(user.id));
                String lastDisabledAppCaller = ps.getLastDisabledAppCaller(user.id);
                pw.print(",");
                pw.print(lastDisabledAppCaller != null ? lastDisabledAppCaller : "?");
                pw.print(",");
                pw.println();
            }
            return;
        }

        pw.print(prefix); pw.print("Package [");
            pw.print(ps.realName != null ? ps.realName : ps.name);   【打印包名】
            pw.print("] (");
         pw.print(Integer.toHexString(System.identityHashCode(ps))); 【PackageSettingid】
            pw.println("):");

        if (ps.realName != null) {
            pw.print(prefix); pw.print("  compat name=");
            pw.println(ps.name); 【包名】
        }

        pw.print(prefix); pw.print("  userId="); pw.println(ps.appId); 【应用唯一id】

        if (ps.sharedUser != null) {
            pw.print(prefix); pw.print("  sharedUser="); pw.println(ps.sharedUser);
        }
        pw.print(prefix); pw.print("  pkg="); pw.println(pkg);【 AndroidPackage 打印 】
        pw.print(prefix); pw.print("  codePath="); pw.println(ps.getPathString());
        if (permissionNames == null) {
            pw.print(prefix); pw.print("  resourcePath=");  【APK资源文件夹】pw.println(ps.getPathString());
            pw.print(prefix); pw.print("  legacyNativeLibraryDir=");
            pw.println(ps.legacyNativeLibraryPathString);
            pw.print(prefix); pw.print("  extractNativeLibs=");
// android:extractNativeLibs
// 软件包安装程序是否将原生库从 APK 提取到文件系统。如果设为 false，则原生库必须保持页面对齐状态并以未压缩的形式存储在 APK 中

            pw.println((ps.pkgFlags & ApplicationInfo.FLAG_EXTRACT_NATIVE_LIBS) != 0
                    ? "true" : "false");
            pw.print(prefix); pw.print("  primaryCpuAbi="); pw.println(ps.primaryCpuAbiString);
            pw.print(prefix); pw.print("  secondaryCpuAbi="); pw.println(ps.secondaryCpuAbiString);
            pw.print(prefix); pw.print("  cpuAbiOverride="); pw.println(ps.cpuAbiOverrideString);
        }
        pw.print(prefix); pw.print("  versionCode="); pw.print(ps.versionCode);
        if (pkg != null) {
            pw.print(" minSdk="); pw.print(pkg.getMinSdkVersion()); 【最小SDK】
            pw.print(" targetSdk="); pw.println(pkg.getTargetSdkVersion()); 【目标SDK】

            SparseIntArray minExtensionVersions = pkg.getMinExtensionVersions();

            pw.print(prefix); pw.print("  minExtensionVersions=[");
            if (minExtensionVersions != null) {
                List<String> minExtVerStrings = new ArrayList<>();
                int size = minExtensionVersions.size();
                for (int index = 0; index < size; index++) {
                    int key = minExtensionVersions.keyAt(index);
                    int value = minExtensionVersions.valueAt(index);
                    minExtVerStrings.add(key + "=" + value);
          
                }

                pw.print(TextUtils.join(", ", minExtVerStrings));
            }
            pw.print("]");
        }
        pw.println();
        if (pkg != null) {
            pw.print(prefix); pw.print("  versionName="); pw.println(pkg.getVersionName());
      //     versionName=15.50.35  【APP版本】
                      
            pw.print(prefix); pw.print("  usesNonSdkApi="); pw.println(pkg.isUsesNonSdkApi());
            pw.print(prefix); pw.print("  splits="); dumpSplitNames(pw, pkg); pw.println();
            final int apkSigningVersion = pkg.getSigningDetails().signatureSchemeVersion;
            pw.print(prefix); pw.print("  apkSigningVersion="); pw.println(apkSigningVersion);
            pw.print(prefix); pw.print("  applicationInfo=");
            pw.println(pkg.toAppInfoToString());
            pw.print(prefix); pw.print("  flags=");
            
            



    /**
     * Flags associated with the application.  Any combination of
     * {@link #FLAG_SYSTEM}, {@link #FLAG_DEBUGGABLE}, {@link #FLAG_HAS_CODE},
     * {@link #FLAG_PERSISTENT}, {@link #FLAG_FACTORY_TEST}, and
     * {@link #FLAG_ALLOW_TASK_REPARENTING}
     * {@link #FLAG_ALLOW_CLEAR_USER_DATA}, {@link #FLAG_UPDATED_SYSTEM_APP},
     * {@link #FLAG_TEST_ONLY}, {@link #FLAG_SUPPORTS_SMALL_SCREENS},
     * {@link #FLAG_SUPPORTS_NORMAL_SCREENS},
     * {@link #FLAG_SUPPORTS_LARGE_SCREENS}, {@link #FLAG_SUPPORTS_XLARGE_SCREENS},
     * {@link #FLAG_RESIZEABLE_FOR_SCREENS},
     * {@link #FLAG_SUPPORTS_SCREEN_DENSITIES}, {@link #FLAG_VM_SAFE_MODE},
     * {@link #FLAG_ALLOW_BACKUP}, {@link #FLAG_KILL_AFTER_RESTORE},
     * {@link #FLAG_RESTORE_ANY_VERSION}, {@link #FLAG_EXTERNAL_STORAGE},
     * {@link #FLAG_LARGE_HEAP}, {@link #FLAG_STOPPED},
     * {@link #FLAG_SUPPORTS_RTL}, {@link #FLAG_INSTALLED},
     * {@link #FLAG_IS_DATA_ONLY}, {@link #FLAG_IS_GAME},
     * {@link #FLAG_FULL_BACKUP_ONLY}, {@link #FLAG_USES_CLEARTEXT_TRAFFIC},
     * {@link #FLAG_MULTIARCH}.
     */
     
 //     flags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ALLOW_BACKUP KILL_AFTER_RESTORE RESTORE_ANY_VERSION LARGE_HEAP ]
 

            printFlags(pw, PackageInfoUtils.appInfoFlags(pkg, ps), FLAG_DUMP_SPEC); pw.println();
            
//   @IntDef(flag = true, prefix = { "PRIVATE_FLAG_" }, value = {
//           PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE,
//           PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION,
//           PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_UNRESIZEABLE,
//           PRIVATE_FLAG_BACKUP_IN_FOREGROUND,
//           PRIVATE_FLAG_CANT_SAVE_STATE,
//           PRIVATE_FLAG_DEFAULT_TO_DEVICE_PROTECTED_STORAGE,
//           PRIVATE_FLAG_DIRECT_BOOT_AWARE,
//           PRIVATE_FLAG_HAS_DOMAIN_URLS,
//           PRIVATE_FLAG_HIDDEN,
//           PRIVATE_FLAG_INSTANT,
//           PRIVATE_FLAG_IS_RESOURCE_OVERLAY,
//           PRIVATE_FLAG_ISOLATED_SPLIT_LOADING,
//           PRIVATE_FLAG_OEM,
//           PRIVATE_FLAG_PARTIALLY_DIRECT_BOOT_AWARE,
//           PRIVATE_FLAG_USE_EMBEDDED_DEX,
//           PRIVATE_FLAG_PRIVILEGED,
//           PRIVATE_FLAG_PRODUCT,
//           PRIVATE_FLAG_SYSTEM_EXT,
//           PRIVATE_FLAG_PROFILEABLE_BY_SHELL,
//           PRIVATE_FLAG_REQUIRED_FOR_SYSTEM_USER,
//           PRIVATE_FLAG_SIGNED_WITH_PLATFORM_KEY,
//           PRIVATE_FLAG_STATIC_SHARED_LIBRARY,
//           PRIVATE_FLAG_VENDOR,
//           PRIVATE_FLAG_VIRTUAL_PRELOAD,
//           PRIVATE_FLAG_HAS_FRAGILE_USER_DATA,
//           PRIVATE_FLAG_ALLOW_CLEAR_USER_DATA_ON_FAILED_RESTORE,
//           PRIVATE_FLAG_ALLOW_AUDIO_PLAYBACK_CAPTURE,
//           PRIVATE_FLAG_REQUEST_LEGACY_EXTERNAL_STORAGE,
//           PRIVATE_FLAG_ODM,
//           PRIVATE_FLAG_ALLOW_NATIVE_HEAP_POINTER_TAGGING,
//   })
// privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION ALLOW_AUDIO_PLAYBACK_CAPTURE PRIVATE_FLAG_REQUEST_LEGACY_EXTERNAL_STORAGE HAS_DOMAIN_URLS PARTIALLY_DIRECT_BOOT_AWARE PRODUCT PRIVATE_FLAG_ALLOW_NATIVE_HEAP_POINTER_TAGGING ]

            int privateFlags = PackageInfoUtils.appInfoPrivateFlags(pkg, ps);
            if (privateFlags != 0) {
                pw.print(prefix); pw.print("  privateFlags="); printFlags(pw,
                        privateFlags, PRIVATE_FLAG_DUMP_SPEC); pw.println();
            }
            if (pkg.hasPreserveLegacyExternalStorage()) {
                pw.print(prefix); pw.print("  hasPreserveLegacyExternalStorage=true");
                pw.println();
            }
            pw.print(prefix); pw.print("  forceQueryable="); pw.print(ps.pkg.isForceQueryable());
            if (ps.forceQueryableOverride) {
                pw.print(" (override=true)");
            }
            pw.println();
            if (ps.pkg.getQueriesPackages().isEmpty()) {
                pw.append(prefix).append("  queriesPackages=").println(ps.pkg.getQueriesPackages());
            }
            if (!ps.pkg.getQueriesIntents().isEmpty()) {
                pw.append(prefix).append("  queriesIntents=").println(ps.pkg.getQueriesIntents());  【请求调用的Intent?】
            }
            File dataDir = PackageInfoWithoutStateUtils.getDataDir(pkg, UserHandle.myUserId());
            pw.print(prefix); pw.print("  dataDir="); pw.println(dataDir.getAbsolutePath());  【应用文件保存路径】
            pw.print(prefix); pw.print("  supportsScreens=[");
            boolean first = true;
            if (pkg.isSupportsSmallScreens()) {
                if (!first)
                    pw.print(", ");
                first = false;
                pw.print("small");
            }
            if (pkg.isSupportsNormalScreens()) {
                if (!first)
                    pw.print(", ");
                first = false;
                pw.print("medium");
            }
            if (pkg.isSupportsLargeScreens()) {
                if (!first)
                    pw.print(", ");
                first = false;
                pw.print("large");
            }
            if (pkg.isSupportsExtraLargeScreens()) {
                if (!first)
                    pw.print(", ");
                first = false;
                pw.print("xlarge");
            }
            if (pkg.isResizeable()) {
                if (!first)
                    pw.print(", ");
                first = false;
                pw.print("resizeable");
            }
            if (pkg.isAnyDensity()) {
                if (!first)
                    pw.print(", ");
                first = false;
                pw.print("anyDensity");
            }
            pw.println("]");
            // 当前APP需要的包Lib-so
            final List<String> libraryNames = pkg.getLibraryNames();
            if (libraryNames != null && libraryNames.size() > 0) {
                pw.print(prefix); pw.println("  dynamic libraries:");
                for (int i = 0; i< libraryNames.size(); i++) {
                    pw.print(prefix); pw.print("    ");
                            pw.println(libraryNames.get(i));
                }
            }
                     // 当前APP需要的包Lib-jar
            if (pkg.getStaticSharedLibName() != null) {
                pw.print(prefix); pw.println("  static library:");
                pw.print(prefix); pw.print("    ");
                pw.print("name:"); pw.print(pkg.getStaticSharedLibName());
                pw.print(" version:"); pw.println(pkg.getStaticSharedLibVersion());
            }

            List<String> usesLibraries = pkg.getUsesLibraries();
            if (usesLibraries.size() > 0) {
                pw.print(prefix); pw.println("  usesLibraries:");
                for (int i=0; i< usesLibraries.size(); i++) {
                    pw.print(prefix); pw.print("    "); pw.println(usesLibraries.get(i));
                }
            }

            List<String> usesStaticLibraries = pkg.getUsesStaticLibraries();
            long[] usesStaticLibrariesVersions = pkg.getUsesStaticLibrariesVersions();
            if (usesStaticLibraries.size() > 0) {
                pw.print(prefix); pw.println("  usesStaticLibraries:");
                for (int i=0; i< usesStaticLibraries.size(); i++) {
                    pw.print(prefix); pw.print("    ");
                    pw.print(usesStaticLibraries.get(i)); pw.print(" version:");
                            pw.println(usesStaticLibrariesVersions[i]);
                }
            }

            List<String> usesOptionalLibraries = pkg.getUsesOptionalLibraries();
            if (usesOptionalLibraries.size() > 0) {
                pw.print(prefix); pw.println("  usesOptionalLibraries:");
                for (int i=0; i< usesOptionalLibraries.size(); i++) {
                    pw.print(prefix); pw.print("    ");
                    pw.println(usesOptionalLibraries.get(i));
                }
            }

            List<String> usesNativeLibraries = pkg.getUsesNativeLibraries();
            if (usesNativeLibraries.size() > 0) {
                pw.print(prefix); pw.println("  usesNativeLibraries:");
                for (int i=0; i< usesNativeLibraries.size(); i++) {
                    pw.print(prefix); pw.print("    "); pw.println(usesNativeLibraries.get(i));
                }
            }

            List<String> usesOptionalNativeLibraries = pkg.getUsesOptionalNativeLibraries();
            if (usesOptionalNativeLibraries.size() > 0) {
                pw.print(prefix); pw.println("  usesOptionalNativeLibraries:");
                for (int i=0; i< usesOptionalNativeLibraries.size(); i++) {
                    pw.print(prefix); pw.print("    ");
                    pw.println(usesOptionalNativeLibraries.get(i));
                }
            }

            List<String> usesLibraryFiles = ps.getPkgState().getUsesLibraryFiles();
            if (usesLibraryFiles.size() > 0) {
                pw.print(prefix); pw.println("  usesLibraryFiles:");
                for (int i=0; i< usesLibraryFiles.size(); i++) {
                    pw.print(prefix); pw.print("    "); pw.println(usesLibraryFiles.get(i));
                }
            }
            final Map<String, ParsedProcess> procs = pkg.getProcesses();
            if (!procs.isEmpty()) {
                pw.print(prefix); pw.println("  processes:");
                for (ParsedProcess proc : procs.values()) {
                    pw.print(prefix); pw.print("    "); pw.println(proc.getName());
                    if (proc.getDeniedPermissions() != null) {
                        for (String deniedPermission : proc.getDeniedPermissions()) {
                            pw.print(prefix); pw.print("      deny: ");
                            pw.println(deniedPermission);
                        }
                    }
                }
            }
        }
        pw.print(prefix); pw.print("  timeStamp=");
            date.setTime(ps.timeStamp);   //安装包的时间吗?
            pw.println(sdf.format(date));
        pw.print(prefix); pw.print("  firstInstallTime=");
            date.setTime(ps.firstInstallTime);
            pw.println(sdf.format(date));
        pw.print(prefix); pw.print("  lastUpdateTime=");
            date.setTime(ps.lastUpdateTime);
            pw.println(sdf.format(date));
        if (ps.installSource.installerPackageName != null) {
            pw.print(prefix); pw.print("  installerPackageName=");
            pw.println(ps.installSource.installerPackageName);
        }
        if (ps.installSource.installerAttributionTag != null) {
            pw.print(prefix); pw.print("  installerAttributionTag=");
            pw.println(ps.installSource.installerAttributionTag);
        }
        if (ps.isPackageLoading()) {
            pw.print(prefix); pw.println("  loadingProgress="
                    + (int) (ps.getIncrementalStates().getProgress() * 100) + "%");
        }
        if (ps.volumeUuid != null) {
            pw.print(prefix); pw.print("  volumeUuid=");
                    pw.println(ps.volumeUuid);
        }
        pw.print(prefix); pw.print("  signatures="); pw.println(ps.signatures); 【包签名】
        pw.print(prefix); pw.print("  installPermissionsFixed=");
        // 将 permissionsFixed 字段标准为 ture ，表示这个 packge 的 permission 进行过修正。后续将禁止对非 system 的 app 的权限进行再次修正
                pw.print(ps.installPermissionsFixed);
                pw.println();
        pw.print(prefix); pw.print("  pkgFlags="); printFlags(pw, ps.pkgFlags, FLAG_DUMP_SPEC);
                pw.println();

        if (pkg != null && pkg.getOverlayTarget() != null) {
            pw.print(prefix); pw.print("  overlayTarget="); pw.println(pkg.getOverlayTarget());
            pw.print(prefix); pw.print("  overlayCategory="); pw.println(pkg.getOverlayCategory());
        }

        if (pkg != null && !pkg.getPermissions().isEmpty()) {
            final List<ParsedPermission> perms = pkg.getPermissions();
            // 该应用自己声明（即自定义）的权限，这里显示了权限名，权限等级
            pw.print(prefix); pw.println("  declared permissions:");
            for (int i=0; i<perms.size(); i++) {  // 
                ParsedPermission perm = perms.get(i);
                if (permissionNames != null
                        && !permissionNames.contains(perm.getName())) {
                    continue;
                }
                pw.print(prefix); pw.print("    "); pw.print(perm.getName());
                pw.print(": prot=");
                pw.print(PermissionInfo.protectionToString(perm.getProtectionLevel()));
                if ((perm.getFlags() &PermissionInfo.FLAG_COSTS_MONEY) != 0) {
                    pw.print(", COSTS_MONEY");
                }
                if ((perm.getFlags() &PermissionInfo.FLAG_REMOVED) != 0) {
                    pw.print(", HIDDEN");
                }
                if ((perm.getFlags() &PermissionInfo.FLAG_INSTALLED) != 0) {
                    pw.print(", INSTALLED");
                }
                pw.println();
            }
        }

        if ((permissionNames != null || dumpAll) && pkg != null
                && pkg.getRequestedPermissions() != null
                && pkg.getRequestedPermissions().size() > 0) {
            final List<String> perms = pkg.getRequestedPermissions();
 //  requested permissions。这里列出的是AndroidManifest.xml文件中所有request的权限，可以看出这里面包含了动态申请的权限和安装时申请的权限
            
            pw.print(prefix); pw.println("  requested permissions:");
            for (int i=0; i<perms.size(); i++) {
                String perm = perms.get(i);
                if (permissionNames != null
                        && !permissionNames.contains(perm)) {
                    continue;
                }
                pw.print(prefix); pw.print("    "); pw.println(perm);
            }
        }

        if (ps.sharedUser == null || permissionNames != null || dumpAll) {
            dumpInstallPermissionsLPr(pw, prefix + "  ", permissionNames, permissionsState, users);
        }

        if (dumpAllComponents) {
            dumpComponents(pw, prefix + "  ", ps);
        }

        for (UserInfo user : users) {  【User 0: 】
            pw.print(prefix); pw.print("  User "); pw.print(user.id); pw.print(": ");
            pw.print("ceDataInode=");
            pw.print(ps.getCeDataInode(user.id));
            pw.print(" installed=");
            pw.print(ps.getInstalled(user.id));
            pw.print(" hidden=");
            pw.print(ps.getHidden(user.id));   // 是否是 隐藏APP
            pw.print(" suspended=");
            pw.print(ps.getSuspended(user.id));
            pw.print(" distractionFlags=");
            pw.print(ps.getDistractionFlags(user.id));
            pw.print(" stopped=");
            pw.print(ps.getStopped(user.id));
            pw.print(" notLaunched=");
            pw.print(ps.getNotLaunched(user.id));
            pw.print(" enabled=");
            pw.print(ps.getEnabled(user.id));
            pw.print(" instant=");
            pw.print(ps.getInstantApp(user.id));
            pw.print(" virtual=");
            pw.println(ps.getVirtulalPreload(user.id));

            if (ps.getSuspended(user.id)) {
                pw.print(prefix);
                pw.println("  Suspend params:");
                final PackageUserState pus = ps.readUserState(user.id);
                for (int i = 0; i < pus.suspendParams.size(); i++) {
                    pw.print(prefix);
                    pw.print("    suspendingPackage=");
                    pw.print(pus.suspendParams.keyAt(i));
                    final PackageUserState.SuspendParams params = pus.suspendParams.valueAt(i);
                    if (params != null) {
                        pw.print(" dialogInfo=");
                        pw.print(params.dialogInfo);
                    }
                    pw.println();
                }
            }

            final OverlayPaths overlayPaths = ps.getOverlayPaths(user.id);
            if (overlayPaths != null) {
                if (!overlayPaths.getOverlayPaths().isEmpty()) {
                    pw.print(prefix);
                    pw.println("    overlay paths:");
                    for (String path : overlayPaths.getOverlayPaths()) {
                        pw.print(prefix);
                        pw.print("      ");
                        pw.println(path);
                    }
                }
                if (!overlayPaths.getResourceDirs().isEmpty()) {
                    pw.print(prefix);
                    pw.println("    legacy overlay paths:");
                    for (String path : overlayPaths.getResourceDirs()) {
                        pw.print(prefix);
                        pw.print("      ");
                        pw.println(path);
                    }
                }
            }

            final Map<String, OverlayPaths> sharedLibraryOverlayPaths =
                    ps.getOverlayPathsForLibrary(user.id);
            if (sharedLibraryOverlayPaths != null) {
                for (Map.Entry<String, OverlayPaths> libOverlayPaths :
                        sharedLibraryOverlayPaths.entrySet()) {
                    final OverlayPaths paths = libOverlayPaths.getValue();
                    if (paths == null) {
                        continue;
                    }
                    if (!paths.getOverlayPaths().isEmpty()) {
                        pw.print(prefix);
                        pw.println("    ");
                        pw.print(libOverlayPaths.getKey());
                        pw.println(" overlay paths:");
                        for (String path : paths.getOverlayPaths()) {
                            pw.print(prefix);
                            pw.print("        ");
                            pw.println(path);
                        }
                    }
                    if (!paths.getResourceDirs().isEmpty()) {
                        pw.print(prefix);
                        pw.println("      ");
                        pw.print(libOverlayPaths.getKey());
                        pw.println(" legacy overlay paths:");
                        for (String path : paths.getResourceDirs()) {
                            pw.print(prefix);
                            pw.print("      ");
                            pw.println(path);
                        }
                    }
                }
            }

            String lastDisabledAppCaller = ps.getLastDisabledAppCaller(user.id);
            if (lastDisabledAppCaller != null) {
                pw.print(prefix); pw.print("    lastDisabledCaller: ");
                        pw.println(lastDisabledAppCaller);
            }

            if (ps.sharedUser == null) {
            // 【gids=[3003]】  gids app的权限的集合
// 一个GIDS相当于一个权限的集合，一个UID可以关联GIDS，表明该UID拥有多种权限

//一个进程就是host应用程序的沙箱，里面一般有一个UID和多个GIDS，每个进程只能访问UID的权限范围内的文件和// GIDs所允许访问的接口，构成了Android最基本的安全基础。

                dumpGidsLPr(pw, prefix + "    ", mPermissionDataProvider.getGidsForUid(
                        UserHandle.getUid(user.id, ps.appId)));
                dumpRuntimePermissionsLPr(pw, prefix + "    ", permissionNames, permissionsState
                        .getPermissionStates(user.id), dumpAll);
            }

            String harmfulAppWarning = ps.getHarmfulAppWarning(user.id);
            if (harmfulAppWarning != null) {
                pw.print(prefix); pw.print("      harmfulAppWarning: ");
                pw.println(harmfulAppWarning);
            }

            if (permissionNames == null) {
                ArraySet<String> cmp = ps.getDisabledComponents(user.id);
                if (cmp != null && cmp.size() > 0) {
                    pw.print(prefix); pw.println("    disabledComponents:");
                    for (String s : cmp) {
                        pw.print(prefix); pw.print("      "); pw.println(s);
                    }
                }
                cmp = ps.getEnabledComponents(user.id);
                if (cmp != null && cmp.size() > 0) {
                    pw.print(prefix); pw.println("    enabledComponents:");
                    for (String s : cmp) {
                        pw.print(prefix); pw.print("      "); pw.println(s);
                    }
                }
            }
        }
    }


```



```
/frameworks/base/core/java/android/content/pm/ApplicationInfo.java
APK的动态权限 记录

    void dumpRuntimePermissionsLPr(PrintWriter pw, String prefix, ArraySet<String> permissionNames,
            Collection<PermissionState> permissionStates, boolean dumpAll) {
        boolean hasRuntimePermissions = false;
        for (PermissionState permissionState : permissionStates) {
            if (permissionState.isRuntime()) {
                hasRuntimePermissions = true;
                break;
            }
        }
        if (hasRuntimePermissions || dumpAll) {
            pw.print(prefix); pw.println("runtime permissions:");
            for (PermissionState permissionState : permissionStates) {
                if (!permissionState.isRuntime()) {
                    continue;
                }
                if (permissionNames != null
                        && !permissionNames.contains(permissionState.getName())) {
                    continue;
                }
                pw.print(prefix); pw.print("  "); pw.print(permissionState.getName());
                         【动态权限是否被授权】
               pw.print(": granted="); pw.print(permissionState.isGranted()); 
               // case FLAG_PERMISSION_GRANTED_BY_DEFAULT: return "GRANTED_BY_DEFAULT";
// case FLAG_PERMISSION_POLICY_FIXED: return "POLICY_FIXED";
// case FLAG_PERMISSION_SYSTEM_FIXED: return "SYSTEM_FIXED";
// case FLAG_PERMISSION_USER_SET: return "USER_SET";
// case FLAG_PERMISSION_USER_FIXED: return "USER_FIXED";
// case FLAG_PERMISSION_REVIEW_REQUIRED: return "REVIEW_REQUIRED";
// case FLAG_PERMISSION_REVOKE_WHEN_REQUESTED: return "REVOKE_WHEN_REQUESTED";
// case FLAG_PERMISSION_USER_SENSITIVE_WHEN_GRANTED: return "USER_SENSITIVE_WHEN_GRANTED";
// case FLAG_PERMISSION_USER_SENSITIVE_WHEN_DENIED: return "USER_SENSITIVE_WHEN_DENIED";
// case FLAG_PERMISSION_RESTRICTION_INSTALLER_EXEMPT: return "RESTRICTION_INSTALLER_EXEMPT";
// case FLAG_PERMISSION_RESTRICTION_SYSTEM_EXEMPT: return "RESTRICTION_SYSTEM_EXEMPT";
// case FLAG_PERMISSION_RESTRICTION_UPGRADE_EXEMPT: return "RESTRICTION_UPGRADE_EXEMPT";
// case FLAG_PERMISSION_APPLY_RESTRICTION: return "APPLY_RESTRICTION";
// case FLAG_PERMISSION_GRANTED_BY_ROLE: return "GRANTED_BY_ROLE";
// case FLAG_PERMISSION_REVOKED_COMPAT: return "REVOKED_COMPAT";
// case FLAG_PERMISSION_ONE_TIME: return "ONE_TIME";
// case FLAG_PERMISSION_AUTO_REVOKED: return "AUTO_REVOKED";
      //android.permission.ACCESS_FINE_LOCATION: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]


                    pw.println(permissionFlagsToString(", flags=",
                            permissionState.getFlags()));
            }
        }
    }
    


```



```

    void dumpInstallPermissionsLPr(PrintWriter pw, String prefix,
            ArraySet<String> filterPermissionNames, LegacyPermissionState permissionsState,
            List<UserInfo> users) {
        ArraySet<String> dumpPermissionNames = new ArraySet<>();
        for (UserInfo user : users) {
            int userId = user.id;
            Collection<PermissionState> permissionStates = permissionsState.getPermissionStates(
                    userId);
            for (PermissionState permissionState : permissionStates) {
                if (permissionState.isRuntime()) {
                    continue;
                }
                String permissionName = permissionState.getName();
                if (filterPermissionNames != null
                        && !filterPermissionNames.contains(permissionName)) {
                    continue;
                }
                dumpPermissionNames.add(permissionName);
            }
        }
        boolean printedSomething = false;
        for (String permissionName : dumpPermissionNames) {
            PermissionState systemPermissionState = permissionsState.getPermissionState(
                    permissionName, UserHandle.USER_SYSTEM);
            for (UserInfo user : users) {
                int userId = user.id;
                PermissionState permissionState;
                if (userId == UserHandle.USER_SYSTEM) {
                    permissionState = systemPermissionState;
                } else {
                    permissionState = permissionsState.getPermissionState(permissionName, userId);
                    if (Objects.equals(permissionState, systemPermissionState)) {
                        continue;
                    }
                }
                if (!printedSomething) {
                // install permissions：安装的时候就赋予的权限 
                    pw.print(prefix); pw.println("install permissions:");
                    printedSomething = true;
                }
                pw.print(prefix); pw.print("  "); pw.print(permissionName);
                pw.print(": granted="); pw.print(
                        permissionState != null && permissionState.isGranted());
                pw.print(permissionFlagsToString(", flags=",
                        permissionState != null ? permissionState.getFlags() : 0));
                if (userId == UserHandle.USER_SYSTEM) {
                    pw.println();
                } else {
                    pw.print(", userId="); pw.println(userId);
                }
            }
        }
    }


```



#### Queries:  【HeadOneTag】

```
Queries:  【HeadOneTag】
  system apps queryable: false
  forceQueryable:
  queries via package name:
  queries via intent:
  queryable via interaction:
  queryable via uses-library:
  

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









## WIFI问题汇总



### 断线问题



### CTS&VTS问题



### 无法连接问题



### wifi无法开启问题



### 热点无法开启问题



### Passpoint网络问题





## GPS问题汇总



### 无法定位问题



### GPS信号弱问题



### AGPS场测问题(LPP)



### 定位缓慢问题



### CTS/VTS 达标问题





## BT问题汇总





## NFC问题汇总






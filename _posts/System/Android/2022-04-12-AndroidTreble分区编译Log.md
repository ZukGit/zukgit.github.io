---
layout: post
title: Android编译Log
category: 安卓
tags: Android Vendor
keywords: Android Vendor Compiler
typora-root-url: ..\..\..\
typora-copy-images-to: ..\..\..\public\zimage
---

## 简介
 * TOC
 {:toc}
 

## Android_编译状态


### Vendor-Img


#### Vendor_1
```
══════════════1════════════════
   583	zukgit_build_nonhlos.sh_56_20221125_011953 $ {command}=vendor/qcom/nonhlos/CDSP.HT.2.8.1/cdsp_proc/build.sh$
  5414	zukgit_build_nonhlos.sh_48_20221125_011953 $ {retVal}=0$
  5416	zukgit_build_nonhlos.sh_50_20221125_011953 $ {retVal}=0$
++++++++++++++++++++++++++
echo zukgit_build_nonhlos.sh_54_20221125_011953  "$ {command}=${command}"
echo zukgit_build_nonhlos.sh_55_20221125_011953    "$ {command}=${command}"
echo zukgit_build_nonhlos.sh_56_20221125_011953    "$ {command}=${command}"
time $command
retVal=$?
echo zukgit_build_nonhlos.sh_57_20221125_011953  "$ {retVal}=${retVal}"
echo zukgit_build_nonhlos.sh_58_20221125_011953    "$ {retVal}=${retVal}"
echo zukgit_build_nonhlos.sh_59_20221125_011953    "$ {command}=${command}"
++++++++++++++++++++++++++
 13441	zukgit_build_nonhlos.sh_57_20221125_011953 $ {retVal}=0$
 13442	zukgit_build_nonhlos.sh_58_20221125_011953 $ {retVal}=0$
 13443	zukgit_build_nonhlos.sh_59_20221125_011953 $ {command}=vendor/qcom/nonhlos/CDSP.HT.2.8.1/cdsp_proc/build.sh$
══════════════════════════════
```

#### Vendor_2
```
══════════════2════════════════
 15483	zukgit_build_nonhlos.sh_50_20221125_011953 $ {retVal}=0$
++++++++++++++++++++++++++
echo zukgit_build_nonhlos.sh_56_20221125_011953    "$ {command}=${command}"
time $command
retVal=$?
echo zukgit_build_nonhlos.sh_57_20221125_011953  "$ {retVal}=${retVal}"
++++++++++++++++++++++++++
 38668	zukgit_build_nonhlos.sh_57_20221125_011953 $ {retVal}=0$
 38669	zukgit_build_nonhlos.sh_58_20221125_011953 $ {retVal}=0$
 38670	zukgit_build_nonhlos.sh_59_20221125_011953 $ {command}=vendor/qcom/nonhlos/ADSP.HT.5.7_6450/adsp_proc/build.sh$
══════════════════════════════
```

#### Vendor_3
```

═════════════3═════════════════
 38994	zukgit_/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/vendor/qcom/nonhlos/Netrani.LA.1.0/common/build.sh_91_20221125_011954 $ {BUILD_SCRIPT_DIR}=vendor/qcom/nonhlos/Netrani.LA.1.0/common$
++++++++++++++++++++++++++
echo zukgit_/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/vendor/qcom/nonhlos/Netrani.LA.1.0/common/build.sh_91_20221125_011954    "$ {BUILD_SCRIPT_DIR}=${BUILD_SCRIPT_DIR}"
cd ${BUILD_SCRIPT_DIR}/build
python build.py --nonhlos --cmm --imf
ret=$?
echo zukgit_/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/vendor/qcom/nonhlos/Netrani.LA.1.0/common/build.sh_92_20221125_011954  "$ {ret}=${ret}"
++++++++++++++++++++++++++
 50383	zukgit_/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/vendor/qcom/nonhlos/Netrani.LA.1.0/common/build.sh_92_20221125_011954 $ {ret}=0$
══════════════════════════════
```

#### Vendor_4
```

═════════════4═════════════════

 50491	zukgit_build_kernel.sh_20_20221125_011953 $ {command}=kernel_platform/build/android/prepare_vendor.sh parrot consolidate$
++++++++++++++++++++++++++
echo zukgit_build_kernel.sh_20_20221125_011953    "$ {command}=${command}"
time $command
retVal=$?
echo zukgit_build_kernel.sh_21_20221125_011953  "$ {retVal}=${retVal}"
echo zukgit_build_kernel.sh_22_20221125_011953    "$ {retVal}=${retVal}"
echo zukgit_build_kernel.sh_23_20221125_011953    "$ {command}=${command}"
++++++++++++++++++++++++++

 60730	zukgit_build_kernel.sh_21_20221125_011953 $ {retVal}=0$
 60731	zukgit_build_kernel.sh_22_20221125_011953 $ {retVal}=0$
 60732	zukgit_build_kernel.sh_23_20221125_011953 $ {command}=kernel_platform/build/android/prepare_vendor.sh parrot consolidate$
══════════════════════════════
```

#### Vendor_5_(Vendor_msi.sh)
```

═════════════5_[Vendor_build_msi_device.sh]═════════════════
 61142	zukgit_build_msi_device.sh_156_20221125_011953 $ {command}=make -j96 TARGET_PRODUCT=genevn TARGET_BUILD_VARIANT=userdebug droid BUILD_NUMBER=eng.userX.221125.012626 dist MOT_NO_GMS=0 MOT_PARTIAL_GMS=0 target-files-package otatools-package ENABLE_AB=true BOARD_DYNAMIC_PARTITION_ENABLE=true$
++++++++++++++++++++++++++
echo zukgit_build_msi_device.sh_156_20221125_011953    "$ {command}=${command}"
time $command
retVal=$?
echo zukgit_build_msi_device.sh_157_20221125_011953  "$ {retVal}=${retVal}"
echo zukgit_build_msi_device.sh_158_20221125_011953    "$ {retVal}=${retVal}"
echo zukgit_build_msi_device.sh_159_20221125_011953    "$ {command}=${command}"
echo zukgit_build_msi_device.sh_160_20221125_011953    "$ {ignore}=${ignore}"
check_return_value $retVal "$command" $ignore
++++++++++++++++++++++++++
332189	zukgit_build_msi_device.sh_157_20221125_011953 $ {retVal}=0$
332190	zukgit_build_msi_device.sh_158_20221125_011953 $ {retVal}=0$
332191	zukgit_build_msi_device.sh_159_20221125_011953 $ {command}=make -j96 TARGET_PRODUCT=genevn TARGET_BUILD_VARIANT=userdebug droid BUILD_NUMBER=eng.userX.221125.012626 dist MOT_NO_GMS=0 MOT_PARTIAL_GMS=0 target-files-package otatools-package ENABLE_AB=true BOARD_DYNAMIC_PARTITION_ENABLE=true$
332192	zukgit_build_msi_device.sh_160_20221125_011953 $ {ignore}=0$
══════════════════════════════


```

#### Vendor_6_(MSI_msi.sh)
```

═════════════6_[MSI_build_msi_device.sh]═════════════════
333847	zukgit_build_msi_device.sh_159_20221124_124334 $ {command}=unzip -o out/dist/merged-msi_genevn-target_files.zip -d /home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/out/target/product/genevn/obj/PACKAGING/target_files_intermediates/genevn-target_files-eng.userX.221125.012626$
++++++++++++++++++++++++++

echo zukgit_build_msi_device.sh_158_20221124_124334    "$ {command}=${command}"
log "Command: \"$command\""
echo zukgit_build_msi_device.sh_159_20221124_124334    "$ {command}=${command}"
time $command
retVal=$?
echo zukgit_build_msi_device.sh_160_20221124_124334  "$ {retVal}=${retVal}"
echo zukgit_build_msi_device.sh_161_20221124_124334    "$ {retVal}=${retVal}"
echo zukgit_build_msi_device.sh_162_20221124_124334    "$ {command}=${command}"
echo zukgit_build_msi_device.sh_163_20221124_124334    "$ {ignore}=${ignore}"


++++++++++++++++++++++++++
350175	zukgit_build_msi_device.sh_160_20221124_124334 $ {retVal}=0$
350176	zukgit_build_msi_device.sh_161_20221124_124334 $ {retVal}=0$
350177	zukgit_build_msi_device.sh_162_20221124_124334 $ {command}=unzip -o out/dist/merged-msi_genevn-target_files.zip -d /home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/out/target/product/genevn/obj/PACKAGING/target_files_intermediates/genevn-target_files-eng.userX.221125.012626$
350178	zukgit_build_msi_device.sh_163_20221124_124334 $ {ignore}=0$
══════════════════════════════

```

#### Vendor_7_COMPLETED
```

═════════════7_Vendor_BUILD COMPLETED═════════════════
copy /home/userX/Desktop/GenevnT/Code1/Vendor4/out/build-genevn.ninja > /home/userX/Desktop/GenevnT/Code1/Vendor4/release/build_info/build-genevn.ninja
zukgit_build_device.bash_49_20221125_011953 $ {platform_dir}=/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4
zukgit_build_device.bash_50_20221125_011953 $ {product}=genevn
zukgit_build_device.bash_51_20221125_011953 $ {build_info_dir}=/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/release/build_info
zukgit_build_device.bash_52_20221125_011953 $ {product}=genevn
zukgit_build_device.bash_53_20221125_011953 $ {product}=genevn
INFO: Copied build-genevn.ninja to the release folder
zukgit_build_device.bash_54_20221125_011953 $ {platform_dir}=/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4
zukgit_build_device.bash_57_20221125_011953 $ {platform_dir}=/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4
zukgit_build_device.bash_58_20221125_011953 $ {platform_dir}=/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4
zukgit_build_device.bash_59_20221125_011953 $ {build_info_dir}=/home/userX/Desktop/AndroidCode/GenevnT/Code1/Vendor4/release/build_info
INFO: Copied soong/build.ninja to the release folder
zukgit_build_device.bash_60_20221125_011953 $ {ENABLE_RBE}=
.......BUILD COMPLETED........
zukgit_build_device.bash_12_20221125_011953 ______func_begin___________ zfunc on_exit ___________
zukgit_build_device.bash_24_20221125_011953 ______func_begin___________ zfunc rbe_exit ___________
zukgit_build_device.bash_25_20221125_011953 $ {rbe_started}=
══════════════════════════════
```


### MSI-Img


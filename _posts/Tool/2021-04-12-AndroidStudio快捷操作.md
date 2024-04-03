---
layout: post
title: AndroidStudio快捷使用操作
category: 工具
tags: AndroidStudio 
keywords: AndroidStudio 
typora-root-url: ..\..\
typora-copy-images-to: ..\..\public\zimage
---

## 简介
 * TOC
 {:toc}

## 功能设置说明  


###  添加系统framework.jar 

#### 引入framework.jar 

```
通过在AOSP中使用命令 make javac-check-framework 编译 framework
一般来说，使用 Android 源码全编译之后，
会生成 out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes.jar
将改 classes.jar 改为 framework.jar 之后导入 AndroidStudio 

 make javac-check-framework

```

  <img src="/public/zimage/tool/androidstudio/2024-04-01_173408.jpg">


#### 确保自动生成 *.iml 按钮开关打开


File->Setting-> Gradle 界面 确保 自动生成 .iml文件按钮被选中(如下图) 
Generate x.iml files for modules imported from Gradle  所描述按钮需要被选中


 <img src="/public/zimage/tool/androidstudio/2024-04-01_180737.jpg">




#### 项目Module的app/build.gradle末尾添加如下代码
```
println("____________________ env info begin ____________________ ");
Map<String,String> envMap = System.getenv();
if(envMap != null ){
    Set<String> keySet = envMap.keySet();
    int keyIndex = 0 ;
    int keySetSize = keySet.size();
    for(String keyItem:keySet){
        String valueItem = envMap.get(keyItem);
        println(" Gradle_EnvMap["+keyIndex+"_"+keySetSize+"]    key["+keyItem+"]  value["+valueItem+"]");
        keyIndex++;
    }
}
println("____________________ env info end ____________________ ")
println()

println("____________________ project info begin ____________________ ");
println("project.rootDir = "+ project.rootDir);
println("project.buildDir = "+ project.buildDir);
println("project.buildFile = "+ project.buildFile);
println("projectDir = "+ projectDir);
println("rootDirName = "+ project.rootDir.getName());
File mainImlFile = new File(project.rootDir.getAbsolutePath()+File.separator+".idea/modules/app/"+project.rootDir.getName()+".app.main.iml");
println("mainImlFile = "+ mainImlFile.getAbsolutePath() +"  isExist:["+mainImlFile.exists()+"]");
println("____________________ project.getProperties() ");
Set<String> projectPropSet = project.getProperties().keySet();
int projectPropIndex = 0 ;
int projectPropSetSize = projectPropSet.size();
for(String keyItem:projectPropSet){
    String valueItem = project.getProperties().get(keyItem);
    println(" Project_Prop["+projectPropIndex+"_"+projectPropSetSize+"]    key["+keyItem+"]  value["+valueItem+"]");
    projectPropIndex++;
}
println();
println("____________________ project info end ____________________ ");
println()

println("____________________ android info begin ____________________ ");

println("android.properties.toString() "+ android.properties.toString())

Set<String> mAndroidKeySet = android.properties.keySet();
if( mAndroidKeySet != null && mAndroidKeySet.size() > 0){
    int mAndroidKeyIndex = 0 ;
    int mAndroidKeySetSize = mAndroidKeySet.size();
    for(String mKeyItem:mAndroidKeySet){
        String mValueItem = android.properties.get(mKeyItem);
        println(" AndroidProp["+mAndroidKeyIndex+"_"+mAndroidKeySetSize+"]    key["+mKeyItem+"]  value["+mValueItem+"]");
        mAndroidKeyIndex++;
    }
}
println("____________________ android info end ____________________ ");


println("____________________ components info begin ____________________ ");

components.toString()
ext.toString()
allprojects.toString()
dependencies.toString()
preBuild.toString()
gradle.toString()
println("components.toString()="+components.getNames().toString());
println("ext.toString()="+ext.getProperties().toString());
println("allprojects.toString()="+allprojects.properties.toString());
println("dependencies.toString()="+dependencies.getProperties().toString());
println("preBuild.toString()="+preBuild.toString());
println("gradle.toString()="+gradle.toString());

println("____________________ components info end ____________________ ");


println("____________________ local.properties info begin ____________________ ");
Properties localProps = new Properties()
localProps.load(new FileInputStream(project.rootProject.file("local.properties")))
Set<String> propKeySet = localProps.keySet();
if( propKeySet != null && propKeySet.size() > 0){
    int propKeyIndex = 0 ;
    int propKeySetSize = propKeySet.size();
    for(String propKeyItem:propKeySet){
        String propValueItem = localProps.getProperty(propKeyItem);
        println(" LocalProp["+propKeyIndex+"_"+propKeySetSize+"]    key["+propKeyItem+"]  value["+propValueItem+"]");
        propKeyIndex++;
    }
}
println("____________________ local.properties info end ____________________ ");
println()


preBuild {
    doLast {
        // 注意：iml的路径要根据自己的实际情况来写 mainImlFile 
		//make ( jdkType="Android SDK") at end of orderEntry list at  ./idea/modules/app/**.app.main.iml
      def imlFile = file(mainImlFile.getAbsolutePath())
        try {
            def parsedXml = (new XmlParser()).parse(imlFile)

            int orderEntry_jdk_indx = -1 ;
            for (int i = 0 ;i <= 10 ; i ++){
                def jdkNode = parsedXml.component[i].orderEntry.find { it.'@type' == 'jdk' }
                if(jdkNode != null){
                    orderEntry_jdk_indx = i;
                    break;
                }
            }
            if(orderEntry_jdk_indx == -1){
                println("error!   not find the jdk in"+ mainImlFile.absolutePath+" !  orderEntry_jdk_indx = "+ orderEntry_jdk_indx)
            } else {


                def jdkNode = parsedXml.component[orderEntry_jdk_indx].orderEntry.find { it.'@type' == 'jdk' }
                println("parsedXml.component["+orderEntry_jdk_indx+"] = "+ parsedXml.component[orderEntry_jdk_indx])


                println("jdkNodne = "+ jdkNode)
                println("jdkName = "+ jdkNode.attributes().get('jdkName'))

                parsedXml.component[orderEntry_jdk_indx].remove(jdkNode)

                def sdkString = jdkNode.attributes().get('jdkName');

                new groovy.util.Node(parsedXml.component[orderEntry_jdk_indx], 'orderEntry', ['type': 'jdk', 'jdkName': sdkString, 'jdkType': 'Android SDK'])
                groovy.xml.XmlUtil.serialize(parsedXml, new FileOutputStream(imlFile))

            }



        } catch (FileNotFoundException e) {

            println(" e = "+ e)
        }
        println(" _________________ finish !!! __________________ ")
    }

}





```


#### 检查 app/build.gradle 对 framework.jar的依赖配置

```
选中SarControlService/app/build.gradle  检查 dependencies {} 配置项中 
存在 compileOnly files('libs\\framework.jar') 配置项,  
如果存在 implementation files('libs\\framework.jar') 那么需要修改为 

implementation files('libs\\framework.jar')    改为
compileOnly files('libs\\framework.jar') 


之后执行Gradle Run命令将正常引入 framework.jar 文件  

```


  <img src="/public/zimage/tool/androidstudio/2024-04-01_182331.jpg">

####   选中 File->Invalid Cache 后重启生效framework.jar 


  <img src="/public/zimage/tool/androidstudio/2024-04-01_173536.jpg">


### 添加Exterl Tool命令

```
选中 File->Setting-> Tool -> External Tool -> + 按钮 -> 弹出 EditTool 窗口 
配置如下信息：　 
 
Name:  ndk-build_jni (可自定义) 
 
Program: C:\Users\xxxx\AppData\Local\Android\Sdk\ndk\26.2.11394342\build\ndk-build.cmd 【依赖PC本地环境自行配置】 
 
Arguments:  NDK_LIBS_OUT=$ProjectFileDir$\app\src\main\libs   【固定】 
 
Working Directory: D:\jira_work\AOSP\APP\app\src\main\jni 【项目jni路径 依赖PC环境 自行配置】         
 
点击OK 按钮后配置成功JNI 编译命令  在 Working Directory 右键 External Tool 就可以执行

```


  <img src="/public/zimage/tool/androidstudio/2024-04-01_175638.jpg">
  <img src="/public/zimage/tool/androidstudio/2024-04-01_175906.jpg">


### 设置Gradle国内镜像

#### gradle/wrapper/gradle-wrapper.properties 修改

Gradle下载的默认路径:

C:\Users\xxxUserNamexxxx\.gradle\wrapper\dists 路径下


 <img src="/public/zimage/tool/androidstudio/2024-04-03_141202.jpg">

##### 腾讯 gradle镜像
需要设置两种镜像:

Gradle镜像:  Gradle文件本身
Gradle依赖镜像: Gradle 能work 它自己所依赖文件的镜像
原始 gradle-wrapper.properties 下载速度极慢 需要更改 distributionUrl  
```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-8.0-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists



```

distributionUrl=https\://services.gradle.org/distributions/gradle-8.0-all.zip

变更为 

distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.1.1-all.zip


```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
# Gradle镜像  一般需要下载 all的镜像 和 bin 的镜像
#distributionUrl=https\://services.gradle.org/distributions/gradle-8.0-all.zip
#distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.2.1-all.zip
distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.1.1-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists


```


#### 设置(Gradle依赖镜像)

C:\Users\xxxUserNamexxxx\.gradle\wrapper\dists\xxx当前gradle目录\init.d\

新建 init.d 目录下文件 init.gradle
C:\Users\xxx\.gradle\wrapper\dists\gradle-6.5-all\2oz4ud9k3tuxjg84bbf55q0tn\gradle-6.5\init.d\init.gradle
之后 就设置了 Gradle依赖镜像 

新建目录 C:\Users\xxx\.gradle\wrapper\dists\gradle-6.5-all\2oz4ud9k3tuxjg84bbf55q0tn\gradle-6.5\init.d\init.gradle

init.gradle
```


 allprojects{
  repositories {
   def ALIYUN_REPOSITORY_URL = 'https://maven.aliyun.com/repository/public/'
   def ALIYUN_JCENTER_URL = 'https://maven.aliyun.com/repository/jcenter/'
   def ALIYUN_GOOGLE_URL = 'https://maven.aliyun.com/repository/google/'
   def ALIYUN_GRADLE_PLUGIN_URL = 'https://maven.aliyun.com/repository/gradle-plugin/'
   all { ArtifactRepository repo ->
    if(repo instanceof MavenArtifactRepository){
     def url = repo.url.toString()
     if (url.startsWith('https://repo1.maven.org/maven2/')) {
      project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_REPOSITORY_URL."
      remove repo
     }
     if (url.startsWith('https://jcenter.bintray.com/')) {
      project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_JCENTER_URL."
      remove repo
     }
     if (url.startsWith('https://dl.google.com/dl/android/maven2/')) {
      project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_GOOGLE_URL."
      remove repo
     }
     if (url.startsWith('https://plugins.gradle.org/m2/')) {
      project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_GRADLE_PLUGIN_URL."
      remove repo
     }
    }
   }
   maven { url ALIYUN_REPOSITORY_URL }
   maven { url ALIYUN_JCENTER_URL }
   maven { url ALIYUN_GOOGLE_URL }
   maven { url ALIYUN_GRADLE_PLUGIN_URL }
  }
 }
 


```
参考  https://blog.csdn.net/sunboylife/article/details/129065316

gradle-6.5\init.d\init.gradle 的另一种配置方式
init.gradle

```

allprojects {
    repositories {
        mavenLocal()
        maven { name "Alibaba" ; url "https://maven.aliyun.com/repository/public" } 
        maven { name "Bstek" ; url "https://nexus.bsdn.org/content/groups/public/" } 
        mavenCentral()
    }
    
    buildscript {
        repositories {
            maven { name "Alibaba" ; url 'https://maven.aliyun.com/repository/public' } 
            maven { name "Bstek" ; url 'https://nexus.bsdn.org/content/groups/public/' } 
            maven { name "M2" ; url 'https://plugins.gradle.org/m2/' }
        }
    }
}

```


#### 查看AGP(AndroidStuido自身 Gradle Plugin)插件

在项目根目录的 build.gradle 指明了 AGP的版本
1.AGP版本 
2.Gradle版本  
3.java版本
4.AndroidStudio版本

不配套版本会存在兼容性问题导致无法编译

```
      classpath 'com.android.tools.build:gradle:4.1.1'
	  
```


https://developer.android.google.cn/build/releases/gradle-plugin?hl=zh-cn#updating-plugin


| AGP插件版本 | 所需的最低Gradle版本 |
| ---- | ---- |
| 8.4 | 8.6-rc-1 |
| 8.3 | 8.4 |
| 8.2 | 8.2 |
| 8.1 | 8.0 |
| 8.0 | 8.0 |
| 7.4 | 7.5 |
| 7.3 | 7.4 |
| 7.2 | 7.3.3 |
| 7.1 | 7.2 |
| 7.0 | 7.0 |
| 4.2.0+ | 6.7.1 |
| 4.1.0+ | 6.5+ |
| 4.0.0+ | 6.1.1+ |
| 3.6.0-3.6.4 | 5.6.4+ |
| 3.5.0-3.5.4 | 5.4.1+ |
| 3.4.0-3.4.3 | 5.1.1+ |
| 3.3.0-3.3.3 | 4.10.1+ |
| 3.2.0-3.2.1 | 4.6+ |
| 3.1.0+ | 4.4+ |
| 3.0.0+ | 4.1+ |
| 2.3.0+ | 3.3+ |
| 2.1.3-2.2.3 | 2.14.1-3.5 |
| 2.0.0-2.1.2 | 2.10-2.13 |
| 1.5.0 | 2.2.1-2.13 |
| 1.2.0-1.3.1 | 2.2.1-2.9 |
| 1.0.0-1.1.3 | 2.2.1-2.3 |


|AS代号| Android_Studio版本	|所需的 AGP 版本|
| ---- | ---- |---- |
| 水母        | 2023.3.1	    |3.2-8.4  |
| Iguana      | 2023.2.1  	    |3.2-8.3  |
| Hedgehog    | 2023.1.1	    |3.2-8.2  |
| Giraffe     | 2022.3.1	    |3.2-8.1  |
| Flamingo    | 2022.2.1	    |3.2-8.0  |
| Electric Eel| 2022.1.1        |3.2-7.4  |
| Dolphin     | 2021.3.1        |3.2-7.3  |
| Chipmunk    | 2021.2.1        |3.2-7.2  |
| Bumblebee   | 2021.1.1        |3.2-7.1  |
| Arctic Fox  | 2020.3.1        |3.1-7.0  |


### 设置代理





## 快捷按钮操作说明

### A

####     打开搜索菜单以及动作的搜索框



```
 Mac：  ？？

 windows:   Ctrl + N     // 搜索菜单以及动作

```


  <img src="/public/zimage/tool/androidstudio/2024-04-03_113613.jpg">

####     添加关键文件到 Favorite 方便查找

```
 Favorite的标签在窗口左下角，用于放置当前用户比较关注的文件以及书签 F11 ， Shift+F11查看
 Favorite 框内有从 Favorite列表删除的 - 按钮选项


 Mac: 通过右键点击文件Tab窗口  选择  add to Favorites  即可添加到 关注列表
 Windows:    通过右键点击文件Tab窗口  选择  add to Favorites  即可添加到 关注列表
              windows下  alt+2 打开 Favorites列表窗口

```
 <img src="/public/zimage/tool/androidstudio/33.jpg">

 <img src="/public/zimage/tool/androidstudio/34.jpg">



####     打开Message Logcat Version-Contrl的快捷键
 Alt + 数字    【界面有显示对应的数字】
```
 Mac:
 command + 1    // 打开Project窗口
 command + 2   //  打开 Favorites窗口
 command + 6   // 打开logcat 窗口
 command + 7   // 打开类结构 class struct 窗口
 command + 8   // 打开类继承关系 窗口
 command + 9   // 打开版本控制窗口


 Windows：
 Alt + 1    // 打开Project窗口
 Alt + 2   //  打开 Favorites窗口
 Alt + 6   // 打开logcat 窗口
 Alt + 7   // 打开类结构 class struct 窗口
 Alt + 8   // 打开类继承关系 窗口
 Alt + 9   // 打开版本控制窗口

```
 <img src="/public/zimage/tool/androidstudio/40.jpg">
 <img src="/public/zimage/tool/androidstudio/35.jpg">

### B

####     tab文件窗口前一个文件 和 后一个文件
 与浏览器的前进与回退相似的功能，查看之前浏览Tab切换位置（回退）
```

 Mac：   command + 【     回退  和   command + 】前进

 Windows:

 Alt +   ←   回退浏览记录
 Alt +   →   前进浏览机器
  Ctrl + Alt +   ←            Tab文件，Tab往后下一个文件
  Ctrl + Alt +   →            Tab文件，Tab往后下一个文件

```
 <img src="/public/zimage/tool/androidstudio/36.jpg">
 <img src="/public/zimage/tool/androidstudio/37.jpg">

### C
####     跳转到大括号{ 对应的}括号地方
```

 Windows :   Ctrl + [   和  Ctrl + ]    //跳转到大括号{ 对应的}括号地方 括号区域查询
```
 <img src="/public/zimage/tool/androidstudio/13.jpg">

 <img src="/public/zimage/tool/androidstudio/14.jpg">

####     选中大括号{ 对应的}括号地方
```

 Windows:   Ctrl + Shift +  [  和 Ctrl + Shift +  ]    
```
 <img src="/public/zimage/tool/androidstudio/22.jpg">

 <img src="/public/zimage/tool/androidstudio/23.jpg">
####     注释选中的代码
```
 注释| 反注释 选中的代码
 Mac :   command + /       // 两个斜杆的注释   单行注释
 Windows：   Ctrl  +  /      

  
 Mac :   command + shift +  /       // 多行注释  /*     */
 Windows：   Ctrl  + shift+  /      

```
 **单行注释**
 <img src="/public/zimage/tool/androidstudio/16.jpg">

 <img src="/public/zimage/tool/androidstudio/17.jpg">

 **多行注释**
 <img src="/public/zimage/tool/androidstudio/18.jpg">

 <img src="/public/zimage/tool/androidstudio/19.jpg">


### D
### E

####     显示最近打开的窗口列表和文件列表 Ctrl + E
```
 最近打开的文件以及窗口 Recent File
 Mac：   command + E
 windows:  
  Ctrl + E    
```
 <img src="/public/zimage/tool/androidstudio/45.jpg">
### F
####     当前代码页面查找 Ctrl +F 搜索
```
 Mac:   command + F //   当前页面内查找

 Windows:   Ctrl + F //   当前页面内查找
```
 <img src="/public/zimage/tool/androidstudio/8.jpg">


####     全局工程搜索 Shift + Ctrl + F 搜索
```
 Mac:  Shift + command + F //   全工程页面内查找

 Windows:   Shift + Ctrl + F //   全工程页面内查找  windows下需要转输入为大写才能打开！
```
 <img src="/public/zimage/tool/androidstudio/9.jpg">


####     全局搜索变量以及方法名
```
 Mac: ？？
 Windows:   Ctrl + Shift + Alt + N
```
 <img src="/public/zimage/tool/androidstudio/46.jpg">
 <img src="/public/zimage/tool/androidstudio/47.jpg">

####     在关键代码出增加标签 bookmark

```

 windows:   F11     // 在代码出新增 标签
 windows:   Shift + F11   // 打开当前记录的标签集合
```
 <img src="/public/zimage/tool/androidstudio/30.jpg">

 <br/>
 <img src="/public/zimage/tool/androidstudio/31.jpg">
 <img src="/public/zimage/tool/androidstudio/32.jpg">


####     对Add Frarorite 增加快捷按键  F7  默认为 Ctrl+Shift+F
```
 1. 打开设置窗口  Ctrl + Alt + S   选中 keymap
 2. 在keymap中搜索 add Favorites
 3. 移除之前的快捷按键 （Step Out ）F7   
 4. 增加新的按键  F7  为   add Favorites



```

### G



####     (git)Update Prjrct
```
 Mac:
 command + T   =>  Update Prjrct

 Windows:
 Ctrl +  T 

 作用:  用于同步更新服务器上的代码，同步其他人的修改。
```
 <img src="/public/zimage/tool/androidstudio/1.jpg">



####     (git)Branch信息
```
 在IDE的右下方是 当前代码的分支信息。

 Remotes Branches：  远程分支
 Local Branches: 当前的本地分支 (每个本地的分支对应一个远程的分支)
     通过点击  new Branch创建一个本地的分支
     可通过点击Remote Branches 的 checkout as new Local branch来产生本地分支

 Current Branch: 显示的是本地的当前分支

 TEMP分支合入Master分支的步骤：
 1. 在 TEMP分支进行修改 然后 commit到本地 并 push到远程分支   origin:TEMP
 2. 在本地分支中切换到 Master本地分支，并在右下方选择 Local Branches中的 TEMP 右键选中 merger
 3. merger 之后 本地的 Master分支就已经合入了 步骤1中的修改，在本地Master分支 测试验证，最后通过
    Shift + command + K  打开 push commits界面把  本地 Master(实际是TEMP的修改) 传到 远程分支 origin：Master
```

 <img src="/public/zimage/tool/androidstudio/5.jpg">




####     (git)Commit Changes

```
 Mac:
 command + K   =>  Commit Changes

 Windows:
 Ctrl +  K 

 作用:  用于提交commit本地代码库
```
 <img src="/public/zimage/tool/androidstudio/3.jpg">
```
 menu菜单说明:

```
 <img src="/public/zimage/tool/androidstudio/2.jpg">



####     (git)Push Commits
```
 Mac:
 command + Shift + K   =>  Push Commits

 Windows:
 Ctrl +  K 

 作用: 用于提交本地commit 提交push到远程分支中去

```
 <img src="/public/zimage/tool/androidstudio/4.jpg">

### H
####     查看当前类的继承关系  Ctrl + H
```

 Mac:   Ctrl + H 

 Windows:   Ctrl + H 
```
 <img src="/public/zimage/tool/androidstudio/12.jpg">
### I
### J
### K
####     keyMap 当前AS快捷键模式使用的类型
```
 keyMap 当前AS快捷键模式使用的类型, 默认Default  Mac类型   Windows类型  Eclipse类型 Visual类型 等， 可依据习惯自由切换

 Ctrl + Shift  + A   ， 再输入mapkey  打开配置 keymap设置框

```
 <img src="/public/zimage/tool/androidstudio/38.jpg">
 <img src="/public/zimage/tool/androidstudio/39.jpg">
 <img src="/public/zimage/tool/androidstudio/41.jpg">


####     keyboard shortcut 为Action增加快捷按键
```
 有些实用的功能默认没有配置快捷按钮，可自定义设置快捷按键


 Windows:
 Clt + Alt + S    首先打开keymap 然后在输入框搜索关键词 找到 Action
 右键Action 增加  keyboard shortcut 
 例如：   Open  in  Explorer  在文件管理器打开该文件   设置为了  Ctrl + Shift + N

```
 <img src="/public/zimage/tool/androidstudio/42.jpg">
### L
####     整理代码格式
```
 Mac:   command + Alt + L 

 Windows:   Ctrl + Alt + L 

```
 <img src="/public/zimage/tool/androidstudio/26.jpg">
 <img src="/public/zimage/tool/androidstudio/27.jpg">
### M
### N

####     全局搜索代码类的名字
```
 Mac ： command + O

 Windows:   Ctrl + N     // 尼玛不一样的快捷键~~~  有必要同一下
```
 <img src="/public/zimage/tool/androidstudio/15.jpg">

####     全局工程搜索文件名
```
 Mac: ?? //   全工程页面内查找文件名

 Windows:   Shift + Ctrl + N //   全工程页面内查找文件名  windows下需要转输入为大写才能打开！
```
 <img src="/public/zimage/tool/androidstudio/10.jpg">


### O
####     显示当前类可以重写override和实现implements的所有父类方法
```
 Mac ： ？？ 

 Windows:  Ctrl + O     // 显示当前类可以重写和实现的方法列表
```
 <img src="/public/zimage/tool/androidstudio/11.jpg">
### P
### Q
### R
### S
####     Setings设置页面快捷键  Preferebce
```
 Mac：  command + ，     // command+逗号键   打开设置界面

 Windows:  Ctrl + Alt + S     // 打开设置界面
```
 <img src="/public/zimage/tool/androidstudio/6.jpg">
####     Setings( Module ) 打开模块工程界面  Project Structure
```
 Mac：  command + ；     // command+分号   打开工程模块界面

 Windows:  Shift + Ctrl + Alt + S     // 打开工程模块界面
           Windows下 选中工程项目(必须是工程目录)   + F4    // 也可打开 工程模块目录
```
 <img src="/public/zimage/tool/androidstudio/7.jpg">

### T
####     打开终端 Terimal
```
 Mac  ：  Alt + Fn + F12  //  打开终端
 Windows:   Alt + F12   //  打开终端


```
 <img src="/public/zimage/tool/androidstudio/25.jpg">

### U
####     把选中的代码，全部大写|小写转换
```
 Mac :   ??
 windows:   Ctrl + Shift + U
```
 <img src="/public/zimage/tool/androidstudio/20.jpg">
 <img src="/public/zimage/tool/androidstudio/21.jpg">
### V
### W
### X
### Y
### Z
####     撤销之前的代码输入
```
 Mac:  command + Z   或者  command + Alt + Z   

 Windows:  Ctrl +  Z    或者  Ctrl + Alt + Z

```
 <img src="/public/zimage/tool/androidstudio/28.jpg">
 <img src="/public/zimage/tool/androidstudio/29.jpg">

### 快捷输入操作

####      对代码块进行整体的上下移动
```

 Windows:
 Alt + ↓
 Alt + ↑
```
 <img src="/public/zimage/tool/androidstudio/43.jpg">
 <br/>
 ---
 <img src="/public/zimage/tool/androidstudio/44.jpg">

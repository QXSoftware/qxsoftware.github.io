---
title: Unity SDK 环境搭建（Android Studio）
tags: Unity
header-img: img/grass.jpg
published: true
---

### Android Studio 环境搭建

本文用于搭建配合 Unity 使用的 Android Studio 开发环境。

#### Android SDK

下载安装 Android SDK，并且安装必要的组件。这里说明一下如何设置 Android SDK 的代理。

打开 Android SDK Manager 后，打开 *Tools/Options*，Http Proxy Server 设置为：mirrors.neusoft.edu.cn，端口：80。

同时选中 Force https://... sources to be fetched using http://...，如下所示：

![](/post_img/proxy.jpg)

设置好了代理后，安装所需的 Android SDK Build-tools，以及所需的 Android SDK Platform 版本。

#### Gradle 以及 Android Studio Gradle Plugin

首先安装 Gradle，可以修改下面这个配置文件：
platform/android/sdkproject/gradle/wrapper/gradle-wrapper.properties

> 这里可以设置为相对路径，如下：
 distributionUrl=../../cache/gradle-3.4.1-all.zip
 其中 ../../ 展开后变成：
 G:\ff\ff_client\platform\android\sdkproject
 也可以设置为 http 链接，如下：
 distributionUrl=http\://10.5.56.167/share/android_studio/gradle-3.4.1-all.zip

然后修改 Android Studio 工程根目录的 build.grade，设置 gradle 插件版本号为合适的版本号：
platform/android/sdkproject/build.gradle

> dependencies {
    classpath 'com.android.tools.build:gradle:2.3.0'
    // NOTE: Do not place your application dependencies here; they belong
    // in the individual module build.gradle files
}

最后设置 Gradle 为离线模式：

![](/post_img/offline-work.jpg)

---
layout: post
title:  "Android Studio Gradle 下载"
date:   2019-01-20
excerpt: "没有VPN又没有钱"
tag:
- Android
- 笔记
---

# Android Studio Gradle 下载

> Android Studio版本

```
Android Studio 3.2.1
Build #AI-181.5540.7.32.5056338, built on October 9, 2018
JRE: 1.8.0_152-release-1136-b06 x86_64
JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
macOS 10.14.2
```

所以第一次打开的时候下载失败

解决办法：https://biuy.github.io/Android-Studio-SDK-下载/[](https://biuy.github.io/Android-Studio-SDK-下载/)

然后`try again`再跑一遍就会开始自动下载各种东西（包括Gradle）

------

如果单纯地只想更新`Gradle`也可以

## 离线更新Gradle

![](https://ws4.sinaimg.cn/large/006tNc79ly1fzd93onoywj318k0u0q9x.jpg)

注意 `Service directory path` 应该是

```
~/.gradle
```

也就是图里写的那个位置`/Users/biu/.gradle`

> 注意勾选`Offline work`

`.gradle`下的目录格式

![](https://ws2.sinaimg.cn/large/006tNc79ly1fzd9m2c3d3j30ta0fudpm.jpg)

然后把在官网下载的gradle包的**zip文件**放在如图所示的Hash值文件夹下面

> **不需要解压，android studio自己会去解包**

### 不推荐离线更新

因为还要下好多东西，如果最开始跑`Hello World`过不了的话，https://biuy.github.io/Android-Studio-SDK-下载/[](https://biuy.github.io/Android-Studio-SDK-下载/)并且去掉勾选`Offline work`可以解决绝大多数问题

## Gradle 构建项目

> 来自[https://blog.csdn.net/wzy_1988/article/details/48652747](https://blog.csdn.net/wzy_1988/article/details/48652747)

使用Gradle来构建项目的时候，需要对Gradle的配置文件有个大概的了解，以我的一个测试应用项目为例，Gradle的配置文件主要有：

1. 每个模块的gradle配置文件。
2. 整个项目的gradle配置文件。
3. 统一管理gradle的gradle-wrapper配置文件。
4. 整个项目的模块引用配置文件。

如下图所示：

![](https://img-blog.csdn.net/20150922141342811)

接下来，我根据上述标记的红色部分进行逐一讲解。

------

## **XYALL/app/build.gradle(模块gradle配置文件)**

我们首先来看一下这个配置文件的内容：

```java
// 声明是android程序
apply plugin: 'com.android.application'

android {
    // 编译SDK的版本
    compileSdkVersion 23
    // build tools的版本
    buildToolsVersion "23.0.1"

    defaultConfig {
        // 应用包名
        applicationId "com.example.wzy.xyall"
        // 支持最低设备sdk的版本
        minSdkVersion 19
        // 支持目标设备sdk的版本
        targetSdkVersion 23
        // 应用版本号
        versionCode 1
        // 应用版本名称
        versionName "1.0"
    }

    buildTypes {
        release {
            // 是否进行混淆
            minifyEnabled false
            // 混淆文件的位置
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }

    // 移除lint检查的error，防止编译终止
    lintOptions {
        abortOnError false
    }
}

dependencies {
    // 编译libs目录下所有的jar包
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:23.0.1'
}
```

有几点需要说明：

1. 文件开头apply plugin，如果是编译apk，值为`com.android.application`，如果编译的是库，则需要改为`com.android.library`。

2. buildToolsVersion必须是你本地安装的版本，可以通过SDK MANAGER来进行查看。这个值配置不对，会造成编译错误。

------

## **XYALL/build.gradle(整个项目的gradle配置文件)**

文件内容如下：

```java
buildscript {
    repositories {
        // gradle插件下载中心为jcenter
        jcenter()
    }
    // gralde插件的具体版本。
    dependencies {
        classpath 'com.android.tools.build:gradle:1.3.0'
    }
}

// 项目中使用到的库、jar包的下载中心
allprojects {
    repositories {
        jcenter()
    }
}
```

------

## **XYALL/gradle/wrapper/gradle-wrapper.properties（gradle版本统一管理文件）**

文件内容如下：

```properties
#Mon Sep 21 12:15:49 CST 2015
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.4-all.zip
```

gradle-wrapper的作用就是使用统一的方式来管理gradle，保证gradle使用的是统一的版本。说明几点：

1. android studio首先从distributionBase/distributionPath查找gradle。
2. 然后，从zipStoreBase/zipStorePath查找gradle。
3.如果上述都没有找到合适的gradle，则从distributionUrl指定的url去下载gradle。
4.
> 注意：这里需要在.bashrc中增加GRADLE_USER_HOME的变量定义。

## **settings.gradle（项目模块引用配置文件）**
这个是全局的项目配置文件，里面主要声明一些需要加入gradle的模块。

```java
include ':app'
```

示例项目的配置表示只要app模块的build.gradle加入到编译中。

------

### 编译
上述配置完成后，就可以使用gradle编译项目了。常用的构建命令如下：

1. gradle clean: 清除之前的构建。
2. gradle test：执行测试。
3. gradle compileJava：编译java。
4. gradle check：代码检查。
5. gradle build：构建打包。

最后再提示一下，构建打包完成后，编译出来的apk位于：

$项目/app/build/outputs/apk/目录下。
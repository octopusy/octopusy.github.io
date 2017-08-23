---
layout:     post

title:      compileSdkVersion, minSdkVersion 和 targetSdkVersion 的区别

category:   [Android]

description: android，sdkVersion

keywords: android,sdkVersion

---

## compileSdkVersion, minSdkVersion 和 targetSdkVersion 的区别

> 在我们发布一个Android项目之后，当google发布一个新的SDK版本之后，我们如何保证我们应用在新的
  SDK上面不会出现异常呢？


Android Sdk是遵循向下兼容的原则，当新的Android Sdk出现后，我们仅需要在我们项目中指定对应的compileSdkVersion,minSdkVersion,targetSdkVersion版本即可，下面我们就来介绍下，三者的作用，控制API的级别和对应应用的兼容模式。



     android {
       compileSdkVersion 23
       buildToolsVersion "23.0.1"

       defaultConfig {
         applicationId "com.example.checkyourtargetsdk"
         minSdkVersion 7
         targetSdkVersion 23
         versionCode 1
         versionName "1.0"
       }
     }

### compileSdkVersion

compileSdkVersion 当前Gradle编译使用的Sdk版本，当使用新的Android API的时候，需要设置对应的Level的Android SDK。

需要强调的是修改compileSdkVersion不会改变运行时的行为,当你修改了compileSdkVersion的时候，可能会出现新的编译警告、编译错误，但新的 compileSdkVersion 不会被包含到 APK 中：它纯粹只是在编译的时候使用。


### minSdkVersion

minSdkVersion 是指当前项目支持最小Android API的版本。

如果 compileSdkVersion 设置为可用的最新 API，那么 minSdkVersion 则是应用可以运行的最低要求。minSdkVersion 是 Google Play 商店用来判断用户设备是否可以安装某个应用的标志之一。

在开发时 minSdkVersion 也起到一个重要角色：lint 默认会在项目中运行，它在你使用了高于 minSdkVersion  的 API 时会警告你，帮你避免调用不存在的 API 的运行时问题。如果只在较高版本的系统上才使用某些 API，通常使用运行时检查系统版本的方式解决。

### targetSdkVersion

targetSdkVersion 是 Android 提供向前兼容的主要依据，在应用的 targetSdkVersion 没有更新之前系统不会应用最新的行为变化。这允许你在适应新的行为变化之前就可以使用新的 API 。


### 总结

  按照上面示例那样配置，三者之间的关系应该是：

  minSdkVersion <= targetSdkVersion <= compileSdkVersion

  minSdkVersion (lowest possible) <= targetSdkVersion == compileSdkVersion (latest SDK)

  用较低的 minSdkVersion 来覆盖最大的人群，用最新的 SDK 设置 target 和 compile 来获得最好的外观和行为。







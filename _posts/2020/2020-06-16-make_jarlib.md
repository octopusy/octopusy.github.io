---
layout:     post

title:      几招搞定开源库的封装和发布

category:   [Android]

description: android,JitPack

keywords: 开源库,封装,发布,JitPack

---

## 几招搞定开源库的封装和发布

开发过程中，我们经常会引用到一些第三方开源包，比如Glide图片库

**compile 'com.github.bumptech.glide:glide:4.3.0'**

那么，如何制作自己的开源项目呢？下面让我们来看下制作属于自己开源项目的步骤。

### JitPack发布

#### 一：发布方法

**本地配置**

**示例：**

![脚本配置](/assets/images/Android/JitPack/jitpack01.png)

**库模块的buil.grade开头添加：**

![脚本配置](/assets/images/Android/JitPack/jitpack02.png)

#### 二：GitHub上创建版本

代码传好后，在GitHub的【Code】页，选择“releases”进去新建版本。注意版本号要认真填写，例如可以是：v1.0.0

release title和描述可以简单写下release notes，写完后点击“Publish release”就完成了一次版本发布。其实这个时候GitHub帮你做的就是打包。

![脚本配置](/assets/images/Android/JitPack/jitpack03.png)

![脚本配置](/assets/images/Android/JitPack/jitpack04.png)

#### 三：JitPack查询

打开JitPack | Publish JVM and Android libraries，输入GitHub项目地址，例如：octopusy/AndroidLibrary
点击查询，则可以查询到发布的版本列表。

![脚本配置](/assets/images/Android/JitPack/jitpack05.png)

![脚本配置](/assets/images/Android/JitPack/jitpack06.png)

#### 四：集成使用
选取一个最新的版本，点击“Get it”，下面会在“How to”里出现集成使用方法，非常简单。示例可以参考：octopusy/AndroidLibrary: 易盾验证码android应用嵌入演示

如果在AndroidStudio工程中集成成功，则JitPack上对应的版本Log一栏会出现绿色的图标，否则会出现红色的出错图标，可以点开查看错误信息。

```
 allprojects {
	repositories {
		...
  		maven { url 'https://jitpack.io' }
 	}
 }


 dependencies {
      implementation 'com.github.octopusy:AndroidLibrary:v1.0.0'
 }
```

![脚本配置](/assets/images/Android/JitPack/jitpack07.png)






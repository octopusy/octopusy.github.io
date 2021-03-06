---
layout:     post

title:     Android透明度设置详解

category:   [Android]

description: android

keywords: 透明度设置

---

## Android透明度设置详解

## 本节前言
> 今天给大家介绍的是关于Android各式各样的透明度，有需要的希望能够帮到你们,我们在学习本节课之前，先来介绍一下万能的颜色透明度

### 颜色透明度
格式：

`android:background="#XXxxxxxx"（颜色可以写在color中）`

说明：半透明颜色值不同于平时使用的颜色，半透明颜色值共8位，前2位是透明度，后6位是颜色。也就是说透明度和颜色结合就可以写出各种颜色的透明度。下面是透明度说明表，供大家参考。


| 透明度 | 百分比 | 前缀 |
| --- | --- | --- |
| 不透明 | 100% | FF |
|  | 95% | F2 |
|  | 90% | E6 |
|  | 85% | D9 |
|  | 80% | CC |
|  | 75% | BF |
|  | 70% | B3 |
|  | 65% | A6 |
|  | 60% | 99 |
|  | 55% | 8C |
| 半透明 | 50 | 80 |
|  | 45% | 73 |
|  | 40% | 66 |
|  | 35% | 59 |
|  | 30% | 4D |
|  | 25% | 40 |
|  | 20% | 33 |
|  | 15% | 26 |
|  | 10% | 1A |
|  | 5% | 0D |
| 全透明 | 0 | 00 |


**部分透明度示例：**

- 全透明：#00000000
- 半透明：#80000000
- 不透明：#FF000000
- 白色半透明：#80FFFFFF
- 红色30%透明：#4Dca0d0d

### 控件透明度

1. **Java代码实现设置透明度**

```
text = (TextView) findViewById(R.id.text);
text.getBackground().setAlpha(12);
```

setAlpha()的括号中可以填0–255之间的数字。数字越大，越不透明。

>注：这里需要注意的是，控件必须是在最外层布局里面，如果直接设置最外层布局会出错

**注意点：**
在5.0以上系统时，有些机型会出现莫名其妙的颜色值不起作用，变成透明了，也就是用此方法会导致其他共用一个资源的布局（例如：@color/white）透明度也跟着改变。比如text用上述方法设置成透明后，项目中，其他用到text颜色值的控件，都变成透明了。

原因：在布局中多个控件同时使用一个资源的时候，这些控件会共用一个状态，例如ColorState，如果你改变了一个控件的状态，其他的控件都会接收到相同的通知。这时我们可以使用mutate()方法使该控件状态不定，这样不定状态的控件就不会共享自己的状态了。


2. 在xml布局中进行设置

```
<TextView
    android:text="Hello World!"
    android:background="#987654"
    android:layout_width="match_parent"
    android:alpha="0.5"
    android:layout_height="100dp" />
```

android:alpha的值为0~1之间的数。数字越大，越不透明。1表示完全不透明，0表示完全透明。

3. 通过颜色透明度进行设置

```
<TextView
        android:id="@+id/text"
        android:text="Hello World!"
        android:background="#80987654"
        android:layout_width="match_parent"
        android:layout_height="100dp" />
```

### Activity透明

说道Activity透明，发现网上的基本上都已经过时，在有v7以上的控件都无法实现，均会报错
> You need to use a Theme.AppCompat theme (or descendant) with the design library.

所以如若你的布局xml文件有 support-V7 上的控件的话，<style name="translucent">里的name要前要添加 AppTheme，如：
```
<style name=" AppTheme.translucent">
```

1. 方法一：

- 在 res/values/color.xml 文件下加入一个透明颜色值，这里的 color 参数，是两位数一个单位，前两位数是透明度（16进制：00 -- FF，最大为256，数值越低越透明），后面每两位一对是16进制颜色数字，示例中为白色。

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
   <color name="translucent_background">#80000000</color>
</resources>
```


- 在 res/values/styles.xml 文件中加入一个自定义样式，代码如下。

```
<!-- item name="android:windowBackground"         设置背景透明度及其颜色值 -->
<!-- item name="android:windowIsTranslucent"      设置当前Activity是否透明-->
<!-- item name="android:windowAnimationStyle"     设置当前Activity进出方式-->
<style name="translucent">
    <item name="android:windowBackground">@color/translucent_background</item>
    <item name="android:windowIsTranslucent">true</item>
    <item name="android:windowAnimationStyle">@android:style/Animation.Translucent</item>
</style>
```


2. 方法二：

- 在Activity的布局xml的根标签中写入透明颜色：

```
android:background="#80000000"
```

- 在 AndroidManifest.xml 找到要实现透明的 Activity，在想要实现透明的 Activity 中配置其属性，如下：

```
android:theme="@android:style/Theme.Translucent.NoTitleBar"
```

---
layout:     post

title:     代码规范和团队协作规章

category:   [协同开发]

description: 代码规范

keywords: 代码规范和团队协作规章

---

> 好的开发习惯，可以帮助我们和团队走的更远

## 代码规范

### Java规范：

#### 一、自定义标识符

##### 1、标识符的细节

① 标识符的组成元素是由字母（a-zA-Z）、数字(0-9) 、下划线(_)、美元符号($)

② 标识符不能以数字开头

③ 标识符是严格区分大小写的

④ 标识符的长度是没有长度限制的

⑤ 标识符的命名一般要有意义（见名知意）

⑥ 关键字、保留字不能用于自定义的标识符

##### 2、标识符的规范

① 类名和接口名单词的首字母大写，其他单词小写（SunTime）

② 变量名与方法名首单词全部小写，其他单词首字母大写，其他单词都要小写 [ doCook() ]

③ 包名全部单词小写

④ 常量全部单词大写，单词与单词之间使用下划线分隔（UP_LENGTH）

3、判断一下那些是符合的标识符

① 12abc_ [不合法。数字不能开头]

② _12abc [合法]

③ $ab12# [不合法。#号不属于标识符组成元素]

④ abc@123 [不合法。@号不属于标识符组成元素]

#### 二、注释

##### 1、注释的类别

第一种： 单行注释 // 注释的内容

第二种： 多行注释 /* 注释的内容 */

第三种： 文档注释 /** 注释的内容 */ 文档注释也是一个多行注释

注意：单行注释可以嵌套使用，多行注释是不能嵌套使用的

##### 2、多行注释与文档注释的区别

多行注释的内容不能用于生成一个开发者文档，而文档注释的内容可以生产一个开发者文档。

##### 3、使用javadoc开发工具即可生成一个开发者文档

使用格式：javadoc -d 指定存放文档的路径 -version –author(可选) 目标文件

注意细节：

① 如果一个类需要使用javadoc工具生成一个软件的开发者文档，那么该类必须使用public修饰

② 文档注释注释的内容一般都是位于类或者方法的上面的

③ @author 作者 @version 版本 @param 方法的参数 @return 返回值

##### 4、规范：一般单行注释是位于代码的右侧，多行注释与文档注释一般是写在类或者方法的上面的。

### Android规范

#### 一： 开发注意事项

##### 1：属性，变量，方法，对象，驼峰式命名，根据用途定义名称，优先用英文，次则中文拼音，不要出现数字填充命名

#####  2：代码要做到重用，复用，尽量不要写重复的代码

#####  3：方法和对象要有注释说明

#####  4：项目需要国际化的，其中涉及到中文的，必须写在配置文件中

##### 5：项目代码里面不要出现原生Log打印

####  二、Android 资源文件命名与使用
##### 1. 【推荐】资源文件需带模块前缀。
##### 2. 【推荐】layout 文件的命名方式。
```
Activity 的 layout 以 module_activity 开头

Fragment 的 layout 以 module_fragment 开头

Dialog 的 layout 以 module_dialog 开头

include 的 layout 以 module_include 开头

ListView 的行 layout 以 module_list_item 开头

RecyclerView 的 item layout 以 module_recycle_item 开头

GridView 的行 layout 以 module_grid_item 开头
```

##### 3. 【推荐】 drawable 资源名称以小写单词+下划线的方式命名，根据分辨率

不同存在不同的 mipmap目录下，建议只使用一套，例如 mipmap-xhdpi。

采用规则如模块名_业务功能描述_控件描述_控件状态限定词 如：module_login_btn_pressed,module_tabs_icon_home_normal

#####  4. 【推荐】anim 资源名称以小写单词+下划线的方式命名，

采用以下规则： 模  块名_逻辑名称_[方向|序号]
```
tween 动画资源 ： 尽可能以通用的动画名称命名，
如  module_fade_imodule_fade_out , module_push_down_in (动画+方向)；

frame 动画资源：尽可能以模 块+功能命名+序号。如：module_loading_grey_0
```
#####  5. 【推荐】 color 资源使用#AARRGGBB 格式

写入 module_colors.xml，文件名格式采用以下规则： 模块名_逻辑名称_颜色 如： #33b5e5e5

#####  6. 【推荐】dimen 资源以小写单词+下划线方式命名

写入module_dimens.xml 文件中采用以下规则： 模块名_描述信息 如： 1dp

#####  7. 【推荐】style 资源采用小写单词+下划线方式命名

写入module_styles.xml 文件中采用以下规则： 父 style 名称.当前 style 名如：

…

#####  8. 【推荐】string资源文件或者文本用到字符需要全部写入
module_strings.xml文件中字符串以小写单词+下划线的方式命名，采用以下规则： 模块名_逻辑名称 如：moudule_login_tips,module_homepage_notice_desc

#####  9. 【推荐】Id 资源原则上以驼峰法命名

View 组件的资源 id 需要以 View 的缩写作前缀。常用缩写表如下： 控件 缩写
```
LinearLayout ll

RelativeLayout rl

ConstraintLayout cl

ListView lv

ScollView sv

TextView tv

Button btn

ImageView iv

CheckBox cb

RadioButton rb

EditText et
```

#### 二、Android 资源文件命名与使用

其它控件的缩写推荐使用小写字母并用下划线进行分割，例如： ProgressBar 对应的缩写为 progress_bar DatePicker 对应的缩写为 date_picker

【推荐】大分辨率图片（单维度超过 1000）大分辨率图片建议统一放在 xxhdpi

目下管理，否则将导致占用内存成倍数增加。 说明： 为了支持多种屏幕尺寸和密度，Android 为多种屏幕提供不同的资源目录进行适配为不同屏幕密度提供不同的位图可绘制对象，可用于密度特定资源的配置限定符下面详述） 包括 ldpi（低）、mdpi（中）、 hdpi（高）、xhdpi（超高）、xxhdpi 超高）和 xxxhdpi（超超超高）。例如，高密度屏幕的位图应使用 mipmap-hd根据当前的设备屏幕尺寸和密度，将会寻找最匹配的资源，如果将高分辨率图片入低密度目录，将会造成低端机加载过大图片资源，又可能造成 OOM，同时也是源浪费，没有必要在低端机使用大图。 正例： 将 144*144 的应用图标 PNG 文件放在 mipmap-xxhdpi 目录 反例： 将 144*144 的应用图标 PNG 文件放在 mipmap-mhdpi 目录

#### 三：其它规范

##### 间距规范 
```
一栏上边距24dp

普通大间距16dp

分割线0.5dp

中框左右边距16dp

文字上下边距4dp

左右边距8dp

默认view高度48dp

区域分割线8dp

字体排版：

大标题34sp

标题20sp

副标题16sp

正文14sp

辅助信息12sp

提示文字7sp

按钮文字14sp

常用字号：

12sp: 小字提示

14sp(桌面端13sp): 正文/按钮文字

16sp(桌面端13sp)：小标题

20sp: AppBar文字

24sp: 大标题

34sp/45sp/56sp/112sp/超大号文字

所有可操作元素最小点击区域尺寸：48dp*48dp

栅格系统的最小单位是8dp,一切距离，尺寸都应该是8dp的整数倍，以下是一些常见的尺寸与距离：

·顶部状态栏高度：24dp

·AppBar最小高度：56dp

·底部导航栏高度：48dp

·悬浮按钮尺寸：56*56dp/40*40dp

·用户头像尺寸：64*64dp/40*40dp

·小图标点击区域：48*48dp

·侧边抽屉到屏幕右边的距离：56dp

·卡片间距：8dp

·分割线上下留白：8dp

·大多数元素的留白距离：16dp

·屏幕左右对齐基线：16dp

·文字左侧对齐基线：72dp
```

## 团队协同开发规章

###  一：事先约定

约定绝对是提高团队协作最有用的方式之一。大家先约定好一些规则，然后接下来各自干各自的，并且遵循着这个约定，这样便大大提高了效率。 一个项目可以约定的东西有很多，比如约定一种 [分支策略](https://link.zhihu.com/?target=https%3A//github.com/zhanzizhen/coding-blog/issues/18)，约定一种代码风格（通过 eslint 插件来执行），约定接口文档等等。

#### 1：统一工具和版本

“工欲善其事必先利其器”，统一团队成员开发工具和依赖插件版本是一件非常重要的事情，可以避免后期大量的项目合并问题。减少项目开发周期。

#### 2：命名约定

命名冲突是开发过程中常见的一个问题，特别是在多人协作开发的项目中。团队协作开发一般会以模块进行进行划分，在命名时，可以通过加上模块名称作为前缀或者后缀，可以避免大部分的命名冲突问题，大大提示团队开发的效率。

### 二：统一管理

#### 1：善用代码版本管理工具

在团队开发过程中，必须约定每日统一上传合并代码，每个团队成员必须单独创建一个分支进行开发，自测试通过后再合并到主分支上，这样可以避免后期花费大量的时间排查和解决问题。



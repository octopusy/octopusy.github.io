---
layout:     post

title:     Android客户端入门开发

category:   [Android]

description: android

keywords: Android入门

---

## Android客户端入门开发

背景

为了让大家对Android开发流程有个整体的认识，我们用一张流程图来描述下一个客户端从开发，测试，到打包，上架的完整流程，具体流程如下图所示：

![1.png](https://upload-images.jianshu.io/upload_images/4677526-c9d4dbbbcea69f48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

开发前准备

1: 开发工具

JDK

因为Android本质上也是用的Java语言开发，所以编译环境也需要用到Java虚拟机，在开发之前，我们需要下载JDK，并配置JDK环境

JDK下载地址：[https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

同意协议后，下载相应版本的JDK，具体的环境配置教程可自行百度，网上很多指导教程，这里就不详述了

![2.png](https://upload-images.jianshu.io/upload_images/4677526-6b59e522b159991c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Android Studio

工欲善其事必先利其器，在准备开发之前，我们同样需要下载和安装Android专业的IDE开发工具，目前针对Android开发主要有两款IDE，分别是Eclipse和Android Studio，但是目前主流用的基本上都是Android studio，下面我们就主要分享一下Android Studio的安装与配置。

Android IDE和插件下载地址：[https://www.androiddevtools.cn/](https://www.androiddevtools.cn/)

![3.png](https://upload-images.jianshu.io/upload_images/4677526-d34d675068502b01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

IDE的版本，可以根据大家的系统和电脑配置进行选择，具体的下载和安装流程，相信应该难不到大家，下面主要和大家分享下IDE的几个重要配置。

*   Android SDK配置，开发之前一定需要先配置这个SDK路径，否则编译运行的时候会报错提示。

![4.png](https://upload-images.jianshu.io/upload_images/4677526-35873d7b2030ae4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![5.png](https://upload-images.jianshu.io/upload_images/4677526-4af98ef2cc846341.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*   gradle配置，这个地方主要配置gradle编译的路径

![6.png](https://upload-images.jianshu.io/upload_images/4677526-3dad73e3d2c484ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


项目开发

为了引导大家入门，下面我们就以一个简单的示例来跟大家分享下Android开发的流程和步骤

1：新建项目

![7.png](https://upload-images.jianshu.io/upload_images/4677526-cb7d65b188243757.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![8.png](https://upload-images.jianshu.io/upload_images/4677526-c392e12545f81ed8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

新建完项目之后，我们通过上图来分析下Android工程的项目结构：

*   libs文件夹主要用于存放项目或者模块需要的第三方依赖包文件
*   main->java目录下主要存放项目的开发代码
*   res下-mipmap和drawable文件夹主要用于存放一些UI设计的资源文件和自定义布局文件
*   res下-layout文件夹主要用于存放UI布局页面文件
*   androidMainfest主要是项目或者模块的配置文件，主要用于注册一些Activity/服务或者广播等配置
*   build.gradle主要是项目或者模块的配置文件，主要用于配置项目的版本和编译打包文件等脚本文件

2：项目架构设计

项目采用MVP设计模式，针对MVP具体的介绍这里就先不展开描述了。

其中业务处理逻辑全部放在P层，像App中网络请求模块全部都放在P层来处理，

```
override fun requestHomeData(url: String) {
    mModel.requestHomeData(object : MySubscriber<HomeDataRespModel>(mView.context) {
        override fun _onNext(t: HomeDataRespModel?) {
            Logger.d(Gson().toJson(t))
            if (t!!.isOk()) {
                //解密 encryptData
                mView.requestResult(t)
            } else {
                mView.requestFailResult(t.errorMsg)
            }
        }

        override fun _onError(message: String?) {
            Logger.e(message)
        }
    }, url)
}
```

view层，负责UI上数据的展示和刷新
```
override fun initViewsAndEvents() {
    super.initViewsAndEvents()
    recycler_home.layoutManager = LinearLayoutManager(this)
    recycler_home.isNestedScrollingEnabled = false
    adapter = HomeDataAdapter()
    recycler_home.adapter = adapter

    adapter!!.setOnItemClickLitener { v, dataModel, position ->
        mBundle = Bundle()
        mBundle!!.putString("title",dataModel.title)
        mBundle!!.putString("url",dataModel.link)
        openActivity(WebViewActivity::class.java,mBundle)
    }

    refresh_home.autoRefresh()
    refresh_home.setOnRefreshListener { refreshlayout ->
        isRefresh = true
        adapter!!.clear()
        currentPageNum = 0
        //上拉刷新
        requestData()
        refreshlayout.finishRefresh(1500)
    }
    refresh_home.setOnLoadmoreListener { refreshlayout ->
        isRefresh = false
        currentPageNum++
        //下拉加载
        requestData()
        refreshlayout.finishLoadmore(1500)
    }
    refresh_home.isEnableLoadmore = true
    refresh_home.refreshHeader = WaterDropHeader(this)
    refresh_home.refreshFooter = BallPulseFooter(this).setSpinnerStyle(SpinnerStyle.Scale)
}

private fun requestData() {
    mPersenter.requestHomeData(ConstantConfig.URL_REQUEST_HOME_DATA + currentPageNum + "/json")
}

override fun requestResult(homeDataRespModel: HomeDataRespModel) {
    try {
        adapter!!.appendToList(homeDataRespModel.data!!.datas)
    } catch (e:Exception) {
        Logger.e(e.message)
    }
}

override fun requestFailResult(errMsg: String) {
    ToastUtils.showShort(errMsg)
}
```

3：打包发布

编译项目后，可以直接选择Build Apk，然后直接取release目录下的apk包，提供外部安装使用。

![9.png](https://upload-images.jianshu.io/upload_images/4677526-c89c77a2ac34afca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后我们来看下最终的效果实现图

App主页

![10.jpg](https://upload-images.jianshu.io/upload_images/4677526-f7d5229914168f90.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


App内容详情页：

![11.jpg](https://upload-images.jianshu.io/upload_images/4677526-5c4b78edfd272210.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后我们附上项目的开源地址，有兴趣的朋友可以clone看下

项目Git传送门：[https://github.com/octopusy/AndroidBaseProject](https://github.com/octopusy/AndroidBaseProject)

项目打包发布

项目配置打包

![12.png](https://upload-images.jianshu.io/upload_images/4677526-9d69df9d60102cb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![13.png](https://upload-images.jianshu.io/upload_images/4677526-864bfe6a656a3368.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们在生成jks签名证书之后，可以直接在模块的build.gradle配置文件中配置，这样打包出来之后的App包则是直接签名后的包，可直接提供外部安装使用。

渠道发布

软件著作软很重要，很多应用市场都会要求上传软件著作权电子扫描件

1、腾讯应用宝 ：[http://open.qq.com](http://open.qq.com) （需要软著）

![14.png](https://upload-images.jianshu.io/upload_images/4677526-5dc262c39775a0da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用小图标：尺寸16x16，大小20K以内，PNG格式的图片

（2）应用图标：尺寸512*512，大小200K以内，PNG格式

（3）应用截图：2-5张截图（尺寸保持一致），单张图片不超过1M，截图不能小于320*480像素，推荐480 * 800像素，JPG、PNG格式 。

（4）版权证明：应用上架必须提供软著版权证明，否则无法上架，请参考[版权证明指引](http://wiki.open.qq.com/wiki/%E7%89%88%E6%9D%83%E8%AF%81%E6%98%8E%E4%B8%8A%E4%BC%A0%E6%8C%87%E5%BC%95)，大小2M以内，支持JPG/PNG格式的图片

注意：腾讯应用宝上传的apk需要经过加固，可以使用它推荐的乐固加固软件，加固之后还需要在本地安装一个乐固的[签名工具](https://console.cloud.tencent.com/ms/reinforce/tool) ，给加固过的安装包添加一个签名，签名是在签名工具里生成的，下载签名工具到电脑安装后打开，之后的具体步骤如下图：

*   首先需要输入云API密钥才能登录 [如何获取密钥？](https://cloud.tencent.com/developer/article/1385239)

![15.png](https://upload-images.jianshu.io/upload_images/4677526-b275760a67ef45f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*   登录完之后现在辅助工具这里生成签名文件和相关的签名信息

![16.png](https://upload-images.jianshu.io/upload_images/4677526-157918937a0c7b4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*   然后再回到加固

![17.png](https://upload-images.jianshu.io/upload_images/4677526-9d4496d4aac75de6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、360手机助手：[http://dev.360.cn](http://dev.360.cn) （需要软著和资质许可证明）

![18.png](https://upload-images.jianshu.io/upload_images/4677526-93ef4a6bbf1c564e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用图标：尺寸：512*512PX，圆角半径弧度：70PX，图片格式：PNG。

（2）应用截图：4-5张截图（尺寸保持一致），支持JPG、PNG格式。截图尺寸要求：不小于800 * 480（480 * 800），单张图片不能超过3M。请去除截图中的顶部通知栏。

（3）纸质软著登记证书：可上传纸质软著登记证书或软著受理函，要求：必须加盖公章，格式jpg、png，大小不超过1M。

（4）资质许可证明：相关版权材料均可压缩为RAR、ZIP格式上传 ，JPG、PNG或压缩包格式，图片大小不能超过1MB，有多个文件请打包为RAR、ZIP格式，大小不能超过10MB。

3、华为：[http://developer.huawei.com/consumer/cn](http://developer.huawei.com/consumer/cn) （需要免责函）

![19.png](https://upload-images.jianshu.io/upload_images/4677526-13407f4d4a10b5dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用图标：尺寸：216*216px，图片格式：PNG

（2）应用截图：最少3张截图，支持JPG、JPEG或PNG格式，建议分辨率：450*800（宽高比为9:16），单张图片最大为2M。

（3）应用版权证书或代理证书：免责函模板： [免责函模板](https://developer.huawei.com/consumer/cn/service/josp/agc/app/views/distribute/basic/appinfo/template/exemptionLetter.docx) JPG、PNG、BMP格式，不能超过4MB [应用版权证书或代理证书详细说明请参考版权资质说明](https://developer.huawei.com/consumer/cn/devservice/doc/80301)

4、阿里应用商店/豌豆荚/PP助手：[http://open.uc.cn](http://open.uc.cn) （需要软著）

![20.png](https://upload-images.jianshu.io/upload_images/4677526-f5013547144ae9f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用图标：png格式图标，背景透明且带圆角，建议上传分辨率512px * 512px（最低256px * 256px）

（2）应用截图：至少4张截图，支持jpg或png格式，不可上传iOS应用截图，分辨率请勿小于480 * 800或800 * 480

（3）证明材料：最多上传六张证明材料

*   请提供软件著作权扫描件，电子版软著提交至电子版权证书处，纸质版软著提交至证明材料处，提交电子版软著后可不用提交纸质版软著 [快速申请软件著作权](http://www.yibanquan.com.cn/copyright/aliguide1.jhtml)
*   若未申请软件著作权，请下载 [开发者声明](http://img.ucdl.pp.uc.cn/upload_files/app_open_platform/docs/%E9%98%BF%E9%87%8C%E5%BA%94%E7%94%A8%E5%88%86%E5%8F%91%E5%BC%80%E5%8F%91%E8%80%85%E5%A3%B0%E6%98%8E%E6%96%87%E4%BB%B6.doc)，填写后提交扫描件
*   所有文件上传仅接受JPEG/PNG格式（*涉及到新闻、股票、医疗、彩票、银行业务等还需要提供相应版权证明）

5、百度手机助手/安卓市场/91助手：[http://app.baidu.com](http://app.baidu.com)

![21.png](https://upload-images.jianshu.io/upload_images/4677526-8fae49bcce5c2f05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用图标：JPG或PNG格式的图标，尺寸为512×512px，容量小于800KB

（2）应用截图：4到6张最小尺寸为480px*800px的png格式或jpg格式的图片，每张照片不得超过1M

6、小米：[https://dev.mi.com](https://dev.mi.com)

![22.png](https://upload-images.jianshu.io/upload_images/4677526-eef013734fbbfa36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）行业资质证明：ICP备案号 、软件著作权书、ICP证或ICP备案截图

（2）应用截图：至少3张720 * 1280或1080 * 1920 应用图片信息如需使用到手机外观图片，禁止使用 iPhone 或其他品牌手机外观素材，应用图片信息中系统状态栏禁止存在与本应用无关的第三方应用图标

7、vivo：[https://dev.vivo.com.cn](https://dev.vivo.com.cn)

![23.png](https://upload-images.jianshu.io/upload_images/4677526-a5c1eda275cc7161.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用图标：支持jpg/png格式，尺寸要求长等于宽，不低于256 * 256，不超过512*512，大小50k以内，仅支持直角图标

（2）应用截图：3-5张清晰截图。尺寸为竖图480*800，格式为jpg/png/jpeg，每张图片尺寸一致，单张图片不超过2MB

8、oppo：[http://open.oppomobile.com](http://open.oppomobile.com) （需要软著或者承诺函）

![24.png](https://upload-images.jianshu.io/upload_images/4677526-4b8e903e4dbc39aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用图标：尺寸：512*512px，图片格式：PNG，小于1M

（2）应用截图：3-5张截图，支持JPG、PNG格式。截图尺寸要求：1080*1920，单张图片不能超过1M。请去除截图中的顶部状态栏的通知图标，图片中不得使用其他品牌的手机作为边框或宣传图

（3）软件版权证明：提供承诺函并上传在其他主流市场（如：应用宝、小米、 华为）后台审核上架的截图。PS：特殊分类必须要提供软著哦（相亲、交友、抢红包、彩票、直播等，具体特殊类别可参考《[https://open.oppomobile.com/wiki/doc#id=10022应用资质审核规范》。应用承诺函模板请点击参考：《https://open.oppomobile.com/wiki/doc#id=10005应用承诺（免责）函模板》](https://open.oppomobile.com/wiki/doc#id=10022%E5%BA%94%E7%94%A8%E8%B5%84%E8%B4%A8%E5%AE%A1%E6%A0%B8%E8%A7%84%E8%8C%83%E3%80%8B%E3%80%82%E5%BA%94%E7%94%A8%E6%89%BF%E8%AF%BA%E5%87%BD%E6%A8%A1%E6%9D%BF%E8%AF%B7%E7%82%B9%E5%87%BB%E5%8F%82%E8%80%83%EF%BC%9A%E3%80%8Ahttps://open.oppomobile.com/wiki/doc#id=10005%E5%BA%94%E7%94%A8%E6%89%BF%E8%AF%BA%EF%BC%88%E5%85%8D%E8%B4%A3%EF%BC%89%E5%87%BD%E6%A8%A1%E6%9D%BF%E3%80%8B)

9、三星：[http://support-cn.samsung.com/App/DeveloperChina/Home/Index](http://support-cn.samsung.com/App/DeveloperChina/Home/Index)

![25.png](https://upload-images.jianshu.io/upload_images/4677526-a44a3427f1cc6849.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要的素材规格：

（1）应用图标：PNG 格式 512 X 512 像素 小于 1024 KB

（2）应用截图：JPG/PNG 格式，最小 320 像素，最大 3840 像素，图片比例 2:1，至少需要4个图片，最多可上传8个。

（3）邮箱

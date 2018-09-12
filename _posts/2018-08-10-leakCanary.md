---
layout:     post

title:     LeakCanary  Android内存泄漏检测利器

category:   [Android]

description: LeakCanary 内存检测

keywords: LeakCanary 内存检测

---

## LeakCanary  Android内存泄漏检测利器

> 内存泄露是Android开发中常见的问题, 经常会造成应用闪退，降低程序的稳定性和用户体验。
LeakCanary是一个专门用来检测内存泄漏的工具，可以帮助我们在应用开发过程中检查出页面的泄露问题, 并提供内存泄漏的具体位置，快速的帮助我们定位和解决问题。


### LeakCanary 集成检测效果
 
![AndroidP](http://pey51suf1.bkt.clouddn.com/page_01.png) &nbsp;&nbsp;&nbsp; ![AndroidP](http://pey51suf1.bkt.clouddn.com/page_02.png)


## 为什么需要LeakCanary？

由于LeakCanary简单，易于发现问题，人人可参与。

- 简单：只需设置一段代码即可，打开应用运行一下就能够发现内存泄露。而MAT分析需要Heap Dump，获取文件，手动分析等多个步骤。

- 易于发现问题：在手机端即可查看问题即引用关系，而MAT则需要你分析，找到Path To GC Roots等关系。

- 人人可参与：开发人员，测试测试，产品经理基本上只要会用App就有可能发现问题。而传统的MAT方式，只有部分开发者才有精力和能力实施。


### 如何集成

#### 1.在 build.gradle 中添加依赖

```
debugCompile 'com.squareup.leakcanary:leakcanary-android:1.5.4'
releaseCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.5.4'
testCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.5.4'
```

#### 2.在应用中引用LeakCanary

```
private RefWatcher mRefWatcher;

@Override
public void onCreate() {
    super.onCreate();
    if (LeakCanary.isInAnalyzerProcess(this)) {
        // This process is dedicated to LeakCanary for heap analysis. release模式下不引用
        return;
    }
    mRefWatcher = LeakCanary.install(this);
}

public static RefWatcher getRefWatcher() {
    return sRefWatcher;
}
```

#### 3.在Activity中模拟一个泄漏


- **在Actvity中引用一个单例来修改TextView控件的值**

```
TextView signature = (TextView) findViewById(R.id.signature);
XXXHelper.getInstance(this).setRetainedTextView(signature);
```

- **单例对象**

```
import android.content.Context;
import android.widget.TextView;
import android.widget.Toast;

public class XXXHelper {

    private Context mCtx;
    private TextView mTextView;

    private static XXXHelper ourInstance = null;

    public static XXXHelper getInstance(Context context) {
        if (ourInstance == null) {
            ourInstance = new XXXHelper(context);
        }
        return ourInstance;
    }

    public void setRetainedTextView(TextView tv){
        this.mTextView = tv;
        mTextView.setText(mCtx.getString(android.R.string.ok));
        Toast.makeText(mCtx,mCtx.getString(android.R.string.ok),Toast.LENGTH_SHORT).show();
    }

    private XXXHelper() {
    }

    private XXXHelper(Context context) {
        this.mCtx = context;
    }

}
```

- **内存泄漏原因**
 
在单例中引用了Activity的Context对象和TextView控件,并长久持有，如果Activity被回收，但是单例中的Context和TextView控件却不会被回收，一直占据在内存中，反复进出几次Activity,就很容易导致内存泄漏了。

- **内存泄漏解决办法**

```
// 删除引用, 防止泄露
public void removeRetained() {
    mTextView = null;
    mCtx = null;
}
```

在Activity的destory() 方法中引用removeRetained()方法,释放对象的引用

```
@Override protected void onDestroy() {
    super.onDestroy();
    // 防止内泄露
    XXXHelper.getInstance(this.getApplication()).removeRetained();
}
```

- **监控某个可能存在内存泄露的对象**

```
AppApplication.getRefWatcher().watch(sLeaky);
```


### 检测对象

- **Activity**
- **Fragment**
- **BroadcastReceiver**
- **Service**
- **其他有生命周期的对象**
- **直接间接持有大内存占用的对象（即Retained Heap值比较大的对象）**

#### 检测时机

由于内存泄露是因为**某个对象在该释放的时候由于被其他对象持有没有被释放**，因此，我们监控需要设置在**对象（很快）被释放的时候**，如Activity和Fragment的onDestroy方法。

### 常用的解决内存泄漏的方法 

- 尽量使用Application的Context而不是Activity的
- 使用弱引用或者软引用
- 手动设置null，解除引用关系
- 将内部类设置为static，不隐式持有外部的实例
- 注册与反注册成对出现，在对象合适的生命周期进行反注册操作。
- 如果没有修改的权限，比如系统或者第三方SDK，可以使用反射进行解决持有关系


**注意**

目前LeakCanary一次只能报一个泄漏问题，如果存在内存泄漏但不是你的模块，并不能说明这个模块没有问题。建议建议将非本模块的泄漏解决之后，再进行检测。

 















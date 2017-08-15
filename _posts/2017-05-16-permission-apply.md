---
layout:     post
title:      android 动态权限申请
category:   [Android]
description: 动态权限申请
keywords: android,动态权限
---

## Android 6动态权限申请

> 什么情况下需要动态获取权限：
  满足两个条件：
- ①6.0以上系统 
- ②编译版本(compileSdkVersion)API23以上


单个的动态授权这里不说了，直接说明如何同时动态授予多个权限

1、首先声明一个数组permissions，将需要的权限都放在里面

<pre><code> 
String[] permissions =   {Manifest.permission.ACCESS_COARSE_LOCATION, Manifest.permission.ACCESS_FINE_LOCATION,
        Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.READ_EXTERNAL_STORAGE,
        Manifest.permission.READ_PHONE_STATE};
</code></pre>


2、创建一个permissionList，逐个判断哪些权限未授予，未授予的权限存储到perrrmissionList中

<pre><code> 
if (ContextCompat.checkSelfPermission(mContext, permissions[0]) != PackageManager.PERMISSION_GRANTED) {
    permissionList.add(permissions[0]);
}
if (ContextCompat.checkSelfPermission(mContext, permissions[1]) != PackageManager.PERMISSION_GRANTED) {
    permissionList.add(permissions[1]);
}
if (ContextCompat.checkSelfPermission(mContext, permissions[2]) != PackageManager.PERMISSION_GRANTED) {
    permissionList.add(permissions[2]);
}
if (ContextCompat.checkSelfPermission(mContext, permissions[3]) != PackageManager.PERMISSION_GRANTED) {
    permissionList.add(permissions[3]);
}
if (ContextCompat.checkSelfPermission(mContext, permissions[4]) != PackageManager.PERMISSION_GRANTED) {
    permissionList.add(permissions[4]);
}
</code></pre>

3、判断permissionList是否为空，不为空的，调用ActivityCompat.requestPermissions()授予权限。如果permissionList为空，表示权限都授予了，执行对应的方法
<pre><code>
/**
 * 判断是否为非空
 */
if (!permissionList.isEmpty()) {
    String[] permission = permissionList.toArray(new String[permissionList.size()]);
    ActivityCompat.requestPermissions(WelcomeActivity.this, permission, 1);
} else {
    initLocation();//初始化定位
}
</code></pre>

4、在权限回调中，判断用户是否授权，用户未同意授权则关闭当前页面。
<pre><code>
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    switch (requestCode) {
        case 1:
            if (grantResults.length > 0) {
                for (int result : grantResults) {
                    if (result != PackageManager.PERMISSION_GRANTED) {
                        ToastUtils.show("必须同意所有权限才能使用本程序");
                        finish();
                        return;
                    }
                }

                initLocation();//初始化定位

            } else {
                ToastUtils.show("发生未知错误");
            }
            break;
    }
}
</code></pre>


哪些权限需要动态授权：
9组24个
![](http://i.imgur.com/id7XLSV.png)








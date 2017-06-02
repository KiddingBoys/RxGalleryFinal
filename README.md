# RxGalleryFinal


## 功能描述（JDK1.8）

   RxGalleryFinal是一个android图片/视频文件选择器。其支持多选、单选、拍摄和裁剪，主题可自定义，无强制绑定第三方图片加载器。

   * <a href="https://github.com/FinalTeam/RxGalleryFinal/blob/master/README.md" target="_blank"> 中文版描述 </a>
   * <a href="https://github.com/FinalTeam/RxGalleryFinal/blob/master/README_English.md" target="_blank"> 英文版描述 </a>

## 版本描述

 <a href="https://github.com/FinalTeam/RxGalleryFinal/wiki/RxGalleryFinal-Issues" target="_blank">History Issues</a>
 <a href="https://github.com/FinalTeam/RxGalleryFinal/wiki/RxGalleryFinal-Version" target="_blank">History Version</a>


### 待完善

    1.视频选择器的回调
    2.卡顿测试，可在 Issues 里搜索：【精】觉得卡顿的点我 #130

   ### 新版本 V 1.0.7
        -- compile 'cn.finalteam.rxgalleryfinal:library:1.0.6'
        1.RxJava升级到最新 - RxJava2.1
        2.修复相关bug -  #136,#135,#134,#129,#99

   ### 新版本 V 1.0.6
        -- compile 'cn.finalteam.rxgalleryfinal:library:1.0.6'
        1.修复 点击Home再返回界面，图片会增加问题。
        2.修复 UI预览的size为0 的问题。
        3.修复 onresume()生命周期调用 onScanCompleted 问题。
        4.修复 剪裁回调及图片MediaActivity关闭问题。
        5.修复 部分机型卡顿的问题。



## 使用
### 下载或添加依赖
  在module gradle中项目依赖代码：

  ```gradle
    compile 'cn.finalteam.rxgalleryfinal:library:1.0.1'
    compile 'com.android.support:recyclerview-v7:24.2.0'
    compile 'com.android.support:appcompat-v7:24.2.0'

    //支持以下主流图片加载器，开发者自行选择
    compile 'com.squareup.picasso:picasso:2.5.2'
    compile 'com.facebook.fresco:fresco:0.12.0'
    compile 'com.github.bumptech.glide:glide:3.7.0'
    compile 'com.nostra13.universalimageloader:universal-image-loader:1.9.5'
  ```


### 配置manifest

 截图：

![image](https://github.com/FinalTeam/RxGalleryFinal/blob/master/a1.png)


* 提供了相关的Api

* 请查看MainActivity的示例代码 <a href="https://github.com/FinalTeam/RxGalleryFinal/blob/master/sample/src/main/java/cn/finalteam/rxgalleryfinal/sample/MainActivity.java" targer="_blank"> 查看 Sample 代码</a>


```java
   //自定义方法的使用
   onClickZDListener();
   //调用图片选择器Api
   onClickSelImgListener();
   //调用视频选择器Api
   onClickSelVDListener();
   //调用裁剪Api
   onClickImgCropListener();
```

```java

   //手动打开日志。
   ModelUtils.setDebugModel(true);

```

* 这里可以配置主题
    <img src="https://github.com/FinalTeam/RxGalleryFinal/blob/master/device-2017-04-11-154816.png" style="zoom:40%"  width=720 height=1080/>

<hr/>


##  Theme

配置Theme请查看sample下的 TestTheme..

* Code

```java
//自定义方法的单选
RxGalleryFinal
.with(context)
.image()
.radio()
.crop()
.imageLoader(ImageLoaderType.GLIDE)
.subscribe(new RxBusResultSubscriber<ImageRadioResultEvent>() {
    @Override
    protected void onEvent(ImageRadioResultEvent imageRadioResultEvent) throws Exception {
        //图片选择结果
        .....
    }
})
.openGallery();
```

----

```java
//自定义方法的多选
RxGalleryFinal.with(MainActivity.this)
.image()
.multiple()
.maxSize(8)
.imageLoader(ImageLoaderType.UNIVERSAL)
.subscribe(new RxBusResultSubscriber<ImageMultipleResultEvent>() {
       @Override
       protected void onEvent(ImageMultipleResultEvent imageMultipleResultEvent) throws Exception {
          toast("已选择" + imageMultipleResultEvent.getResult().size() + "张图片");
       }
       @Override
       public void onCompleted() {
       super.onCompleted();
           Toast.makeText(getBaseContext(), "OVER", Toast.LENGTH_SHORT).show();
       }
}).openGallery();

```

---


```java
 //得到图片多选的事件
 RxGalleryListener.getInstance().setMultiImageCheckedListener(new IMultiImageCheckedListener() {
       @Override
       public void selectedImg(Object t, boolean isChecked) {
            //这个主要点击或者按到就会触发，所以不建议在这里进行Toast
       }
       @Override
       public void selectedImgMax(Object t, boolean isChecked, int maxSize) {
           toast("你最多只能选择" + maxSize + "张图片");
       }
});
```
----

```java
 //注解诠释
 RxGalleryFinal.with(context)
      .image()//图片
      .radio()//单选
      .crop()//裁剪
      .video()//视频
      .imageLoader(ImageLoaderType.GLIDE)
      //里面可以选择主流图片工具  PICASSO  GLIDE  FRESCO UNIVERSAL(ImageLoader)
      .subscribe(rxBusResultSubscriber)
      .openGallery();
```

----
```java
    //调用裁剪.RxGalleryFinalApi.getModelPath()为默认的输出路径
    RxGalleryFinalApi.cropScannerForResult(MainActivity.this, RxGalleryFinalApi.getModelPath(), inputImg);
```
----

```java
    //获取和设置 保存路径
    RxGalleryFinalApi.getImgSaveRxCropDirByFile();//得到裁剪路径
    RxGalleryFinalApi.getImgSaveRxCropDirByStr();//得到裁剪路径
    RxGalleryFinalApi.getImgSaveRxDirByFile();//得到图片路径
    RxGalleryFinalApi.getImgSaveRxCropDirByStr();//得到图片路径

    //获取和设置 保存路径
    //…… setImgSaveXXXXX().
    //图片自动会存储到下面，裁剪会自动生成路径；也可以手动设置裁剪的路径；
    RxGalleryFinalApi.setImgSaveRxSDCard("dujinyang");
```
----


```java
    //自定义裁剪
   rx.cropAspectRatioOptions(0, new AspectRatio("3:3",30, 10))
   .crop()
   .openGallery();
```
----

```java
  //4.演示 单选裁剪 并且增加回掉 （裁剪必须在open之前）
  RxGalleryFinalApi.getInstance(this)
     .onCrop(true)//是否裁剪
     .openGalleryRadioImgDefault(new RxBusResultSubscriber() {
             @Override
             protected void onEvent(Object o) throws Exception {
                  Logger.i("只要选择图片就会触发");
             }
      })
     .onCropImageResult(new IRadioImageCheckedListener() {
             @Override
             public void cropAfter(Object t) {
                  Logger.i("裁剪完成");
             }

             @Override
             public boolean isActivityFinish() {
                  Logger.i("返回false不关闭，返回true则为关闭");
                  return true;
             }
     });
```

* 添加权限

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

* 注册activity
```xml
<application
    ...
    android:theme="@style/Theme_Light">
<activity
    android:name="cn.finalteam.rxgalleryfinal.ui.activity.MediaActivity"
    android:screenOrientation="portrait"
    android:exported="true"
    android:theme="@style/Theme_Light.Default"/>
<activity
    android:name="com.yalantis.ucrop.UCropActivity"
    android:screenOrientation="portrait"
    android:theme="@style/Theme_Light.Default"/>
</application
```

## 混淆配置
```xml
#1.support-v7-appcompat
-keep public class android.support.v7.widget.** { *; }
-keep public class android.support.v7.internal.widget.** { *; }
-keep public class android.support.v7.internal.view.menu.** { *; }

-keep public class * extends android.support.v4.view.ActionProvider {
    public <init>(android.content.Context);
}

#2.rxjava
-keep class rx.schedulers.Schedulers {
    public static <methods>;
}
-keep class rx.schedulers.ImmediateScheduler {
    public <methods>;
}
-keep class rx.schedulers.TestScheduler {
    public <methods>;
}
-keep class rx.schedulers.Schedulers {
    public static ** test();
}
-dontwarn sun.misc.**
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
    long producerIndex;
    long consumerIndex;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode producerNode;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode consumerNode;
}

#3.retrolambda
-dontwarn java.lang.invoke.*

#4.support-v4
-keep class android.support.v4.** { *; }
-keep interface android.support.v4.** { *; }

#5.ucrop
-dontwarn com.yalantis.ucrop**
-keep class com.yalantis.ucrop** { *; }
-keep interface com.yalantis.ucrop** { *; }

#6.photoview
-keep class uk.co.senab.photoview** { *; }
-keep interface uk.co.senab.photoview** { *; }

#7.rxgalleryfinal
-keep class cn.finalteam.rxgalleryfinal.ui.widget** { *; }

-keepclassmembers class * extends android.app.Activity {
   public void *(android.view.View);
}
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}
-keep class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator *;
}
-keepclassmembers class **.R$* {
    public static <fields>;
}

-keepattributes *Annotation*
-keepclasseswithmembernames class * {
    native <methods>;
}
-keepclassmembers public class * extends android.view.View {
   void set*(***);
   *** get*();
}
```

## Q&A
* 1、出现图片倒立问题，如何解决
* 2、如何压缩图片
* 3、Android 7.0闪退
* 4、授权说明

## 联系
    如果有紧急事件可联系作者或加Q群：
    - Q群号： 218801658
    - Q群号： 246231638

## Wiki

   * <a href="https://github.com/FinalTeam/RxGalleryFinal/wiki/GalleryFinal-%E9%97%AE%E9%A2%98%E7%B3%BB%E5%88%97" targer="_blank"> GalleryFinal 问题系列 </a>
   * <a href="https://github.com/FinalTeam/RxGalleryFinal/wiki/RxGalleryFinal-%E9%97%AE%E9%A2%98%E7%B3%BB%E5%88%97" targer="_blank"> RxGalleryFinal 问题系列 </a>
   * <a href="https://github.com/FinalTeam/RxGalleryFinal/wiki/RxGalleryFinal-Version" targer="_blank"> RxGalleryFinal Version </a>



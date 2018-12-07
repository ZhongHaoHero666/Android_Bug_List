<a href="#1">`1.index of bound expection（相册）`</a>  
<a href="#2">`2.Error: Please select Android SDK`</a>  
<a href="#3">`3.新建一个webview 加载h5调起微信支付`</a>  
<a href="#4">`4.ARouter 报 There's no path match...`</a>  
<a href="#5">`5.EditText不设置背景时，键盘覆盖了输入框底部`</a>  
<a href="#6">`6.android8.0的时候主题不能同时设置指定方向和activity透明`</a>  
<a href="#7">`7.double 类型数值计算不精准的问题`</a>  
<a href="#8">`8.沉浸式状态栏 + Edittext  软件盘遮挡输入框`</a>  
<a href="#9">`9.WebView加载长页面上下滑动时闪屏或者滑动异常。`</a>  
<a href="#10">`10.集成友盟推送（UPush）时的问题`</a>  
<a href="#11">`11.Viewpager中Fragment的生命周期问题`</a>  
<a href="#12">`12.Before Android 4.1, method android.graphics.PorterDuffColorFilter android.support...`</a>  
<a href="#13">`13. Rejecting re-init on previously-failed class java.lang.Class<com.taobao.accs.utl.k>: java.lang.NoClassDefFoundError`</a>  
<a href="#14">`14.集成umeng分享，在8.0 系统，分享到微博会报错`</a>  
<a href="#15">`15. View.setClcikAble(false)无作用`</a>  
<a href="#16">`16. Android studio 编译问题：com.android.dex.DexIndexOverflowException: method ID not in [0, 0xffff]: 65536`</a> 
<a href="#17">`17. 删除GreenDao生成的文件后，无法再次生成DaoMaster等`</a>  
<a href="#18">`18. 渠道打包修改applicationID时，微信登录无法回调`</a>  
<a href="#19">`19. 华为7.0+系统,报OOM，Throwing OutOfMemoryError "pthread_create (1040KB stack) failed: Out of memory"，其他手机机型不报该错误`</a>  
<a href="#20">`20. 软键盘可以调出，但是无法隐藏的问题。`</a>  
<a href="#21">`21. Android P webview加载http:// uri 页面空白。`</a>  
<a href="#22">`22. Umeng的Mapping文件上传10M限制`</a>  
<a href="#23">`23. 调试微信支付时的注意事项`</a>  
<a href="#24">`24. AndroidStudio设置本地代理后（host：127.0.0.1）后，无法清空代理配置，导致代理错误，链接被拒`</a>  
<a href="#25">`25. android studio 模拟器显示不出高德地图（黑屏）`</a>  
<a href="#26">`26. 关于代码中手动调用 System.gc() 的说明`</a>  
<a href="#27">`27. 多个组件中依赖版本不一致导致的编译错误问题`</a>  
<a href="#28">`28. java.lang.IllegalStateException: Only fullscreen opaque activities can request orientation`</a> 
  
<a id="1"/>

#### 1.index of bound expection（相册）

1)调用手机相册，在5.0（21）及5.1（22）系统下打开手机相册时出现闪退。
这是因为5.0直接返回的是图片路径，5.0以下是一个和数据库有关的索引值,6.0也是一个索引值  
以下为解决方案：

  ```java
Uri uri = data.getData();  
if (null != uri) {  
    int sdkVersion = Integer.valueOf(android.os.Build.VERSION.SDK);  
    String path = "";  
    if (sdkVersion == 21 || sdkVersion == 22){  
        path = uri.getPath();//5.0直接返回的是图片路径，5.0以下是一个和数据库有关的索引值,6.0也是一个索引值  
    }else {  
        String[] proj = {MediaStore.Images.Media.DATA};  
        //好像是android多媒体数据库的封装接口，具体的看Android文档  
        Cursor cursor = managedQuery(uri, proj, null, null, null);  
        //按我个人理解 这个是获得用户选择的图片的索引值  
        int column_index = cursor.getColumnIndexOrThrow(MediaStore.Images.Media.DATA);  
        //将光标移至开头 ，这个很重要，不小心很容易引起越界  
        cursor.moveToFirst();  
        //最后根据索引值获取图片路径  
        path = cursor.getString(column_index);  
    }  

        carFile = new File(path);  
}               
  ```
  
  <a id="2"/>
  
#### 2.Error: Please select Android SDK
由于在协同开发中有人上传了修改后的app.iml文件中 android SDK的配置
导致项目在编译时期找不到对应的sdk编译平台。

解决： 只需要在app.iml保持一致即可，不能早git上频繁的上传该文件。
标准配置代码为：

 ```xml
<orderEntry type="jdk" jdkName="Android API 25 Platform" jdkType="Android SDK" />
```

<a id="3"/>

#### 3.新建一个webview 加载h5调起微信支付
 需要重写的是 
 ```java
shouldOverrideUrlLoading(WebView view, String url)
//去微信支付
if (webView == null) {
    webView = new WebView(this);
    WebViewHelper.initWebViewSettings(webView);
    webView.setWebViewClient(new BaseWebViewClient(getMvpViewHelper()) {
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            // 如下方案可在非微信内部WebView的H5页面中调出微信支付
            if (url.startsWith(UrlConfig.WECHAT_PAY_START)) {
                try {
                    Intent intent = new Intent();
                    intent.setAction(Intent.ACTION_VIEW);
                    intent.setData(Uri.parse(url));
                    startActivity(intent);
                } catch (ActivityNotFoundException e) {
                    ToastUtil.showToast("请安装微信最新版！");
                }
            } else {
                view.loadUrl(url);
                if (url.startsWith(UrlConfig.WECHAT_PAY_RESULT)) {//支付成功后
                    //do something
                }
            }
            return true;
        }
    });
    ProgressBar progressBar = findViewById(R.id.webpage_progressbar);
    webView.setWebChromeClient(new BaseWebChromeClient(getContext(), progressBar));
}
String url = URLDecoder.decode(result.getPayParam().getPrepayUrl(), "UTF-8");
webView.loadUrl(url);
 ```
 
  <a id="4"/>

#### 4.ARouter 报 There's no path match...
按照文档说明配置后仍然出现此错误。
最后发现在gradle中配置了获取modelName的名称，该名称必须与路由路径中的第一个值相同，否则会无法匹配。
所以每个model下的Activity的路由名称的第一个值必须是模块名。

#### 5.EditText不设置背景时，键盘覆盖了输入框底部
解决方式 添加透明背景
 ```xml
<?xml version="1.0" encoding="UTF-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- 主体背景颜色值 -->
    <item>
        <shape android:shape="rectangle">
            <solid android:color="@color/transparent" />
            <padding android:bottom="xxdp" />
        </shape>
    </item>
</layer-list>

 ```

  <a id="6"/>

#### 6.android8.0的时候主题不能同时设置指定方向和activity透明
报Only fullscreen activities can request orientation
解决方式:
1 清单文件中activity主题style设置windowIsTranslucent = true
2在activity代码中 onCreat方法之前setTheme(windowIsTranslucent  = false 的主题)

  <a id="7"/>

#### 7.double 类型数值计算不精准的问题
2.01 * 100 = 200.9999999999 = (int) = 200
由于浮点和整形的保存方法是完全不同的，所以double y = 2.01在内存中实际是
FF FF FF FF FF 1F 69 40
经过IEEE的标准换算过来其实是2.00999999997, 这样，乘以100后得到200.99999997，强制转换整形的算法是向下取整，这样导致结果成了200
可以使用BigDecimal实现精确加减乘除运算。

 <a id="8"/>

#### 8.沉浸式状态栏 + Edittext  软件盘遮挡输入框
解决方案： [Android EditText被软键盘遮盖的处理方法](http://www.jb51.net/article/94705.htm)
延伸： [随手记Android沉浸式状态栏的踩坑之路](https://juejin.im/post/5a25f6146fb9a0452405ad5b)
一般沉浸式的框架都使用getWindow().getDecorView().setSystemUiVisibility方法设置SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN或SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION ，或者设置了window.addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS)，此时便会引发EditText被键盘遮盖问题。

解决方法呢，内容根布局（不包括titleBar）改成scrollView，然后动态的计算键盘高度，并设置scrollView的paddingBottom来达到适配。 （工具： KeyboardPatch，直接在文件中搜索就可以找到该类）

注意：如果根布局是releativelayout 那么marginBotton 和 alignParentBotton将会失效。

 <a id="9"/>

#### 9.WebView加载长页面上下滑动时闪屏或者滑动异常。
当application设置成硬件加速的时候，webview可能会出现闪屏的情况。
当设置成软件加速的时候，可能在某些机型中出现活动异常。
这个可以根据业务需求做不用取舍。
或者可以做直接webview.setLayorType(none，null); //设置成不用加速

 <a id="10"/>

#### 10.集成友盟推送（UPush）时的问题
1）集成系统通道时，小米通道的x-secret对应的是小米开放凭条的secret
2）推送的初始化逻辑不应该添加进程判断。（channl进程也需要初始化UPush）
3）如果注册失败，code = -11，msg = accs bindapp error。 是因为主工程内没有和upush 模块相同的so文件库，只要将push 模块的so文件复制到app 的libs 就好了。

 <a id="11"/>

#### 11.Viewpager中Fragment的生命周期问题
1）由于缓存及预加载的原因，Fragment 的 onResume（）和 onPause（）方法对于开发者来说并不可靠（实时），所以一般会重写Fragment 的 setUserVisibleHint（boolean isVisibleToUser）方法来判断 Fragment 对于用户来说，是否可见。

 <a id="12"/>

#### 12.Before Android 4.1, method android.graphics.PorterDuffColorFilter android.support...
这个是因为davilk虚拟机的一个Bug,其可能会重写父类私有方法，所以art虚拟机会报错。如果不是自己的代码，可以直接忽略。

 <a id="13"/>

#### 13. Rejecting re-init on previously-failed class java.lang.Class<com.taobao.accs.utl.k>: java.lang.NoClassDefFoundError
阿里云客服说这是一个系统级别的bug，可忽略，推测可能和Umeng推送的反射有关。

 <a id="14"/>
 
#### 14.集成umeng分享，在8.0 系统，分享到微博会报错
需要在配置分享的回调清单文件中去掉朝向的代码
 ```xml
 <activity
    android:name="com.umeng.socialize.media.WBShareCallBackActivity"
    android:configChanges="keyboardHidden|orientation"
    android:exported="false"
    //android:screenOrientation="portrait"
    android:theme="@android:style/Theme.Translucent.NoTitleBar" /> 
 ```
 
  <a id="15"/>
 
#### 15. View.setClcikAble(false)无作用
因为View.setOnClickListener(）方法中调用了View.setClcikAble(true),
在使用bufferKnife的时候会出现因为时间线异步导致不能setClcikAble（）方法不能起到效果。
可以使用View.setEnable()，设置点击事件是否可用。

 <a id="16"/>

#### 16. Android studio 编译问题：com.android.dex.DexIndexOverflowException: method ID not in [0, 0xffff]: 65536
方法数超过65535问题，使用 multiDex 即可。 [延伸阅读](https://github.com/TangXiaoLv/Android-Easy-MultiDex)

 <a id="17"/>

#### 17. 删除GreenDao生成的文件后，无法再次生成DaoMaster等
GreenDao plugin 运行在 andorid studio 的代码检查后，所以如果项目中有处理GreenDao的工具类，那么在删除插件生成的文件（DaoMaster等）后，make project 会无法生成文件，此时需要将工具类注释，然后先生成数据库辅助文件，再打开注释编译运行即可。

 <a id="18"/>
 
#### 18. 渠道打包修改applicationID时，微信登录无法回调
在使用多渠道打包时，在flavor 的渠道中修改applicationId，结果发现原本可以正常使用的微信快捷登录无法收到WXEntryActivity类的回调，导致确认授权后程序没有反应。
这是因为WXEntryActivity默认是在包名（这里的包名指的是applicationId）下的wxapi路径下，如果你的应用程序可能有多个applicationId，那么需要在每个applicationId路径下新建一个WXEntryActivity类（清单文件中也要添加）。

 <a id="19"/>
 
#### 19. 华为7.0+系统,报OOM，Throwing OutOfMemoryError "pthread_create (1040KB stack) failed: Out of memory"，其他手机机型不报该错误
[详细解读](https://www.jianshu.com/p/e574f0ffdb42)

该问题出现在华为andorid7.0+系统手机，此时堆内存和物理内存，但仍然产生OOM，是因为，华为android7.0+手机在系统层级对单个进程的线程线程数进行限制（最多只能有500个线程，原生系统的线程数限制>500），当应用内线程数量过多超过临界值时，便会产生OOM。

解决方案：梳理代码逻辑，巧用线程池，优化线程控制。

 <a id="20"/>
 
#### 20. 软键盘可以调出，但是无法隐藏的问题。
 软键盘可以调出，但是无法隐藏的问题。
```java
//弹出软键盘
InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
        if (imm != null) {
            imm.showSoftInput(view, InputMethodManager.SHOW_FORCED);
        }

//收起软键盘
InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
        if (imm != null && view != null) {
            imm.hideSoftInputFromWindow(view.getWindowToken(), 0);
        }
```
在```java showSoftInput(View view , int flag) ``` 方法中，flag 表示 调出类型


当```java flag = InputMethodManager.SHOW_FORCED ``` 时，表示强显式调用，非强制不收起。此参数导致hideSoftInputFromWindow失效。


当```java flag = InputMethodManager.SHOW_IMPLICIT ``` 时，表示隐式调用，可以正常收起。

 <a id="21"/>
 
#### 21. Android P webview加载http:// uri 页面空白
`自Android P 开始，谷歌全面禁止http:// 访问，所有的网络请求均需要使用https://（包括webview）`

<a id="22"/>

####  22. Umeng的Mapping文件上传10M限制
由于友盟对maping文件在上传时有不超过10m的限制，所以需要对过大的mapping文件进行压缩处理。
将下列代码保存为一个.sh文件  直接运行，便可以把同目录下的“mapping.txt”文件压缩成“newmapping.txt”文件。

``` find ./ -name mapping.txt | xargs cat | awk '{if($2!=$4) print $0}'>./newmapping.txt  ```

<a id="23"/>

####  23. 调试微信支付时的注意事项

* 1.debug调试时需要配置debug签名（这个一般项目中都会将debug和release配置成同一套签名）。
* 2.调试时需要确定后台返回的“appid，包名，签名”与前端是否一致（多applicationId时情况例外）。
* 3.微信登录和分享以及小程序支付的回调类是WXEntryActivity，微信原生支付的回调类是WXPayEntryActivity，可以将业务逻辑统一在WXEntryActivity中onResp方法中处理，然后让WXPayEntryActivity 继承 WXEntryActivity并在清单文件中声明即可。

<a id="24"/>

####  24. AndroidStudio设置本地代理后（host：127.0.0.1）后，无法清空代理配置，导致代理错误，链接被拒
在 AndroidStudio 设置本地代理后（host 127.0.0.1）后，编译项目时报代理错误（http 500 ），链接请求被拒绝。关闭AS的代理后仍然报错。且项目 gradle.properties 文件中没有代理配置。
这是因为在AS3.2之后，AS将代理配置文件放在了/user/.gradle/gradle.properties文件，不再存放在项目目录下的gradle.properties文件中。此时只需要清空该文件中的代理配置即可。


<a id="25"/>

####  25. android studio 模拟器显示不出高德地图（黑屏）
报错信息：No implementation found for void com.autonavi.ae.gmap.GLMapEngine.nativeInitParam()
高版本模拟器（Android 8.0+）对SO HEADER部分进行检查，这与高德对模拟器SO的压缩方案有冲突；（真机没有问题）
如果去除压缩x86平台包体积会增加到11M，为了满足大部分用户对包体积的要求，官网中为已压缩版本；

相关链接：[官方论坛](http://lbsbbs.amap.com/forum.php?mod=viewthread&tid=42965)

<a id="26"/>

####  26. 关于代码中手动调用 System.gc() 的说明

在android5.0之前，gc方法源码如下：
```
/**
 * 向JVM表明现在是垃圾收集器运行的好时机。注意，这只是一个提示。不能保证垃圾收集器将实际运行。
 */
public static void gc() {
    Runtime.getRuntime().gc();
}
```
在android5.0之后，gc方法发生了改变：
```
/**
 * 向JVM表明现在是垃圾收集器运行的好时机。注意，这只是一个提示。不能保证垃圾收集器将实际运行。
 */
public static void gc() {
    boolean shouldRunGC;
    synchronized(lock) {
        shouldRunGC = justRanFinalization;
        if (shouldRunGC) {
            justRanFinalization = false;
        } else {
            runGC = true;
        }
    }
    if (shouldRunGC) {
        Runtime.getRuntime().gc();
    }
}
```

增加关键条件shouldRunGC，该变量只有在runFinalization（ ）方法中被赋值。
```
/ * *
  * 向VM提供一个提示，说明尝试它是有用的
  * 执行任何未完成的对象终结。
  * /
public static void runFinalization() {
    boolean shouldRunGC;
    synchronized(lock) {
        shouldRunGC = runGC;
        runGC = false;
    }
    if (shouldRunGC) {
        Runtime.getRuntime().gc();
    }
    Runtime.getRuntime().runFinalization();
    synchronized(lock) {
        justRanFinalization = true;
    }
}
```
通过注释可以了解到 最后执行对象的finalize()方法。

且还存在一个变量runGC，在System.gc()里会赋值为true，而在System.runFinalization()里只有runGC为true时，才会真的去触发gc。

故在android 5.0之后，如果想要执行gc，有以下两种方式：
* System.gc()和System.runFinalization()同时调用
* 直接调用Runtime.getRuntime().gc()

PS:不过如果开发者在代码中手动调用了gc，在AndroidStudio内部仍然会报一个“不要觉得你比gc更聪明的警告”，建议开发者不要直接使用gc方法，破坏gc的一般运行规则。


<a id="27"/>

####  27. 多个组件中的依赖版本不一致导致的编译错误问题
该问题由多个组件（尤其是第三方）引用的基础依赖（如v4、v7）版本不一致导致。

解决方式很简单，可以使用如下配置方式在项目root build.gradle中声明强制依赖版本：
```
allprojects {
  configurations.all {
        // 为了保证编译正常 强制依赖统一版本
        resolutionStrategy.force "com.android.support:support-v4:${feineConfig.v4-version}"
  }
}
```

<a id="28"/>

####  28. java.lang.IllegalStateException: Only fullscreen opaque activities can request orientation
当开发时targetSdk = 27+，且在android 8.0手机上运行时，报此错误。意为：当确定屏幕方向时，当前页面不可使用全屏属性。

相关链接：
* [解析1](https://zhuanlan.zhihu.com/p/32190223) 
* [解析2](https://blog.csdn.net/starry_eve/article/details/82777160)  
具体解决方案由业务决定。

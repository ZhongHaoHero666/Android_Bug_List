
### 1.index of bound expection（相册）

1)调用手机相册，在5.0（21）及5.1（22）系统下打开手机相册时出现闪退。
这是因为5.0直接返回的是图片路径，5.0以下是一个和数据库有关的索引值,6.0也是一个索引值  
以下为解决方案：

  ``` 
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
### 2.Error: Please select Android SDK
由于在协同开发中有人上传了修改后的app.iml文件中 android SDK的配置
导致项目在编译时期找不到对应的sdk编译平台。

解决： 只需要在app.iml保持一致即可，不能早git上频繁的上传该文件。
标准配置代码为：

 ` <orderEntry type="jdk" jdkName="Android API 25 Platform" jdkType="Android SDK" /> `

### 3.新建一个webview 加载h5调起微信支付
 需要重写的是 
 ```
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

### 4.ARouter 组件化开发使用转场动画 出现老页面退出时黑屏
需要设置activity Theme 的属性
 ` <item name="android:windowIsTranslucent">true</item> `

最后分析问题，发现时转场动画写的有问题，要求需要4个anim文件，分别是创建_in，创建_our，销毁_in，销毁_out，并且进入和退出的动画要衔接上（要求无缝衔接），才不会出现黑屏。

### 5.EditText不设置背景时，键盘覆盖了输入框底部
解决方式 添加透明背景
 ```
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

### 5 android8.0的时候主题不能同时设置指定方向和activity透明
报Only fullscreen activities can request orientation
解决方式:
1 清单文件中activity主题style设置windowIsTranslucent = true
2在activity代码中 onCreat方法之前setTheme(windowIsTranslucent  = false 的主题)

### 6 double 类型数值计算不精准的问题
2.01 * 100 = 200.9999999999 = (int) = 200
由于浮点和整形的保存方法是完全不同的，所以double y = 2.01在内存中实际是
FF FF FF FF FF 1F 69 40
经过IEEE的标准换算过来其实是2.00999999997, 这样，乘以100后得到200.99999997，强制转换整形的算法是向下取整，这样导致结果成了200
可以使用BigDecimal实现精确加减乘除运算。

### 7 沉浸式状态栏 + Edittext  软件盘遮挡输入框
解决方案： [Android EditText被软键盘遮盖的处理方法](http://www.jb51.net/article/94705.htm)
延伸： [随手记Android沉浸式状态栏的踩坑之路](https://juejin.im/post/5a25f6146fb9a0452405ad5b)
一般沉浸式的框架都使用getWindow().getDecorView().setSystemUiVisibility方法设置SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN或SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION ，或者设置了window.addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS)，此时便会引发EditText被键盘遮盖问题。

解决方法呢，内容根布局（不包括titleBar）改成scrollView，然后动态的计算键盘高度，并设置scrollView的paddingBottom来达到适配。 （工具： KeyboardPatch，直接在文件中搜索就可以找到该类）

注意：如果根布局是releativelayout 那么marginBotton 和 alignParentBotton将会失效。

### 8 WebView加载长页面上下滑动时闪屏或者滑动异常。
当application设置成硬件加速的时候，webview可能会出现闪屏的情况。
当设置成软件加速的时候，可能在某些机型中出现活动异常。
这个可以根据业务需求做不用取舍。
或者可以做直接webview.setLayorType(none，null); //设置成不用加速


### 9 集成友盟推送（UPush）时的问题
1）集成系统通道时，小米通道的x-secret对应的是小米开放凭条的secret
2）推送的初始化逻辑不应该添加进程判断。（channl进程也需要初始化UPush）
3）如果注册失败，code = -11，msg = accs bindapp error。 是因为主工程内没有和upush 模块相同的so文件库，只要将push 模块的so文件复制到app 的libs 就好了。


### 10 Viewpager中Fragment的生命周期问题
1）由于缓存及预加载的原因，Fragment 的 onResume（）和 onPause（）方法对于开发者来说并不可靠（实时），所以一般会重写Fragment 的 setUserVisibleHint（boolean isVisibleToUser）方法来判断 Fragment 对于用户来说，是否可见。


### 11.Before Android 4.1, method android.graphics.PorterDuffColorFilter android.support...
这个是因为davilk虚拟机的一个Bug,其可能会重写父类私有方法，所以art虚拟机会报错。如果不是自己的代码，可以直接忽略。


### 12. Rejecting re-init on previously-failed class java.lang.Class<com.taobao.accs.utl.k>: java.lang.NoClassDefFoundError
阿里云客服说这是一个系统级别的bug，可忽略，推测可能和Umeng推送的反射有关。


### 13.集成umeng分享，在8.0 系统，分享到微博会报错
需要在配置分享的回调清单文件中去掉朝向的代码
 ``` 
 <activity
    android:name="com.umeng.socialize.media.WBShareCallBackActivity"
    android:configChanges="keyboardHidden|orientation"
    android:exported="false"
    //android:screenOrientation="portrait"
    android:theme="@android:style/Theme.Translucent.NoTitleBar" /> 
 ```
 
### 13. View.setClcikAble(false)无作用;
因为View.setOnClickListener(）方法中调用了View.setClcikAble(true),
在使用bufferKnife的时候会出现因为时间线异步导致不能setClcikAble（）方法不能起到效果。
可以使用View.setEnable()，设置点击事件是否可用。





# Android_DataLinks

ViewPager                        
        http://blog.csdn.net/wangjinyu501/article/details/8169924
        
精致UI布局                      
         http://www.androidchina.net/1992.html
         
         
fragment 郭霖                 
         http://blog.csdn.net/guolin_blog/article/details/8881711
         
         
点对点视频 anychat sdk    
         http://bbs.anychat.cn/forum.php?mod=viewthread&tid=193&extra=page%3D1
         
         
各类实用资料                    
         http://www.open-open.com/lib/view/open1436262653692.html
         
         
Github上优秀的三方控件    
         http://www.cnblogs.com/hawkon/p/3593709.html
         
         
屏幕适配方案                     
         http://blog.csdn.net/lmj623565791/article/details/45460089
         
         
网络框架
        Retrofit2.0  Asynchttpclient 
        
        
Gson解析json


星星评分控件     
        系统自带（可以重写自定义） RatingBar 
        
        
圆形椭圆形图片     
        RoundImageView      
        https://github.com/vinc3m1/RoundedImageView
        
        
仿照qq消息侧滑的ListView
        swipemenulistview
        swipRecyclerView
          https://github.com/baoyongzhang/SwipeMenuListView
          
          
图片压缩算法
         http://blog.csdn.net/leechee_1986/article/details/25049243
         
         
自动轮播的bander    
         bga-bander
        （少于或等于2张图时，点击之后轮播）    
   https://github.com/bingoogolapple/BGABanner-Android
   
   
自适应的Tag管理布局控件    流式布局
        TagFlowLayout  from github
        
        
国内大神博客汇集
        http://www.zhihu.com/question/19775981
        
        
6.0权限问题解决方案
        如果把targetSDK设置成22（小于23），则用户的手机按照6.0之前的规则进行权限处理，但是如果用户在权限管理的地方取消了权限，程序会崩溃，所以改变targetSDK无法彻底解决问题。
        运行时权限的处理如下：
        http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0830/3387.html
        http://www.tuicool.com/articles/rYBnau
        android：6.0+上的动态获取权限
        https://developer.android.com/training/permissions/requesting.html#perm-request
         使用的三方权限处理工具
        // https://github.com/k0shk0sh/PermissionHelper
        用这个http://blog.csdn.net/u011068996/article/details/50602100


播放器或者直播
          ijkplayer   b站的开源视频播放器
           腾讯的点播服务  https://www.qcloud.com/product/vod.html
           
           
视频通话
       http://cn.agora.io/ liaoyi在用这个   但是环信也是有这个的
           
           
直播平台
      乐视云直播
        
        
内存泄漏检测工具  leakcanary
      http://www.tuicool.com/articles/RvURJv
      

React-Native学习指南
       http://www.tuicool.com/m/articles/zaInUbA
       

二维码扫描
        https://github.com/yipianfengye/android-zxingLibrary


TabLayout做indactor   类似新闻客户端Tab的效果
MagicIndicator   
        https://github.com/hackware1993/MagicIndicator
Tablayout(tab字体不能变大)      
        http://blog.csdn.net/chenguang79/article/details/4880412
CoordinatorLayout的多种使用方式（类似魅族的应用商城动画效果）
        http://blog.csdn.net/huachao1001/article/details/51554608       
        http://blog.csdn.net/xyz_lmn/article/details/48055919
        http://blog.csdn.net/aotian16/article/details/51985215
        
        
ORM（对象关系映射）GreenDao  数据库辅助框架
        http://www.open-open.com/lib/view/open1438065400878.html


批量下载：okhttpUtils 


列表下拉刷新上拉加载
            https://github.com/jianghejie/XRecyclerView
上拉加载方便的recycleradapter 辅助工具 from github
recyclerviewadapterhelper
        https://github.com/CymChad/BaseRecyclerViewAdapterHelper
        http://blog.csdn.net/cym492224103/article/details/51171802
        下边这个可以很方便的实现下拉刷新和上拉加载
        http://www.jianshu.com/p/f3c3a84c2e99
下拉刷新
        https://github.com/liaohuqiu/android-Ultra-Pull-To-Refresh/blob/master/README-cn.md
        https://github.com/android-cjj/BeautifulRefreshLayout（特效较多）
        https://github.com/liaoinstan/SpringView（推荐）
        http://blog.csdn.net/liaoinstan/article/details/51023907（同上）
RecvvlerView Item单选
        http://www.tuicool.com/articles/RnQNzuN
        
        
图片框架   
        Glide(推荐)        .thumbnail(0.1f)//Glide压缩到指定倍数的方法
        http://blog.csdn.net/liuyangqiao/article/details/51735133（直接用Glide内部类实现圆形，圆角）
        http://m.blog.csdn.net/article/details?id=53229914（有毛玻璃 和高斯模糊效果）
        https://github.com/wasabeef/glide-transformations（基于glide的圆形，滤镜，模糊处理 ）
        Picasso   
        fresco（增加apk体积1.5-2.0m，但默认实现了圆角和两级的内存缓存+一级磁盘缓存，性价比最好）http://fresco-cn.org/
        各种三方图片库性能对比 http://m.blog.csdn.net/article/details?id=46959649
        picasso和glide的性能对比 http://blog.csdn.net/fancylovejava/article/details/44747759
        或者使用webp图片格式作性能优化
        
        
        
图片选择器    
        PhotoPicker  GitHub
        https://github.com/donglua/PhotoPicker
        AlbumSelector  支持多种样式 的图片选择器，更好用一点(妇儿医院项目中用的是这种)
        https://github.com/lijunguan/AlbumSelector
        MultiImageSelector
        https://github.com/lovetuzitong/MultiImageSelector

张鸿翔图片处理的一些干货
        http://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650820998&idx=1&sn=c9670674dcfb71a24521e898776f234e&scene=4#wechat_redirect

UI给的图片进行压缩，TinyPng   可已经200K左右的图片
压缩至40K
         https://tinypng.com/
         
         
         
图片裁剪工具  CropUtils  圆形，方形
         https://github.com/glassLake/CropUtils
         
         
         
仿微信的图片压缩工具，上传服务器  luban
          https://github.com/Curzibn/Luban
          
          
          
陈阳义用的图片压缩框架
        https://github.com/zetbaitsu/Compressor
        
        
        
Gif图片的处理
          http://blog.csdn.net/hss01248/article/details/52155673
          
          
          
高斯模糊
        开源框架 NativeStackBlur
        https://github.com/Commit451/NativeStackBlur
        fresco封装好的高斯模糊处理
        https://github.com/wasabeef/fresco-processors
        
        
        
沉浸式状态栏
        http://blog.csdn.net/guolin_blog/article/details/51763825
        SystemBarTint 这个库可以
        http://jaeger.itscoder.com/android/2016/03/27/statusbar-util.html  更方便

腾讯x5浏览器内核
        http://x5.tencent.com/doc?id=1003
        
        
        
关于gradle下载的问题
        http://blog.csdn.net/afei__/article/details/51479546
        

Android BottomNavigationBar底部导航控制器的使用（Google推荐的）
        http://blog.csdn.net/u010046908/article/details/50962081
        

大文件解压工具类
        http://blog.csdn.net/l349440843/article/details/50402419

一个可以用来设置下载进度和一键清理清理的进度控件
        https://github.com/zjun615/ProgressBar
        
        
        
消息数小圆点
        使用github上的 viewbadger  http://blog.csdn.net/u011443509/article/details/52295472
        或者自己用textView实现
        
        
        
直播的弹幕效果
        https://github.com/Bilibili/DanmakuFlameMaster（bilibili 的开元弹幕库）
        http://blog.csdn.net/sinyu890807/article/details/51933728
        
        
        
时间选择器
        wheelview 
        https://github.com/saiwu-bigkoo/Android-PickerView

安卓集成第三方.so的一些常见异常
        http://blog.csdn.net/arios171/article/details/52538104
        
        

Android Studio检测内存
        http://wetest.qq.com/lab/view/?id=99


Dex分包架构
        链接：http://pan.baidu.com/s/1nv7528D 密-码：rkwk
        
        
        
仿照探探首页卡片
        https://github.com/search?l=Java&q=Card++Stack+&type=Repositories&utf8=%E2%9C%93
        

引导说明
      https://github.com/binIoter/GuideView
      
      
      
丰富的UI特效
      http://www.itlanbao.com/
      
      

loading加载进度动画
      http://blog.csdn.net/u012551350/article/details/51779358
      
      

Dialog新手引导（带动画）
      https://github.com/yipianfengye/android-adDialog
      
      
Activity设置成透明背景（文中自定义的透明主题需要继承application主题）
        http://blog.csdn.net/yuejingjiahong/article/details/6668265
        
        
        
提升gradle效率的方法、
        http://www.jianshu.com/p/f9b0592383b8
        
        
        

ViewPager实现画廊效果
http://www.open-open.com/lib/view/open1462751945204.html



android进程保活
http://www.jianshu.com/p/63aafe3c12af



页面翻书翻页效果
http://blog.csdn.net/aigestudio/article/details/42741781



清除缓存
http://blog.csdn.net/u014756517/article/details/51731978



底部栏
  BottomNavigationBar  此为Google推荐

    也可以使用raidiobutton配合Textview
    桌面外的图标消息数
            ShortcutBadger（github）
        https://github.com/leolin310148/ShortcutBadger



各类架构
        https://github.com/CameloeAnthony/AndroidArchitectureCollection



模仿ios pop菜单
popMenuTableView
test

Recent Native
        http://reactnative.cn/
        http://reactnative.cn/cases.html
        http://facebook.github.io/react-native/
        
        

RxJava 介绍
        http://gank.io/post/560e15be2dca930e00da1083



人脸识别
        http://mp.weixin.qq.com/s?__biz=MzA3NzMxODEyMQ==&mid=2666453532&idx=1&sn=b4f9e2d37c7c203823a5fe0d2f32ac7c&chksm=8449a89ab33e218c87191c5112a70494aebeb15dd81f0e594fb49c3c5cae6a4d3242a791f91a&mpshare=1&scene=23&srcid=0205bfeeVhLCYLI7qcpC2SnE#rd



流式布局 flowlayout
http://blog.csdn.net/lmj623565791/article/details/38352503/



一些国外的开源项目
        https://blog.aritraroy.in/20-awesome-open-source-android-apps-to-boost-your-development-skills-b62832cf0fa4#.kt2clqsrg



模拟数据的api管理工具（apizza 是一个API管理工具，可以用来作为前期演示demo定义模拟接口和moke数据）
        http://apizza.cc/
        
        

模拟数据 
        http://mockhttp.cn/
        
        

各大平台架构合集
        https://github.com/CameloeAnthony/AndroidArchitectureCollection
        
        

Mvp学习文章
        http://mp.weixin.qq.com/s?__biz=MzA3ODg4MDk0Ng==&mid=403539764&idx=1&sn=d30d89e6848a8e13d4da0f5639100e5f#rd
        
        
        
Gradle 下载地址
        http://services.gradle.org/distributions/

# libGDX 与 OC交互
官网链接：http://libgdx.badlogicgames.com/
## 1、简介

**libGDX**是一个跨平台的2D/3D的游戏开发框架，它由Java/C/C++语言编写而成。C/C++代码是为了处理一些对性能要求很高的操作，比如物理引擎或者音频处理。作为用户，你只需要关注`Java`的封装就可以了，它已经把所有的本地代码封装好了。

其代码托管于Github ：https://github.com/libgdx/libgdx

libgdx兼容多种平台系统（Windows、Linux、Max OS X、Java Applet、Javascript/WebGL），Android和iOS。通过**RoboVM**可以实现iOS兼容

## 2、环境设置 

1、 下载并安装 **Java Development Kit ==7+== (JDK)** 。（不要根据系统提示去官网下载jdk 6 版本，不支持。）

下载地址 ：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html 

下载安装后 使用 ```java -version```
查看jdk 版本。

2、下载安装 **Intellij IDEA** ：http://www.jetbrains.com/idea/download/  （Community版本即可）。

3、下载**Android SDK**

下载地址 ： https://developer.android.com/studio/index.html

不需要下载 Android Studio ，页面拖到最下面，选择`仅获取命令行工具`一栏的tool。

下载解压后，放到合适的路径，设置环境变量：

- 启动Terminal终端工具

- 输入cd ~/ 进入当前用户的home目录

-  创建：
touch .bash_profile
- 打开并编辑：
open .bash_profile
- 在文件中写入以下内容：
```
export PATH=${PATH}:/Users/GameLife/Development/android-sdk-macosx/platform-tools
export PATH=${PATH}:/Users/GameLife/Development/android-sdk-macosx/tools
export ANDROID_HOME=/Users/youzu/android-sdk-macosx

```
配置的时候修改为你MAC下的的Android SDK文件夹目录的路径. 
- 执行如下命令：source .bash_profile 
- 验证：输入adb回车。如果未显示command not found，说明此命令有效，环境变量设置完成。

4 、**xcode**

5 、下载最新版本的 **MobiDevelop's RoboVM plugin** 

下载地址 ：http://robovm.mobidevelop.com

下载完成导入到IntelliJ IDEA 中即可。
Preferences -> Plugins -> Install plugin from disk...


## 3、创建一个工程 

libGDX 提供了一个简单的向导工具 (gdx-setup.jar)
- 下载文件 ：https://bitly.com/1i3C7i3

cd进文件目录后控制行命令（或者直接双击打开）
```
java -jar ./gdx-setup.jar

```

点击Generate直接创建即可

![image](http://note.youdao.com/yws/res/11379/0269F8523D2C4163B892708E5071EA59)

平台项目文件夹说明：

- core：Java 工程，包含所有游戏代码，主要在这个项目中编写代码，其他项目会自动关联引用
- desktop：桌面平台项目（也是一个 Java 项目），包含桌面平台启动器类
- android：Android平台项目，包含安卓项目的一些特有配置和 Android 平台启动器类
- iOS：iOS 平台项目，包含iOS项目的一些特有配置和 iOS 平台启动器类
- html：HTML5 平台的 GWT 项目


## 4、 导入工程到Intellij IDEA ,然后RUN

几个libGDX游戏链接可供测试 ：
- 2048: https://github.com/xietansheng/Game2048ForGDX 
- FlappyBird: https://github.com/xietansheng/FlappyBirdForGDX 

### 导入步骤：

打开 Intellij IDEA --> Import Project --> 选择工程根目录的build.gradle文件 --> 点击ok

![image](http://note.youdao.com/yws/res/11382/7020719E06E640EB8F7BDDF20CD06FB2)

打开后会执行 build.gradle ，会帮你下载依赖库与设置工程之间依赖，需要几分钟。

#### 可能遇到的错误：

- 如果遇到 **Error:SDK location not found.错误**

![image](https://note.youdao.com/src/987811A7E1FB4F6D8DDEC32D0A5CAADD)

关闭工程
在工程目录新建文件 文件名：**local.properties** 

用文本格式打开
输入文本 ：(目录改成自己的AndroidSDK目录)
```
sdk.dir=/Users/youzu/android-sdk-macosx
```
然后控制台执行 ：
```
set sdk.dir=/Users/youzu/android-sdk-macosx
```

- 如果遇到错误 **failed to find target android**：

![image](https://note.youdao.com/src/9E3FCB58D9AF464D91529BAE9207FB07)

修改工程目录--> android --> build.gradle 

![image](https://note.youdao.com/src/4DE09171823240A9A52C239B9D0E9402)

版本改成你自己导入SDK版本 （命令行查看 ：`android -v`）

然后 重新import 执行 build.gradle 

等待项目准备完成： ![image](https://note.youdao.com/src/992044B46C6341218B28D4AD6C8F3AC8)

点击 OK 。

### 把游戏跑到iOS模拟器（真机）上： 

步骤 ： 
- run --> Edit configurations --> 点击（+）号 --> 选择 RoboVM IOS --> 选择调试设备（模拟器）
名字随便起个 ，apply --> OK ； 
- run -->run iOS

iOS 程序入口位置在 ios/src/cn.appkf.game2048/IOSLauncher.java

```
package cn.appkf.game2048;

import org.robovm.apple.foundation.NSAutoreleasePool;
import org.robovm.apple.uikit.UIApplication;

import com.badlogic.gdx.backends.iosrobovm.IOSApplication;
import com.badlogic.gdx.backends.iosrobovm.IOSApplicationConfiguration;
import cn.appkf.game2048.MainGame;

public class IOSLauncher extends IOSApplication.Delegate {
    @Override
    protected IOSApplication createApplication() {
        IOSApplicationConfiguration config = new IOSApplicationConfiguration();
        return new IOSApplication(new MainGame(), config);
    }

    public static void main(String[] argv) {
        NSAutoreleasePool pool = new NSAutoreleasePool();
        UIApplication.main(argv, null, IOSLauncher.class);
        pool.close();
    }
}
```

## 5 、在工程中集成iOS静态库 

1.  创建一个静态库 （.a）。
```
#import "GDXObjectC.h"
#import "GDXTest1.h"
#import "GDXTest2.h"

@implementation GDXObjectC


-  (NSString *)delegateTest
{
    GDXTest1 * test1 = [GDXTest1 new];
    self.delegate = test1 ;
    return [NSString stringWithFormat:@"%s-->%@",__func__,[test1 printName]];
}

- (NSString *)categoryTest
{
    GDXTest2 * test2 = [GDXTest2 new];
    return [NSString stringWithFormat:@"%s-->%@",__func__,[test2 extendMethod]];
}

- (NSInteger)sumA:(NSInteger)a B:(NSInteger)b
{
    return a+b ;
}
```
2.  把 静态库 拖到 项目的ios工程目录 （不需要头文件）
3.  打开文件 工程目录  --> iOS --> roboVM.xml 设置静态库路径。

![image](https://note.youdao.com/src/F91887C9AB5D4E83925241EB6DDDBAFE)

4. 如果需要在core工程里面调用静态库 ，那么需要修改工程目录下的 `build.gradle` 在里面把 project:ios 中关于ios的依赖列表的依赖配置 剪到 core 工程的依赖列表里。重新执行 build.gradle。

![image](https://note.youdao.com/src/902CD4B5F3F54EADB11E3C19C2A92541)

5. 创建一个java文件，class名需要和你静态库里面的**类名一致**
```
package cn.appkf.game2048.bridge;

/**
 * Created by youzu on 2017/1/12.
 */
import org.robovm.apple.foundation.NSObject;
import org.robovm.objc.ObjCRuntime;
import org.robovm.objc.annotation.Method;
import org.robovm.objc.annotation.NativeClass;
import org.robovm.rt.bro.annotation.Library;

@Library(Library.INTERNAL)
@NativeClass

public class GDXObjectC extends NSObject{
    static {
        ObjCRuntime.bind(GDXObjectC.class);
    }

    @Method(selector = "delegateTest")
    public native String delegateTest();

    @Method(selector = "categoryTest")
    public native String categoryTest();

    @Method(selector = "sumA:B:")
    public  native int sum(int a,int b);
}
```
@Method 里面输入在oc里面写的方法名就行,有参数的请打上冒号（类似与oc的SEL）
 然后在下面定义方法的时候只要返回值和参数正确 

然后我们在core/src/MainGame.java 里写下面代码

```java
System.out.println("*****************测试开始*************************");
    GDXObjectC test1 = new GDXObjectC() ;
    int a = test1.sum(3,5);
    System.out.println("test return a + b = " + a);
    System.out.println("test delegate = " + test1.delegateTest());
    System.out.println("test category = " + test1.categoryTest());
System.out.println("*****************测试结束*************************");
```

#### 运行程序：

![image](http://note.youdao.com/yws/res/11397/DF103CCA9D914C649F5B192B81A3CA92)


## 6、 绑定的一些方法与总结

详细方法请参考 ：https://github.com/BlueRiverInteractive/robovm-ios-bindings


- ### 在IOS中绑定类
```objc
@interface ClassName : ExtendedClassName<ImplementedInterfaceName>
```
那么在java中就应该是：
```java
@NativeClasspublic class GADBannerView extends UIView {}
```
这里一般形式是：
```java
@NativeClasspublic class GADBannerView extends NSObject {}
```

==ps==:如下方式绑定oc的类时 java类必须个oc的一致，否则会因为找不到该类而报错
```
public class GDXObjectC extends NSObject{
    static {
        ObjCRuntime.bind(GDXObjectC.class);
        }
    }
```
报错：

    java.lang.ExceptionInInitializerError
    Caused by: org.robovm.objc.ObjCClassNotFoundException: LibGDX

- ### 在IOS中绑定方法
```objc
- (void)loadRequest:(GADRequest *)request;
```
在java中
```java
@Method(selector = "loadRequest:") //必须和IOS里面的方法名一样不然无法找到
public native void loadRequest(GADRequest request);//这里方法名可以任意写 但是参数必须一样
```
如果有多个参数的方法呢？
```objc

- (void)setLocationWithLatitude:(CGFloat)latitude longitude:(CGFloat)longitude accuracy:(CGFloat)accuracyInMeters;
```
在java写法是这样的：
```java
@Method(selector = "setLocationWithLatitude:longitude:accuracy:")
public native void setLocation(float latitude, float longitude, float accuracyInMeters);
```
- ### 在IOS中绑定构造方法
```objc
- (id)initWithAdSize:(GADAdSize)size;
```
那么在java中实现：
```java
@Method(selector = "initWithAdSize:")private native 
@Pointer long init(GADAdSize size);//构造方法需要值，因为GADAdSize实际上是long类型
```
这里的方法写成私有的，因为不是任何人都可以调用的，所以还得写个java里的构造方法
```java
public GADBannerView(GADAdSize size)
{
super((SkipInit)null);
initObject(init(size));
    
}
```
必须这样写，不然会被java调用两次

- ### 在IOS中绑定成员变量

在IOS中有很多很简便的写法，可以让开发者少些很多代码
```objc
@property(nonatomic, copy) NSString *adUnitID;
```
property这个意思是OC会自动实现getter和setter 不用手动实现，那么我们知道他的意思后就很好绑定了，和绑定方法一样一样的
```java
@Property(selector = "adUnitID")public native String getAdUnitID ();
@Property(selector = "setAdUnitID:")public native void setAdUnitID (String id);
```
注意：并不是所有属性都有getter和setter的 有些只有getter比如：
```objc
@property(nonatomic, readonly) BOOL hasAutoRefreshed;
```
还有一种写法在绑定的时候必须强引用，不然程序会报错：
```objc
@property(nonatomic, assign) NSObject<GADBannerViewDelegate> *delegate
```
在绑定的时候必须这样写：
```java
@Property(selector = "delegate")public native GADBannerViewDelegate getDelegate();
@Property(selector = "setDelegate:", strongRef = true)public native void setDelegate(GADBannerViewDelegate delegate);
```

对于一些没有getter和setter方法的成员变量，感觉可以通过KVC拿到。

==ps :== 如果方法名写错会直接报 `unReconizedSelector` .


## React Native学习<二> ReactNative搭建开发环境

[ReactNative+AndroidStudio环境搭建___Windows系统](http://www.jianshu.com/p/1a3452aaa578)

[ReactNative+AndroidStudio环境搭建___Linux系统](http://blog.csdn.net/dream_an/article/details/53787183)

### 一、搭建开发环境
根据你所使用的操作系统、针对的目标平台不同，具体步骤有所不同。如果想同时开发iOS和Android也没问题，你只需要先选一个平台开始，另一个平台的环境搭建只是稍有不同。

### 二、安装必需的软件

- Python 2  [官网下载](https://www.python.org/downloads/release/python-2710/)

>注意目前不支持Python 3版本。

- Node  [官网下载](https://nodejs.org/en/)

- Android Studio

React Native目前需要[Android Studio](http://developer.android.com/sdk/index.html)2.0或更高版本。
>Android Studio需要Java Development Kit [JDK] 1.8或更高版本。你可以在命令行中输入 javac -version
来查看你当前安装的JDK版本。如果版本不合要求，则可以到 [官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)上下载。 或是使用包管理器来安装（比如choco install jdk8或是 apt-get install default-jdk）

Android Studio包含了运行和测试React Native应用所需的Android SDK和模拟器。
>除非特别注明，请不要改动安装过程中的选项。比如Android Studio默认安装了 Android Support Repository
，而这也是React Native必须的（否则在react-native run-android时会报appcompat-v7包找不到的错误）。

- 确定所有安装都勾选了，尤其是Android SDK和Android Device Emulator。

- 在初步安装完成后，选择Custom安装项：

![custom installation](http://upload-images.jianshu.io/upload_images/2761423-a9f8fb48673f4048.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 检查已安装的组件，尤其是模拟器和HAXM加速驱动。

![verify installs](http://upload-images.jianshu.io/upload_images/2761423-3a91883f60272cae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 安装完成后，在Android Studio的欢迎界面中选择Configure | SDK Manager。

![configure sdk](http://upload-images.jianshu.io/upload_images/2761423-8306e667dd83a706.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 在SDK Platforms窗口中，选择Show Package Details，然后在Android 6.0 (Marshmallow)中勾选Google APIs、Android SDK Platform 23、Intel x86 Atom System Image、Intel x86 Atom_64 System Image以及Google APIs Intel x86 Atom_64 System Image。

![platforms](http://upload-images.jianshu.io/upload_images/2761423-bf6f740042f18e64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 在SDK Tools窗口中，选择Show Package Details，然后在Android SDK Build Tools中勾选Android SDK Build-Tools 23.0.1（必须是这个版本）。然后还要勾选最底部的Android Support Repository.

![build tools](http://upload-images.jianshu.io/upload_images/2761423-703b5b05925f649e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三、ANDROID_HOME环境变量
- 确保ANDROID_HOME环境变量正确地指向了你安装的Android SDK的路径。

打开控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 高级 -> 环境变量 -> 新建

>具体的路径可能和下图不一致，请自行确认。

![env variable](http://upload-images.jianshu.io/upload_images/2761423-034428a843b9aa9c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>你需要关闭现有的命令符提示窗口然后重新打开，这样新的环境变量才能生效。

### 四、将Android SDK的Tools目录添加到PATH变量中

你可以把Android SDK的tools和platform-tools目录添加到PATH变量中，以便在终端中运行一些Android工具，例如android avd或是adb logcat等。

打开控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 高级 -> 环境变量 -> 选中PATH -> 双击进行编辑
>注意你的具体路径可能和下图不同

![env variable](http://upload-images.jianshu.io/upload_images/2761423-0d62c466e56b0b44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 五、测试安装
```
react-native init ReactNative01
cd ReacNative01
npm install
npm start
react-native run-android
```
提示：你可以使用--version参数创建指定版本的项目。例如react-native init MyApp --version 0.39.2
。注意版本号必须精确到两个小数点。

**Windows用户请注意，请不要在命令行默认的System32目录中init项目！会有各种权限限制导致不能运行！**

### 六、修改项目
现在你已经成功运行了项目，我们可以开始尝试动手改一改了：
- 使用你喜欢的文本编辑器打开index.android.js并随便改上几行
- 按两下R键，或是用Menu键（通常是F2，在Genymotion模拟器中是⌘+M）打开开发者菜单，然后选择 *Reload JS* 就可以看到你的最新修改。
- 在终端下运行adb logcat *:S ReactNative:V ReactNativeJS:V可以看到你的应用的日志。

完成了！[](http://reactnative.cn/docs/0.45/getting-started.html#%E5%AE%8C%E6%88%90%E4%BA%86-)
恭喜！你已经成功运行并修改了你的第一个React Native应用。
![](http://upload-images.jianshu.io/upload_images/2761423-c5aa5cddd74b9b5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来如果你想要在真机上运行应用，请参阅[在设备上运行](http://reactnative.cn/docs/0.45/running-on-device-android.html#content)。

如果你碰到了一些问题，请参阅[常见问题](http://bbs.reactnative.cn/topic/130)。
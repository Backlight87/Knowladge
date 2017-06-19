![程序不能安装错误](http://oi2e3199v.bkt.clouddn.com/006ffe45626c239233cf9c062b8fff6d.png?imageView2/2/w/700/h/500)


这个错误有点坑爹，一开始我看下面的警告以为是没有卸载以前遗留的apk导致现在安装不上去，
搞了半天才发现第一句才是重点，这是由于使用了native libraries 。该native libraries 不支持当前的cpu的体系结构（我在项目里引入了jni），我当前是x86的，所以应当换成arm的。

解决方法：

安装genymotion:

[下载安装地址](https://www.genymotion.com/download/)


安装ARM_Translation_Android系列包

由于genymotion是基于x86的，而大部分应用都是基于ARM的，因此，我们需要安装一个ARM_Translation_Android系列包来增强兼容性。

安装步骤

点击下载ARM_Translation_Android系列包

1) Android 4.4及以下： ARM Translation Installer v1.1
2) Android 5.x： ARM_Translation_Lollipop
3) Android 6.x： ARM_Translation_Marshmallow (网盘->工具)

将下载的zip包，拖进Genymotion模拟器窗口，按照提示安装

安装成功后，重启Genymotion模拟器即可。

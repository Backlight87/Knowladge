
 原来是用有道云笔记进行的markdown文章的编写，但是如果有图片就很麻烦。于是决定转用atom编辑器来做书写工具，各种因为版本问题，网络问题搞了一天，一言难尽。。
<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

  * [教程背景](#教程背景)
* [**Atom编辑器的安装**](#atom编辑器的安装)
* [**markdown环境的配置**](#markdown环境的配置)
  * [备注：安装这个插件会出现蛮多的问题](#备注安装这个插件会出现蛮多的问题)
  * [到这里就完成了整个markdown环境的配置了，可以尽情的享受atom带来的编写快捷的感受同样是有几个步骤：](#到这里就完成了整个markdown环境的配置了可以尽情的享受atom带来的编写快捷的感受同样是有几个步骤)
  * [小tips：](#小tips)

<!-- tocstop -->
#### 教程背景

这篇教程是在可以翻墙的前提下，系统是win

### **Atom编辑器的安装**
[Atom编辑器下载地址](https://atom.io/)
通过这个下载的他会自动默认装到C盘，一些设置默认下去就好。安装完成后整个界面大致是这个样子


![atom的标题-new](http://oi2e3199v.bkt.clouddn.com/c438ae2652ee1fc9652f0d97f893770b.png)


### **markdown环境的配置**
1.  **安装qiniu-uploader插件**
  - 首先用快捷键ctrl+，唤出插件的管理界面

![管理插件的界面](http://oi2e3199v.bkt.clouddn.com/839d57fb3b0e4f03c4a8238d85d6962c.png)
 - 然后在输入框里输入qiniu-uploader，点击packages，注意他会搜索一段时间，请保持耐心

![qiniu——uploader](http://oi2e3199v.bkt.clouddn.com/f597351d3d270343b68089c307392800.png)

 - 安装完成后点击设置进行填写下图里的四个填框就完成这个插件的配置了

![上传设置](http://oi2e3199v.bkt.clouddn.com/4abafa058b4da7b0b55cc83833e48f40.png)

#### 备注：安装这个插件会出现蛮多的问题
- 第一个：当你搜索出packages点击install的时候，会很慢而且一段时间后显示如下的错误提示：

![安装插件问题1](http://oi2e3199v.bkt.clouddn.com/8d8554a71cd05f7a21b2cfe1bb70625e.png)
解决方案是：
 1. 这个插件需要一些C或C++的代码支持，因此需要你的电脑安装VC或者VisualStudio，博主装的是VS。
 2. 需要安装NodeJS
 3. 请把你的VPN开启全局模式<br>
 这样就可基本就能完成插件install了。

- 第二个：由于版本问题，你安装完他会显示如下所示的错误

>“Uncaught EvalError: Refused to evaluate a string as JavaScript because 'unsafe-eval' is not an allowed source of script in the following Content Security Policy directive: "script-src 'self'".”  

 研究了一下应该是网络请求同步七牛云的时候出现了错误,解决方法是：
 C/user/你的用户名/.atom/packages/qiniu-uploader/node_modules/qiniu/qiniu/这个地址下的io.js文件、rpc.js文件、zone.js文件替换成链接中的三个文件。

  [三个文件的地址](https://github.com/jingchen2222/nodejs-sdk.v6/commit/19f5dc7909a6b1e30ead24535a1875615bf68791)

2.  **安装markdown-assistant插件**
    - 首先同样是在输入框里输入markdown-assistant，由于上一个插件的问题基本解决了所以这个插件的安装会比较顺利，搜索完成后点击install
    - 安装完成，点击setting，设置uploader，其实他默认就是我们第一步下的qiniu-uploader插件，所以基本不用改

![set](http://oi2e3199v.bkt.clouddn.com/407587a382ae5c2318e38b788bd632ba.png)



#### 到这里就完成了整个markdown环境的配置了，可以尽情的享受atom带来的编写快捷的感受同样是有几个步骤：
1. 点击File->New File,然后在文档的右下角选择文档格式为Github MarkDown格式，**因为只有这样到时候才能预览**

 ![文档格式](http://oi2e3199v.bkt.clouddn.com/9865bb32c3aa31a52360823c34623114.png)

2. 在文档中愉快的填入图片
- 截图或复制文件
- Ctrl+V
- 填入title
- 回车


2. ctrl + shift + P 调出界面，输入 markdown preview toggle，即可实时预览（快捷键是ctrl + shift + M）
![实时预览](http://oi2e3199v.bkt.clouddn.com/812ff7c843df91bbeb0e987a2a850771.png)



#### 小tips：

对于markdowm一个避不开的问题就是：我们要如何改变图片的大小，很遗憾，markdown并没有这种语法。不过七牛云给我们提供了一个接口，我们用七牛云插件的时候可以在链接的后面添加参数来控制返回后的图片的规格。
比如： http://oi2e3199v.bkt.clouddn.com/812ff7c843df91bbeb0e987a2a850771.png?imageView2/2/w/700/h/500   这个链接就是代表我们要七牛云返回一个宽最大700，高最大500的图片给我们，其他的接口参照[七牛云图片基本处理](https://developer.qiniu.com/dora/manual/1279/basic-processing-images-imageview2)

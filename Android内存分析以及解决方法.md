#### 背景：工具是 AndroidStudio 版本号2.0+

#### 知识提要



- **什么是内存泄漏**
   内存泄漏指的是进程中某些对象（垃圾对象）已经没有使用价值了，但是它们却可以直接或间接地引用到gc roots导致无法被GC回收。无用的对象占据着内存空间，使得实际可使用内存变小，形象地说法就是内存泄漏了。

- **什么是内存溢出（OOM）**

   当一个app内存泄漏严重的时候，因为释放不掉内存，只能一直申请内存导致程序占用内存一直变大，当占用的内存超过系统分配的额度的时候就会内存溢出了。
  当然造成内存溢出的还有很多原因，内存泄漏只是其中的一部分。

  ```
  ActivityManager manager = (ActivityManager)getSystemService(Context.ACTIVITY_SERVICE);
  int heapSize = manager.getMemoryClass();

  //我们可以通过这个代码获取这个手机的（heap size）堆内存，以后我们就
  //可以根据这个大小来合理分配我们的资源，防止出现 out of memory error。
  ```

- **什么是GC**

   GC其实就是我们常说的垃圾回收，Android会在一定的阶段手动或自动的进行垃圾回收。有以下四种GC的原因：
   - GC_CONCURRENT:   当我们应用程序的堆内存快要满的时候，系统会自动触发GC操作来释放内存。
   - GC_FOR_MALLOC:   当我们的应用程序需要分配更多内存，可是现有内存已经不足的时候，系统会进行GC操作来释放内存。
   - GC_HPROF_DUMP_HEAP:   当生成HPROF文件的时候，系统会进行GC操作，关于HPROF文件我们下面会讲到。
   - GC_EXPLICIT:   这种情况就是我们刚才提到过的，主动通知系统去进行GC操作，比如调用System.gc()方法来通知系统。或者在DDMS中，通过工具按钮也是可以显式地告诉系统进行GC操作的，还有我们用AndroidStudio左下角的分析图里点垃圾车也是这个回收方式。

  回收的对象 ：一般是指没有被引用的对象，比如被用完的匿名类，比如没有走不到的类等。

  GC的时候我们可以在log里面查看大概是如下图这么一种格式的信息（对于art虚拟机也是差不多）：

  ![GC内存](http://oi2e3199v.bkt.clouddn.com/2feb8b70730956a428669415f76fb0f0.png)

  意思是分别是GC的对象，GC的原因，GC释放了多少内存，目前应用占用了总内存的比例，停止的时间。举个例子上图的意思是24699这个对象因为内存快要满了释放了1033K内存，还有13%的剩余内存，总共占用了38ms的时间。

   备注：这里解释一下为什么占用时间用31+7ms，因为以前的版本大致是2.3版本以前，一旦GC你的程序就会进入阻塞状态，必须等GC结束你才能运行，2.3以后Google对这个进行了优化，可以并发执行GC了，只是会在GC前后卡顿一下，对于图里的31和7就是GC前后卡顿的时间，也正是因为GC的这个优化，后面的ui卡顿中因为GC的比例就基本不怎么重要了。




#### **内存泄漏的原因：**
1. 非静态内部类的锅：

  举个例子我们可能会这么写内部类
  ```
  public class MainActivity extends Activity
  {

      @Override
      public void onCreate(BundlesavedInstanceState)
      {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);

      }
      class Inclass
      {
        void doSomething()
        {
           System.out.print("nothing");
        }
      }
  }

  ```

  内部类会持有外部类的引用，只要内部类的生存周期不超过外部类，是不会造成外部类无法回收的，但是问题就出在这里，有很多种情况会导致内部类的生存周期大于外部类：
  - 比如在内部类里开启子线程或者Asynctask 处理事务，那么事务没有处理完之前这个外部类不能被回收
  - 有一些生命周期很长的对象持有这个内部类的引用，比如static变量持有这个内部类或者单例模式的实例化对象持有（单例模式的对象的生命周期和application一样长），以及我们常用handle，因为他发出去的message不是立马执行的，所以handle实例得等message从队列中被取出了才能回收，同理如果我们的这个handle内部类不是静态的就很容易造成外部类无法回收。


     解决方法： 如果一定要内部类，请用静态内部类 ,让他持有外部类的弱引用


2.  属性动画中的无限动画

   我们使用了无限动画但是没有定制动画，那么尽管无法在界面上看到效果他还是会一直播放下去，而动画播放会劫持view，view又有对应activity的引用，所以释放不掉。


 解决方法：

 ```
 //在activity 的 OnDestroy（）方法中停止动画
   animator.cancel();

 ```
2.  一些IO流打开了通道没有关闭

  备注：有几点需要注意：
  - BufferedReader br = new BufferedReader(new FileReader(file)); close掉BufferedReader也会隐式close掉FileReader；
  - 这些通道流包括file，数据库的cursor等等
  解决方案：尽量写成下面这样
```
try { return; } finally { resource.close(); }
//这样即使try块中有return语句，也能保证finally块中的close
```

3. 注册对象后没有反注册
4. 使用某些集合收集对象的引用没有及时清理

  比如我们写一键退出功能时会写一个activity基类，用一个栈来管理所有活动，那么如果不释放掉这些引用他们对应的activity也是无法释放的。

5. bitmap问题
  其实bitmap并不会内存泄漏，不过如果你用完及时的recycle（提示系统这是可以回收的垃圾了）或者加载的时候用一定的采样率去展示图片会给内存减少很多压力，因此这个问题其实更多的会跟OOM扯上关系。


#### 内存泄漏的检测方法：
1. Android Studio已经开始支持自动进行内存泄漏检查了

   打开项目运行在真机或者模拟器上，然后点击工具栏的小机器人图标（ Android monitor），你就可以在右下角的找到大致如下图的画面：
   ![memory管理器](http://oi2e3199v.bkt.clouddn.com/35f48e35c7f8578f73c1f642518fa1eb.png) 我们通过这个图可以大致的判断有没有大的泄漏，如果重复app的某些功能，GC完内存一直升高，就很可能有泄漏了。
   ![开启关闭按钮](http://oi2e3199v.bkt.clouddn.com/218fdd61052c3964b7da1126e90f14a5.png) 这个按钮是暂停按钮
![手动回收](http://oi2e3199v.bkt.clouddn.com/c43fe7c8794d05cc3f5d72820ae40cb9.png)这个是手动回收的按钮（点一下就会执行GC）
![文件](http://oi2e3199v.bkt.clouddn.com/c281199f590f09ff02d1b5a6fd3f3170.png)点击这个按钮可以进入HPROF Viewer界面，查看Java的Heap：

   ![界面](http://oi2e3199v.bkt.clouddn.com/5f023067d78c5081c64ab85a85d2cc39.png)

   在这个界面里我们可以查看对象所占的内存，对象释放后的内存，对象引用等等，从而找出是哪个地方引起的泄漏。



  2. 我们可以根据日志判断
![log日志](http://oi2e3199v.bkt.clouddn.com/52a3dea824011efaddd33adb1b4aee62.png)如图你会发现在频繁的GC后你所能使用的内存越来越少，基本可以断定有泄漏了，
  缺点：通过log日志和memory管理器的图其实只能找到大的泄漏，像一些小的泄漏是比较难发现的。

 3. 神器之LeakCanary的使用
[LeakCanary的github地址](https://github.com/square/leakcanary)
[LeakCanaryde 使用指南](https://www.liaohuqiu.net/cn/posts/leak-canary-read-me/)
备注：值得注意的是当你在gradle中配置引用的时候（如下图）可能会失败
![引用](http://oi2e3199v.bkt.clouddn.com/b4c6fdabb00384b3194bb90daca38483.png)
博主的原因是博主的版本是release版本，但是release版本对应的库是空的，因此解决方法是只引入debug版本就好。
![解决方法](http://oi2e3199v.bkt.clouddn.com/2c93e59c2065f219560d360ae3a8e5c2.png)
在项目里引用进去之后你运行项目，操作app的时候如果有泄露就会自动弹出来泄露的地方，方便我们解决
![泄漏](http://oi2e3199v.bkt.clouddn.com/c904d15dd4d08eaa2eb2fde439db0ad1.png)

 4. DDMS配合MAT找出泄漏
 这个方法应该是最完善的一个方法，能够找到一些很小的泄漏，并且从eclipse时代一直延续到现在，但是挺繁杂的性价比不高，以后补充。
 具体可以参考郭霖的博客（最佳性能实践二后半部分）、[ddms开发工具使用](http://blog.csdn.net/Alex_SHT_JAVA/article/details/44098909)。

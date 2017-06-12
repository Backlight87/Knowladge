#### ***一、理论知识***


- 要知道人眼是有一定的感知界线，如果超出这个感知界限人眼与大脑之间的协作就无法感知过到画面更新。换言之，人们就会觉得这个画面很流畅。

- 而Android就把达到这种流畅的帧率规定为60fps。

- 卡顿一般是由丢帧造成了，那么丢帧是什么意思：
  因为Android的帧率是60fps，就是每秒刷新60帧，1/60fps＝16.6ms，每帧要16.6ms，也就是说每16.6ms系统就会通知并执行[我要刷新一次屏幕了]
  但是如果你的view的加载时间超过了16ms，打个比方你的view加载需要28ms，那么在发起刷新后的32ms（两次刷新）其实你看到的都是同一帧的画面，这就在人眼的感知范围内了，因此人们就会觉得不是很流畅。

- 而对于丢帧，则有可能是你的布局写的不合理，嵌套重叠挥着过度绘制，导致渲染时间超过16.6ms，也有可能是频繁GC造成的（虽然新的GC方式只在回收前后会停止运行代码，但是累积寄来还是有影响的）。
总得来说有以下这么几种可能:
  1. 人为在UI线程中做轻微耗时操作，导致UI线程卡顿，一旦操作繁杂了很可能直接ANR了；

  2. 布局Layout过于复杂，无法在16ms内完成渲染；

  2. 同一时间动画执行的次数过多，导致CPU或GPU负载过重；

  2. View过度绘制，导致某些像素在同一帧时间内被绘制多次，从而使CPU或GPU负载过重；

  2. View频繁的触发measure、layout，导致measure、layout累计耗时过多及整个View频繁的重新渲染；

  2. 内存频繁触发GC过多（同一帧中频繁创建内存），导致暂时阻塞渲染操作；

  2. 冗余资源及逻辑等导致加载和执行缓慢；



#### ***二、解决方法以及工具***
对于UI卡顿问题：正常的思路是先用工具检测出是否有问题，然后倒推回去得到解决方法。

1.  Hierarchy Viewer

      小知识点：
       Remeasureing Views（重新测量views）：
      - RelativeLayouts经常需要measure所有子节点两次才能把子节点合理的布局。如果子节点设置了weights属性，LinearLayouts也需要measure这些节点两次，才能获得精确的展示尺寸。

     - 如果子控件向父控件提供自己的尺寸来决定展示的位置时，遇到冲突的时候，父节点可以强制子节点重新measure（由此可能导致measure的时间消耗为原来的2-3倍）。
     - 并不是只有用户看得见的view才会被渲染，所有的view都会被渲染，所以移除不必要的view对于UI优化很重要。

      由于版本问题，在新版的AndroidStudio中，Hierarchy Viewer被集成到了Android Device Monitor中，所以以往的在Terminal中输入hierarchyviewer启动的方法失效了，因此我们要通过Android Device Monitor去打开。
![第一步](http://oi2e3199v.bkt.clouddn.com/de9c29fe7af12c62259257ae7971a07f.png)
![下一步](http://oi2e3199v.bkt.clouddn.com/2404d76e6a22354447b8325ee4b4267e.png)
![第三步](http://oi2e3199v.bkt.clouddn.com/2b887fd662c129a85782d8f8a294009d.png)
修改更正：上图的View properties是查看某一个节点的view属性
我们可以得到这个Activity的view树可以分析出View嵌套的冗余层级。
至于具体的每一个节点的信息我们可以借用这么一张图来解释：
![每个节点](http://oi2e3199v.bkt.clouddn.com/81a19c6863eabf31577cf8e225b76957.png)
  最上面的方框是指这个节点在activity屏幕中的位置（比如标题啊，导航栏等等）
  中间的方框信息：
  1view代表这个view的节点下只有一个view，然后分别是Measure、layout、Draw的绘制时间
  下面的方框信息：
  button是这个节点的控件名称  ，id是控件ID，
  下面三个点则是表示该view在那一层树形结构里measure，layout和draw所花费的相对时间（从左到右）。绿色表示最快的前50%，黄色表示最慢的前50%，红色表示那一层里面最慢的view。显然，红色的部分是我们优先优化的对象。

       ***优化的小tips：***
       - 首先选择最顶部的节点看一下总的渲染时间，初略看一下屏幕上每一个部分基本对应几个view，然后看一下每一部分树的层次结构
       - 对于这个view树尽可能的扁平化（就是不要有很深的层次），叠加的view越多，(节点所处位置越深，套嵌带来的measure越多，计算就会越费时)渲染就会越费时，减少view树形结构的深度，app每一帧的渲染就会变快。
          - 尽量减少relativelayout的嵌套：RelativeLayout里的measure都会发生两次，套嵌的view会导致measure时间的增加。
          经常看到这么一种结构，第四层次的view通过relativelayout连接到第三层次，第三层次的也是用relativelayout连接到第二层次， 这种就可以考虑一下是不是把第四第三层次放一块移到第二层来，这样就可以少渲染一层。
          - 移除冗余的布局，有一些可能是连续两个relativelayout连接，对于这种可以删除其中一个。
          - 使用三种标签：

            - 布局重用标签：
            ```
              //<include/>标签用法是：
               <include layout="@layout/demo"/>
            ```
            - 减少视图层级：
            ```
              //<merge/>标签用法是：
              <merge xmlns:android="http://schemas.android.com/apk/res/android">

               <Button
               android:layout_width="fill_parent"
               android:layout_height="wrap_content"
               android:text="@string/button2"/>

              <Button
              android:layout_width="fill_parent"
              android:layout_height="wrap_content"
              android:text="@string/button1"/>

            </merge>
            //</merge>标签一般用于。如果xml文件里最外层的view只是用来承载子view的，
            //那么你可以用这个标签，比如上面的代码他就只会加载里面的两个button
            ```
            - 需要时使用<ViewStub />
             <ViewStub />标签最大的优点是当你需要时才会加载，使用他并不会影响UI初始化时的性能。
             各种不常用的布局想进度条、显示错误消息等可以使用<ViewStub />标签.
       - 可以尽量减少relativelayout，如果可以的话用linearlayout或者其他的布局替代试试看。
2. Android Studio的lint
冗余资源及逻辑等也可能会导致加载和执行缓慢，我们可以用Lint来尝试解决这个问题。
![第一步](http://oi2e3199v.bkt.clouddn.com/043ecc7af3b3a3186892c984eed5caf7.png)
![第二步](http://oi2e3199v.bkt.clouddn.com/a308d2ed007d4cbb72ec704f4e988691.png)
 ![结果](http://oi2e3199v.bkt.clouddn.com/9225a0afb48fca6506c1b8a9c179a0db.png)
在最后这里选择多余的资源文件和布局文件进行删除。

3. Android开发者选项之GPU过度绘制
GPU过度绘制就指的是在屏幕一个像素上绘制多次(超过一次)，GPU过度绘制或多或少对性能有些影响。
原因而能有一下两种：
    - 布局嵌套
    - 追求华丽的特效不得不用非常多的层叠组件

     举个例子：我们的布局文件有可能window上有一个背景，textview上有一个背景，那我们进行渲染的时候就要渲染一次window上的背景然后在渲染一次textview上的背景，然而window上的背景其实是没有用的，还加长了我们总的渲染时间。
工具：
我们在手机的开发者模式可以开启GPU显示过度绘制区域（这个具体的开启方式和名称手机不一样也会有略微的差别）
开启后运行app你会发现手机变成如下图所示：
![具体情况](http://oi2e3199v.bkt.clouddn.com/85423d87d40be67d3c7302a3b3627d9c.png)
![过度绘制示意图](http://oi2e3199v.bkt.clouddn.com/0247a2dbf6d93e782b819871c1e4d724.png)
根据颜色我们可以区分显示区域被过度绘制的情况，是一倍两倍还是三倍（注意如果app是混合开发的，H5部分的界面是分析不出来的）
我们根据页面的过度绘制区域然后对过度绘制的区域回去viewTree中进行优化。
 另外如果你在开发者选项者勾选允许在adb上显示，那么你会在AndroidStudio中的Monitors上看到GPU的使用情况
![在as查看](http://oi2e3199v.bkt.clouddn.com/e22f98c4114461889e0411e309699a66.png)
4. GC垃圾回收导致的卡顿
因为GC导致的卡顿，博主目前遇到一般是因为大批量加载资源导致一瞬间内存不足只能疯狂的GC，这可能涉及到了资源的缓存、压缩和优化了。

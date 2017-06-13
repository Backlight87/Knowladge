### 输入


正常我们输入流会得到一个byte数组，我们如果想看数据是看不了的，我们要把他转成string才能够去看，那么如何转：
不是正常的toString();
而是把byte数组作为String的构造函数的参数传进去就得到字符串了
```
byte[] byteArray = new byte[] {87, 79, 87, 46, 46, 46};
String value = new String(byteArray);
```



有时候我们需要知道我们要上传的文件的大小
备注：如果这个要上传的不是本地是网络文件，这个方法会反而一个0，也就是不能用，解决方法 参照链接
http://hold-on.iteye.com/blog/1017449

```
//我们可以调用输入流的available方法，他会返回一个int类型的字节长度
//转换  1字节=1B  所以1M=1024*1024字节

inputStream.available()

```

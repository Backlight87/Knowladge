情景：Android 6.0在我们原有的AndroidManifest.xml声明权限的基础上，
又新增了运行时权限动态检测，把权限分为危险权限和非危险权限，危险权限需要在运行时判断并授权：

需要授权的：

```
身体传感器
日历
摄像头
通讯录
地理位置
麦克风
电话
短信
存储空间
```

在我们需要的地方授予权限

```
//判断checkSelfPermission是否包含WRITE_EXTERNAL_STORAGE权限（有的话会返回0），PackageManager.PERMISSION_GRANTED的值是0，代表有包含。
//Read_EXTERNAL_STORAGE_REQUEST_CODE是一个自定义的任意大于0的数字，后面会用到
int Read_EXTERNAL_STORAGE_REQUEST_CODE = 1;
if (ContextCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE)
              != PackageManager.PERMISSION_GRANTED) {
          //申请Read_EXTERNAL_STORAGE权限
          ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                  Read_EXTERNAL_STORAGE_REQUEST_CODE);
      }
```

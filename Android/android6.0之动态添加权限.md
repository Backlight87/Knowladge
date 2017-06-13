情景：Android 6.0在我们原有的AndroidManifest.xml声明权限的基础上，
又新增了运行时权限动态检测，把权限分为危险权限和非危险权限，危险权限需要在运行时判断并授权：

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [在Activity申请权限](#在activity申请权限)
* [在Fragment中申请权限](#在fragment中申请权限)

<!-- tocstop -->
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
### 在Activity申请权限
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
 如果我们要完善一点，需要对用户拒绝授权的操作进行捕获操作

 ```
 //用户选择允许或拒绝后，会回调onRequestPermissionsResult方法, 该方法类似于onActivityResult
   @Override
   public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
       super.onRequestPermissionsResult(requestCode, permissions, grantResults);
       doNext(requestCode,grantResults);
   }
 //我们接着需要根据requestCode和grantResults(授权结果)做相应的后续处理
 //这里的requestCode就是我们上面自定义的Read_EXTERNAL_STORAGE_REQUEST_CODE的值，这里来判断是哪个权限的回调
 private void doNext(int requestCode, int[] grantResults) {
       if (requestCode == WRITE_EXTERNAL_STORAGE_REQUEST_CODE) {
           if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
               // Permission Granted
           } else {
               // Permission Denied
           }
       }
   }

 ```


 ###  在Fragment中申请权限

 ```
/*
* Fragment中运行时权限的特殊处理

 在Fragment中申请权限，不要使用ActivityCompat.requestPermissions, 直接使用Fragment的requestPermissions方法，否则会回调到Activity的onRequestPermissionsResult

 如果在Fragment中嵌套Fragment，在子Fragment中使用requestPermissions方法，onRequestPermissionsResult不会回调回来，建议使用getParentFragment().requestPermissions方法，
 这个方法会回调到父Fragment中的onRequestPermissionsResult，加入以下代码可以把回调透传到子Fragment
*/

   @Override
   public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
       super.onRequestPermissionsResult(requestCode, permissions, grantResults);
       List<Fragment> fragments = getChildFragmentManager().getFragments();
       if (fragments != null) {
           for (Fragment fragment : fragments) {
               if (fragment != null) {
                   fragment.onRequestPermissionsResult(requestCode,permissions,grantResults);
               }
           }
       }
   }

 ```

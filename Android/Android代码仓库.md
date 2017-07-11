1、handle 相关代码

2、
```
//将drawable转换为一个bitmap
Drawable drawable= getResources().getDrawable(R.drawable.phone);
Bitmap mBitmap = ((BitmapDrawable) drawable) .getBitmap();
canvas.drawBitmap(mBitmap,null, rect0,null);//分别是第一个参数，对图片裁剪的矩形，图片放哪的矩形，最后一个是画笔
```

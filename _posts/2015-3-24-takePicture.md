---
layout: post
title: android 调用系统照片返回
description: "android 调用系统预设intent 通过调用相机软件获取照片,存储本地"
modified: 2014-12-24
tags: [android camera ,bitmap]
---

##Image capture intent
>通过预设intent调用相机应用获取照片，无需相机权限
>方式一，回调直接获取图片数据

{% highlight java%}
private static final int CAPTURE_IMAGE_ACTIVITY_REQUEST_CODE = 100;

Intent intentCamera = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
startActivityForResult(intentCamera,CAPTURE_IMAGE_ACTIVITY_REQUEST_CODE);
{% endhighlight %}

>Activity 回调一,直接返回原图，返回数据大小看相机优化情况。（ex:酷派手机7298D，500档元手机，系统相机返回图片大小400k+，使用camera360返回图片大小40k左右，肉眼观察效果相差无几）
{% highlight java%}
	@Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		if (requestCode == CAPTURE_IMAGE_ACTIVITY_REQUEST_CODE) {
			if (resultCode == RESULT_OK) {
			 if (data != null && resultCode == Activity.RESULT_OK) {
				 Bundle extras = data.getExtras();
				 Uri uri = data.getData();
				 extras.get("data");
				 Bitmap bitmap = (Bitmap) extras.get("data");
				 picture.setImageBitmap(bitmap);
				 tv_info.setText(bitmap.getHeight() + "," + bitmap.getWidth());
			 } else {
			}
		}
	}
}
{% endhighlight %}

>方式二，调用相机保存本地，回调访问uri
{% highlight java%}
Uri fileUri;

Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
fileUri = getOutputImageFileUri(MainActivity.this);//创建一个本地文件，返回文件Uri
// create a file to save the image
intent.putExtra(MediaStore.EXTRA_OUTPUT, fileUri);
// set the image file name , start the image capture Intent
startActivityForResult(intent,CAPTURE_IMAGE_ACTIVITY_REQUEST_CODE);
{% endhighlight %}

>回调二
{% highlight java%}
//返回文件
protected void onActivityResult(int requestCode, int resultCode, Intent data) ...
	Bitmap map = MediaStore.Images.Media.getBitmap(this.getContentResolver(), fileUri);
//图片处理
...
{% endhighlight %}




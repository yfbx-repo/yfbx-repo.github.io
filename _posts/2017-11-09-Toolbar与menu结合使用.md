
![toolbar2.png](http://upload-images.jianshu.io/upload_images/2434162-e58ef38a0dc22a97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 1. 首先 在res目录下新建menu文件夹，并创建menu文件：
share.xml
```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/toolbar_share"
        android:icon="@drawable/ic_share_white_24dp"
        android:title="share"
        app:showAsAction="always" />
</menu>
```
app:showAsAction="always"，这个属性决定菜单是一直显示还是在overflow中
###### 2. 在Activity中初始化菜单，并处理选中事件：
```
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        //不同的界面可以根据需要填充不同的菜单
        getMenuInflater().inflate(R.menu.share, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        if (item.getItemId() == R.id.toolbar_share) {
            Toast.makeText(this, "点击了分享菜单", Toast.LENGTH_SHORT).show();
            // TODO: 2017/11/9 实际的分享动作 
        }
        return true;
    }
```









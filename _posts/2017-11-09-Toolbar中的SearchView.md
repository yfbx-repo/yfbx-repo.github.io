在 [Toolbar与menu结合使用总结](http://www.jianshu.com/p/fcb0a163fffc) 这篇文中了解了Toolbar中menu的使用，本文中的SearchView也属于menu的使用，但又有一点不同，所以单独记下来。

- 首先还是创建menu:
search.xml
```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/toolbar_search"
        android:title="search"
        app:actionViewClass="android.support.v7.widget.SearchView"
        app:showAsAction="always" />
</menu>
```
注意，这里设置的`app:actionViewClass="android.support.v7.widget.SearchView"`搜索图标默认是黑色的，包括输入框中的文字以及关闭图标都是黑色的，尝试了很多奇怪的招数都没能成功改变颜色，最后才发现SearchView的样式是在Toolbar的主题中设置的，可以参考[Toolbar的使用经验总结](http://www.jianshu.com/p/8eb69622885c) 最后一部分。

- 创建SearchActivity,重写创建菜单方法：
```
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.search, menu);
        SearchManager searchManager = (SearchManager) getSystemService(Context.SEARCH_SERVICE);
        SearchView searchView = (SearchView) menu.findItem(R.id.toolbar_search).getActionView();
        searchView.setSearchableInfo(searchManager.getSearchableInfo(getComponentName()));
        return true;
    }
```
其实写到这里已经可以实现搜索功能了，只要监听Enter键，然后`
  searchView.getQuery()` 获取搜索框中输入的内容就可以了。
但我们要用高端一点的实现方式（虽然写起来有点复杂）。
- 创建searchable文件
在res目录下新建xml文件夹，在文件夹中创建searchable.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<searchable xmlns:android="http://schemas.android.com/apk/res/android"
    android:hint="姓名"
    android:imeOptions="actionSearch"
    android:label="@string/app_name">

</searchable>
```
- 配置Manifest
```
 <!--搜索界面-->
        <activity android:name=".SearchActivity">
            <meta-data
                android:name="android.app.default_searchable"
                android:value=".SearchResultActivity" />

            <!--如果搜索结果在同一界面，则只需要以下配置-->
            <!--<meta-data-->
            <!--android:name="android.app.searchable"-->
            <!--android:resource="@xml/searchable" />-->

            <!--<intent-filter>-->
            <!--<action android:name="android.intent.action.SEARCH" />-->
            <!--</intent-filter>-->
        </activity>
```
如果搜索结果在不同界面，则可以创建一个SearchResultActivity,然后进行配置：
```
 <!--搜索结果界面-->
        <activity
            android:name=".SearchResultActivity"
            android:parentActivityName=".SearchActivity">
            <meta-data
                android:name="android.support.PARENT_ACTIVITY"
                android:value=".SearchActivity" />
            <!-- meta tag and intent filter go into results activity -->
            <meta-data
                android:name="android.app.searchable"
                android:resource="@xml/searchable" />
            <intent-filter>
                <action android:name="android.intent.action.SEARCH" />
            </intent-filter>
        </activity>
```
可以通过 ` getIntent().getStringExtra(SearchManager.QUERY);` 来获取搜索框中输入的内容，然后就可以实现搜索了。

- SearchView 改变图标以及文字颜色的另类方法
为改变SearchView的颜色做了很多尝试,由于不了解SearchView的相关属性，所以去看了一下源码，发现它并没有提供图标及文字颜色等相关属性的设置方法，于是想要重写SearchView,但它里面的控件都是私有的也没有提供获取方法，实现起来也比较麻烦，于是最后出现这样的实现方式：
```
 @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.search, menu);
        SearchManager searchManager = (SearchManager) getSystemService(Context.SEARCH_SERVICE);
        SearchView searchView = (SearchView) menu.findItem(R.id.toolbar_search).getActionView();
        searchView.setSearchableInfo(searchManager.getSearchableInfo(getComponentName()));

        //搜索图标的id是在SearchView的源码中找到的，同样可以找到其他控件的id,并进行设置
        ImageView searchBtn = searchView.findViewById(R.id.search_button);
        searchBtn.setImageResource(R.drawable.ic_search_white_24dp);

        return true;
    }
```
这种方法也可以实现颜色的改变，虽然看起来好像有点奇怪，但不妨碍它确实能达到目的。所以，如果以后再碰到类似这种不知道在哪设置属性的情况可以尝试这种方法。
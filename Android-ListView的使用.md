---
title: Android-ListView的使用
date: 2017-08-06 18:06:15
tags: Android
description: 对Android中ListView界面及数据绑定的总结
---
<!--more-->
 **ListView 的实现步骤**

  1. 新建数据适配器 
  2. 适配器加载数据源 
  3. ListView加载适配器 
  4. 为每一个Item添加点击事件  适配器分两种：ListAdapter 和SimpleAdapter

 **案例一：（使用ListAdapter实现ListView（只包含文字哦））**   
 貌似要先来张照片（这次把图片整小点吧）   
![](https://ws1.sinaimg.cn/mw690/006PThdlly1fvfypgy97ij30920e8aai.jpg)

 布局文件中添加：

 
```xml
<ListView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/main_lv">
</ListView>
```
 Activity中

 
```java
private ListView mListView;
private ArrayAdapter<String> mArrayAdapter;
```
 
```java
mListView=(ListView)findViewById(R.id.main_lv);
//1.新建数据适配器
String[] arr={"《上邪》",
         "Tony-Chen",
         "上邪,",
         " 我欲与君相知,",
         " 长命无绝衰.",
         " 山无陵,",
         " 江水为竭.",
         " 冬雷震震,",
         " 夏雨雪.",
         " 天地合,",
         " 乃敢与君绝."};

//ArrayAdapter(context,当前ListView加载的每一个列表项对应的布局文件，数据源)

//2.适配器加载数据源
mArrayAdapter=new ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,arr);
//3.ListView 加载适配器
mListView.setAdapter(mArrayAdapter);
```
 **案例一：（使用SimpleAdapter实现ListView（包含图片和文字））**

 先看看效果：   
![](https://ws1.sinaimg.cn/mw690/006PThdlly1fvfypnlkdbj309c0ffaax.jpg)

 main.xml

 
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.mylistview.MainActivity">
        <ListView
            android:id="@+id/main_lv"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:background="?attr/colorControlHighlight">
        </ListView>

    <!--悬浮按钮-->
    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="@dimen/fab_margin"
        android:onClick="fabClick"
        app:backgroundTint="@android:color/holo_blue_dark"
        app:srcCompat="@mipmap/icon_serch" />

</android.support.design.widget.CoordinatorLayout>
```
 listview_content.xml

 
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_vertical"
    android:orientation="horizontal">

    <ImageView
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:id="@+id/image"
        android:layout_marginLeft="20dp"/>

    <TextView
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:id="@+id/image_text"
        android:padding="10dp"
        android:textSize="20sp"
        android:textStyle="bold"
        android:textColor="#FF0F00"/>

    </LinearLayout>
```
 activity

 
```java
@Override
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //悬浮按钮
        FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                        .setAction("Action", null).show();
            }
        });

        String[] arr = new String[]{"《上邪》",
                "Tony-Chen",
                "上邪,",
                " 我欲与君相知,",
                " 长命无绝衰.",
                " 山无陵,",
                " 江水为竭.",
                " 冬雷震震,",
                " 夏雨雪.",
                " 天地合,",
                " 乃敢与君绝."};
        ListAdapter mAdapter=new MyAdapter(this,arr);
        ListView lv=(ListView) findViewById(R.id.main_lv);
        lv.setOnItemClickListener(new AdapterView.OnItemClickListener()
        {
            @Override
            public void onItemClick(AdapterView<?> adapterView, View view, int i, long l)
            {
                if(i==0)
                {
                        Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                                .setAction("Action", null).show();
                }
            }
        });

        lv.setAdapter(mAdapter);
    }
    class MyAdapter extends ArrayAdapter<String>{
        public MyAdapter(Context context, String[] values) {
            super(context, R.layout.listview_content, values);
        }

        @Override
        public View getView(int position, View convertView, ViewGroup parent)
        {
            LayoutInflater inflater=LayoutInflater.from(getContext());
            View view = inflater.inflate(R.layout.listview_content, parent, false);
            String text=getItem(position);
            TextView textview=(TextView) view.findViewById(R.id.image_text);
            textview.setText(text);
            ImageView imageview=(ImageView) view.findViewById(R.id.image);
            imageview.setImageResource(android.R.drawable.ic_menu_info_details);
            if("最".equals(text))
                imageview.setImageResource(android.R.drawable.ic_menu_add);

            return view;
        }
    }
```
   
  
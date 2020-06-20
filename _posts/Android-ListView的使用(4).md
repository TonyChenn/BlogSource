---
title: Android-ListView的使用(4)
date: 2017-08-06 18:06:15
updatetime: 2020-06-12 18:06:15
tags: Android
description: 对Android中ListView界面及数据绑定的总结
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/android/listview.jpg
---
# ListView基本实现

**ListView 的实现步骤**
1. 创建ListView的Item模板 
2. 新建自定义适配器
3. 绑定适配器

下面使用ListView实现一个展示新闻的界面

## 创建一个ListView
```xml
<ListView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/myListView"/>
```
## 创建ListView的Item模板
在layout目录下创建news_item.xml，包括了一个图片，一个标题，一个摘要
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">
    <!--图片-->
    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/pic"/>
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1">
        <!--标题-->
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/title"
            android:textStyle="bold"
            android:textColor="#000000"/>
        <!--摘要-->
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textColor="#ff0000"
            android:id="@+id/summary"/>
    </LinearLayout>
</LinearLayout>
```
## 创建自定义适配器
创建自定义适配器并继承**ArrayAdapter**,并将泛型指定为News类，代码如下：
```java
public class NewsAdapter extends ArrayAdapter<News> {
    int resId;
    public NewsAdapter(@NonNull Context context, int resource, @NonNull List<News> objects) {
        super(context, resource, objects);
        resId=resource;
    }

    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        News item=getItem(position);
        //获取item的View
        View view= LayoutInflater.from(getContext()).inflate(resId,parent,false);
        
        //拿到控件
        ImageView pic=view.findViewById(R.id.pic);
        TextView title=view.findViewById(R.id.title);
        TextView summary =view.findViewById(R.id.summary);

        //绑定数据
        pic.setImageResource(R.mipmap.ic_launcher);
        title.setText(item.getTitle());
        summary.setText(item.getSummary());
        
        return view;
    }
}
```
其中LayoutInflater.from().inflate(...);的作用与findViewById类似，只不过findViewById是找到组件，而LayoutInflater是从res/layout/下找xml布局文件。它的作用是：
- 对于一个没有被载入或者想要动态载入的界面，都需要使用LayoutInflater.inflate()来载入;
- 对于一个已经载入的界面，就可以使用Activiyt.findViewById()方法来获得其中的界面元素;

## 绑定适配器
在NewsActivity中进行初始化数据，并绑定适配器，核心代码如下：

```java
List<News> newsList=new ArrayList<>();
//向newsList加数据
InitData();
// 上下文   template    数据
NewsAdapter adapter=new NewsAdapter(getApplicationContext(),R.layout.news_item,newsList);
myListView.setAdapter(adapter);
```
到此即可实现带图片的ListView,看看效果图吧：
![listview](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/0806/listview.jpg)

# ListView的优化
1. 在上面创建适配器的时候，每次都会重调用<kbd>getView</kbd>中每次都要重新获取布局，当滚动屏幕快时，就达到了性能的瓶颈。解决方法是：使用<kbd>getView</kbd>方法中的convertView进行缓存，下次直接使用。
2. 同样在<kbd>getView</kbd>中每次也都会重新通过<kbd>findViewById</kbd>获取控件，也可以对其进行优化。解决方案是：空间换时间，定义一个ViewHolder类去存储控件的引用。

代码如下：

```java
@NonNull
@Override
public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
    News item=getItem(position);

    View view;
    ViewHolder holder;

    if(convertView == null){
        view = LayoutInflater.from(getContext()).inflate(resId,parent,false);

        holder=new ViewHolder();
        holder.pic= view.findViewById(R.id.pic);
        holder.title=view.findViewById(R.id.title);
        holder.summary=view.findViewById(R.id.summary);
        view.setTag(holder);    //设置标签
    }
    else {
        view=convertView;
        holder=(ViewHolder) view.getTag();  //取出
    }

    holder.pic.setImageResource(R.mipmap.ic_launcher);
    holder.title.setText(item.getTitle());
    holder.summary.setText(item.getSummary());
    return view;
}
class ViewHolder
{
    public ImageView pic;
    public TextView title;
    public TextView summary;
}
```
# ListView的点击事件
通过<kbd>setOnItemClickListener</kbd>给所有item添加点击事件，点击Itemh后再WebView中打开新闻详细内容。核心代码如下：
```java
myListView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
    @Override
    public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
        News item=newsList.get(i);
        Intent intent=new Intent(getApplicationContext(),WebViewActivity.class);
        intent.putExtra("url",item.getUrl());
        startActivity(intent);
    }
});
```
再看下：
![previous](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/0806/previous.gif)
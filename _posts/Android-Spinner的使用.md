---
title: Android-Spinner的使用
date: 2017-06-26 13:39:26
tags: Android
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2017/06.26/spinner.jpg
---

 **1.第一步**   
 在string中添加一个字符数组：如下所示：

 
```xml
<resources>
    <string name="app_name">Spinner</string>
    <string-array name="weekend">
        <item>Monday</item>
        <item>Tuesday</item>
        <item>Wednesday</item>
        <item>Thusday</item>
        <item>Friday</item>
        <item>Wednesday</item>
        <item>Sunday</item>
    </string-array>

</resources>
```
 **Spinner的部分XML 的属性**

 
```
entries             直接在xml布局文件中绑定数据源
spinnerMode         Spinner的显示形式
prompt              在Spinner弹出对话框时对话框的标题
```
 **2.第二步**   
 在布局文件中添加Spinner控件；

 
```xml
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="368dp"
        android:layout_height="wrap_content"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="190dp"
        android:entries="@array/weekend"/>
```
 **第三步**   
 在activity中 **使用默认的spinner展开样式**：

 
```java
        //初始化Spinner控件
        Spinner spinner= (Spinner) findViewById(R.id.spinner);
        //建立数据源并绑定
        String[] items=getResources().getStringArray(R.array.weekend);
        //simple_spinner_item
        //simple_spinner_dropdown_item
        ArrayAdapter<String> adapter=new ArrayAdapter<String>(this,android.R.layout.simple_spinner_dropdown_item,items);
        //把adapter绑定到spinner；
        spinner.setAdapter(adapter);
        //设定选择监听器
        spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
            //获取选择的并保存在数组 ch[] 内；
                String[] ch=getResources().getStringArray(R.array.weekend);
                Toast.makeText(MainActivity.this,"Today is ："+ch[i],Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onNothingSelected(AdapterView<?> adapterView) {

            }
        });
```
   
  
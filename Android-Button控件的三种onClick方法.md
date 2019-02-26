---
title: Android-Button控件的三种onClick方法
date: 2017-07-30 12:28:53
tags: Android
---
 **1.方法一**

 
```xml
<Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1"
        tools:layout_editor_absoluteX="148dp"
        tools:layout_editor_absoluteY="81dp" />
```
 
```java
 Button b1= (Button) findViewById(R.id.button1);
 b1.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
          Toast.makeText(MainActivity.this,"方法一",Toast.LENGTH_SHORT).show();
   }
});
```
 **方法二**

 
```xml
<Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 2"
        tools:layout_editor_absoluteX="148dp"
        tools:layout_editor_absoluteY="137dp" />
```
 
```java
//放进onCreate()方法内
Button b2= (Button) findViewById(R.id.button2);
b2.setOnClickListener(btn_2Listener);
```
 
```java
//OnCreate()方法外
Button.OnClickListener btn_2Listener=new Button.OnClickListener(){
        public void onClick(View v) {
        Toast.makeText(MainActivity.this, "方法二", Toast.LENGTH_SHORT).show();
        }
};
```
 **方法三**

 
```xml
<Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="btn_3Click"
        android:text="Button 3"
        tools:layout_editor_absoluteX="148dp"
        tools:layout_editor_absoluteY="193dp" />
```
 
```java
public void btn_3Click(View v){
        Toast.makeText(MainActivity.this,"方法三",Toast.LENGTH_SHORT).show();
}
```
   
  
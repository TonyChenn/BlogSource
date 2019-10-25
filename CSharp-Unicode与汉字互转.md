---
title: 'CSharp-Unicode与汉字互转'
date: 2018-06-11 12:27:58
tags: CSharp
description: 将Unicode 转化为汉字
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/06.11/icon.jpg
---

```csharp
public string UnicodeToString(string unicode)
{
    string result = "";
    //正则匹配
    Regex reg = new Regex(@"(?i)//u([0-9a-f]{4})");
    result = reg.Replace(unicode, delegate (Match m)
    {
        return ((char)Convert.ToInt32(m.Groups[1].Value, 16)).ToString();
    });
    return result;
}
```
 2.将汉字转话为Unicode

 
```csharp
public static string StringToUnicode(string value)
{
    byte[] bytes = Encoding.Unicode.GetBytes(value);
    StringBuilder str= new StringBuilder();
    for (int i = 0; i < bytes.Length; i += 2)
    {
        // 取两个字符，每个字符都是右对齐。
        str.AppendFormat("u{0}{1}", bytes[i + 1].ToString("x").PadLeft(2, '0'), bytes[i].ToString("x").PadLeft(2, '0'));
    }
    return str.ToString();
}
```
 TonyChen   
 2018.6.11   
 Sometimes you got to run before you can walk———-先做再说
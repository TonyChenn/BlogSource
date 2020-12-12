---
title: Unity笔记
date: 2020-12-08 13:13:00
tags:
    - Unity
top:
password:
description:
img:
---
# Unity动态设置Splash图片

# 截屏
```csharp
/// <param name="camera">截屏的摄像机</param>
/// <param name="rect">图片大小</param>
/// <returns>屏幕快照</returns>
Texture2D CaptureCamera(Camera camera, Rect rect)
{
    RenderTexture rt = new RenderTexture((int)rect.width, (int)rect.height, 0);
    RenderTexture originRT = camera.targetTexture;      // 临时把camera中的targetTexture替换掉
    camera.targetTexture = rt;
    camera.RenderDontRestore();                         // 手动渲染
    camera.targetTexture = originRT;

    RenderTexture.active = rt;
    Texture2D screenShot = new Texture2D((int)rect.width, (int)rect.height);
    screenShot.ReadPixels(rect, 0, 0);                  // 读取的是 RenderTexture.active 中的像素
    screenShot.Apply();
    GameObject.Destroy(rt);
    RenderTexture.active = null;

    return screenShot;
}
```

# 剪切板
```csharp
static void CopyString(string str)
{
    TextEditor te = new TextEditor();
    te.text = str;
    te.SelectAll();
    te.Copy();
}
```

# 网络相关
## 是否无线连接
```csharp
public static bool IsWifi
{
    get
    {
        return Application.internetReachability == NetworkReachability.ReachableViaLocalAreaNetwork;
    }
}
```

## 网络可用
```csharp
public static bool NetAvailable
{
    get
    {
        return Application.internetReachability != NetworkReachability.NotReachable;
    }
}
```

# 文件相关
## 计算字符串的MD5值
```csharp
public static string md5(string source)
{
    MD5CryptoServiceProvider md5 = new MD5CryptoServiceProvider();
    byte[] data = System.Text.Encoding.UTF8.GetBytes(source);
    byte[] md5Data = md5.ComputeHash(data, 0, data.Length);
    md5.Clear();

    string destString = "";
    for (int i = 0; i < md5Data.Length; i++)
    {
        destString += System.Convert.ToString(md5Data[i], 16).PadLeft(2, '0');
    }
    destString = destString.PadLeft(32, '0');
    return destString;
}
```
## 计算文件的MD5值
```csharp
public static string md5file(string file)
{
    try
    {
        FileStream fs = new FileStream(file, FileMode.Open);
        System.Security.Cryptography.MD5 md5 = new System.Security.Cryptography.MD5CryptoServiceProvider();
        byte[] retVal = md5.ComputeHash(fs);
        fs.Close();

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < retVal.Length; i++)
        {
            sb.Append(retVal[i].ToString("x2"));
        }
        return sb.ToString();
    }
    catch (Exception ex)
    {
        throw new Exception("md5file() fail, error:" + ex.Message);
    }
}
```

# 编码/解码
```csharp
/// <summary>
/// Base64编码
/// </summary>
public static string Encode(string message)
{
    byte[] bytes = Encoding.GetEncoding("utf-8").GetBytes(message);
    return Convert.ToBase64String(bytes);
}

/// <summary>
/// Base64解码
/// </summary>
public static string Decode(string message)
{
    byte[] bytes = Convert.FromBase64String(message);
    return Encoding.GetEncoding("utf-8").GetString(bytes);
}
```
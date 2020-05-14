---
title: Unity-入坑AssetBundle（一）
date: 2018-12-13 16:54:58
tags: 
- Unity
- 热更新
top:
password:
description:
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/12.13/icon.jpg
---

AssetBundle（以下称AB）是学习热更新的基础，这也是第一次学，边学边总结边记录下来，希望能给也在开始学热更新的你一些帮助。
# AssetBundle的用处？
首先，学习AssetBundle是为之后学习Lua做准备。
1. AB包是一个压缩包，包含贴图，模型，声音，场景等文件；
2. AB包本身保存着各个AB包之间的依赖关系；
3. AB包可以使用[LZMA](https://baike.baidu.com/item/LZMA/10542921?fr=aladdin)或[LZ4](https://lz4.github.io/lz4/)的方式进行压缩，减小AB包的大小，从而更快的网络传输。
4. AssetBundle允许Unity在运行时通过使用一些方法动态加载数据资源。
5. 打包软件时，将一些文件放在AB包中，从而减小安装包的大小。
# 什么是AssetBundle?
## 从文件层面上看：
AB包分为两部分
- serialized file:资源打碎存放在一个对象中，最后保存在一个序列化文件中（只有一个）
- resource files:将一些图片，音乐等二进制资源单独保存起来，方便快速加载（多个）
## 程序上看:
是一个对象，通过使用代码从AB包中加载出来的对象。

# 生成AB包
1. 压缩方式选择
- None 使用LZMA压缩算法，压缩后体积最小，加载时间最长，使用前进行整体解压，解压后重新使用LZ4压缩
- UncompressedAssetBundle 不进行压缩，体积最大，加载速度最快
- ChunkBasedCompression 使用LZ4算法压缩，压缩效率没有LZMA好，但使用资源时不用整体解压，使用哪个解压哪个。
2. 打包的AB包不允许多平台使用。
## 使用代码进行生成
```CSharp
//dir   生成后保存的文件夹
//BuildAssetBundleOptions   压缩方式
//BuildTarget   打包平台
BuildPipeline.BuildAssetBundles(dir,
                BuildAssetBundleOptions.None, 
                BuildTarget.StandaloneWindows64);
```
## 使用插件
[https://github.com/Unity-Technologies/AssetBundles-Browser](https://github.com/Unity-Technologies/AssetBundles-Browser)

# 上传AB包
略
# 加载AB包中的资源
## 从文件加载
```csharp
//同步
private void LoadFromFile(string path)
{
    AssetBundle abPack = AssetBundle.LoadFromFile(path);
    if (abPack == null)
    {
        Debug.Log("error");
        return;
    }
    var prefab = abPack.LoadAsset<GameObject>("Cube");
    Instantiate(prefab);
}
//异步
private IEnumerator LoadFromFileAsync(string path)
{
    var req = AssetBundle.LoadFromFileAsync(path);
    yield return req;
    AssetBundle abPack = req.assetBundle;
    var prefab = abPack.LoadAsset<GameObject>("Cube");
    Instantiate(prefab);
}
```
## 从内存加载
```csharp
//同步
private void LoadFromMemory(string path)
{
    var abPack = AssetBundle.LoadFromMemory(File.ReadAllBytes(path));
    var prefab = abPack.LoadAsset<GameObject>("Cube");
    Instantiate(prefab);
}
//异步
private IEnumerator LoadFromMemoryAsync(string path)
{
    var req=AssetBundle.LoadFromMemoryAsync(File.ReadAllBytes(path));
    yield return req;
    AssetBundle abPack = req.assetBundle;
    var prefab = abPack.LoadAsset<GameObject>("Cube");
    Instantiate(prefab);
}
```
## 从服务器加载（推荐）
```csharp
private IEnumerator UseWebReq(string path)
{
    string uri = "http://www.xxx.xxx.xxx/AssetBundles/cubewall.u3d";
    var req = UnityWebRequestAssetBundle.GetAssetBundle(uri);
    yield return req.SendWebRequest();
    AssetBundle abPack = DownloadHandlerAssetBundle.GetContent(req);
    var prefab = abPack.LoadAsset<GameObject>("Cube");
    Instantiate(prefab);
}
```
## WWW的方式从服务器加载（逐渐被淘汰）
```csharp
private IEnumerator Use3w(string path)
{
    WWW www = WWW.LoadFromCacheOrDownload(@"file:///" + path,1);
    //WWW www = WWW.LoadFromCacheOrDownload(@"https://" + url);
    yield return www;
    if (!string.IsNullOrEmpty(www.error))
    {
        Debug.Log("error");
    }
    AssetBundle abPack = www.assetBundle;
    var prefab = abPack.LoadAsset<GameObject>("Cube");
    Instantiate(prefab);
}
```

通过上面的方法，就可以实现将打包到AB包的预制体实例化出来了，但是出现了问题，预制体处于材质丢失状态，所以我们需要将预制体所依赖的贴图，材质等加载到内存中。<b>那么问题来了，每个预制体上有1-n个各种类型的素材，三五个还好能手动添加到内存，但是多了这样就不行了！！</b>那就引出了minifest文件：

![minifest](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/12.13/minifest.jpg)

打开查看：

![open](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/12.13/open.jpg)

里面包含了cubewall的所有依赖库，下面就是加载minifest文件，在加载所有依赖文件：
# 加载minifest和依赖文件
```csharp
private void _LoadMinifest(string path)
{
    AssetBundle AB = AssetBundle.LoadFromFile(path);
    var minifest = AB.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
    //获取所有cubewall.u3d的依赖的名称
    string[] names = minifest.GetAllDependencies("cubewall.u3d");
    //加载所有cubewall.u3d的依赖
    foreach (var item in names)
    {
        Debug.Log(item);
        AssetBundle.LoadFromFile(Application.dataPath + "/AssetBundles/" + item);
    }
} 
```
![](https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/12.13/show.jpg)
# 卸载AssetBundle
当一些资源使用过后不会再使用的资源，我们就可以将他们卸载了，从而减少内存对的使用，当然当资源还需要使用就被卸载就会导致资源丢失。所以何时卸载资源就是一个问题了！
## 卸载所有资源，即使资源还正在使用也依然会被卸载
```csharp
AssetBundle.UnLoad(true);
```
## 卸载所有没有使用的资源，但是会导致没有卸载的资源永远不可卸载，若资源会多次加载卸载会导致内存泄漏。
```csharp
AssetBundle.UnLoad(false);
```

# 参考
- https://docs.unity3d.com/ScriptReference/AssetBundle.html
- siki-AssetBundle
<div align="right">TonyChenn<br> </div>
---
title: Unity编辑器拓展-生成图集
date: 2021-02-04 10:04:00
tags:
    - Unity
    - 编辑器拓展
top:
password:
description: 使用代码生成SpriteAtlas
img:
---
Unity2017之后开始支持SpriteAtlas，相较于旧版的SpritePacker变得更加好用，UGUI的图集概念又重新出来了。并且支持运行时访问图集。下面是记录做一键打包图集过程的记录。

# 为什么要打图集？
1. 图集将图片打包为2的幂次方的大小，提升性能
2. 减少DrawCall
3. 减小包体大小

# 创建图集
## 设置图集信息
```csharp
//Packing 模块(对应)
SpriteAtlasPackingSettings atlasSetting = new SpriteAtlasPackingSettings()
{
    blockOffset = 1,
    enableRotation = true,
    enableTightPacking = true,
    padding = 2,
};
//Texture 模块
SpriteAtlasTextureSettings textureSetting = new SpriteAtlasTextureSettings()
{
    readable = false,
    generateMipMaps = false,
    sRGB = true,
    filterMode = FilterMode.Bilinear,
};
// 平台 设置
TextureImporterPlatformSettings platformSettings = new TextureImporterPlatformSettings()
{
    maxTextureSize = 2048,
    format = TextureImporterFormat.Automatic,
    crunchedCompression = true,
    textureCompression = TextureImporterCompression.Compressed,
    compressionQuality = 50,
};
```
```csharp
public static void CreateAtlas()
{
    string atlasPath = "Assets/UI/UIAtlas/"
    //获取所有图集文件夹
    DirectoryInfo[] foldersArray = FolderHelper.GetSubFolders(PathConfig.UIAtlasFolder);
    if (foldersArray != null && foldersArray.Length > 0)
    {


        // 创建图集
        for (int i = 0, iMax = foldersArray.Length; i < iMax; i++)
        {
            string atlasName = string.Format("{0}/{1}.spriteatlas", atlasPath, foldersArray[i].Name);

            SpriteAtlas atlas = new SpriteAtlas();
            atlas.SetPackingSettings(atlasSetting);
            atlas.SetTextureSettings(textureSetting);
            atlas.SetPlatformSettings(platformSettings);

            AssetDatabase.CreateAsset(atlas, atlasName);

            //添加文件夹
            var obj = AssetDatabase.LoadAssetAtPath(string.Format("{0}/{1}", atlasPath, foldersArray[i].Name), typeof(object));
            atlas.Add(new[] { obj });

            //添加文件
            //var files = FileHelper.GetAllFiles(foldersArray[i]);
            //for (int j = 0, jMax = files.Length; j < jMax; j++)
            //{
            //    if (files[j].Name.ToLower().EndsWith(".jpg") || files[j].Name.ToLower().EndsWith(".png") ||
            //       files[j].Name.ToLower().EndsWith(".psd"))
            //    {
            //        var temp = AssetDatabase.LoadAssetAtPath<Sprite>(string.Format("{0}/{1}/{2}", atlasPath, foldersArray[i].Name, files[j].Name));
            //        atlas.Add(new[] { temp });
            //    }
            //}


            //设置AssetBundle名
            AssetDatabase.Refresh();
            ABTool.Singlton.SetAssetBundleName(atlasName,
                string.Format("ui/uiatlas/{0}", foldersArray[i].Name));
            AssetDatabase.Refresh();
        }
        Debug.Log("<color='green'>图集生成成功！</color>");
    }
}
```

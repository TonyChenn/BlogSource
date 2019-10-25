---
title: Unity-单机游戏的存档与读档
date: 2018-04-06 17:18:18
tags: Unity
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2018/04.06/icon.jpg
---

# 1.PlayerPrefs;
存储数据
```csharp
PlayerPrefs.SetString(“Name”,”Tony”); //设置String

PlayerPrefs.SetInt(“Age”,11); //设置Int

PlayerPrefs.SetFloat(“Score”,123.5f); //设置Float

PlayerPrefs.Save(); //保存数据
```
读取数据
```csharp
PlayerPrefs.GetString(“Name”); //获得Name

Playerf.GetInt(“Age”); //获得Age

PlayerPrefs.GetFloat(“Score”); //获得Float

PlayerPrefs.HasKey(“Score”); //包含“Score”键值
```

# 2.序列化与反序列化

Serialization(序列化) 将对象转化为字节流；

Deseriallzation(反序列化) 将字节流转化为对象；


## 二进制

序列化：新建或打开一个二进制文件，通过二进制格式器将对象写入二进制文件；

反序列化：打开待反序列化的文件，通过二进制格式器将文件解析成对象；

//简单，可读性差； 

### 二进制保存
```csharp
Save save = CreateSaveObject();
//创建二进制格式化文件
BinaryFormatter bf = new BinaryFormatter();
//创建爱你文件流
FileStream fs = File.Create(Application.dataPath + "/StreamingFile" + "/ByBin.dat");
//序列化
bf.Serialize(fs, save);
fs.Close();
if(File.Exists(Application.dataPath+ "/StreamingFile" + "/ByBin.dat"))
{
    Debug.Log("二进制保存成功");
}
```

### 二进制读取
```csharp
stringfilePath= Application.dataPath + "/StreamingFile" + "/ByBin.dat";
if(File.Exists(filePath))
{
    BinaryFormatter bf = new BinaryFormatter();
    FileStream fs =File.Open(filePath,FileMode.Open);
    //反序列化
    Save save =(Save)bf.Deserialize(fs);
    fs.Close();
    SetGame(save);
}
else
{
    Debug.Log("找不到游戏档案！");
}
```

## XML

可读性强，文件大，冗余信息多；
```xsharp
Using System.Xml;
```

### 序列化
```csharp
Save save = CreateSaveObject();
string path =Application.dataPath + "/StreamingFile" + "/ByXml.xml";
XmlDocument xmlDoc = new XmlDocument();
XmlElement root = xmlDoc.CreateElement("body");

root.SetAttribute("name", "GameData");
XmlElement animal, pos, type;

for(int i=0;i<save.livingTarget.Count;i++)
{
    animal = xmlDoc.CreateElement("animal");
    pos = xmlDoc.CreateElement("pos");
    pos.InnerText =save.livingTarget[i].ToString();
    type = xmlDoc.CreateElement("type");
    type.InnerText =save.livingType[i].ToString();

    animal.AppendChild(pos);
    animal.AppendChild(type);

    root.AppendChild(animal);

}

XmlElement score = xmlDoc.CreateElement("score");
score.InnerText = save.Score.ToString();
root.AppendChild(score);

XmlElement shootNum = xmlDoc.CreateElement("shootNum");
shootNum.InnerText = save.ShootNumber.ToString();

root.AppendChild(shootNum);

xmlDoc.AppendChild(root);

//保存文件
xmlDoc.Save(path);
if(File.Exists(path))
{
    Debug.Log("Xml保存成功");
}
```

### 反序列化
```csharp
string path =Application.dataPath + "/StreamingFile" + "/ByXml.xml";
if(File.Exists(path))
{
    Save save = new Save();
    //加载XML文档
    XmlDocument xmlDoc = new XmlDocument();
    xmlDoc.Load(path);

    XmlNodeList list =xmlDoc.GetElementsByTagName("animal");

    if(list.Count!=0)
    {
        foreach(XmlNode node in list)
        {
            XmlNode targetPos =node.ChildNodes[0];
            //位置
            int index = int.Parse(targetPos.InnerText);
            //把得到的值存储到save中
            save.livingTarget.Add(index);

            XmlNode targetType =node.ChildNodes[1];
            //类型
            int type = int.Parse(targetType.InnerText);
            save.livingType.Add(type);
        }
    }

//射击数
XmlNodeList shootNum =xmlDoc.GetElementsByTagName("shootNum");
int targetShootNum = int.Parse(shootNum[0].InnerText);
save.ShootNumber = targetShootNum;
//分数
XmlNodeList score =xmlDoc.GetElementsByTagName("score");
int targetScore = int.Parse(score[0].InnerText);
save.Score = targetScore;

//加载动物
SetGame(save);
}
```

## Json

//格式简单，易于读写，但是不够直观，可读性比XML差；

1.引用LitJson.dll
```csharp
Using LitJson;
```

### 序列化
```csharp
Save save = CreateSaveObject();
string path =Application.dataPath + "/StreamingFile" + "/byJson.json";
//序列化
stringstr_json = JsonMapper.ToJson(save);
//写入文件
StreamWriter sw = new StreamWriter(path);
sw.Write(str_json);
sw.Close();
if(File.Exists(path))
{
    Debug.Log("Json存储完成");
}
```

### 反序列化
```csharp
string path=Application.dataPath + "/StreamingFile" + "/byJson.json";
if(File.Exists(path))
{
    StreamReader sr = new StreamReader(path);
    string str_json = sr.ReadToEnd();
    sr.Close();
    //反序列化
    Save save =JsonMapper.ToObject<Save>(str_json);
    SetGame(save);
}
```
## 用到的方法：
```csharp
private Save CreateSaveObject()
{
    Save save = new Save();
    foreach(GameObjecttargetGO intargetGOs)
    {
        TargetManager targetManager =targetGO.GetComponent<TargetManager>();
        if(targetManager.activeMonster!=null)
        {
            save.livingTarget.Add(targetManager.TargetPositionIndex);
            int type =targetManager.activeMonster.GetComponent<MonsterManager>().MonstorType;
            save.livingType.Add(type);
        }
    }
    save.ShootNumber = UIManager._instance.shootNum;
    save.Score = UIManager._instance.score;
    return save;
}


//可序列化的
[System.Serializable]

public class Save{
    publicList<int>livingTarget = newList<int>();
    publicList<int>livingType = newList<int>();
    publicint ShootNumber = 0;
    publicint Score = 0;
}

```

//2018.4.6

//TonyChen
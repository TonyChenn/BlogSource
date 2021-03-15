---
title: Unity编辑器之数据存储
date: 2021-2-27 18:01:00
tags:
    - Unity
    - UntyEditor
top:
password:
description: 编辑器拓展过程中存储数据的方式
img:
---
在编辑器拓展过程中如果需要保存一些数据以便在下次打开后使用，如：项目的设置，编辑器拓展需要的参数等。可以通过下面几种方式实现。不要在OnGUI中对EditorPrefs进行保存操作，OnGUI会被多次调用，负荷高。谨慎使用`EditorPrefs.ClearAll()`.

# EditorPrefs
首先说下EditorPrefs,它和PlayerPrefs相似，只是后者在Runtime下使用，前者在编辑器模式下使用。在EditorPrefs中存储的数据不会限制于项目。用于存储一些编辑器相关数据，由于是明文存储，所以不适合存放密码。对于一些敏感数据，可以存放到EditorUserSettings中以二进制进行加密，使用`EditorUserSettings.Set / GetConfigValue`读，取。

可以使用`EditorPrefs.Get/Set` 保存一些基础数据类型如：int, float,bool,string。

当然通过一些方法也是可以保存其他类型的数据，下面是封装的一些方法，相信你以看就会。
1. 枚举类型
```csharp
// Get
public static T GetEnum<T>(string prefs_key, T defaultValue)
{
    string val = GetString(prefs_key, defaultValue.ToString());
    string[] names = System.Enum.GetNames(typeof(T));
    System.Array values = System.Enum.GetValues(typeof(T));

    for (int i = 0; i < names.Length; ++i)
    {
        if (names[i] == val)
            return (T)values.GetValue(i);
    }
    return defaultValue;
}
// Set
public static void SetEnum(string prefs_key, System.Enum val) 
{ 
    SetString(prefs_key, val.ToString()); 
}
```

2. 泛型
```csharp
// Get
public static T Get<T>(string prefs_key, T defaultValue) where T : Object
{
    if (!key_obj_list.Contains(prefs_key))
        key_obj_list.Add(prefs_key);

    string path = EditorPrefs.GetString(prefs_key);
    if (string.IsNullOrEmpty(path)) return null;

    T retVal = SUGUIEditorTool.LoadAsset<T>(path);

    if (retVal == null)
    {
        int id;
        if (int.TryParse(path, out id))
            return EditorUtility.InstanceIDToObject(id) as T;
    }
    return retVal;
}
// Set
public static void SetObject(string prefs_key, Object obj)
{
    if (!key_obj_list.Contains(prefs_key))
        key_obj_list.Add(prefs_key);

    if (obj == null)
    {
        EditorPrefs.DeleteKey(prefs_key);
    }
    else
    {
        if (obj != null)
        {
            string path = AssetDatabase.GetAssetPath(obj);
            EditorPrefs.SetString(prefs_key, string.IsNullOrEmpty(path) ? obj.GetInstanceID().ToString() : path);
        }
        else EditorPrefs.DeleteKey(prefs_key);
    }
}
```

3. 颜色
```csharp
// Get
public static Color GetColor(string prefs_key, Color c)
{
    string strVal = GetString(prefs_key, c.r + " " + c.g + " " + c.b + " " + c.a);
    string[] parts = strVal.Split(' ');

    if (parts.Length == 4)
    {
        float.TryParse(parts[0], out c.r);
        float.TryParse(parts[1], out c.g);
        float.TryParse(parts[2], out c.b);
        float.TryParse(parts[3], out c.a);
    }
    return c;
}
// Set
public static void SetColor(string prefs_key, Color c) 
{ 
    SetString(prefs_key, c.r + " " + c.g + " " + c.b + " " + c.a); 
}
```

基本上所有类型都可以通过泛型的方式进行保存。

## 存放位置
|平台|位置|
|---|---|
|Windows(Unity4.x) |HKEY_CURRENT_USER\Software\Unity Technologies\UnityEditor 4.x|
|Windows(Unity5.x) |HKEY_CURRENT_USER\Software\Unity Technologies\UnityEditor 5.x|
|MacOS X(Unity4.x) |~/Library/Preferences/com.unity3d.UnityEditor4.x.plist|
|MacOS X(Unity5.x) |~/Library/Preferences/com.unity3d.UnityEditor5.x.plist|

# ScriptableObject
用来存放项目中共享的数据，可以当作本地数据库，存储大量数据。ScriptObject的用途还是挺大的，篇幅过大之后专门写一篇关于ScriptObject的各种方便开发的用法。

# Json
第三种当然是Json啦。当然网上有着各种已经成熟的Json的序列化/反序列化插件了。这里就不介绍了。下面说下Unity本身自带的一个非常小型的Json序列化工具类。但是只支持简单的数据类型，不支持`List`和`字典`这些数据结构。下面也是做一个小拓展，完善它的一些不足。通过将列表字典转化为对象的形式再进行序列化与反序列化，实现支持List，Dictionary 类型。
代码如下:
```csharp
// List<T>
[Serializable]
public class Serialization<T>
{

    [SerializeField]
    List<T> target;
    public List<T> ToList() { return target; }

    public Serialization(List<T> target)
    {
        this.target = target;
    }
}


// Dictionary<TKey, TValue>
[Serializable]
public class Serialization<TKey, TValue> : ISerializationCallbackReceiver
{
    [SerializeField]
    List<TKey> keys;
    [SerializeField]
    List<TValue> values;

    Dictionary<TKey, TValue> target;
    public Dictionary<TKey, TValue> ToDictionary() { return target; }

    public Serialization(Dictionary<TKey, TValue> target)
    {
        this.target = target;
    }

    public void OnBeforeSerialize()
    {
        keys = new List<TKey>(target.Keys);
        values = new List<TValue>(target.Values);
    }

    public void OnAfterDeserialize()
    {
        var count = Math.Min(keys.Count, values.Count);
        target = new Dictionary<TKey, TValue>(count);
        for (var i = 0; i < count; ++i)
        {
            target.Add(keys[i], values[i]);
        }
    }
}
```

用法：
```csharp
// List<T> -> Json ( 例 : List<Enemy> )
string str = JsonUtility.ToJson(new Serialization<Enemy>(enemies)); 
// 输出 : {"target":[{"name":"怪物1,"skills":["攻击"]},{"name":"怪物2","skills":["攻击","恢复"]}]}

// Json-> List<T>
List<Enemy> enemies = JsonUtility.FromJson<Serialization<Enemy>>(str).ToList();

// Dictionary<TKey,TValue> -> Json( 例 : Dictionary<int, Enemy> )
string str = JsonUtility.ToJson(new Serialization<int, Enemy>(enemies)); 
// 输出 : {"keys":[1000,2000],"values":[{"name":"怪物1","skills":["攻击"]},{"name":"怪物2","skills":["攻击","恢复"]}]}

// Json -> Dictionary<TKey,TValue>
Dictionary<int, Enemy> enemies = JsonUtility.FromJson<Serialization<int, Enemy>>(str).ToDictionary();
```
---
title: UWP-学习笔记
date: 2020-04-26 11:17:16
tags: 
    - UWP
    - CSharp
img:
description: Windows10原生APP学习记录
top:
---
# 各种选择框
## 文件夹选择
```csharp
FolderPicker pick = new FolderPicker();
pick.FileTypeFilter.Add("*");
var folder = await pick.PickSingleFolderAsync();
if (folder != null)
{
    TB_HexoPath.Text = folder.Path;
}
```
# ContentDialog
[张高兴的 UWP 开发笔记：定制 ContentDialog 样式](https://www.cnblogs.com/zhanggaoxing/p/6617806.html)


# 在本地保存和加载设置
## 保存设置
```csharp
ApplicationDataContainer localSetting=ApplicationData.Current.RoamingSettings;
localSetting.Values[key] = value;
```
## 加载设置
```csharp
ApplicationDataContainer localSetting=ApplicationData.Current.RoamingSettings;
string value = localSetting.Values[key] as string;
```

# 通知消息
首先需要通过NuGet安装 Microsoft.Toolkit.Uwp.Notifications
```csharp
ToastContent Content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText(){Text = "正在等待用户完成操作..."},
                new AdaptiveText(){Text = "请开启文件系统权限"},
                new AdaptiveText(){Text = "随后点击下方的立即启动"}
            }
        }
    },

    Actions = new ToastActionsCustom
    {
        Buttons =
        {
            new ToastButton("立即启动","Restart")
            {
                ActivationType =ToastActivationType.Foreground
            },
            new ToastButtonDismiss("稍后")
        }
    }
};
ToastNotificationManager.CreateToastNotifier().Show(new ToastNotification(Content.GetXml()));
```

# 访问其他磁盘文件权限
## Package.appxmanifest 中添加权限
```xml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```
## 判断是否有权限
```CSharp
void Method()
{
    bool isFileAccessible = await CheckFileAccessAuthority().ConfigureAwait(false);
}
Task<bool> CheckFileAccessAuthority()
{
    return Task.Run(() =>
    {
        try
        {
            var folder = StorageFolder.GetFolderFromPathAsync(Environment.GetLogicalDrives().FirstOrDefault()).AsTask().Result;
            return true;
        }
        catch
        {
            return false;
        }
    });
}
```
## 进入设置开启权限
```csharp
await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-broadfilesystemaccess"));
```

# 获取APP信息
## 获取版本号
```csharp
Windows.ApplicationModel.Package.Current.Id.Version;
```
## 更多
[UWP 应用获取各类系统、用户信息 (1) - 设备和系统的基本信息、应用包信息、用户数据账户信息和用户账户信息](https://void2.dev/uwp-system-info-collect-1/)

# 获取窗口大小
## 获取主窗体大小
```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds
```
## 获取当前窗体大小
```csharp
Window.Current.Bounds
```
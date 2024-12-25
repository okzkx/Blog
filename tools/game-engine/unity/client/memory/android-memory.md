## 安卓内存

- [Unity游戏内存分布概览 - 知乎](https://zhuanlan.zhihu.com/p/370467923)
- [Unity如何统计安卓PSS内存？ - 知乎](https://zhuanlan.zhihu.com/p/372883142)

### 获取当前 Android PSS

在 Unity C# 应用中获取当前进程的 PSS（Proportional Set Size）信息在安卓系统中也是有一定的挑战。Unity 应用同样受限于沙盒环境，无法直接获取操作系统级别的信息。然而，你可以尝试通过调用 Android 系统的 Java API 来实现获取进程内存信息的功能。

以下是一种可能的方法，在 Unity C# 应用中调用 Android Java API 获取当前进程的 PSS 信息：

1. **创建一个 Java 类**：首先，在 Unity 项目中创建一个 Android 插件，编写一个 Java 类来获取当前进程的 PSS 信息。

java

```
// MemoryInfo.java
package com.example.memoryinfo;

import android.app.ActivityManager;
import android.content.Context;

public class MemoryInfo {
    public static long getCurrentProcessPss(Context context) {
        ActivityManager activityManager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        android.os.Debug.MemoryInfo[] memoryInfo = activityManager.getProcessMemoryInfo(new int[]{android.os.Process.myPid()});
        
        return memoryInfo[0].getTotalPss();
    }
}
```

2. **在 Unity C# 中调用 Java 方法**：然后，在 Unity C# 脚本中通过 AndroidJavaObject 来调用上述 Java 方法。

csharp

```
using UnityEngine;

public class MemoryInfo : MonoBehaviour
{
    void Start()
    {
        AndroidJavaClass unityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
        AndroidJavaObject currentActivity = unityPlayer.GetStatic<AndroidJavaObject>("currentActivity");
        AndroidJavaClass memoryInfoClass = new AndroidJavaClass("com.example.memoryinfo.MemoryInfo");
        
        long pss = memoryInfoClass.CallStatic<long>("getCurrentProcessPss", currentActivity);
        
        Debug.Log("Current Process PSS: " + pss);
    }
}
```

这样，当 Unity 应用在安卓设备上运行时，它将调用 Java 方法来获取当前进程的 PSS 信息。请确保将 Java 类正确放置在 Unity 项目的对应位置，并将 AndroidManifest.xml 配置正确以允许你的应用访问系统信息。
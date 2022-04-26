# Mod for Unity Game

## 必要工具

在 github 里下载 release 版本

- dnSpy :  反编译 dll 软件
- AssetStudio : 游戏解包软件
- [BepInEx](https://github.com/BepInEx/BepInEx) : Mod 框架，一般只能做 Mono 打包的游戏
  - UnityExplore : BepInEx 插件,游戏内窗口
  - Harmony : 游戏源码修改
  - NStrip : 修改程序集内成员访问限定

## 步骤

1. 把 BepIn 的所有文件复制到游戏根目录
2. 从 Steam 启动游戏
3. BeplnEx 文件夹会自动生成几个文件
4. BeplnEx.cfg Enabled 打开为 True，可以看到 Log 窗口
5. UnityExplore 作为 BepInExEx 的插件放入 Plugin
6. 新建 .Net 类库，选择合适的 Framework，引用游戏内 Unity.Core 两个程序集，Bepln Unity 和 Core 两个程序集。
7. 配置输出 dll 到 Plugin，设置不复制依赖的程序集
8. 编写代码，继承 BaseUnityPlugin
9.  使用 Object Explorer找场景中的单例管理器
10. 使用 Sci 反编译 Assembly 并分析代码
11. 使用 Sci 分析
12. 继承 BaseUnityPlugin 的类相当于单例 Mono
13. 使用 Config 命名空间和类来使用配置文件加载参数
14. 使用 Harmony 创建游戏方法的前置后置
    1. 引用 0Harmony 程序集
    2. 使用 [HarmonyPostfix,Harmonypatch(typeof(StaticData),"FunctionName")
    3. 填入 Harmony 关键字
    4. 在 Start 使用 Harmony.CraeteAndPatchAll(typeof(ClassName)) 注入类里边的所有方法进程序
    5. Harmony 前置方法可以返回 false 拒绝游戏内函数执行
    6. Harmony 前置方法使用 ref 关键字修改形参数据
    7. Traverse 相关方法通过反射修改类对象成员数据
    8. 可通过 MethodType 特性修改各种方法，属性，构造函数，协程等
    9. 使用 Type，ArgumentType 来精确匹配方法

1. 使用 shell 命令 NStrip.exe -p Assembly-CSharp.dll 生成全部成员都是 public 的 Assembly 程序集
2. 属性上设置：允许不安全代码

1. 断点调试 BepInEx, 过于复杂，一般用 Console 来调试

1. 在 OnGUI() 方法里写 IMGUI 相关代码在 Unity 里显示 GUI 进行调试 

1. 在 Unity 里打包 AssetBundle， 然后复制到游戏内文件夹，BaseUnityPlugin 对象内些代码进行加载

1.
# 调试安卓机

## 使用 profiler 调试

#### 步骤：

1. 首先打包时要选择打 Develop 或 Debug 包。
2. 手机运行游戏。
3. Unity Profiler 选择监测对应机器。
4. 如果没有效果，需要手动打开 Adb 服务器。
5. 在Unity 的 platform-tools 对应目录使用命令行打开。
6. adb forward tcp:34999 localabstract:Unity-com.xxx.xxx

#### Notes：

1. 使用新版 Build Pipline 打包后需要手动打开 Adb 服务，不然 Profile 监测不到。
2. 打开 Adb 时，端口，包名，要和 Unity 中的一致。

#### 我的工程示例：

> cd C:\Program Files\Unity\2019.3.06f\Editor\Data\PlaybackEngines\AndroidPlayer\SDK\platform-tools
>
> adb forward tcp:34999 localabstract:Unity-com.zyq.subscenedemo

## 输出 Unity 打印日志

#### 命令

`adb logcat -s Unity -d`

#### 我的工程示例

> adb logcat -s Unity -d &gt; C:\Users\zyq\Desktop\log.txt

## 查看机器 OpenGL 版本

[unity3d 获取OpenGL版本号 android](https://blog.csdn.net/qqo_aa/article/details/95215532)

### Unity SDK 配置

1. 直接在 Hub 里安装相应的安卓开发工具包
2. 使用 Android Studio &gt; Tool &gt; SDK Manager 下载 SDK 然后设置进 Unity Editor

## 使用 ADB 安装应用

> adb install -r D:\xmfunny\subscenedemo\Builds\es30.apk

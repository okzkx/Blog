```bat
:: 让当前批处理窗口支持UTF-8，就是避免中文乱码

chcp 65001

:: 从下一行开始关闭回显

@echo off

  

:: 1、拖拽的apk路径

set apk_path=%1

  

:: 2、判断APK路径是否有传入，比如用户直接运行脚本就不会传这个参数

:check_apk

if not exist "%apk_path%" (

    echo "APK文件不存在，请把APK文件拖拽到这个脚本上！"

    goto :error

)

  

:: 3、检查adb命令是否可用，2>&1 代表将stderr重定向到stdout(标准输出流)

:check_adb

adb --version >nul 2>&1

:: 如果命令执行成功会返回0，neq代表：不等于

if %ERRORLEVEL% neq 0 (

    echo "adb.exe不存在，请先添加此文件到该目录下，或者配置PATH环境变量！"

    goto :error

)

  

:: 4、安装APK

:install

echo "安装 %apk_path%..."

adb install -r "%apk_path%"

if %ERRORLEVEL% neq 0 (

    echo "安装失败..."

    goto :error

)

echo "安装成功，你可以在安卓设备上打开此APP啦~"

  

adb shell am start -n com.xmfunny.GR/com.unity3d.player.UnityPlayerActivity

  

adb shell top

  

:error

pause
```
---
title: Adb
date: 2020-06-19 10:00:07
categories: Unity
description:

---





cd C:\Program Files\Unity\2019.3.06f\Editor\Data\PlaybackEngines\AndroidPlayer\SDK\platform-tools

adb forward tcp:34999 localabstract:Unity-com.zyq.subscenedemo

adb logcat -s Unity -d > C:\Users\zyq\Desktop\log.txt
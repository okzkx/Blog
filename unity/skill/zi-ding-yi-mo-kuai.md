# 自定义模块

## 将文件夹制作成 Custom Package

1. 在文件夹下新建 package.json 文件
2. 按照格式填写 json 中的各项属性值

```javascript
{
    "name": "com.zyq.scenesplit",
    "displayName": "SceneSplit",
    "description": "基于 KDTree 的场景分割工具",
    "version": "1.0.1",
    "unity": "2019.3",
    "license": "MIT",
    "dependencies": {
        "com.whinarn.unitymeshsimplifier": "2.3.3"
    }
}
```

大功告成

现在这个文件夹下的所有内容就在一个 Custom Package 中了，

## 导入 Custorm Package

### 两种操作：

1. 使用 Package Manager 进行操作
2. 直接修改 /Packages/manifest.json

实际上操作 1 的结果也是修改 manifest.json

### 导入方式

1.  拖入文件夹导入

    直接将 Custom Package 文件夹拖入 /Packages/，与 manifest.json 同一目录，Unity 将自动导入。

    其他方式都是 修改 manifest ，或者直接用 PackageManager。
2.  指定文件夹导入

    `"com.unity.entities": "file.//com.unity.entities@0.5.1-preview.11",`

    使用绝对或者相对路径指定 Custom Package 文件夹
3.  指定 git 地址导入

    `"com.zyq.phonemove": "http://git.tube/zengkaixiang/phonemove.git",`
4.  官方平台导入

    `"com.unity.modules.ai": "1.0.0",`

    如果包名在官方的库中存在，指定版本号即可直接从官方处下载。

### 注意事项

1.  不可修改导入的包的内部文件、

    以引用的方式导入包，不是以拷贝的方式导入包。

    所以不可修改包内文件，会导致原始的包一起变动。

    如果是非本地导入，倒是影响不大，毕竟不会影响到远端。

    单如果 manifist 更新将会刷新全部包体信息，如有修改将会重新下载。
2.  包体依赖

    package dependencies 可以指定包体的依赖，如果项目中不存在对应的依赖包名，该 CustomPackage 将会报错。如果包名是官方包，将会自动下载对应包体。
3.  包内依赖

    包内一般 Editor Assembly 依赖于 Runtime Assembly，有些 Assembly 会依赖于外部 Assembly。Assembly 依赖不能选择依赖于 GUID，因为换项目后，GUID 会变化，Assembly name 不会。

## 视频资料

[https://www.bilibili.com/video/BV1BV411m7mF](https://www.bilibili.com/video/BV1BV411m7mF)

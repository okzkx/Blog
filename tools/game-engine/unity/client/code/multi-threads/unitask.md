- [开源库UniTask笔记-CSDN博客](https://blog.csdn.net/sinat_34014668/article/details/127602629)
- [UniTask中文使用指南(一) - 知乎](https://zhuanlan.zhihu.com/p/572670728)


```
UniTask.Void(Push);

private async UniTaskVoid Push() {  
    await UniTask.SwitchToThreadPool();  
    string gitPath = "git"; // 设置你的 git.exe 路径  
    string projectPath = Application.dataPath.Replace("/Assets", "");  
  
    // 推送到远程仓库  
    Process.Start(gitPath, $"-C {projectPath} push")?.WaitForExit();  
  
    Debug.Log("Push success");  
}
```

不能 

await  UniTaskVoid

要 
await
async UniTask

用

Unitask.Void( 返回 UniTaskVoid)

UniTask.Void(async () => Push().Forget());
UniTask.Void(async () => await Push());

Unitask.Create( 返回 UniTask).Forget


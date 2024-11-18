- [开源库UniTask笔记-CSDN博客](https://blog.csdn.net/sinat_34014668/article/details/127602629)
- [UniTask中文使用指南(一) - 知乎](https://zhuanlan.zhihu.com/p/572670728)


```
UniTask.Void(Push);

private async UniTaskVoid Push() {  
    await UniTask.SwitchToThreadPool();  

    Process.Start(gitPath, $"-C {projectPath} push")?.WaitForExit();  
  
    Debug.Log("Push success");  
}
```

不能  await  返回 async UniTaskVoid 的方法

要 await 返回 async UniTask 的方法

从主线程调用子线程

Unitask.Void( 返回 UniTaskVoid)
UniTask.Void(async () => Push().Forget());
UniTask.Void(async () => await Push());

Unitask.Create( 返回 UniTask).Forget

Unitask 默认在主线程运行, 要切到子线程

await UniTask.SwitchToThreadPool(); 
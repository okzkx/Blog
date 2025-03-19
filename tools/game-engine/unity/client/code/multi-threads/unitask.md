- [开源库UniTask笔记-CSDN博客](https://blog.csdn.net/sinat_34014668/article/details/127602629)
- [UniTask中文使用指南(一) - 知乎](https://zhuanlan.zhihu.com/p/572670728)

#### 示例

```
UniTask.Void(Push);

private async UniTaskVoid Push() {  
    await UniTask.SwitchToThreadPool();  

    Process.Start(gitPath, $"-C {projectPath} push")?.WaitForExit();  
  
    Debug.Log("Push success");  
}
```

```
  
UniTask.Void(async () => {  
    GameUtil.OpenUIs<LoadingCanvas>();  
  
    AsyncOperation asyncLoad = SceneManager.LoadSceneAsync("Battle");  
    await UniTask.WaitUntil(() => asyncLoad.isDone);  
    var entryBattle = GameObject.FindFirstObjectByType<EntryBattle>();  
    entryBattle.LevelName = levelName;  
});
```
#### await

- 不能  await  返回 async UniTaskVoid 的方法
- UniTaskVoid 只用于最后一层 Unitask.Void(()=>UniTaskVoid)， 所以用 Lambda 之后 UniTaskVoid 就不会出现了
- 要 await 返回 async UniTask 的方法

#### 从主线程调用子线程

// Fire and Forget 不管异步返回值

- Unitask.Void( 返回 UniTaskVoid)
- UniTask.Void(async () => Push().Forget());
- UniTask.Void(async () => await Push());
- UniTask.Void(async () => { ... });
- Push().Forget() // 编译器警告

- Unitask.Create( 返回 UniTask).Forget

- Unitask 默认在主线程运行, 要切到子线程

- await UniTask.SwitchToThreadPool(); 

## 其他资料

- [每个人都能写UniTask!!!自定义一个TaskLike并完成一个简单的Delay延时教程 - 知乎](https://zhuanlan.zhihu.com/p/12674069664)


## 视频教程

- [Unitask 教程](https://space.bilibili.com/69585/lists/548041?type=season)
- [UniTask 示例](https://gitee.com/yicunjun/share-github-sample/tree/master)
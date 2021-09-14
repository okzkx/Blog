---
categories: Git
---

# 同步

## Git 远端同步理解

### Commit

Git 维护一条链表，这条链表上的节点称为 Commit，Commit 存储着一个版本的提交记录。

你的链表上有几个 Commit，就代表你对这个项目保存了几个版本。

### Branch

我们习惯将一条链表称为分支（Branch）

### Branches

这里只关注单分支的 Git。

使用了 Github 或其他服务作为远端后，你的项目将有两条 Branch。

一条在远端服务器上 \(RemoteBranch\)，

一条在本地 \(NativeBranch\)

Git 在还本地保存了一个 RemoteBranch 的副本\(RemoteBranchClone\)

### 本地项目修改

你对项目进行修改完成并提交保存版本后，将在 NativeBranch 最后新增一个 Commit 节点。\(Commit\)

更新 RemoteBranchClone 同步远端的 RemoteBranch 。\(Fetch\)

### 远端项目同步

此时可能会出现项目分叉。

首先，你项目修改前 RemoteBranchClone 和 NativeBranch 是完全一致的，我们可以认为他们是同一条链。其最新的 Commit 假设为 A。

有人从 RemoteBranch A 处提交了 Commit B。

你在 NativeBranch A 处提交了 Commit C。

你在 Fetch 后会发现，A 处分开了两条岔路 B 与 C。

Git 需要不允许链表末端分叉，于是你可以做如下选择：

1. 放弃 B 或 放弃 C
2. 让你的 C 接在 B 的后面（Rebase）
3. 根据 C 生成一个 D 接在 B 的后面，一般 D 与 C 基本一致。（Merge）

### Merge

选择 Merge 后你的链表中间分叉了，在最后又连起来了。

这个方法最常用，因为不会损失信息。

但是又会带来一个问题，文件冲突（conflix），B 与 C 都改了文件 T ，D 该怎么生成呢？

Git 也不知道，他会跟你说这个文件哪些行冲突了，你改好后跟 Git 说 D 用这个新的文件去生成。

### 最终

你的选择将会更新 RemoteBranchClone ，然后再把它更新到 RemoteBranch 和 NativeBranch 。

至此，完成了一次本地与远端的工程 Branch 同步，最终所有 Branch（3个）变成同一链表。

## Git 工程同步工作流

### 情景假设

我从远端 clone 了一个工程到本地，

我修改了工程代码，并上传回远端，希望远端能更新我这部分代码。

可不幸的是，我这部分代码在我还没上传回去前，被人修改过了，文件出现了冲突。

于是我解决这个冲突，让远端更新我这部分代码。

### 具体操作步骤

1. 代码修改
2. 保存代码变更 \(Stage Changes\)：这里的变更可以随时修改
3. 提交代码变更 \(Commit\)：根据已经保存的变更生成一个不可修改的记录，
4. 拉取远端仓库 \(Fetch\)
5. 解决文件冲突 \(Resolve Conflix\)
6. 提交合并变更 \(Merge Commit\)
7. 推送远端仓库 \(Push\)


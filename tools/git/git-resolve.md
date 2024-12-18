# Git 使用出问题时的解决办法

### 解决 git 换行符问题

[CRLF和LF的差异 - 知乎](https://zhuanlan.zhihu.com/p/380574688)

```
git config --global core.autocrlf true
```

### git 全局修改提交用户名

[Git 修改提交用户名称](https://stackoverflow.com/questions/750172/how-to-change-the-author-and-committer-name-and-e-mail-of-multiple-commits-in-gi)

```
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='okzkx'; GIT_AUTHOR_EMAIL='okzkx@qq.com'; GIT_COMMITTER_NAME='okzkx'; GIT_COMMITTER_EMAIL='okzkx@qq.com';" HEAD
```

### git 指定项目用户名邮件

```
git config user.name "okzkx"
git config user.email "okzkx@qq.com"
```

### git 指定全局用户名邮件

```
git config --global user.name "okzkx"
git config --global user.email "okzkx@qq.com"
```

#### git vpn 代理

``` cmd
git config --global http.proxy 127.0.0.1:9788 --replace-all
```

git 更新所有 Submodule

``` sh
#!/bin/bash

git fetch origin
git checkout develop
git reset --hard origin/develop
git submodule foreach --recursive 'git fetch origin'
git submodule foreach --recursive 'git checkout develop'
git submodule foreach --recursive 'git reset --hard origin/develop'
read -p "Press Enter to continue..."
```

## Git tag

```
git tag --annotate tag-name commit-hash -m message
```

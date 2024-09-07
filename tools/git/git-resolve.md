# Git 使用出问题时的解决办法

### 解决 git 换行符问题

```
git config --global core.autocrlf true
```

### git 批量修改提交用户名

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
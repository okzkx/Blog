# Git 用户

[Git 修改提交用户名称](https://stackoverflow.com/questions/750172/how-to-change-the-author-and-committer-name-and-e-mail-of-multiple-commits-in-gi)

### git 批量修改提交用户名

```
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='okzkx'; GIT_AUTHOR_EMAIL='okzkx@qq.com'; GIT_COMMITTER_NAME='okzkx'; GIT_COMMITTER_EMAIL='okzkx@qq.com';" HEAD
```

### git 查看用户名邮件

```
git config user.name
git config user.email
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
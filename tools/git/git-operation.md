# Git 操作

[C\#/.NET使用git命令行来操作git仓库的方法示例](https://www.jb51.net/article/158808.htm)

[C\#中运行命令获取Git commit id的方法](https://blog.csdn.net/lingyanpi/article/details/72472958)

[Git 修改提交用户名称](https://stackoverflow.com/questions/750172/how-to-change-the-author-and-committer-name-and-e-mail-of-multiple-commits-in-gi)

### git 批量修改提交用户名

```
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='okzkx'; GIT_AUTHOR_EMAIL='okzkx@qq.com'; GIT_COMMITTER_NAME='okzkx'; GIT_COMMITTER_EMAIL='okzkx@qq.com';" HEAD
```

### git 指定项目用户名邮件

```
git config user.name "okzkx"
git config user.email "okzkx@qq.com"
```


---
title: git&github使用
date: 2024-01-01
---

虽然可以在vscode可以直接按按钮，但一些基本的命令行和原理还是要掌握的

## git tag

> vscode: 标记

git tag给某一次commit做一次静态标注。可以commit注释。常用于git release发布版本（放置大文件到git release）

**默认使用当前branch的最新commit**

```
git tag -a v1.0.0 -m "Release version 1.0.0" 
git push origin --tags # 推送本地所有tag
```
删除标签

```
git tag -d {标签名}
git push origin :refs/tags/{标签名}
```

- **git 本地tag和远程tag对应不上 vscode里pull不下代码**

```cmd
git ls-remote -t # 查看远程tags                 
git tag -l  # 查看本地tag
git tag -d xxx # 删除本地tag
git fetch origin --prune-tags # 最后远程拉取远程tags   
git push origin --delete tag 标签名  # 删除远程tags
```

- vscode按按钮同步tag
- [VSCode配置git push --tags_vscode push tag-CSDN博客](https://blog.csdn.net/u010214511/article/details/127030248)

## git lfs使用和删除

[github大文件上传：使用LFS （以及如何将lfs从仓库中移除！）_github lfs-CSDN博客](https://blog.csdn.net/weixin_39278265/article/details/121103819)

删除：

```
git lfs uninstall
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch results_bears/results_bears_processed_machine_1.tar.lrz results_bears/results_bears_processed_machine_2.tar.lrz" --prune-empty --tag-name-filter cat -- --all
git push origin master -f 
```

## .gitignore失效

[.gitignore文件不起作用的解决方法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/334908553)

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push
```

- **仓库删除model文件夹，改为release上传，避免git lfs爆免费额度**
- **更新`README.md`表述**

## 合并dev分支并删除
要将 `dev` 分支合并到 `master` 并删除 `dev` 分支，同时将这些操作应用于远程仓库，以下是详细的步骤：

1. 切换到 `master` 分支
```bash
git checkout master
```

2. 合并 `dev` 分支到 `master`
```bash
git merge dev
```

 3. 删除本地 `dev` 分支
```bash
git branch -d dev
```

如果分支还没有完全合并且你确实要强制删除，可以用 `-D` 选项：
```bash
git branch -D dev
```
4. 推送合并后的 `master` 分支到远程
```bash
git push origin master
```

5. 删除远程 `dev` 分支
```bash
git push origin --delete dev
```


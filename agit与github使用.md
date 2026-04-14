---
title: git&github使用
date: 2026-04-14
---

虽然可以在vscode可以直接按按钮，但一些基本的命令行和原理还是要掌握的

## git config

建议用github的noreply邮箱，见https://github.com/settings/emails

104623550+silverwolf-x@users.noreply.github.com.

```
git config user.name "silverwolf-x"
git config user.email 104623550+silverwolf-x@users.noreply.github.com.

git config --global --edit
git config --edit
```

## git tag（vscode可以按按钮）

git tag给某一次commit做一次静态标注。可以commit注释（我感觉最好不要加，保持commit都是代码修改引起的有效修改）。常用于git release发布版本（放置大文件到git release）

**默认使用当前branch的最新commit**

```
git tag -a v1.0.0 -m "Release version 1.0.0" 
git push origin --tags # 推送本地所有tag
```

**删除标签**

```
git tag -d {标签名}
git push origin :refs/tags/{标签名}
```

**在本地tag和远程tag取交集**

在本地tag和远程tag取交集，删除本地和远程的其他tag。然后同步两边的仓库。在win和linux都实现

```bash
#!/bin/bash

# 获取本地和远程标签
local_tags=$(git tag)
remote_tags=$(git ls-remote --tags origin | sed 's/.*\///')

# 计算交集
intersection=$(echo "$local_tags" "$remote_tags" | tr ' ' '\n' | sort | uniq -d)

# 删除本地没有交集的标签
for tag in $local_tags; do
  if [! echo "$intersection" | grep -q "$tag"]; then
    echo "Deleting local tag: $tag"
    git tag -d "$tag"
  fi
done

# 删除远程没有交集的标签
for tag in $remote_tags; do
  if [! echo "$intersection" | grep -q "$tag"]; then
    echo "Deleting remote tag: $tag"
    git push --delete origin "$tag"
  fi
done

# 推送交集标签到远程
for tag in $intersection; do
  echo "Pushing tag to remote: $tag"
  git push origin "$tag"
done

# 拉取远程标签到本地
git fetch --tags
echo "Sync complete."

```

## .gitignore失效

[.gitignore文件不起作用的解决方法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/334908553)

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push
```

```
如果 optimize 里真正想保留的只有 gold_vector_backtest.py 这一个文件，可以这样写：
*
!.gitignore
!requirements.txt
!show.ipynb
!strategy.py

# 针对 optimize 目录，先不忽略目录本身
!optimize/

# 只允许这个具体文件（路径相对根目录）
!optimize/gold_vector_backtest.py
```

## git lfs与大文件上传

推荐**仓库删除model文件夹，改为release上传，避免git lfs爆免费额度**

或者直接用huggingface存储模型

## git 将默认分支从master改为main

https://iamyuanyu.github.io/p/github%E4%BB%93%E5%BA%93%E4%BF%AE%E6%94%B9%E9%BB%98%E8%AE%A4%E5%88%86%E6%94%AF/

在终端运行以下命令，设置 `main` 为未来所有新仓库的默认分支：

```
git config --global init.defaultBranch main
git config --global init.defaultBranch  # 应输出 `main`
```

修改现有本地仓库的主分支名称

如果已有本地仓库仍在使用 `master`，需手动重命名分支

```
git branch -m master main  # 将本地分支从 master 重命名为 main
```

更新远程仓库（如 GitHub）

1. 推送 `main` 分支到远程：`git push -u origin main`

2. 在远程仓库设置中将 `main` 设为默认分支：**GitHub**: `Settings` → `General` → `Default branch` → 选择 `main` → 点击 `Update`。
3. 删除远程的 `master` 分支（可选）：`git push origin --delete master`

通知协作者运行以下命令更新其本地分支：

```
git fetch origin
git branch -m master main           # 重命名本地分支
git branch -u origin/main main      # 关联本地 main 到远程 main
git remote set-head origin -a       # 更新远程 HEAD 引用
```

验证修改是否成功

```
git branch -a  # 应显示 `main` 而非 `master`
git remote show origin  # 查看 HEAD branch 是否为 `main`
```

## 本地强制同步远程

```
git fetch origin && git reset --hard origin/main
```

## 申请学生认证

用https://edu.chatgpt.org.uk/生成英语的学生证，然后用手机来定位并拍摄

https://linux.do/t/topic/653825

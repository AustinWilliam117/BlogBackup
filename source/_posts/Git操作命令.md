---
title: Git操作命令
date: 2021-10-10 17:37:27
tags: ["git"]
---

> 创建仓库

```bash
git init
```

> 使用固定目录作为本地仓库

```bash
git init 目录
```

> 提交

```bash
git add .
git add 具体文件
git commit -m "说明文案"
```

<!--more-->

> 克隆远程仓库到本地

```bash
git clone git 地址
```

> 显示当前 git 配置

```bash
git config --list
```

> 设置提交用户名 - 全局

```bash
git config --global user.name "runoob"
git config --global user.enail test@runoob.com
```

> 简单命令

```bash
git add 				添加文件到仓库
git status				查看仓库当前的状态，显示有变更的文件
git diff				比较文件得不同，即暂存区和工作区得差异
git commit				提交暂存区到本地仓库
git reset				回退版本
git rm					删除工作区文件
git mv					移动或重命名工作文件
git log					查看历史提交记录
git blame <file> 		以列表形式查看指定文件得历史修改记录
git remote				远程仓库操作
git fetch				从远程获取代码库
git pull				下载远程代码并合并
git push				上传远程代码并合并
```

> 分支管理

```bash
创建分支：git branch 分支名
切换分支：git checkout 分支名
合并分支：git merge
列出所有分支：git branch
创建分支别切换：git checkout -b 分支名
删除分支：git branch -d 分支名
```

> 分支合并

```bash
git merge 新分支
```

> 冲突管理

```bash
git diff		查看冲突git commit -am "修改代码说明"git merge 		新分支名cat 冲突文件名手动修改后git status -sgit add 冲突文件名git commit 提交
```

> Git 查看提交历史

```bash
git log --onelinegit blame 文件名
```






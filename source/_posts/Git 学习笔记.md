---
title: Git学习笔记
data: 2022-05-26
tags: 
- tech
- git
---
# Git 学习笔记

## Git基本命令

> **git --version** : 查看git的版本信息
> **git config --global user.name 用户名** : 设置用户签名
> **git config --global user.email 用户邮箱** : 设置用户邮箱

### 操作暂存区中的文件

> **git init** : 初始化本地仓库
>
> **git status** : 查看本地库的状态
>
> **git add 文件名** : 添加到暂存区
>
> **git rm --cached 文件名** : 删除暂存区中的文件

### 将暂存区中的文件提交到本地库

> **git commit -m "版本名称" 文件名** : 提交本地库

### 查看全部版本信息

> **git reflog** : 查看版本信息的命令
> **git log** : 查看详细的版本信息

### Linux中的命令在Git Bash也能使用

> **ll** : 查看当前目录下的文件
>
> **vim 文件名** : 新建或编辑文件
>
> **cat 文件名** ：查看文件

### 版本穿梭语句：即HEAD指向，HEAD指向哪里当前就访问哪里

> **git reset --hard 版本号** : 版本穿梭

### 分支操作：切换分支，创建分支等

> **git branch -v** : 查看分支
> **git branch 分支名** : 新建分支
> **git checkout 分支名** : 切换分支(可以根据版本号来切换，上下同，即分支名可以是查看分支中的唯一标识)
> **git merge 分支名** : 将指定分支合并到当前分支

### 操作远程仓库

> **git remote -v** : 查看当前所有远程地址别名
> **git remote add 别名 远程地址** : 新建远程地址别名

### 推送分支

>  **git push 别名/远程仓库url地址 分支** : 把本地的分支推送到远程仓库

### 从远程仓库拉取代码

>  **git pull 别名/远程仓库url地址 分支** : 将远程库中的代码拉取到本地库中

### 生成免密登录公钥(SSH免密登录GitHub&Gitee，用于不用登录然后直接根据ssh拉取或推送代码)(建议在user用户目录下创建.ssh文件保存密钥信息)

>  **ssh-keygen -t rsa -C ddddreem@163.com**  (-t : 指定使用哪种加密算法生成密钥, rsa : 非对称加密协议, -C : 描述(即后面跟着的就是描述))

### token令牌：用于在idea中登录github账户，由于网速问题，GitHub很难登录上去，因此使用token能快速登录上

```txt
gitee token : dd1747b69b6cae131628c848df8b9c9d // gitee网站上生成的token
github token : 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 // GitHub网站生成的token
```




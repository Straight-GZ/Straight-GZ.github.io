---
title: Git
tags:
  - Git
  - 笔记
---

## 1、git 本地仓库

git 就是一条命令

六行配置

```bash
git config --global user.name 你的英文名
git config --global user.email 你的邮箱
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "code --wait"
git config --global core.autocrlf input
```

 <!-- more -->

### 1.让你代码有版本：

git init :初始化（指定目录），创建 .git 目录

**git add** 路径:选择哪些变动是需要提交的

.gitignore：描述哪些文件是不需要提交的

git commit -m ：字符串：提交，说明理由 （字符串有空格，用引号）

**git commit -v**：回顾修改的内容，添加详细提交理由**（推荐）**

git reset --hard 提交号前六位：回到指定提交版本

git log： 查看历史 git reflog：查看所有的历史

### 2.分支：

git branch ：基于当前 commit 创建分支，在哪个分支提交，就创建在哪个分支

git checkout：切换分支

git merge:合并分支

**步骤**：

1. 选择需要保留的分支 master

2. 运行 git merge x（合并的分支）

3. 冲突

   - 发现冲突：

     合并分支的时候**conflict**提示

     使用 git status -sb 查看冲突文件

   - 解决冲突：

     依次打开文件

     搜素‘====’四个等于号

     选择要保留的代码，删除不用的代码

     删除’====‘’&lt;<<<''&gt;>>>'标记

     **git add 对应文件**

     再次使用 git status -sb 解决下一个冲突文件

     直到没有冲突，运行**git commit （不需要加选项）**

4. 合并完删除无用的分支 git branch -d x

### 总结：

- git 目录就是本地仓库
- git add 处理的是文件变化，而不是文件，删除文件后，依然需要 git add 添加到提交区
- 常用的命令：**git add** 和**git commit -v**

## 2、git 远程仓库

### 生成 ssh key（[帮助文档](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent "https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#")）：

- 运行 ssh-keygen -t rsa -b 4096 -C +邮箱地址
- 一直回车键到没有提示
- cat ~/.ssh/id_rsa.pub 得到公钥内容，复制内容
- 打开 Github，在设置页面粘贴公钥内容

### 测试：

- 运行 ssh -T git@github.com
- 询问 yes/no,回答 yes

### 上传代码：

1. 新建 Github Repo，复制 ssh 地址

2. 运行 git remote add origin git@xxxxx

   （在本地添加远程仓库地址，origin 为远程仓库默认名字）

3. git push -u origin master

- 推送本地 master 分支到远程 originmaster 分支
- 提示应该 git pull...
- -u origin master 意思为设置上游分支，之后不用再设置，直接 git pull，git push

**上传其它分支：**

一：git push x:x

二：git checkout x

```
git push -u origin x
```

**上传到两个本地仓库**

- git remote add origin2 git@xxxxx
- git push -u origin master

**git pull 冲突**：查看合并分支冲突

### 下载代码：

1.  git clone git@xxx
2.  不同机器需要先上传新的 ssh key(一机一 key)
3.  **cd 目标路径**
4.  运行 git add/git commit/\[git pull\]/git push

下载某个分支：先下载整个仓库，再切换分支

**git clone**：

- git clone git@?/xxx.git:会在当前目录下创建一个 xxx 目录，xxx/.git 是本地仓库，一般需要接 cd xxx 进入目录

- git clone git@?/xxx.git yyy：会在本地新建一个 yyy 目录，cd yyy 进入目录

- git clone git@?/xxx.git .:最后一个字符是点，使用当前目录容纳代码和.git，需要先新建一个目录，cd 进入目录

### 总结：

常用命令：git alone/git pull/git push

远程仓库：本地仓库的备份，要先 commit 到本地仓库，然后 push 到远程仓库

```
 无法下载部分代码，只能先clone整个仓库生成ssh key（[帮助文档](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#)）：
```

- 运行 ssh-keygen -t rsa -b 4096 -C +邮箱地址
- 一直回车键到没有提示
- cat ~/.ssh/id_rsa.pub 得到公钥内容，复制内容
- 打开 Github，在设置页面粘贴公钥内容

### 测试：

- 运行 ssh -T git@github.com
- 询问 yes/no,回答 yes

### 上传代码：

1. 新建 Github Repo，复制 ssh 地址

2. 运行 git remote add origin git@xxxxx

   （在本地添加远程仓库地址，origin 为远程仓库默认名字）

3. git push -u origin master

- 推送本地 master 分支到远程 originmaster 分支
- 提示应该 git pull...，就运行 git pull!
- -u origin master 意思为设置上游分支，之后不用再设置，直接 git pull，git push

**上传其它分支：**

一：git push x:x

二：git checkout x

```
git push -u origin x
```

**上传到两个本地仓库**

- git remote add origin2 git@xxxxx
- git push -u origin master

**git pull 冲突**：查看合并分支冲突

### 下载代码：

1.  git clone git@xxx
2.  不同机器需要先上传新的 ssh key(一机一 key)
3.  **cd 目标路径**
4.  运行 git add/git commit/\[git pull\]/git push

下载某个分支：先下载整个仓库，再切换分支

**git clone**：

- git clone git@?/xxx.git:会在当前目录下创建一个 xxx 目录，xxx/.git 是本地仓库，一般需要接 cd xxx 进入目录

- git clone git@?/xxx.git yyy：会在本地新建一个 yyy 目录，cd yyy 进入目录

- git clone git@?/xxx.git .:最后一个字符是点，使用当前目录容纳代码和.git，需要先新建一个目录，cd 进入目录

### 总结：

常用命令：git alone/git pull/git push

远程仓库：本地仓库的备份，要先 commit 到本地仓库，然后 push 到远程仓库

```
 无法下载部分代码，只能先clone整个仓库
```

## 3、git 高级操作

bash alias 简化命令： ga="git add" gc="git commit -v" gl="git pull" gp="git push" gco="git checkout" gst="git status -sb"

美化历史命令：git rebase -i xxx

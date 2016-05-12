---
layout: post
title:  "使用git更新本地代码至github"
date:   2016-02-23 09:14:54
categories: github
---

* content
{:toc}

最开始用上传代码至github时候，不会用git命令行，使用的是Windows github桌面客户端，每次运行的时候速度很慢，而且同步的时候经常会失败，直到后来实在是忍受不了，开始学习git，发现真的是高大上很多啊，同步速度相当赞，所以建议使用git命令行，github客户端就是坑。

关于git网上有很多学习资料，据说廖雪峰的Git教程就很好，但是我没有系统的学习。如果你的目的只是将本地的代码上传到github上，那么下面步骤足够满足要求。

[参考链接:http://blog.csdn.net/u010520912/article/details/18993001](http://blog.csdn.net/u010520912/article/details/18993001)

### 1 注册GitHub账号

1）到https://github.com/注册GitHub账号

### 2 在GitHub上建立GitHub仓库
1）登录后点击右下方的“new repository”按钮新建一个仓库

2）填写完仓库信息后点击“creat repository”按钮创建仓库（仓库名字随意填写）

注意不要勾选Initialize this repository with a README

### 3 下载并安装git版本管理工具

1）到[http://git-scm.com/downloads](http://git-scm.com/downloads)下载并安装git 版本管理工具

### 4 配置git版本管理工具
1）配置用户名和用户邮件

打开命令行或Git Bash，输入下面的命令：

git config --global user.name “YourName” 

git config --global user.email “YouEmailAddress”

若省略了“--global”，则只配置当前仓库用户信息


### 5 使用SSH密钥进行认证
1 ）生成SSH密钥，使用SSH方式认证登录
打开Git Bash（window下右击桌面菜单Git Bash选项/mac下直接使用终端），输入下面的命令：
ssh-keygen -C “YouEmailAddress” -t rsa

然后直接按回车使用默认路径保存密钥文件到当前用户文件夹中
密钥文件为（.ssh），接着设置密码和再次输入密码

2）添加SSH密钥到GitHub
到该目录找到.ssh文件夹（id_rsa为私钥文件，id_rsa.pub为公钥文件）
使用笔记本打开id_rsa.pub文件并复制文件内容
到GitHub中点击右上角的account settings

然后选择左边栏中的SSH Keys添加SHH Key粘贴刚才复制的内容到Key文本框中，title文本框随意填写

### 6 创建本地仓库并上传代码到GitHub
1）新建Text文件夹作为仓库根目录（文件夹名字随意命名）

2）将需要上传的代码文件加入到Text根目录

3）在根目录下建立仓库
使用命令行或Git Bash，输入下面命令：先进入到Text根目录下，再输入git init（初始化一个仓库）

4）将所有文件添加到仓库
使用命令行或Git Bash，输入下面命令：git add .

5）提交
使用命令行或Git Bash，输入下面命令：git commit -m “CommitInfo”

6）添加源到GitHub
git remote add origin git@github.com:YourName/YourRepositroy.git

7）上传源到GitHub
git push -u origin master

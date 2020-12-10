<!--
 * @Author: your name
 * @Date: 2020-12-04 11:29:33
 * @LastEditTime: 2020-12-04 15:49:36
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \mengmwq.github.io\source\_posts\git.md
-->
## git学习


#  git 的安装和配置
地址：https://git-scm.com/
# 配置   git  config  --global  全局  配置
git config --global use.name 配置用户名
git config --global user.email
git config --list 查看配置项
git init  创建仓库初始化仓库
ls 查看项目
git status 查看状态
git add . 添加暂存区
git commit -m '说明'  保存
git log  日志
git checkout - 回到上次版本
# git tag标签
apple  git add . && git commit -m  '说明'  
git  log --oneline
git tag -a v1 -m  '版本 说明'

#  git branch

git branch 分支名字           创建分支
git  checkout    分支名字     切换分支


# 合并分支
默认master  git add  . 
  git commit -m ''  
   git tag  v1 '' 
     git checkout -b  '分支名ffff'
     git add .  git commit -m '提交到ffff'
     git checkout master 
     git merge ffff
     解决chongtu
     git add .

# 远程仓库
gitub  码云  coding  
1.本地已经有文件 传到gitub上
git remote -v 查看目前的远程分支
git remote add  'gitub上的地址'
git push -u master


配置
git config --global user.name "<姓名>" 设置提交者姓名。
git config --global user.email "<邮箱>" 设置提交者邮箱。
基础操作
git init [目录名] 在指定目录创建仓库，如果没有指定目录名将在当前目录创建仓库。
git clone <远程仓库地址> [目录名] 从指定地址克隆仓库，若不指定目录名将默认创建与远程同名目录。
git add <目录名|文件名> 将文件或目录中已修改的代码添加追暂存区。
git commit -m "<注释>" 提交暂存区内容。
git status 查看仓库状态
比对 diff
git diff 比对当前内容和暂存区内容。
git diff HEAD 比对当前内容和最近一次提交。
git diff HEAD^ 比对当前内容和倒数第二次提交。
git diff HEAD^ HEAD 比对最近两次提交。
历史 log
git log [--oneline] [--all] 查看提交历史。
git log --oneline 打印为单行log。
git log --all 打印所有记录（忽略HEAD的位置）。
git log --graph 打印示意图（忽略HEAD的位置）。
分支 branch
git branch [分支] 有分支：创建分支，无分支：列出所有分支。
git checkout <分支> 切换至分支。
git checkout -b <分支> 创建并切换至分支分支。
git merge <分支> 将分支与当前分支合并。
远程
git pull 拉取远程仓库。
git push <远程仓库> <分支> 推送至远程仓库。
git remote add origin https://xxx.git 新增远程仓库origin
git remote set-url origin https://xxx.git 修改远程仓库origin

 


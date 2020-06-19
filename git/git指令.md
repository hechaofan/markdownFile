# git管理项目的流程

## 1. 本地管理

​    创建.gitignore文件, 并指定需要忽略文件/文件夹

​    git init 

​    git add *

​    git commit -m "提交文件"

## 2. 创建远程仓库

![img](https://upload-images.jianshu.io/upload_images/2915863-5ea725bac6c6359c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

![img](https://upload-images.jianshu.io/upload_images/2915863-4c641fd724e30173.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

![img](https://upload-images.jianshu.io/upload_images/2915863-270d87be63c7c3dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

![img](https://upload-images.jianshu.io/upload_images/2915863-9a3e49176b6892fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 



## 3. 推送到远程

​    git remote add origin 地址

​    git push origin master

## 4. 修改代码

​    git add .

​    git commit -m "update"

​    git push origin master

## 5. 拉取项目

​    git clone 地址

​    git pull origin master

  （如果改动代码，不需要推上去更新，要用git stash git pull origin master）



# git指令说明

mkdir XX：创建一个空目录 XX指目录名

pwd：显示当前目录的路径

git init：吧当前的目录变成可以管理的git仓库，生成隐藏的.git文件

touch xx：新建xx文件文件

git add xx：把xx文件添加到暂存区

git commit -m “xx”a.txt ：提交文件 -m后面的是注释

git status：查看仓库状态

git log：查看历史记录

git reset --hard HEAD^：网上回退一个版本

cat xx：查看xx文件内容

git reflog：查看历史记录的版本号id

git checkout -- xx：把xx文件在工作区的修改全部撤销

git rm xx：删除xx文件 之后要commit

git remote add origin 地址关联一个远程库

git push -u（第一次要用-u以后不用）origin master：把当前master分支推送到远程库

git clone 地址 从远程库中克隆

git checkout -b dev：创建dev分支 并切换到dev分支上

git branch：查看当前所有的分支

git checkout master：切换回master分支

git merge dev：在当前分支合并dev分支

git branch -d dev：删除dev分支

git branch xxx：创建分支xxx

git remote：查看远程库信息

git remote -v查看远程库的详细信息

git push origin master：git会把master分支推送到远程库对应的分支上



# 三年 Git 使用心得 & 常见问题整理

 https://mp.weixin.qq.com/s?__biz=MzIzNTU2ODM4Mw==&mid=2247492078&idx=2&sn=fb2885b7f75500eaee73c47aafe0448b&chksm=e8e78c7edf90056842a4193f0944e503605a476fe5e604733a5072b33256f280dff60a827138&scene=126&sessionid=1592529223&key=5cb5aa6ea207af94d330f73ed6ffe2c6cb0a36d3345fa9614ec618cc27e2ed7fcbf7994738372685f6343d0298e7f696a531221b9a5653fa870237376874ec34eb21663875ca76751d99c6d2fc9cbc8d&ascene=1&uin=MjAwMDUwNzMyMQ%3D%3D&devicetype=Windows+10+x64&version=62090070&lang=zh_CN&exportkey=A1nS1fOERlnaltv6TzSazpM%3D&pass_ticket=W6wIdF6GwQkEC%2BdNH%2BfZ2EXxYlMkUKq53qjS6aJr5xDrU5lfYhXjgmeK4mtvtpha 
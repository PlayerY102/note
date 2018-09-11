# Git Study Note

## 配置

* git config --system user.name ""  
    git config --system user.email 系统中对所有用户都普遍适用的配置  
    git config --global user.name 当先用户配置  
    git config --list 检查已有的配置信息  

## 修改

* git clone repository链接  
    git add 文件名  
    git commit -m '描述'  
    git commit -am '描述' 相当于 git add 加 git commit -m

## push

* git push origin master  
    git push <远程主机名> <本地分知名>:<远程分支名>  
    可以简单用为 git push

## pull

* git pull <远程主机名> <远程分支名>:<本地分支名>
    git pull
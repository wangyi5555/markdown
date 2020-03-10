# 使用命令



git init :初始化一个Git仓库

git add :添加文件到Git提交流

git commit -m "…":把提交流中的文件提交到Git仓库中并加上注释

git status:查看工作区状态

git diff:查看仓库中文件与目录文件中的不同之处

git log：查看git的提交日志 --pretty=oneline:省略部分信息并单行打印

log状态下输入：

q：退出返回到命令行

s：把日志信息保存到文件中

git reset --hard 版本号：退回到指定版本

git reflog:查看所有对git库的操作

git checkout -- 文件名：用版本库里的版本替换工作区的版本

git rm 文件名：删除git库中的文件

 

git remote add origin git@server-name:path/repo-name.git：关联一个GitHub上的仓库

git push （-u） origin master：推送本地仓库到GitHub上

 

git branch dev：创建一个新的分支

git switch dev：切换到新的分支

git switch -c dev：创建并切换到新的分支

git branch：查看当前分支

git merge dev：把目标分支合并到当前分支

git branch -d dev：删除分支

 

git tag -a  v1.4 -m "version 1.4" 创建一个标签

git push origin v1.4  把标签推送到远程git上

git tag -d v1.4 删除标签
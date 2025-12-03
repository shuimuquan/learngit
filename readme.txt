Git is a version control system.
Git is free software.
Git is a version control system.
Git is free software.
git init 把目录变成git可以管理的仓库
git add 文件1路径 文件2路径
不小心add某个文件，将该文件不add：git reset HEAD -- 文件路径
git commit -m "提交说明"

注：add把工作区修改存入暂存区，commit把暂存区的修改保存至本地版本库，生成提交记录

查看仓库当前状态：
PS D:\gitDemo\learngit1> git status
On branch master  当前所在分支
Changes not staged for commit:   工作区存在修改但没加入暂存区，无法提交
  (use "git add <file>..." to update what will be committed)  可以用add加入暂存区然后提交
  (use "git restore <file>..." to discard changes in working directory)  也可以将文件恢复到上一次提交的状态
        modified:   readme.txt    工作区被修改的文件名

no changes added to commit (use "git add" and/or "git commit -a")  暂存区为空，没有可提交的内容，
注意"git commit -a"，-a 参数会自动暂存「所有已追踪文件」的修改，直接提交；

查看工作区修改 vs 【暂存区 / 最新提交版本】的差异对比
git diff 或者 git diff 文件路径
最后按“q”推出信息打印。


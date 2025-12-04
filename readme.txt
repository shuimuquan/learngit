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

5.1 版本回退：

git log  查看每次提交的版本号
回退到指定版本号：
git reset --hard 对应版本号（即commid_id,可以是前若干位数）
工作区、暂存区、当前分支指针 会全部跳转到 <版本号> 对应的提交状态，所有未提交的修改会被永久删除（无法恢复）；
回退到指定版本号后又想回到最新的版本号此时git log已找不到之前最新的版本号,git reflog查看命令历史，以便确定要回到未来的哪个版本;



5.4 撤销修改：

让工作区文件回退到最近一次add（暂存区）或commit（版本库）后的状态：
git checkout -- 文件路径
注意：如果没有 -- 就变成的切换分支了。
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

若已经add到暂存区，想回退暂存区且不影响当前工作区：
git reset HEAD 文件名
HEAD 就是你当前分支「最后一次提交的版本」，是 Git 判定「文件原始状态」的参考标准，
git reset HEAD <文件> 就是让文件的「暂存区状态」回到这个参考标准。
若add且commit，要撤回的话只能用“git reset --hard 对应版本号”版本回退（前提是还没推到远程仓库）。

5.5 删除文件
手动删除工作区文件后，如果不是误删则git rm 文件名，再commit就好；
如果是误删要恢复：git checkout -- 文件路径


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


6.1 将本地仓库于github仓库关联并推送分支：
1. 修改分支名称，旧版git和部分新版git的默认分支命名位master，
而GitHub/GitLab 等平台已将默认分支名从 master 改为 main（出于命名规范考量），
如果本地分支名和远程默认分支名不一致，推送时会出现「分支不匹配」报错，因此必须统一为 main。
git branch：分支操作命令；
-M：--move --force 的缩写，「强制重命名分支」；
main：目标分支名（最终要改成的名字）。
git branch -M main 
2. 关联远程仓库
git remote add origin https://github.com/shuimuquan/learngit.git

git remote：远程仓库管理命令；
add：添加远程仓库关联；
origin：给远程仓库起的「别名」（可自定义，比如叫 github，但 origin 是行业通用默认名）；
后面的 URL：GitHub 上你的仓库专属地址（HTTPS 协议）。

建立「本地仓库」和「GitHub 上 learngit 远程仓库」的关联关系，让 Git 知道「要把代码推送到哪个远程地址」。

3. 推送本地代码到远程仓库
git push -u origin main

git push：推送本地代码到远程仓库；
-u：--set-upstream 的缩写，「设置上游关联」；
origin：要推送的远程仓库别名（对应上一句的 origin）；
main：要推送的本地分支名（对应第一步重命名后的 main 分支）

① 把本地 main 分支的所有提交记录（比如你之前的 first commit）推送到 origin 对应的 GitHub 远程仓库；
② -u 建立「本地 main 分支」和「远程 origin/main 分支」的关联，后续推送只需输 git push 即可，不用再写全 git push origin main。

----------
若因为网络问题push失败，电脑配置梯子后，设置：
# 配置代理（替换为你的代理地址，常见：127.0.0.1:7890/1080）
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# 重新推送
git push

# 推送完成后，清除代理配置
git config --global --unset http.proxy
git config --global --unset https.proxy

------------------7 分支管理-----------------------
git checkout -b dev
dev分支上修改/add/commit
git checkout main
git merge dev   (将dev上的修改合并到main上)
git branch -d dev  (已经合并的情况下删除dev，若没合并想删除dev则用-D)


Creating a new branch is quick AND simple.


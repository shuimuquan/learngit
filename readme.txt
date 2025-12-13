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

若上面的合并出现冲突，则解决冲突后在main分支再add和commit，之后可以用git log --graph命令可以看到分支合并图。
Creating a new branch is quick AND simple.


---------------------------bug分支-------------------
在main分支：git checkout -b dev1
在dev1中修改了一半，临时有个bug要修改，怎么办？
在dev1分支先把修改的推进栈：git stash
切换到main分支：git checkout main
拉最新代码：git pull
切换新分支：git checkout -b dev2
在dev2修复bug后，在dev2中add+commit
切到main分支：git checkout main
在main分支合并dev2：git merge dev2
可用 git log 查看刚刚提交的编号，如cc9e08149977eb0279f8165b31ca2e3c01647d11
切换到dev1分支：git checkout dev1
在dev1分支先把main的修改复制过来：git cherry-pick cc9e08149977eb0279f8165b31ca2e3c01647d11
dev1中把之前推进栈的内容提出来：git stash pop
此时会有冲突：处理完冲突就好了
dev1中改完后add+commit
切换会main分支，合并dev1，git merge dev1
此时会有冲突：处理完冲突就好了
main中add+commit+push


-------------dev1 first commit-------------
第二开发者提交
第一开发者提交

---------------------多人协作-------------------
1. 新建开发分支
第一人拉取最新main分支，切出新分支dev1，dev1中修改后，git push,此时远程端也会多出一个dev1分支；
2.在另一个地方clone仓库（模拟第二个人的开发），clone完成后 git branch 查看本地分支只能看到有main分支，因为git默认clone只拉去主分支；
git branch -r 查看远程分支除了main还有dev1，在main分支下用以下指令新建一个本地dev1分支且该分支是从远程的dev1分支下载下来的：
git checkout -b dev1 origin/dev1
此时第二个人就可以在dev1分支上修改并提交push代码；

3.第一个人在dev1分支上修改完代码后add+commit，最后在push时失败，
因为远程端存在第一个人本地没有的提交（第二个人push了新代码）；
4. 此时第一个人需要先拉取最新代码 git pull ，pull后可能有冲突，解决冲突（即合并了两个人代码）后重新add+commit+push即可；



fitst
second

first1
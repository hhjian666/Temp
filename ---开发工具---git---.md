```markdown
Git --- 开源 --- 分布式版本控制系统 --- 敏捷高效地处理任何或小或大的项目
Git 与常用的版本控制工具 SVN, CVS, Subversion 等不同，采用了分布式版本库的方式，不必服务器端软件支持。

Git 与 SVN 区别：不仅仅是个版本控制系统 --- 内容管理系统(CMS) --- 工作管理系统等
	1、核心 --- 分布式 ---  不是分布式
	2、内容存储方式 --- 元数据 --- 文件 —————— 所有资源控制系统都是把文件的元信息隐藏在一个类似 .svn .cvs 的文件夹里
	3、分支 --- * --- 版本库中的另外一个目录
	4、全局版本号 --- 无 --- 有 ——————  Git 缺少的最大的一个特征
	5、内容完整性 --- 优 --- 不优 —————— SHA-1 哈希算法确保代码内容完整性，确保遇到磁盘故障和网络问题时降低对版本库的破坏

分区：
	工作区：就是你在电脑里能看到的目录
	暂存区：stage 或 index，一般存放在 .git/index 中，也叫作索引
	版本库：工作区有一个隐藏目录 .git，是 Git 的版本库
```

```bash
git init #初始化仓库

git clone git://github.com/schacon/grit.git #克隆
git clone git://github.com/schacon/grit.git mygrit #克隆到指定的目录
git clone git@github.com:fsliurujie/test.git    --SSH协议，速度较快，还可以配置公钥免输入密码
git clone git://github.com/fsliurujie/test.git    --GIT协议
git clone https://github.com/fsliurujie/test.git    --HTTPS协议

git config --list
git config -e #编辑 git 配置文件，针对当前仓库 
git config -e --global #编辑 git 配置文件，针对系统上所有仓库
#设置提交代码时的用户信息，如果去掉 --global 参数只对当前仓库有效
git config --global user.name "runoob" 
git config --global user.email test@runoob.com
```

![img](D:\【Programming Project】\999、【Markdown】learn_Programming_By_Hhjian\---开发工具---git---.assets\1352126739_7909.jpg)

```bash
#从工作区提交到暂存区
git add .
git add [dir]
git add [file1] [file2] ... 

#从暂存区提交到版本库 --- 在 Linux 系统中使用单引号，在 Windows 系统中使用双引号
git commit -m [message]
git commit [file1] [file2] ... -m [message]
git commit -a #设置修改文件后不需要执行 git add 命令，直接来提交

#回退版本，可以指定退回某一次提交的版本，取消已缓存的内容
#默认为 --mixed，谨慎使用 –hard 参数，它会删除回退点之前的所有信息。
git reset [--soft | --mixed | --hard] [HEAD] 
git reset HEAD^ #回退所有内容到上一个版本  
git reset HEAD^ hello.php # 回退 hello.php 文件的版本到上一个版本  
git reset 052e # 回退到指定版本
git reset --soft HEAD~3 #回退上上上一个版本，用于回退到某个版本
git reset --hard HEAD~3 #回退上上上一个版本，撤销工作区未提交的内容，将暂存区与工作区回退，并删除之前的所有信息提交
git reset --hard bae128 #回退到某个版本，回退点之前的所有信息
git reset --hard origin/master #将本地的状态回退到和远程的一样
#HEAD^ HEAD^^ HEAD^^^ --- HEAD~1 HEAD~2 HEAD~3

#把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除
git rm --cached <file>

#操作分支
git branch #查看所有分支
git branch (branchname) #创建分支
git checkout (branchname) #切换分支
git checkout -b (branchname) #创建新分支并立即切换到该分支下
git branch -d (branchname) #删除分支
git merge (branchname) #合并分支，合并完后就可以删除分支
#两个分支同时修改内容后 进行合并，出现合并冲突，我们需要手动去修改它，add, commit，解决合并冲突，提交结果

-------------------------------------------------------------------------------------------------------------------

git status #查看仓库当前的状态，显示有变更的文件
git status -s #使用 -s 参数来获得简短的输出结果 --- AM 状态：这个文件在添加到缓存之后又有改动

git diff [file] #尚未缓存的改动，显示暂存区和工作区的差异
git diff --cached [file] | git diff --staged [file] #查看已缓存的改动，显示暂存区和上一次提交(commit)的差异
git diff [first-branch]...[second-branch] #查看已缓存的与未缓存的所有改动，显示两次提交之间的差异
git diff --stat #显示摘要而非整个 diff

git rm –r * #可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件
git rm <file> #将文件从暂存区和工作区中删除
git rm -f <file> #如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f

git mv [file] [newfile] #移动或重命名一个文件、目录或软连接
git mv -f [file] [newfile] #如果新但文件名已经存在，但还是要重命名它

git log	#查看历史提交记录
git log --oneline #查看历史记录的简洁的版本
git log --graph #查看历史中什么时候出现了分支、合并
git log --reverse --oneline #逆向显示所有日志
git log --author=Linus --oneline -5 #查找指定用户的提交日志
git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges #三周前且在四月十八日之后，隐藏合并提交
git blame <file> #以列表形式查看指定文件的历史修改记录

git tag #查看所有标签，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
git tag -a v1.0 #创建一个带注解的标签，希望永远记住那个特别的提交快照
git tag -a v0.9 85fc7e7 #忘了给某个提交打标签，又将它发布了，我们可以给它追加标签
git tag -a <tagname> -m "runoob.com标签" #指定标签信息命令
git tag -s <tagname> -m "runoob.com标签" #PGP签名标签命令

-------------------------------------------------------------------------------------------------------------------

ssh-keygen -t rsa -C "hhjian666@qq.com" #复制 /.ssh/id_rsa.pub 中的 key， 放到 github 的 ssh keys
ssh -T git@github.com
#ssh 访问 gitHub 出错 Host key verification failed.
#将GitHub添加到信任主机列表，ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts

mkdir runoob-git-test
cd runoob-git-test/ 
echo "# 菜鸟教程 Git 测试" >> README.md  

git init  
git add README.md  
git commit -m "添加 README.md 文件"
git remote add origin git@github.com:tianqixin/runoob-git-test.git
#git remote add origin2 git@gitee.com:imnoob/runoob-test.git
git push -u origin master
#git push origin2 master

git remote -v #查看当前配置有哪些远程仓库
git remote rm origin #删除远程仓库
git remote rename old_name new_name  # 修改仓库名

git fetch origin #从远程仓库下载新分支与数据，该命令执行完后需要执行 git merge 远程分支到你所在的分支
git merge origin/master
git pull origin master #下载远程代码并合并，git fetch 和 git merge FETCH_HEAD 的简写
```

![img](D:\【Programming Project】\999、【Markdown】learn_Programming_By_Hhjian\---开发工具---git---.assets\git-command.jpg)
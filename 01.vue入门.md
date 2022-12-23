# Git介绍

git：分布式版本控制工具

> git工作机制

- 工作区通过git add添加进暂存区，暂存区通过git commit提交到本地库

工作区：存放代码的地方，在该目录下面对代码进行开发

暂存区：在工作区写完代码后，需要 Git 来追踪代码，即对这个文件负责，判断它有没有被修改，所以就要把项目添加到这个临时区域（不会生成历史版本），该区域实际是一个文件，保存了下次将提交的文件列表信息

本地库：将暂存区的内容提交到本地的仓库，对于提交的内容，会生成新的历史版本



# Git常用命令

配置用户信息

```shell
$ git config --global user.email liujian@hailan.com
```

配置成功后可以在.gitconfig文件中查看

```bash
[user]
	name = Vanliujian
	email = liujian@hailan.com
[credential]
	username = Vanliujian
	helper = store
[http]
	sslVerify = false
```



1. 初始化本地库

   git init

2. 初始化完成后通过git status查看状态

   ```bash
   //当前分支
   On branch master
   //目前未提交任何东西
   No commits yet
   //没有需要提交的东西
   nothing to commit (create/copy files and use "git add" to track)
   ```

3. 添加一个新的文件后查看状态

   ```bash
   On branch master
   
   No commits yet
   //未被追踪的文件
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           hello.txt
   //提示通过git add来追踪
   nothing added to commit but untracked files present (use "git add" to track)
   ```

4. 把其提交到暂存区后再查看状态git add

   ```bash
   On branch master
   No commits yet
   Changes to be committed:
     (use "git rm --cached <file>..." to unstage)
           new file:   hello.txt
   ```

   可以通过git rm --cached hello.txt可以将这个文件从暂存区删除.

5. 提交到本地库

   git commit -m "修改信息" hello.txt

   ```bash
   $ git commit -m "first commit" hello.txt
   warning: LF will be replaced by CRLF in hello.txt.
   The file will have its original line endings in your working directory
   [master (root-commit) f8d058b] first commit
    1 file changed, 3 insertions(+)
    create mode 100644 hello.txt
   ```

   ```bash
   提交后再次查看状态
   On branch master
   nothing to commit, working tree clean
   ```

6. 查看版本信息

   git reflog和git log

   ```bash
   //reflog是显示基本信息
   $ git reflog
   f8d058b (HEAD -> master) HEAD@{0}: commit (initial): first commit
   
   //log是显示详细信息
   $ git log
   commit f8d058b5339bc1ea397817317620832104fb6a2d (HEAD -> master)
   Author: Vanliujian <liujian@hailan.com>
   Date:   Wed Jul 6 14:35:43 2022 +0800
   
       first commit
   ```

7. 当对文件进行修改后需要再次进行添加暂存区并提交本地库

8. 版本穿梭

   从当前版本可以回退到以前的版本,也可以前进到新的版本(如果有的话)

   ```bash
   //这里的f8d058b是版本号的前8位,通过git reflog查看
   $ git reset --hard f8d058b
   HEAD is now at f8d058b first commit
   ```

   

# 分支操作

1. 什么是分支

   简单理解为副本，但是底层也是指针的引用。

2. 分支操作

   | 命令名称            | 作用                         |
   | ------------------- | ---------------------------- |
   | git branch 分支名   | 创建分支                     |
   | git branch -v       | 查看分支                     |
   | git checkout 分支名 | 切换分支                     |
   | git merge 分支名    | 把指定的分支合并到当前分支上 |

```bash
$ git branch -v
  hot-fix f8d058b first commit
* master  f8d058b first commit

$ git checkout hot-fix
Switched to branch 'hot-fix'
```



# 产生冲突

当在一个分支下修改某个文件并提交后，在另外一个分支下也修改后提交。

进行合并的时候会有提示信息，让我们手动合并。

```bash
星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (hot-fix)
$ vim hello.txt

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (hot-fix)
$ git add hello.txt

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (hot-fix)
$ git commit -m "hot-fix first commit" hello.txt
[hot-fix 27023a1] hot-fix first commit
 1 file changed, 1 insertion(+)

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (hot-fix)
$ git checkout master
Switched to branch 'master'

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master)
$ vim hello.txt

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master)
$ git add hello.txt

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master)
$ git commit -m "master modify" hello.txt
[master a5d2194] master modify
 1 file changed, 1 insertion(+)

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master)
$ git merge hot-fix
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.

//查看一下本地状态，发现还未合并成功
星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master|MERGING)
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

```bash
通过查看hello.txt来手动合并
hello world,hello,h
world
woasdhffksdgf
<<<<<<< HEAD	//当前分支
master modify
=======			//要合并的代码
hot-fix modify
>>>>>>> hot-fix
```

修改后再进行添加暂存区并提交本地库，这时候提交就不要带有文件名了。

```shell
星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master|MERGING)
$ git add hello.txt

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master|MERGING)
$ git commit -m "merge"
[master e84088a] merge

星空之尘@DESKTOP-O4PHEC7 MINGW64 /d/gitbash/test/git_demo2 (master)
此时的状态已经变更为master，表示合并成功
```

合并成功后，再返回hot-fix分支，hot-fix分支内容并不会修改



克隆master分支
git clone uri

克隆非master分支
git clone -b uri branchName

将工作空间、暂存区和本地仓库一起会退到某个版本
git reset --hard commitId

将暂存区和本地仓库会退到某个版本，工作空间不变
git reset -- commitId

将本地仓库会退到某个版本，暂存区和工作空间不变
git rest --soft commitId
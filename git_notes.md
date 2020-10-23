**git基础**
[toc]

### 1. 基本工作方式

工作区 -> `git add` -> 暂存区 -> `git commit` -> 本地库 -> `git push` -> 远程库 -> `git clone` -> 本地库


### 2. 命令行操作

#### 2.1 本地库初始化

命令：
```linux
git init
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git init
Initialized empty Git repository in /home/jincheng/GitTest/.git/
```

#### 2.2 配置签名

**1. 仅在本仓库范围内有效：**

命令：
```linux
git config [选项] [值]
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git config user.name "jindaxia"
jcbetter@lenovo:~/GitTest$ git config user.email "jc_better@163.com"
jcbetter@lenovo:~/GitTest$ git config --list
user.name=jindaxia
user.email=jc_better@163.com
```

**2. 对当前系统所有用户有效：**

命令：
```linux
git config --global [选项] [值]
```

#### 2.3 状态查看

查看工作区、暂存区状态。

命令：
```linux
git status
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

#### 2.4 添加

将工作区“新建或修改”的文件添加到暂存区。

命令：
```linux
git add [文件名]
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ vim README.md
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md
```

#### 2.5 提交

将暂存区的内容提交到本地库。

命令：
```linux
git commit [文件名] -m "[日志信息]"
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git commit README.md -m "first commit"
[master (root-commit) 73c62ca] first commit
 1 file changed, 3 insertions(+)
 create mode 100644 README.md
```

#### 2.6 查看历史记录

查看历史提交记录。（已作多次提交）

命令 1：
```linux
git log [选项]
```
- `--pretty=oneline`
- `--oneline`

效果：
```linux
# 生成多次提交记录

jcbetter@lenovo:~/GitTest$ vim README.md
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git commit README.md -m "add aaa"
[master 9bbdc8f] add aaa
 1 file changed, 2 insertions(+)
jcbetter@lenovo:~/GitTest$ vim README.md
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git commit README.md -m "add bbb"
[master 2481e32] add bbb
 1 file changed, 2 insertions(+)
jcbetter@lenovo:~/GitTest$ vim README.md
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git commit README.md -m "add ccc"
[master b5ddab6] add ccc
 1 file changed, 2 insertions(+)

# 查看历史提交记录

jcbetter@lenovo:~/GitTest$ git log
commit b5ddab62245a34e307e37aada8b8f4b305820920 (HEAD -> master)
Author: jindaxia <jc_better@163.com>
Date:   Thu Oct 22 23:23:41 2020 +0800

    add ccc

commit 2481e32183845eda7929f7214498a68d87ccdb7f
Author: jindaxia <jc_better@163.com>
Date:   Thu Oct 22 23:23:17 2020 +0800

    add bbb

commit 9bbdc8f6a80720deb8feccf08b619280b4797d0d
Author: jindaxia <jc_better@163.com>
Date:   Thu Oct 22 23:22:50 2020 +0800

    add aaa

commit 73c62ca8fd466bcd8b1f480d36b9319a88a4e8d7
Author: jindaxia <jc_better@163.com>
Date:   Thu Oct 22 23:16:16 2020 +0800

    first commit
jcbetter@lenovo:~/GitTest$ git log --pretty=oneline
b5ddab62245a34e307e37aada8b8f4b305820920 (HEAD -> master) add ccc
2481e32183845eda7929f7214498a68d87ccdb7f add bbb
9bbdc8f6a80720deb8feccf08b619280b4797d0d add aaa
73c62ca8fd466bcd8b1f480d36b9319a88a4e8d7 first commit
jcbetter@lenovo:~/GitTest$ git log --onelinee
b5ddab6 (HEAD -> master) add ccc
2481e32 add bbb
9bbdc8f add aaa
73c62ca first commit
```

命令 2：
```linux
git reflog
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git reflog
b5ddab6 (HEAD -> master) HEAD@{0}: commit: add ccc
2481e32 HEAD@{1}: commit: add bbb
9bbdc8f HEAD@{2}: commit: add aaa
73c62ca HEAD@{3}: commit (initial): first commit
```

#### 2.7 前进与后退

本质：修改 HEAD 指针的指向。

**1. 基于索引值**

命令：
```linux
git reset --hard [索引值]
```
效果：
```linux
b5ddab6 (HEAD -> master) HEAD@{0}: commit: add ccc
2481e32 HEAD@{1}: commit: add bbb
9bbdc8f HEAD@{2}: commit: add aaa
73c62ca HEAD@{3}: commit (initial): first commit
jcbetter@lenovo:~/GitTest$ git reset --hard 2481e32
HEAD is now at 2481e32 add bbb
```

**2. 使用 `^` 符号**

这种方式只能后退，带有几个 `^` 就后退几步。

命令：
```linux
git reset --hard HEAD^
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git reflog
2481e32 (HEAD -> master) HEAD@{0}: reset: moving to 2481e32
b5ddab6 HEAD@{1}: commit: add ccc
2481e32 (HEAD -> master) HEAD@{2}: commit: add bbb
9bbdc8f HEAD@{3}: commit: add aaa
73c62ca HEAD@{4}: commit (initial): first commit
jcbetter@lenovo:~/GitTest$ git reset --hard HEAD^
HEAD is now at 9bbdc8f add aaa
```

**3. 使用 `~` 符号**

这种方式只能后退。

命令：
```linux
git reset --hard HEAD~[后退步数]
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git reflog
9bbdc8f (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
2481e32 HEAD@{1}: reset: moving to 2481e32
b5ddab6 HEAD@{2}: commit: add ccc
2481e32 HEAD@{3}: commit: add bbb
9bbdc8f (HEAD -> master) HEAD@{4}: commit: add aaa
73c62ca HEAD@{5}: commit (initial): first commit
jcbetter@lenovo:~/GitTest$ git reset --hard HEAD~1
HEAD is now at 73c62ca first commit
jcbetter@lenovo:~/GitTest$
```

**reset 命令参数对比：**

- `--soft`：
  - 仅在本地库移动 HEAD 指针。
- `--mixed`：
  - 在本地库移动 HEAD 指针；
  - 重置暂存区。
- `--hard`：
  - 在本地库移动 HEAD 指针；
  - 重置暂存区；
  - 重置工作区。

#### 2.8 比较文件差异

**1. 将工作区中的文件与暂存区进行比较**

命令：
```linux
git diff [文件名]
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ vim README.md    # add "test diff"
jcbetter@lenovo:~/GitTest$ git diff README.md
diff --git a/README.md b/README.md
index e214021..7f9135f 100644
--- a/README.md
+++ b/README.md
@@ -7,3 +7,5 @@ aaa
 bbb

 ccc
+
+test diff
```

**2. 将工作去的文件与本地库历史记录比较**

命令：
```linux
git diff [本地库历史版本] [文件名]
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git reflog
b5ddab6 (HEAD -> master) HEAD@{0}: reset: moving to b5ddab6
2481e32 HEAD@{1}: reset: moving to HEAD~1
b5ddab6 (HEAD -> master) HEAD@{2}: reset: moving to b5ddab6
73c62ca HEAD@{3}: reset: moving to HEAD~1
9bbdc8f HEAD@{4}: reset: moving to HEAD^
2481e32 HEAD@{5}: reset: moving to 2481e32
b5ddab6 (HEAD -> master) HEAD@{6}: commit: add ccc
2481e32 HEAD@{7}: commit: add bbb
9bbdc8f HEAD@{8}: commit: add aaa
73c62ca HEAD@{9}: commit (initial): first commit
jcbetter@lenovo:~/GitTest$ git diff 2481e32 README.md
diff --git a/README.md b/README.md
index f6be60a..e214021 100644
--- a/README.md
+++ b/README.md
@@ -5,3 +5,5 @@ This project is for learning use.
 aaa

 bbb
+
+ccc
```


### 3. 分支管理

概念：使用分支意味着我们可以从开发主线上分离出来，在不影响主线的同时继续推进工作。等到自己负责的分支开发完成后，将分支与其他分支或主分支进行合并即可。

作用：可以同时并行推进多个功能的开发，提高开发效率，并且各个分支之间没有任何影响。

#### 3.1 查看分支

命令：
```linux
git branch -v
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git branch -v
* master b5ddab6 add ccc
```

#### 3.2 创建分支

命令：
```linux
git branch [分支名]
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git branch future
jcbetter@lenovo:~/GitTest$ git branch -v
  future b5ddab6 add ccc
* master b5ddab6 add ccc
```

#### 3.3 切换分支

命令：
```linux
git checkout [分支名]
```
效果：
```linux
jcbetter@lenovo:~/GitTest$ git checkout future
Switched to branch 'future'
```

#### 3.4 合并分支

方法：
- 第一步：切换到接受修改（被合并）的分支；
  ```linux
  git checkout [被合并分支名]
  ```
- 第二步：执行 `merge` 操作。
  ```linux
  git merge [合并分支名]
  ```

效果：
```linux
# 先在 future 分支上做一个修改，再切换到 master 分支将 future 分支所作的修改合并。

jcbetter@lenovo:~/GitTest$ vim README.md    # add ddd
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git commit README.md -m "test merge"
[future c3bae3e] test merge
 1 file changed, 2 insertions(+)
jcbetter@lenovo:~/GitTest$ git checkout master
Switched to branch 'master'
jcbetter@lenovo:~/GitTest$ git merge future
Updating b5ddab6..c3bae3e
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

#### 3.5 冲突解决

冲突：若被合并的分支与要合并的分支均对同一文件提交过修改，那么在进行合并时，git 无法决定到底保留哪一个修改，需要人为决定冲突文件的内容。

方法：
- 第一步：编辑产生冲突的文件，删除特殊的标记符号，并将文件修改至满意成都；
  ```linux
  vim [文件名]
  ```
- 第二步：将文件添加到暂存区；
  ```linux
  git add [文件名]
  ```
- 第三步：提交至本地库（不可带具体文件名）。
  ```linux
  git commit -m "[日志信息]"
  ```

效果：
```linux
# 让 master 分支与 future 分支均对 README.md 进行修改

jcbetter@lenovo:~/GitTest$ vim README.md
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git commit README.md -m "test conflict by master"
[master 886379e] test conflict by master
 1 file changed, 2 insertions(+)
jcbetter@lenovo:~/GitTest$ git checkout future
Switched to branch 'future'
jcbetter@lenovo:~/GitTest$ vim README.md
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git commit README.md -m "test conflict by future"
[future cbe52a0] test conflict by future
 1 file changed, 2 insertions(+)

# 切换到 master 分支进行 merge 操作

jcbetter@lenovo:~/GitTest$ git checkout master
Switched to branch 'master'
jcbetter@lenovo:~/GitTest$ git merge future
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
jcbetter@lenovo:~/GitTest$ vim README.md    # 人为决定 README.md 的内容
jcbetter@lenovo:~/GitTest$ git add README.md
jcbetter@lenovo:~/GitTest$ git commit -m "conflict solved"
[master 156da89] conflict solved
```


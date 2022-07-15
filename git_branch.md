# 分支介绍

假设现在有一个工作目录，里面包含了三个将要被暂存和提交的文件。 暂存操作会为每一个文件计算校验和，然后会把当前版本的文件快照保存到 Git 仓库中（Git 使用 blob 对象来保存它们），最终将校验和加入到暂存区域等待提交：

```console
$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
```

当使用 `git commit` 进行提交操作时，Git 会先计算每一个子目录（本例中只有项目根目录）的校验和，然后在 Git 仓库中这些校验和保存为树对象。 随后，Git 便会创建一个提交对象，它除了包含上面提到的那些信息外，还包含指向这个树对象（项目根目录）的指针。如此一来，Git 就可以在需要的时候重现此次保存的快照。

现在，Git 仓库中有五个对象：三个 blob 对象（保存着文件快照）、一个树对象（记录着目录结构和 blob 对象索引）以及一个提交对象（包含着指向前述树对象的指针和所有提交信息）。

![image-20220713173205855](/Users/I525926/Library/Application Support/typora-user-images/image-20220713173205855.png)

做些修改后再次提交，那么这次产生的提交对象会包含一个指向上次提交对象（父对象）的指针。

![image-20220713173429359](/Users/I525926/Library/Application Support/typora-user-images/image-20220713173429359.png)

Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 `master`。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 `master` 分支。 它会在每次的提交操作中自动向前移动。

![image-20220713173520133](/Users/I525926/Library/Application Support/typora-user-images/image-20220713173520133.png)

1. 创建分支：`git branch test`,本质上就是创建了一个可以移动的新的指针

   ![image-20220713173944912](/Users/I525926/Library/Application Support/typora-user-images/image-20220713173944912.png)

2.切换分支并提交

![image-20220713174531430](/Users/I525926/Library/Application Support/typora-user-images/image-20220713174531430.png)

 1. 查看分支指向对象：gi t log --oneline --decorate

 2. 切换分支：`git checkout testing`

 3. 再次提交后，分支情况：

    ![image-20220714093407255](/Users/I525926/Library/Application Support/typora-user-images/image-20220714093407255.png)

4. 切回master分支后，再次提交，分支情况：

   ![image-20220714093505318](/Users/I525926/Library/Application Support/typora-user-images/image-20220714093505318.png)

   # 分支的新建与合并

   1. 新建分支

      ```
      git checkout -b iss53
      # 等价于
      git branch iss53
      git checkout iss53
      ```

<<<<<<< HEAD
   2. 切换分支：`git checkout iss53`,此前必须留意你的工作目录和暂存区里那些还没有被提交的修改，它可能会和你即将检出的分支产生冲突从而阻止 Git 切换到该分支。 最好的方法是，在你切换分支之前，保持好一个干净的状态。 有一些方法可以绕过这个问题（即，保存进度（stashing） 和 修补提交（commit amending）），使用`git stash`

   3. 合并分支：
      1. 无冲突：`git merge iss53`,将iss53分支合并到当前分支，通过比较两个分支的最末端与公共父节点来比较合并
      2. 有冲突：git会做合并，但是不会创建一个新的合并提交，而是等解决合并的冲突后再提交，可以通过get status查看状况。

   4. 删除分支：`git branch -d iss53`

   5. 切换分支：`git checkout iss53`

   6. 合并分支：
      1. 无冲突：`git merge iss53`,将iss53分支合并到当前分支，通过比较两个分支的最末端与公共父节点来比较合并
      2. 有冲突：git会做合并，但是不会创建一个新的合并提交，而是等解决合并的冲突后再提交，可以通过get status查看状况。

   7. 将分支上传github：

         1. `git push --set-upstream origin test`

            
# 分支管理

1. 查看分支：-v 表示最后一次提交

   ```
   I525926@C02CF1W9MD6P git- % git branch -v
     main 628f76a rename file_name
   * test 769a991 commit
   ```

# 分支开发工作流

1. 长期分支：

   ![image-20220714175105559](/Users/I525926/Library/Application Support/typora-user-images/image-20220714175105559.png)

   ![image-20220714175134713](/Users/I525926/Library/Application Support/typora-user-images/image-20220714175134713.png)

2. 特性分支：是一种短期分支，它被用来实现单一特性或其相关工作。

   ![image-20220714175301069](/Users/I525926/Library/Application Support/typora-user-images/image-20220714175301069.png)

   ![image-20220714175419448](/Users/I525926/Library/Application Support/typora-user-images/image-20220714175419448.png)

# 远程分支

1. 显式获得远程引用的完整列表：`git ls-remote`
2. 获得更多远程分支信息：`git remote show`
3. 新建分支并上传服务器：	`git push --set-upstream origin test1`
4. 添加新的远程仓库到当前分支：`git remote add [name] [url]`
5. 同步远程：
   1. `git fetch origin`：
      1. 抓取本地没有的数据，并更新本地数据库，移动本地的origin/master指针指向新的、更新后位置；
      2. 当抓取到新的远程跟踪分支时，本地不会自动生成一份可编辑的副本（拷贝）。
      3. 这种情况下，不会有一个新的 `serverfix` 分支 - 只有一个不可以修改的 `origin/serverfix` 指针。
      4. 可以运行 `git merge origin/serverfix` 将这些工作合并到当前所在的分支。
      5. 如果想要在自己的 `serverfix` 分支上工作，可以将其建立在远程跟踪分支之上。
6. 推送：
   1. 公开分享分支：		`git push (remote name) (branch name)`,用本地的branch name分支更新远程的branch name分支
   2. 避免每次输入密码：使用token 获得 使用SSH

7. 拉取：
   1. 当 `git fetch` 命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。 它只会获取数据然后让你自己合并。 
   2. `git pull` 在大多数情况下它的含义是一个 `git fetch` 紧接着一个 `git merge` 命令。`git pull` 都会查找当前分支所跟踪的服务器与分支，从服务器上抓取数据然后尝试合并入那个远程分支。
8. 删除远程分支：
   1. `git push origin --delete [remote_branch_name]`
      1. 从服务器上移除这个指针。 Git 服务器通常会保留数据一段时间直到垃圾回收运行，所以如果不小心删除掉了，通常是很容易恢复的。

# 变基

在 Git 中整合来自不同分支的修改主要有两种方法：`merge` 以及 `rebase`。

## 变基的基本操作

1. 分叉的提交历史：

   ![image-20220715105434093](/Users/I525926/Library/Application Support/typora-user-images/image-20220715105434093.png)

2.merge整合分叉的历史：

![image-20220715105517886](/Users/I525926/Library/Application Support/typora-user-images/image-20220715105517886.png)

3. 变基：提取在 `C4` 中引入的补丁和修改，然后在 `C3` 的基础上应用一次。变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。

   1. 可以使用 `rebase` 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

      ```console
      $ git checkout experiment
      $ git rebase master
      First, rewinding head to replay your work on top of it...
      Applying: added staged command
      ```

   2. 原理：首先找到这两个分支（即当前分支 `experiment`、变基操作的目标基底分支 `master`）的最近共同祖先 `C2`，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底 `C3`, 最后以此将之前另存为临时文件的修改依序应用。

      ![image-20220715105757672](/Users/I525926/Library/Application Support/typora-user-images/image-20220715105757672.png)

      现在回到 `master` 分支，进行一次快进合并。

      ```console
      $ git checkout master
      $ git merge experiment
      ```

![image-20220715110020992](/Users/I525926/Library/Application Support/typora-user-images/image-20220715110020992.png)

​			此时，`C4'` 指向的快照就和上面使用 `merge` 命令的例子中 `C5` 指向的快照一模一样了。 这两种整合方法的最终结果没有任何区			别，但是变基使得提交历史更加整洁。



4. `git rebase --onto master server client`:将server分支和client分支去掉公共部分后的client分支合并到master分支上。

![image-20220715110734967](/Users/I525926/Library/Application Support/typora-user-images/image-20220715110734967.png)

5. 变基的风险:

   **不要对在你的仓库外有副本的分支执行变基。**

   变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。 如果你已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果你用 `git rebase` 命令重新整理了提交并再次推送，你的同伴因此将不得不再次将他们手头的工作与你的提交进行整合，如果接下来你还要拉取并整合他们修改过的提交，事情就会变得一团糟。

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

   2. 切换分支：`git checkout iss53`,此前必须留意你的工作目录和暂存区里那些还没有被提交的修改，它可能会和你即将检出的分支产生冲突从而阻止 Git 切换到该分支。 最好的方法是，在你切换分支之前，保持好一个干净的状态。 有一些方法可以绕过这个问题（即，保存进度（stashing） 和 修补提交（commit amending）），使用`git stash`
   3. 合并分支：
      1. 无冲突：`git merge iss53`,将iss53分支合并到当前分支，通过比较两个分支的最末端与公共父节点来比较合并
      2. 有冲突：git会做合并，但是不会创建一个新的合并提交，而是等解决合并的冲突后再提交，可以通过get status查看状况。
   4. 删除分支：`git branch -d iss53`
# 撤销操作

1. 撤销commit，对暂存区文件修改后重新提交：

   ````
   git commit -m 'initial commit'
   git add forgotten_file
   git commit -amend
   ````

2. 将暂存区文件取出到已修改状态：

   ```console
   git reset HEAD <file>
   for example:
   git reset HEAD CONTRIBUTION.md
   ```

3. 撤销修改，还原到上次提交的样子,通过拷贝另一个文件覆盖它：

   ```
   git checkout --file_name
   ```



# 远程仓库的使用

1. 查看远程仓库,列出指定的每一个远程服务器的简写,默认origin：

   ```console
   git clone [url]
   cd *
   git remote
   # 指定选项 -v,显示需要读写的远程服务器的简称及对应url
   gi t remote -v
   ```

2. 添加远程仓库，同时可以指定简写：

   ```console
   $ git remote
   $ git remote add pb https://github.com/paulboone/ticgit
   $ git remote -v
   $ #可以用简称代替url
    git fetch pb
   ```

3. 从远程仓库中抓取和拉取：

   1. `git fetch [remote-name]`:
      1. 该命令会访问远程仓库，从中拉取所有你还没有的数据。 
      2. 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
      3. 将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。
   2. `get pull`:
      1. 自动抓取然后合并远程分支到当前分支

4. 推送到远程仓库：

   1. `git push [remote-name] [branch-name]`:
      1. 有写入权限，且与别人同时克隆后没有别人推送，才能生效。必须把别人的工作先拉下来才能推送。

5. 查看远程仓库：

   1. `git remote show [remote-name]:`

      1. 列出远程仓库的 URL 与跟踪分支的信息。 
      2. 列出了当你在特定的分支上执行 `git push` 会自动地推送到哪一个远程分支。 
      3. 列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了
      4. 当你执行 `git pull` 时哪些分支会自动合并。

      ```console
      I525926@C02CF1W9MD6P git- % git remote show origin
      * remote origin
        Fetch URL: https://github.com/jiangweiguang/git-.git
        Push  URL: https://github.com/jiangweiguang/git-.git
        HEAD branch: main
        Remote branch:
          main tracked
        Local branch configured for 'git pull':
          main merges with remote main
        Local ref configured for 'git push':
          main pushes to main (fast-forwardable)
      ```

6. 远程仓库的移除与重命名：

   1. 修改远程仓库的简写名：
      1. `git remote rename old_name new_name`
      2. 这也会修改你的远程分支名字
   2. 移除远程仓库：
      1. `git remote rm name`

# 打标签

用于给历史中某个提交打标签，以示重要。例如标记发布版本。

1. 列出标签：

   1. `git tag`:以字母顺序列出标签；但是它们出现的顺序并不重要。
   2. 使用特定的模式查找标签:`git tag -l 'v1.8.5*'`

2. 创建标签：

   1. 附注标签：存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证。
      1. `git tag -a tag_name -m 'tag_message'`:
         1. `-m` 选项指定了一条将会存储在标签中的信息
      2. 查看标签信息：
         1. `git show tag_name`
   2. 轻量标签：
      1. `git tag tag_name`:将提交校验和存储到一个文件中 - 没有保存任何其他信息

3. 后期打标签：在命令的末尾指定提交的校验和（或部分校验和）

   ```console
   git log --pretty=oneline
   166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
   9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
   git tag -a v1.2 9fceb02d
   ```

4. 共享标签：标签只有共享才会被传送到远程仓库服务器
   1. `git push origin tag_name`
   2. 一次性推送多个标签：`git push origin --tags`
5. 检出标签：
   1. 
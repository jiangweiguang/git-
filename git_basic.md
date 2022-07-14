# git配置：

1. /etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 git config 时带上 --system 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）
2. ~/.gitconfig 或 ~/.config/git/config 文件：只针对当前用户。 你可以传递 --global选项让 Git 读写此文件，这会对你系统上 ***\*所有\**** 的仓库生效。
3. 当前使用仓库的 Git 目录中的 config 文件（即 .git/config）：针对该仓库。 你可以传递 --local 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）
4. 每一个级别会覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。
5. 查看配置信息：git config —list
6. 获取帮助:git help <verb> | git <verb> help

# 获取git仓库

1. 现有目录中初始化仓库：git init,创建.git子目录，但是现有目录中的文件尚未被跟踪；
2. 实现对指定文件的跟踪：git add ***

# 克隆现有仓库

1. 三种：
2. 1. git clone [url]:可使用https:// | git:// | SSH传输协议

# git文件生命周期

![image-20220712165629986](/Users/I525926/Documents/:Users:I525926:Library:Application Support:typora-user-images:image-20220712165629986.png)

1. 检查当前文件状态：git status ｜ git status -s
2. Git add:用于将文件状态转换到下一步

1. 忽略文件： 创建.gitignore
2. 查看已暂存和未暂存的修改:
3. 1. Git diff:比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。
   2. Git diff —cached or git diff —staged:查看已暂存的将要添加到下次提交里的内容
   3. 分析文件差异的插件：git difftool —tool-help | git difftool emerge and so on
4. 提交更新：
5. 1. Git commit
   2. 1. 启动编辑器输入提交说明，通过git config —global core.editor 设定喜欢的编辑器
      2. Git commit 只提交暂存的修改
      3. Git commit -m 查看提交信息
6. 跳过使用暂存区域：
7. 1. Git commit -a:将所有已经跟踪的文件暂存并一并提交，跳过git add 步骤
8. 移除文件：
9. 1. 必须从已跟踪文件清单中移除（暂存区）： git rm
   2. 删除前修改并已经放入暂存区： git rm -f
   3. 从暂存区删除，但保留到工作目录： git rm —cached
   4. 支持模式匹配
10. 重命名文件：
11. 1. 文件改名： git mv file_from file_to == mv file_from file_to git rm file_from git add file_to
12. **查看历史提交:**
13. 1. **git log :**
    2. 1. -p:用于显示每次提交的内容差异
       2. -2:最近两次提交
       3. — stat：简略统计信息：所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。
       4. —pretty:指定使用不同于默认格式的方式展示提交历史
       5. 1. Git log —pretty=format:”%h - %an, %ar :%s"

1. 按提交时间列出所有的更新，最近的更新排在最上面。
2. 这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

14. 限制输出长度：

    | 选项                  | 说明                               |
    | :-------------------- | :--------------------------------- |
    | `-(n)`                | 仅显示最近的 n 条提交              |
    | `--since`, `--after`  | 仅显示指定时间之后的提交。         |
    | `--until`, `--before` | 仅显示指定时间之前的提交。         |
    | `--author`            | 仅显示指定作者相关的提交。         |
    | `--committer`         | 仅显示指定提交者相关的提交。       |
    | `--grep`              | 仅显示含指定关键字的提交           |
    | `-S`                  | 仅显示添加或移除了某个关键字的提交 |

    



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
   1. 用于将工作目录的项目版本切换到指定标签版本
   1. `git checkout -b branchname tagname`
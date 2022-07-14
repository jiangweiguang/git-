# 1. git配置：

1. /etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 git config 时带上 --system 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）
2. ~/.gitconfig 或 ~/.config/git/config 文件：只针对当前用户。 你可以传递 --global选项让 Git 读写此文件，这会对你系统上 ***\*所有\**** 的仓库生效。
3. 当前使用仓库的 Git 目录中的 config 文件（即 .git/config）：针对该仓库。 你可以传递 --local 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）
4. 每一个级别会覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。
5. 查看配置信息：git config —list
6. 获取帮助:git help <verb> | git <verb> help

# 2. 获取git仓库

1. 现有目录中初始化仓库：git init,创建.git子目录，但是现有目录中的文件尚未被跟踪；
2. 实现对指定文件的跟踪：git add ***

# 3. 克隆现有仓库

1. 三种：
2. 1. git clone [url]:可使用https:// | git:// | SSH传输协议

# 4. git文件生命周期

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

    
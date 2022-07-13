# 撤销操作

1. 撤销commit，对暂存区文件修改后重新提交：

   ````
   git commit -m 'initial commit'
   git add forgotten_file
   git commit -amend
   ````

2. 将暂存区文件取出到已修改状态：

   ```
   git reset HEAD <file>
   for example:
   git reset HEAD CONTRIBUTION.md
   ```

   
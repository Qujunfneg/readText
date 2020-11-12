#### git撤销回退

1. `git log` 查看提交日志

   - `git log --pretty=oneline` 一行显示

     执行回退命令

     `git reset --hard HEAD^`   回退到上个版本

     `git reset --hard [HEAD]` 填写log的指定版本

     - 修改了文件未放入暂缓区

       `git checkout -- [file]`可以丢弃工作区的修改

     - git checkout 如果没有-- ,就会变成创建分支的命令

   ####  创建与合并分支

   1. 创建并切换分支 `git chekcout -b [name]` ，相当于`git branch dev  ; git chekout dev`
   2. 切换分支  `git checkout [name]`
   3. 删除分支 `git branch -d [name]`

   ####  git stash的用法

   - 在新分支`dev`干活遇到紧急问题需要处理，但不想提交代码
   - `git stash`
   - 切换主分支，`git chekout master` ;创建解决问题的分支`git checkout -b issue-404`
   - 修改完提交合并
   - 切换至干活分支`git chekout dev`
   - 查看`git stash list`
   - 两种方法恢复至以前的文件
     - `git stash apply`恢复，恢复后，stash内容并不删除，你需要使用命令`git stash drop`来删除
     - 另一种方式是使用`git stash pop`,恢复的同时把stash内容也删除了

   

   ![git流程](../assets/git.jpg)

   

   

   

   

   
   
   
   
   
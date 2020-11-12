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

   

   

   

   

   

   
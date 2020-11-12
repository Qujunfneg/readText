#### git撤销回退

1. `git log` 查看提交日志

   `git log --pretty=oneline` 一行显示

   执行回退命令

   `git reset --hard HEAD^`   回退到上个版本

   `git reset --hard [HEAD]` 填写log的指定版本
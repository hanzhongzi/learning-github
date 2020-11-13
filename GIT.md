# GIT学习
## 基础操作
```shell
git commit #直接提交本分支
git branch <分支名称> #生成新的分支，但是指针在原分支 
git checkout <目标分支名称> #指针跳转到目标分支
git merge <目标分支名称> #将目标分支名称的分支，合并到当前分支 指针不动
git rebase <目标分支名称> #将当前分支移动目标分支 带着项目
```

## 进阶操作
```shell
    HEAD 是一个指向当前的指针，你可以通过git checkout 《节点哈希值》 将HEAD指针移动到该节点处
git log 
    查看提交记录的hash值很40位置 可以输入前几个字符完成锁定。
    使用 ^ 向上一个提交记录移动，例如 master^^ 是指向master的父节点的父节点， 使用 ~num 可以向上多个提交记录移动，或者 HEAD^ 移动。（这里面有相对引用的概念）
    强制移动分支   git branch -f master <HEAD~3 这就是个位置> 
git reset HEAD~1 #git reset HEAD~1 撤回这个分支并且将指针上移一个单位，撤回还在但是在缓存区
git revert HEAD #撤销这个分支，并重新提交一次，重新提交的就是和上上次一样的。（远程分支）
```

加油！
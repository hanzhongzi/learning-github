# GIT学习
## 基础操作
```shell
git commit #直接提交本分支
    --amend 进行修改
git branch <分支名称> #生成新的分支，但是指针在原分支 
git checkout <目标分支名称> #指针跳转到目标分支
git merge <目标分支名称> #将目标分支名称的分支，合并到当前分支 指针不动，（或移动至最新）
git rebase <目标分支名称> #将当前分支移动目标分支 带着项目
```

## 进阶操作
```shell
    HEAD 是一个指向当前的指针，你可以通过git checkout 《节点哈希值》 将HEAD指针移动到该节点处
git log 
    查看提交记录的hash值很40位置 可以输入前几个字符完成锁定。
    使用 ^ 向上一个提交记录移动，例如 master^^ 是指向master的父节点的父节点， 使用 ~num 可以向上多个提交记录移动，或者 HEAD^ 移动。（这里面有相对引用的概念） ^2是另一个父节点
    强制移动分支   git branch -f master <HEAD~3 这就是个位置> 
git reset HEAD~1 #git reset HEAD~1 撤回这个分支并且将指针上移一个单位，撤回还在但是在缓存区
git revert HEAD #撤销这个分支，并重新提交一次，重新提交的就是和上上次一样的。（远程分支）
```

## 自由修改提交数
```shell
整理提交笔记
git cherry-pick <提交号>... #将提交号（指针\hash值） 提交到当前指针下方
     cherry-pick 可以将提交树上任何地方的提交记录取过来追加到 HEAD 上
rebase 交互式
    -i --interactive 
    1.调整提交记录的顺序（通过鼠标拖放来完成）
    2.删除你不想要的提交（通过切换 pick 的状态来完成，关闭就意味着你不想要这个提交记录）
    3.合并提交。
    git rebase -i HEAD~2

```

## 提升技巧 （将之前的记录进行修改）
```shell
法一
    git rebase -i HEAD~2 #修改顺序
    git commit --amend #新增提交
    git rebase -i HEAD~2 #将顺序改回来
法二
    git cherry-pick C2
    git commit --amend
    git cherry-pick C3

```

## 设置 永远指向某个提交记录的标识
```shell
    git tag <版本> <HEAD> 
    git describe <ref> #<ref> 可以是任何能被 Git 识别成提交记录的引用，如果你没有指定的话，Git 会以你目前所检出的位置（HEAD）
    它输出的结果是这样的：
    <tag>_<numCommits>_g<hash>
    tag 表示的是离 ref 最近的标签， numCommits 是表示这个 ref 与 tag 相差有多少个提交记录， hash 表示的是你所给定的 ref 所表示的提交记录哈希值的前几位。当 ref 提交记录上有某个标签时，则只输出标签名称

```

## 操控远程仓库
```shell
    git init # 将目录初始化为git
    git clone 地址 #将项目克隆到本机目录
    git pull 项目 #拉取远程项目
    git fetch 
        远程仓库下载本地仓库中缺失的提交记录
        更新远程分支指针(如 o/master)

```

加油！
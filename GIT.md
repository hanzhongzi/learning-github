# GIT学习
## 基础操作
```shell
git commit #直接提交本分支
    --amend 进行修改
git branch <分支名称> #生成新的分支，但是指针在原分支 
git checkout <目标分支名称> #指针跳转到目标分支
git merge <目标分支名称> #将目标分支名称的分支，合并到当前分支 指针不动，（或移动至最新）
git rebase <目标分支名称> #将当前分支移动目标分支 带着项目
git rebase <被跟随分支> <要移动的分支>
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
    git pull 项目 #拉取远程项目 相当于git fetch ; git merge o/master;
    git fetch 
        从远程仓库下载本地仓库中缺失的提交记录，仅下载
        更新远程分支指针(如 o/master)
    git push 上传项目，默认再  push.default 中有默认配置
         基与上次下载的更新提交 git fetch;git rebase o/master; git push;==git pull --rebase; git push;
                              git fetch;git merge o/master; git push;==git pull ; git push;
    git rebase <被跟随分支> <要移动的分支>
    git checkout -b totallyNotMaster o/master
        就可以创建一个名为 totallyNotMaster 的分支，它跟踪远程分支 o/master。
        另一种设置远程追踪分支的方法就是使用：git branch -u 命令，执行：

    git branch -u o/master foo
        这样 foo 就会跟踪 o/master 了。如果当前就在 foo 分支上, 还可以省略 foo：
        git branch -u o/master
```

## 命令的说明
```shell
git push origin master  # git push <romte> <place>
    把这个命令翻译过来就是：
    切到本地仓库中的“master”分支，获取所有的提交，再到远程仓库“origin”中找到“master”分支，将远程仓库中没有的提交记录都添加上去，搞定之后告诉我。

要同时为源和目的地指定 <place> 的话，只需要用冒号 : 将二者连起来就可以了：
git push <romte> <source>:<destination>
    这个参数实际的值是个 refspec，“refspec” 是一个自造的词，意思是 Git 能识别的位置（比如分支 foo 或者 HEAD~1）

如果你像如下命令这样为 git fetch 设置 的话：

git fetch origin foo
    Git 会到远程仓库的 foo 分支上，然后获取所有本地不存在的提交，放到本地的 o/foo 上。
    因为是同步，所以和git push 的参数是相反的
    “如果我们指定 <source>:<destination> 会发生什么呢？”
    如果你觉得直接更新本地分支很爽，那你就用冒号分隔的 refspec 吧。不过，你不能在当前检出的分支上干这个事，但是其它 分支是可以的。
    这里有一点是需要注意的 —— source 现在指的是远程仓库中的位置，而 <destination> 才是要放置提交的本地仓库的位  置。它与 git push 刚好相反，这是可以讲的通的，因为我们在往相反的方向传送数据。
```

## 删除远程仓库分支
```shell
git push origin  :foo
```

## 创建新的本地分支
```shell
git fetch origin :bar
```


## git pull 衍生
```shell
git pull origin foo 相当于：
    git fetch origin foo; git merge o/foo
    还有...
    git pull origin bar~1:bugFix 相当于：
    git fetch origin bar~1:bugFix; git merge bugFix
    看到了? git pull 实际上就是 fetch + merge 的缩写, git pull 唯一关注的是提交最终合并到哪里（也就是为 git  fetch 所提供的 destination 参数）


git pull origin master:foo
    它先在本地创建了一个叫 foo 的分支，从远程仓库中的 master 分支中下载提交记录，并合并到 foo，然后再 merge 到我们的当前检出的分支 bar 上。操作够多的吧？！


```
加油！
gitee 推荐练习地址：https://oschina.gitee.io/learn-git-branching/?demo
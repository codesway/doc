#让开发变得不是那么费劲之-GIT
***
>这里不去聊什么GIT是谁写的，哪一年诞生的。太浪费时间，直奔主题。就聊一下GIT的套路

###1.关于git的套路
```
首先GIT有几个很重要的概念：

你的本地仓库由 git 维护的三棵“树”组成。
第一个是你的 工作目录，它持有实际文件；
第二个是 暂存区（Index），它像个缓存区域，临时保存你的改动；
最后是 本地版本库，HEAD，指向你最近一次提交后的结果。
HEAD 当前本地最新提交的指针 就是最新提交的commit的当前hash值
FETCH_HEAD 远程库最新提交的指针

```
![](http://www.bootcss.com/p/git-guide/img/trees.png)

***

```
第二个就是分支(branch)
这与SVN的分支有本质的不同
Git 保存的不是文件的变化或者差异，而是一系列不同时刻的文件快照。
提交的内容会包含一个指向暂存内容快照的指针。（hash值）
不仅分支的内容提交会产生，就连合并，拉取，推送都会有指针记录。
```
![](http://p1.bqimg.com/1949/74e9191f987d180a.jpg)

使用的套路基本就是：
在基础分支上（默认分支是master）检出一个特性分支用来做开发。开发到可以提交的阶段，先把文件从工作区加入到暂存区，在从暂存区提交到本地的分支，最后推送到远端的代码库。就ok了
***

###2.到底git的优点在于哪里

>团队协作，最重要的就是减少冲突和麻烦。更省心的干活。


```
分支隔离开发，提交审核：
更详细的日志和更详尽的比较：
可以无限的回滚任何版本：
```

###3.学习git的成本

```
学习任何工具的使用都会有一定的成本，经过一些总结，会让大家减少很多弯路，更快的上手新工具：
1.git add 添加文件到暂存区
2.git commit 添加文件到本地库(会产生一条log)
3.git push origin branchName(分支推送到远端库)
像git merge、git branch、git remote、git rebase、git fetch、git pull等等都是最基本的操作，涉及参数也非常的多。
这些只是最基本的git操作。只会这些还远远不够。
```


###4.一些能让大家在用git上更狂野的招数
>这些才是真正的干货，也是我在实际开发中遇到的问题，总结出的东西

```
场景一：到底是谁动了我的文件之git blame

总有些时候不知道是谁改动了你提交的文件造成一些失误和问题，git的日志和和操作记录非常的详细，可以让你不用为此背锅
```

```
场景二：临时插入需求，又不能把开发一半的分支提交或废弃掉之 git stash

总会有些小任务插进来，你当前开发的分支又不到提交的时候，未提交代码还不能切换分支，写了一半也不能废弃。 
那我们可以直接隐藏工作区，当前的分支暂停开发，切换新的特性分支来处理新增的需求
```

```
场景三：我到底改动了哪些文件之git diff & git show

我们改动了文件以后，为了安全会重新审视一下自己改动的代码，
未提交前选择git diff
已经加入暂存区的可以选择 git diff —cached
提交以后可以选择git show
比较两个提交中的某个文件可以选择git diff logHash:filename logHash:filename
```

```
场景三：操作失误了，我要回滚之 git clean & git reset 

干掉没有加入到索引跟踪的文件(新增文件) git clean -fxd
回滚没有加入到索引跟踪的文件(已有文件) git checkout filename
回滚已加入暂存区还未提交的版本库的文件 git reset
已提交到版本库 git reset --[soft|hard] logHash
```
```
场景四：快速拿到一个提交的所有内容之 git cherry-pick

git cherry-pick logHash 可以获取到该提交的所有变更
```

```
最常用的莫过于 git log

git log filename  单文件的log
git log —author=xxx  查看关于操作人的日志
git log —grep=“xxx”   查看包含xxx关键字的日志
git log —stat   查看影响了哪些文件
git log -p		查看影响并列出影响的行(我的最爱)
```



###5.切换到git开发的方案
我接触过两种方案

第一种：

```
每个RD都在开发机上有独立的用户，每个用户配置自己的git等等。
IDE远程连接到开发机做开发。不需要每次调试改动都需要commit
```

第二种：

```
沿用现在的方式。分支的创建都在本地完成，改动和调试需要commit->push之后才能看到效果(比较麻烦，费时间)
```


###6.关于管理git库和上线

```
应用的最多的应该就是gitlab来管理git库，web界面很友好，操作简单，功能详细
```


不建议每个人都有权限上线,上线应该放在固定的管理员或组长手里

每天可以有固定的发版时间,特性分支统一处理,非特别紧急的bug统一处理发布

线上可以用git只拉取master分支的代码(测试环境同理),所有人都没有操作master的权限,这样线上更安全

或者用脚本取出最新的代码,构建打包好以后分发到服务器,解包后修改入口文件的引入目录,这样线上可以多保留一些版本.回滚在几秒钟内就能完成



搬运工：刘林亮





完全空白项目关联

首先讲一种简单的场景，如果我们新建项目的时候没有生成任何的初始化文件，我们需要做的就是将本地的项目和空白的仓库关联起来，这种情况下的命令是：

#git初始化
git init
#设置remote地址
git remote add 地址
#将全部文件加入git版本管理 .的意思是将当前文件夹下的全部文件放到版本管理中
git add .
#提交文件 使用-m 编写注释
git commit -m "注释"
#推送到远程分支
git push

有文件的项目关联

然后就考虑最开始说的情况，就是远程仓库里已经有文件，这时候你执行上面的步骤是不可以的，因为需要把远程仓库的文件先更新下来。步骤如下

#git初始化
git init
#设置remote地址
git remote add  origin 地址
#获取远程仓库master分支上的内容
git pull origin master
#将当前分支设置为远程仓库的master分支
git branch --set-upstream-to=origin/master master
#将全部文件加入git版本管理 .的意思是将当前文件夹下的全部文件放到版本管理中
git add .
#提交文件 使用-m 编写注释
git commit -m "注释"
#推送到远程分支
git push


git 创建分支
git branch 分支名
切换分支
git checkout 分支名
提交分支
git push origin 分支名
合并分支
git merge 分支名
删除本地分支
git branch -d 分支名
删除远程分支
git push origin --delete 分支名 


git命令
1.何谓同步远程分支？有下面几种情况，
1.本地有新分支，远程仓库没有。
    
2.远程仓库有新分支，本地没有。

3.本地删除了分支，远程也想删除。

4.远程删除了分支，本地也想删除。

第一种情况很好解决，将本地分支推送到远程仓库即可。
本文主要讲解后面几种情况的解决办法。

2.第二种情况：远程仓库有新分支，本地没有。
这在之前我先介绍几个命令。
1.将某个远程主机的更新，全部取回本地：git fetch

2.查看远程分支：git branch -a

3.查看本地分支：git branch

4.切换分支：git checkout 分支

熟悉了以上命令，接下来我们通过一个例子来讲解第二种情况的解决办法。

假如我本地有个git仓库，别人推送了一个新分支到远程仓库，我要获取这个分支到本地，该怎么办？

1.我需要git branch查看一下本地分支，再git branch -a查看一下远程分支，对比下，远程存在哪些本地没有的新分支。但发现，本地和远

程的一样。奇怪，在远程仓库（gitlab/github）明明看到了新分支啊。 

原来现在本地上的现在的远程分支记录是克隆仓库时当时的分支记录。所以我需要
1.首先将某个远程主机的更新，全部取回本地：git fetch

2.再次查看远程分支：git branch -a 发现远程的分支已经可以看见了。

3.然后拉取远程分支到本地：git checkout -b 远程分支名 origin/远程分支名

注：直接克隆整个仓库，可以直接使用git checkout 分支名切换到分支。因为克隆时候已经有远程所有的分支记录。但若之前已经克隆过，后来其他电脑新push一个分支，此时是无法切换到新分支的。使用上述命令可拉取最新分支（原理是在本地新建一个分支和远程分支关联起来）

3.第三种情况：本地删除了分支，远程也想删除。
这在之前我先介绍几个命令。
1.删除远程分支: git push origin -d 分支名

2.删除本地分支: git branch -d 分支名

熟悉了以上命令，接下来我们通过一个例子来讲解第三种情况的解决办法。

假如我在本地想要删除某个分支，我也想把远程仓库的这个分支也要删掉怎么办？

1.使用git branch -d 分支名来删除本地分支。
2.使用git push origin -d 分支名直接来删除远程分支。在次使用git branch -a,发现分支已经不存在了。

或者
1.使用git branch -d 分支名来删除本地分支。
2.最简单的解决办法就是直接到gitlab/github进行删除.


假如我只想把远程的删除掉怎么办？

1.使用git push origin -d 分支名直接来删除远程分支。此时删除的只是远程的分支，本地仍然存在

或者

1.直接到gitlab/github进行删除.


4.第四种情况：远程删除了分支，本地也想删除。。
这在之前我先介绍几个命令。
1.查看远程分支和本地分支的对应关系:git remote show origin  

2.删除远程已经删除过的分支:git remote prune origin

熟悉了以上命令，接下来我们通过一个例子来讲解第四种情况的解决办法。

假如我直接到gitlab/github删除了某个分支，我在本地使用git branch -a查看远程分支，依然存在并且可以切换使用。我本地也想把远程分支记录删除怎么办？

1.git branch -a查看远程分支，红色的是本地远程远程分支记录。

2.执行下面命令查看远程仓库分支和本地仓库的远程分支记录的对应关系：

  git remote show origin  

3.会看到：
 
 refs/remotes/origin/远程仓库已经删除的分支名              stale (use 'git remote prune' to remove)

 其中：

 Local refs configured for 'git push':  命令下面的分支是本地仓库的远程分支记录中仍存在的分支，但远程仓库已经不存在。

4.输入git remote prune origin来删除远程仓库已经删除过的分支

5.验证 git branch -a

  此时可以看到本地远程分支记录已经和远程仓库保持一致了。


实际测试截图






image






image

作者：YINdevelop
链接：https://www.jianshu.com/p/811b07b129e8
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。

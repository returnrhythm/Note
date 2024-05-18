# 怎么把本地项目推到github上进行管理
1. 创建一个新文件夹，右键git bash here 输入git init把这个文件夹变成git可管理的仓库，这时候会发现里面多了一个隐藏文件.git
2. 把项目粘贴到这个本地仓库里面，之后 git add .
3. 之后提交git commit -m "first commit"
4. 之后new一个仓库出来，在本地仓库里输入 git remote add origin https://github.com/xxxxxx
5. git push -u origin main (以前是master但是现在是main)
# gitee 
1. 远程创建仓库，之后观察本地是否有.git文件
2. 有的话在项目那层右键git bash here,然后git remote add origin `https://gitee.com/gao__hao/xxxx` 。 (gite
3. git add . git commit -m "first commit"
4. git push -u origin "master"
# 多人协作常用命令
1. git pull origin xx(要拉取的分支名)拉取同事代码合并到本地
## 初始一个git仓库
git init(注意要在git的终端里面)
## git的三个区域
工作区，暂存区，版本库，使用时，写代码的文件叫做工作区，当需要保存时的准备区域为暂存区，保存暂存区的地方叫做版本库，产生一次版本快照  
使用时，首先使用git的终端，使用git add 文件名(从终端到目标文件的相对路径)或者git add .(保存终端目录下所有改动的文件)将改动的文件放在暂存区，之后使用 git commit -m "注释"将暂存区的代码放在版本库中
## git从暂存区覆盖工作区 删除暂存区
git restore 文件(相对路径)  
git rm --cached 文件(相对路径)  
## git查看版本库的所有记录
git log --oneline  
git reflog --oneline(这个是回退版本后可以查看所有的记录)
## git回退版本
1. git reset --soft 版本id 不会改变工作区和缓存区，使用的场景：比如我有3个版本，回到第二个版本，这样可以修改要提交第三个版本的东西
2. git reset --hard 版本id 改变工作区和缓存区
3. git reset --mixed 版本id 不改变工作区 改变缓存区
总结来说：soft或者mixed可以用于合并版本，比如版本库有很多次提交可以合并，而hard需要谨慎使用
## git删除文件
从本地删除后使用git add .
## 忽略文件
有一些文件没有必要用git进行管理，可以让git忽略他们，步骤如下：
1. 在根目录下建立.gitignore文件
2. 在里面写上想要忽略的文件或者文件夹
## 分支
分支是一个指针，指向提交记录，默认叫master(也可能是别的)，还有一个HEAD指针来指向分支，HEAD指针指向的才是正在操纵的代码，也就是说我们可以有多个分支，这样我们就可以进行一个并行的开发，互不影响，下面是几个常用的命令
1. git branch 查看所有分支名字
2. git branch 分支名 创建一个分支
3. git checkout 分支名 切换分支，使得HEAD指针去指向那个分支
### 分支的合并与删除
当我们解决完一个分支后，可以将其合并到主线上，分三步
1. 切回到要主线(要合并的分支)上 git checkout 分支名
2. 合并其他分支 git merge 被合并的分支
3. 删除合并的指针 git branch -d 被合并的分支
合并之后，会将被合并的分支连在主线后面，同时及主线的指针，以及HEAD都会自动前进指向到最后一个版本
#### 使用
1. git checkout -b (branchName) //创建本地分支并切换

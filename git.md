### git操作命令
#### git rm -r --cached .
最近遇到个问题，在项目建立之后才新增.gitignore文件，发现即使添加忽略的文件和目录，但是执行`git add .`命令的时候，git还是会添加需要忽略的文件，
原来，在项目目录中，新建的文件在git中会有缓存，如果某些文件已经被纳入版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的，这时候就要先将本地缓存删除，
然后再进行add，这样就不会出现忽略的文件了，删除缓存的命令为：
```
git rm -r --cached .
```

#### git stash 储藏当前的修改
当遇到需要切换分支但是当前分支上的修改还不想提交，或者当git莫名增加了些unstage的修改但是可以确定这些修改可以丢弃的时候，可以使用
`stash`命令将当前的修改储藏起来，这样当前分支的工作状态就变得非常干净了
```
git sash
```
然后通过`list`命令查看储藏列表
```
git stash list
```
如果你想要重新应用之前的储藏，则通过`git stash apply`命令进行启用，默认启用最近的储藏，也可以通过指定储藏的名字应用指定的储藏
```
git stash apply stash@{1}
```
或者通过`git stash pop`应用储藏，同时立即将其从堆栈中移走。

如果想丢弃某个储藏，则使用`git stash drop`加上储藏的名称进行丢弃

#### 查看远程分支列表
```javascript
git branch -r
```

#### 拉取远程分支并创建本地分支
（1）本地创建新分支并自动切换到该本地分支
```javascript
git checkout -b 本地分支名 origin/远程分支名
```
（2）本地创建新分支，但不会自动切换到该分支，需要手动checkout
```javascript
git fetch origin 远程分知名:本地分支名
```
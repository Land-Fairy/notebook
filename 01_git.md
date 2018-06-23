## Git

### 1. 标签操作

```
1. 切换到 需要打标签分支
$ git checkout master
2. 打新标签
$ git tag v1.0
3. 查看标签
$ git tag
4. 标签默认打在最新的commit上，如果想要打到其他commit id
$ git tab v2.0 f52c633
5. 查看标签具体信息
$ git show v1.0
6. 创建带有说明的标签
$ git tag -a v3.0 -m 'version release' 1094adb
7. 删除标签
$ git tag -d v4.0
```

### 2. 创建与合并分支

```
1. 创建分支(在当前分支上创建一个新的分支)， checkout -b 表示创建并切换分支
$ git checkout -b dev
2. 查看当前分支
$ git branch
3. 切换分支
$ git checkout master
4. 合并分支(合并指定的分支到当前所在的分支)
$ git merge dev
	默认时，Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
--no-ff 表示禁用 Fast forward模式, 此种方式会创建一个新的分支，所以可以用 -m 添加描述
$ git merge --no-ff -m "merge with no-ff" dev
5. 删除分支
$ git branch -d dev
6. 储存现场，灯恢复现场之后继续工作
$ git stash
	查看储存的现场
	$ git stash list
	恢复现场（恢复的同时，把stash内容也删除)
	$ git stash pop 
	恢复现场，但是恢复后，stash内容并不删除，你需要用git stash drop来删除
	$ git stash apply
```

### 3. 版本回退

```
1. 回退到上一个版本
$ git reset --hard HEAD^
2. 回退到某一版本
$ git reset --hard 1094a
3. 查看已操作的命令
$ git reflog
```

### 4. 工作区和暂存区

![微信截图_20180623190151](.\contents\微信截图_20180623190151.png)

- 工作区

> 直接操作，修改的区域

- 版本库(Repository)

> 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库

- 文件添加到版本库

```
1. 工作区 -> 暂存区(stage)
$ git add把文件添加进去，实际上就是把文件修改添加到暂存区
2. 暂存区(stage) -> 分支
$ git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支
```

### 5. 修改

#### 5.1 丢弃工作区修改

```
$ git checkout -- file
```

#### 5.2 丢弃暂存区修改

```
git reset HEAD <file>
```




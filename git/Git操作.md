### Git操作

#### 1.基础操作

1. git add 存入占存区
2. git commit -m "把占存区的内容提交到了主分支"
3. git status 查看状态
4. git push 提交到远程仓库



#### 2.版本回滚

- `git log`命令显示从最近到最远的提交日志，我们可以看到3次提交

- git log  --pretty=oneline  美化显示最近3次提交

- git reset --hard HEAD^   回到上一个版本  

  上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`

- git reset --hard 1094a   回到指定版本,版本号写commit id的前几位就好了,可以在git log里看到,或者在远程仓库可以看到

#### 3.常用命令

- 删除文件 ,并提交到仓库,分三步
  - 删除文件资源管理器删除或者rm file       file是文件名加后缀
  - git rm file  版本库里删除
  - git commit -m  提交

- `git checkout -- file`   file是文件名,例如:git checkout -- readme.txt可以丢弃工作区的修改,简单说就是回到最后一次git add 或者 git commit时的状态
- `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区,然后重新再调用 `git checkout -- file`  也可以回到原始状态

#### 4.关联远程仓库

- 已经有了本地仓库,关联到远程仓库

```
1.关联远程仓库地址  git@github.com:michaelliao/learngit.git改为自己的地址
git remote add origin git@github.com:michaelliao/learngit.git
2.首次同步本地文件 第一次推送master分支的所有内容
git push -u origin master
3.后面
```

- 有了远程仓库,克隆到本地

```
从远程仓库克隆到本地,git@github.com:michaelliao/gitskills.git换成自己的地址
git clone git@github.com:michaelliao/gitskills.git
```

#### 5.分之管理

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

- 举个例子

```
$ git checkout -b dev  创建Dev分之 并且切换到Dev
$ git branch   查看当前分之
添加一个测试文件后,操作同步
$ git add readme.txt 
$ git commit -m "branch test"
切换回master分之
$ git checkout master
$ git merge dev  把dev分支的工作成果合并到master分支上
$ git branch -d dev  删除dev分之
$ git branch  删除后，查看branch，就只剩下master分支了：
```

- 多人办公解决分之冲突

```
查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
```


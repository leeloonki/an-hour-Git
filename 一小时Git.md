#### Git简介

git：分布式版本控制系统

使用名为**仓库或版本库（Repository）**的**数据库**记录文件变化，仓库中的**每个文件**都有一个完整的**版本历史纪录**-谁（who）什么时候（when）修改哪些（where）文件哪些内容（what），每个文件修改添加删除

版本控制系统

+ 集中式SVN
+ 分布式GIT



#### Git配置

```shell 
#用户名 邮箱配置
git global user 
git global email

#创建仓库
git init  #本地创建
git clone #远程克隆
```



#### 初始化仓库

```shell
mkdir learn_git
cd learn_git/
git init
```



#### 工作区、暂存区、本地仓库

工作区域和文件状态

+ 工作区	 ：文件区域

+ 暂存区     ：索引，保存临时文件即保存即将提交到Git仓库的修改内容

+ 本地仓库 ：git init或者git clone创建的文件夹，包含完整项目历史和元数据

暂存区文件经commit命令添加到暂存区后，暂存区和本地仓库内容相同，使用git ls-files



文件四种状态（除未跟踪外其余状态均被暂存区管理）

+ 未跟踪（untrack）：刚创建的文件，还未被Git管理
+ 未修改（unmodified）：暂存区中的文件未修改
+ 已修改（modified）：暂存区中的文件被修改，还未再次添加到暂存区
+ 已暂存（staged）：修改后且添加到暂存区里的



#### 提交与回退

```shell
git rm -f --cached file #将添加到暂存区的文件删除
git restore file		#丢弃工作区的更改，即使用暂存区文件内容覆盖工作区的文件内容
```

文件A添加到暂存区后，再对A修改，修改完后使用git rm -f --cached A,那么A会被移除暂存区，此时A处于未被跟踪的修改后的A

```shell
#暂存区文件提交本地仓库
git commit  #进入提交页面，该页面编辑提交注释信息，wq保存后自动提交
```

版本提交（提交到本地仓库的）回退

```shell
git reset --soft	#回退某个版本，工作区内容（保留）暂存区内容（保留）
git reset --hard	#回退某个版本，工作区内容（丢弃）暂存区内容（丢弃）
git reset --mixed	#回退某个版本，工作区内容（保留）暂存区内容（丢弃）
#保留即不清空，丢弃即清空
```

假设工作区文件夹有三个文件分别为1.txt 2.txt 3.txt，按顺序进行三次提交，提交的哈希码hash1，hash2，hash3。将文件夹复制三份，分别进行soft，hard，mixed操作。

git reset --soft hash2 ：

提交历史：	两次提交，分别为hash1 hash2，工作区1.txt 2.txt 3.txt  暂存区：1.txt 2.txt 3.txt  查看状态，待提交文件 3.txt

| 回退类型                    | 提交历史 git log            | 工作区 ls         | 暂存区内容 git ls-files | git status       |
| --------------------------- | --------------------------- | ----------------- | ----------------------- | ---------------- |
| **git reset --soft hash2**  | 两次提交，分别为hash1 hash2 | 1.txt 2.txt 3.txt | 1.txt 2.txt 3.txt       | 待提交文件 3.txt |
| **git reset --hard hash2**  | 两次提交，分别为hash1 hash2 | 1.txt 2.txt       | 1.txt 2.txt             |                  |
| **git reset --mixed hash2** | 两次提交，分别为hash1 hash2 | 1.txt 2.txt 3.txt | 1.txt 2.txt             |                  |

hard模式提交版本回退

```shell
git reflog #查找提交记录
git reset --hard 误操作前版本hash码
```



git reflog命令可以查看所有分支的所有操作记录信息（包括已经被删除的 commit 记录和 reset 的操作）

git log命令可以显示当前分支所有提交过的版本信息，不包括已经被删除的 commit 记录和reset的操作。(注意: 只是当前分支操作的信息)。



#### git diff

+ 查看工作区、暂存区、本地仓库间的差异
+ 查看提交版本、文件在两特定版本间的差异
+ 查看分支差异

```shell
# 查看工作区、暂存区、本地仓库间的差异
git diff 		# 工作区与暂存区差异
git diff HEAD 	# 工作区与版本库差异
git diff cached # 暂存区与版本库差异

# 查看提交版本、文件在两特定版本间的差异
git diff hash1 hash2	# 比较两个版本差异
git diff HEAD~ HEAD		# 比较当前版本与上一个版本差异
git diff HEAD^ HEAD		# 比较当前版本与上一个版本差异
git diff HEAD~2 HEAD	# 比较当前版本与之前两个版本的差异
git diff HEAD~2 HEAD file1.txt	 # 比较当前版本与之前两个版本中文件file1.txt的差异
```



#### 删除文件

```
git rm file.txt #将文件同时从暂存区工作区删除
git rm file.txt #将文件从暂存区删除，工作区保留
```



#### .gitignore

使GIt忽略对工作区指定文件和文件夹的管理

应忽略的文件



Blob匹配模式

+ 系统或者软件自动生成的文件
+ 编译产生的中间与结果文件
+ 运行时产生的日志文件、缓存文件、临时文件
+ 涉及身份、密码、口令、密钥等敏感信息文件



#### 关联远程仓库

```shell
git remote add origin git@github.com:username/repo.git		#origin为远程仓库别名
git remote -v		#查看远程仓库别名与地址
git branch -M main	#修改本地分支别名为main
git push -u origin main:main #将本地分支远程分支关联（main:main分别表示 本地main分支，远程仓库origin的main分支）

#远程仓库修改后，同步到本地
git pull origin main:main		#将远程分支拉取到本地再进行合并，如果远程修改与本地仓库无冲突，则合并成功，否则合并失败，需要手动解决
git pull origin main:main		#将远程分支拉取到本地，不合并，需手动合并
```



#### 切换合并分支

```shell
git branch 			#查看分支
git branch dev		#创建分支
git switch dev		#切换到dev分支

git merge dev		#将dev分支合并到当前分支
git log --graph --oneline --decorate --all #查看分支合并过程
git branch			#查看分支列表
git branch -d dev	#分支合并后使用该命令删除dev分支
git branch -D dev	#分支未合并但不需要该分支使用该命令删除dev分支
```

分支合并冲突

两个分支对同一个文件进行了修改并提交

```shell 
git merge dev		#提示合并冲突，合并结果保存在当前分支的冲突文件，修改这个文件即可，再次进行提交
git -am "commit"	#再次进行提交

git merge --abort 	#中止合并

#git rebase ，假设两分支 main dev
git switch dev
git rebase main	#将dev分支合并到main分支最后

git switch main
git rebase dev	#将main分支合并到dev分支最后
```

| 区别 | Merge                                  | Rebase                                                       |
| ---- | -------------------------------------- | ------------------------------------------------------------ |
| 优点 | 不会破坏原分支提交历史，方便回溯和查看 | 不会新增额外额的提交历史，形成线性历史，比较直观简洁         |
| 缺点 | 产生额外的提交节点，分支图比较复杂     | 会改变提交历史，改变了当前分支branch out的节点，避免在共享分支使用 |
|      |                                        |                                                              |



#### 工作流程

GitFlow

GithubFlow
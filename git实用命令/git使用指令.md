# git

[TOC]

## 1.初始化git项目

```bash
$ git init
```

## 2. 把文件添加到仓库

```bash
$ git add 文件名
```

## 3. 把文件提交到仓库

```bash
$ git commit -m "备注"
```

## 4. 查看仓库状态

```bash
$ git status
```

## 5. 查看文件修改内容

```bash
$ git diff

# 比较特定版本
$ git diff commitid
```

## 6. 查看修改的信息

```bash
$ git log

# 只查看版本号
$ git log --pretty=oneline
```

## 7. 版本回滚

```bash
# 回退到上一个版本
$ git reset --hard HEAD^

# 回退到特定版本
$ git reset --hard commitid

# 隔日回滚（记录执行的指令）
$ git reflog
```

## 8. 丢弃工作区内容

```bash
$ git checkout -- 文件名
```

## 9. 从版本库中删除该文件

```bash
$ git rm 文件名
$ git commit -m "备注"
```

## 10. 远程仓库管理

* 创建密钥

```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```

* 关联远程仓库

```bash
$ git remote add origin git@github.com:"用户名"/learngit.git
```

* 推送到远程仓库

```bash
$ git push -u origin master
```

* 提交到远程仓库

```bash
$ git push origin master
```

* 删除远程仓库

```bash
$ git remote rm origin
```

* 仓库克隆

```bash
$ git clone 地址
```

## 11. 分支管理

* 创建分支

```bash
$ git branch dev
```

* 切换分支

```bash
$ git check dev
```

* 查看分支

```bash
$ git branch
```

* 创建并切换分支

```bash
$ git checkout -b dev
```

* 合并分支

```bash
$ git merge dev

# 禁用快速合并
$ git merge --no-ff -m "备注" dev
```

* 删除分支

```bash
$ git branch -d dev
```

* 查看分支历史

```bash
$ git log -graph
```

* bug分支

```bash
# 先将工作区预存
$ git add 文件名

$ git stash

# 新建bug分支
$ git check -b 分支名

$ git add .
$ git commit -m "备注"

# 合并分支
$ git merge --no-ff -m "备注"

# 恢复现场
$ git stash list
$git stash apply
```

* 推送分支

```bash
# 将本地的master分支推送到远程的master分支上
$ git push origin master

$ git push origin dev
```

* 从远程仓库拉取分支

```bash
$git pull origin "分支名"
```

## 12. 标签管理

* 创建标签

```bash
# 给分支打上标签
$ git tag v1.0
```

* 给历史节点补标签

```bash
$ git tag v0.9 commitid
```

* 查看当前标签列表

```bash
$ git tag
```

* 查看指定版本的提交信息

```bash
$ git show v0.9
```

* 创建带有说明的标签

```bash
$ git tag -a v0.1 -m "备注" commitid
```

* 删除标签

```bash
$ git tag -d v0.1
```

* 远程删除标签

```bash
# 先本地删除
$ git tag -d v0.9
# 再从远程删除
$ git push origin :refs/tags/v0.9
```

* 使用版本号回滚

```bash
$ git reset v0.9
```


---
title: Git常用命令
date: 2021-12-13
description: 简单总结Git的常用命令
category: Git
---

## 1.从已有分支切分支
1. 切换到被copy的分支（master），从服务器拉取最新版本

   ```bash
   git checkout master (git switch master)
   git pull
   ```

2. 从当前分支copy出新的开发分支 命名dev分支

   ```bash
   git checkout -b dev (git switch -c dev)
   ```

3. 把新建的分支push到远端

   ```bash
   git push origin dev
   ```

4. 将该分支与远程分支进行关联

   ```bash
   git branch --set-upstream-to=origin/dev
   ```

## 2.将指定分支合并到当前分支
   ```bash
   // 例如将dev分支合并到当前分支
   git merge dev
   ```

## 3.将版本回退至指定的commit_id版本

```bash
git reset --hard commit_id
```

## 4.将指定的提交（commit）应用于当前分支

```bash
git cherry-pick <HashA>    //{A}
git cherry-pick <HashA> <HashB>  //{A,B}
git cherry-pick A..B          // (A,B]
git cherry-pick A^..B       // [A,B]
```

## 5.打标签tag

1. 切换到要打标签的分支上

2. 开始打标签

   ```bash
   // 例: git tag v3.2.1
   git tag <name>
   ```

3. 通过 git tag 命令可以查看所有标签默认标签是打在最新提交的commit上的。

   想要在指定的commit上打tag： 

   ```bash
   // 例：git tag v3.2.2 f54oc33
   git tag <tagname> <commit_id>
   ```

   标签是按字母顺序列出的，使用如下命令查看具体标签信息
   
   ```bash
   git show <tagname>
   ```

   创建带有说明的标签，用 -a 指定标签名， -m 指定说明文字
   
   ```bash
   // 例：git tag -a v3.2.2 -m "标签说明" f54oc33
   git tag -a <tagname> -m "说明" <commit_id>
   ```

   如果标签打错了，可以删除。
   
   ```bash
   //例：git tag -d v3.2.2
   git tag -d <tagname>
   ```

   创建的标签不会自动推送到远程。推送某个标签到远程。
   
   ```bash
   // 例：git push origin v3.2.2
   git push origin <tagname>
   ```

   一次性推送全部尚未推送到远程的本地标签
   
   ```bash
   git push origin --tags
   ```

   如果标签已推送到远程，想要删除远程标签

   1. 先从本地删除
   
   ```bash
   // 例：git tag -d v0.9
   git tag -d <tagname>
   ```
   
   2. 远程删除
   
   ```bash
   // 例：git push origin :refs/tags/v0.9
   git push origin :refs/tags/<tagname>
   ```



## 6.在不提交当前分支修改的内容的情况下切换到其他分支进行操作：

1. 查看当前分支的修改

   ```bash
   git status
   ```

2. 将当前分支修改的内容stash

   ```bash
   git stash
   ```

3. 查看stash列表

   ```bash
   git stash list
   ```

4. 然后才切换想要去的分支

   ```bash
   git switch  目的分支
   ```

5. 切换到之前的分支开发状态

   1. 切换到之前的分支dev

      ```bash
      git switch dev
      ```

   2. 查看stash列表

      ```bash
      git stash list
      ```

   3. 恢复之前的stash内容

      ```bash
      git stash apply stash@{0}
      ```

   4. 删除指定的stash记录

      ```bash
      git stash drop stash@{0}
      ```

## 7.[修改已经push的commit信息](https://www.jianshu.com/p/ec45ce13289f)

## 8.一些查看提交信息的常用命令

```bash
// 查看未传送代码库提交的次数
git status

// 查看未传送代码库提交的描述/说明（唯一id）
git cherry -v

// 查看未传送代码库提交的详细信息
git log master ^origin/master
```

## 9.修改本地仓库所关联的远程仓库地址

```bash
// 查看远程仓库名称
git remote

// 查看远程仓库地址
git remote get-url origin

// 修改远程仓库地址。如果未设置ssh-key，此处仓库地址为http://... 开头
git remote set-url origin git@dev.risinghf.com:website/helium-web.git
```

## 10.查看/修改用户名和邮箱

```bash
// 查看用户名和邮箱
git config user.name
git config user.email

// 修改全局用户名和邮箱
git config --global user.name "xxx"
git config --global user.email "xxx"

// 修改指定项目的用户名和邮箱（先cd到指定的项目仓库下）
git config user.name "xxx"
git config user.email "xxx"
```

## 11.切换仓库的源地址，将http改为ssh。ssh方式可以避免输入用户名和密码的验证方式

```bash
// 查看源地址
git remote -v

// 移除源地址
git remote rm origin

// 添加源地址
git remote add origin [ssh地址]
```

## 12. git报错：OpenSSL SSL_read: Connection was reset, errno 10054

```bash
问题原因： 网络不稳定，连接超时
git config --global http.sslVerify "false"
```

## 13.生成SSH密钥

1. 查看是否已经有SSH密钥。如果没有密钥则不会有此文件夹，有则备份删除。

   ```bash
   cd ~/.ssh
   ```

2. 生成密钥。按3个回车，密码为空。最后得到两个文件 `id_rsa`和`id_rsa.pub`

   ```bash
   ssh-keygen -t rsa -C "用户的email地址"
   ```

3. 将公钥添加到github账号的`SSH and GPG keys`中

## 14.删除本地分支和远程分支

1. 查看所有分支

   ~~~bash
   git branch -a
   ~~~

2. 查看当前所在分支

   ~~~bash
   git branch
   ~~~

3. 删除本地分支

   ~~~bash
   git branch -d x
   ~~~

4. 删除远程分支

   ~~~shell
   git push origin --delete xxx
   ~~~
   

## 15.本机使用了代理，为git设置代理

1. 查看当前的代理地址

   ```shell
   git config --global http.proxy
   git config --global https.proxy
   ```

2. 取消当前代理

   ```shell
   git config --global --unset http.proxy
   git config --global --unset https.proxy
   ```

3. 设置代理

   ```shell
   git config --global http.proxy localhost:1080
   git config --global https.proxy localhost:1080
   ```

## git命令集合

![git命令集合](https://cdn.jsdelivr.net/gh/microhawk/image-host/f0c3e2f54854475485f3fbf6d410a5b5.jpg)
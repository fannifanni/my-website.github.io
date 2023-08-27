---
title: Github代码管理
---

# Github代码管理




## Squash and Rebase

You don't need React to write simple standalone pages.
我们日常的软件开发通常会在一个单独分支（my-branch）上进行，每个分支（master除外）都有一个基分支（base branch）， 因为是团队多人开发，当 base branch 有其她人的新提交的时候，就需要给我们的 my-branch 进行 rebase （必要时，还有 squash）


### 检查my-branch是否有未提交的代码
git checkout my-branch
git status
如果 git status 显示有未提交的代码，有两种方式处理：
#### 方法一：提交代码

```
git add .
git commit -m "commit-message"
```

#### 方法二：stash
```
git stash
```
:::tip My tip

用 git status 确保没有未提交的代码，git stash 对新增或者删除了的文件是不起作用的，这个时候我们只能用方法一

:::

### 更新本地base-branch
```
git checkout base-branch
git pull origin base-branch
```
### Squash and Commits(非必须)
:::tip My tip

如果分支上有多次提交，建议先 squash:

:::

```
git checkout my-branch
git rebase -i HEAD~n
```

n 等于 my-branch 上的 commit 次数，举个例子，如果有 3 次提交：
```
git rebase -i HEAD~3
```

回车，会弹出编辑器界面，如下：
```
pick 343r3r First commit message
pick j6f3f3 Second commit message
pick ze32t5 Third commit message
```

将后面 (n -1) 个 pick 改成 f:
```
pick 343r3r First commit message
f j6f3f3 Second commit message
f ze32t5 Third commit message
```
编辑器保存退出(ctrl+o=>enter=>ctrl+x)，如果看到

:::tip My tip

保存退出的一系列快捷键：=>ctrl+o  =>enter  =>ctrl+x
:::

```
Successfully rebased ...
```
代表 squash 成功,最后 push squash 后的 commit：
```
git push origin my-branch -f
```

### Rebase

如果 my-branch 只有一次提交，或者已经完成 squash，就可以开始 rebase
```
git checkout my-branch
git rebase base-branch
```

如果看到如下

```
$ git rebase base-branch
First, rewinding head to replay your work on top of it...
Applying: ...
Applying: ...
...
```

或者

```
$ git rebase base-branch
Successfully rebased ...
```

代表 rebase 成功，最后执行

```
git push origin my-branch -f
```

如果有冲突 （CONFLICT），解决完冲突之后，运行

```
git add .
git rebase --continue
```

如果仍然有冲突，解决冲突，重复上看两条指令，直到没有冲突为止，如果看到类似如下结果：

```
Applying: ....
```

并且没有看到 CONFLICT 开头的句子，代表本地 Rebase 完成，最后 push 到 remote：

```
git push origin my-branch -f
```
最后，如果之前使用了(方法二) git stash，记得将代码 stash 回来，用：

```
git stash pop
```

## 补签名

### 多个commit未签名：

```
git rebase --exec 'git commit --amend --no-edit -n -S' -i ""
```

然后编辑器保存退出ctrl+o,enter,ctrl+x

### 单个commit未签名：

```
git commit -S --amend 
```

:::tip My tip

最后都：git push origin "" -f

:::

## 创建并使用GPG key 提交代码

对自己的代码进行签名认证，向 GitHub 上的开发者证明某一段代码确实是你自己写的，提高代码在公司团队中的的安全性
下文的 Answer 详细罗列如何在本地新建一个 GPG key，将其上传到自己的 GitHub 账号，并在提交代码的时候用其进行签名

### 新建 GPG key

打开终端，执行

```
gpg --full-generate-key
```

:::tip My tip

key size 必须是 4096 bits

email 必须是 GitHub 注册邮箱

:::

### 获取 GPG key ID：

```
gpg --list-secret-keys --keyid-format=long
```

得到如下结果

```
$ gpg --list-secret-keys --keyid-format=long

/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot <hubot@example.com>
ssb   4096R/4BB6D45482678BE3 2016-03-10
```

在上面的例子中， GPG key ID 是 3AA5C34371567BD2
后续如果需要删除本地 GPG key 可使用

```
gpg --delete-secret-keys 3AA5C34371567BD2
```

配置 Git，在提交代码的时候自动使用这个 GPG key

```
git config --global user.signingkey 3AA5C34371567BD2
```

然后，将这个 ID 复制粘贴到如下指令中，得到 GPG key 的 ASCII armor 原文格式：

```
gpg --armor --export 3AA5C34371567BD2
```

复制 GPG key（包含 -----BEGIN PGP PUBLIC KEY BLOCK----- 和 -----END PGP PUBLIC KEY BLOCK----- 以及中间的内容）

### 上传 GPG key 至 GitHub

请参照 GitHub 官方文档

### 使用 GPG key 提交代码

第一次提交代码之前，配置 Git 提交使用的邮箱：

```
git config --global user.email "your_email@abc.example"
```

:::tip My tip

your_email@abc.example 必须是 GitHub 注册邮箱

:::

:::tip My tip

如果是首次使用 github，还需要执行 

git config --global user.name "<你的 github username>"

:::

以后，所有的代码提交均使用这个指令格式

```
git commit -S -m ""
```

:::tip My tip

-S 代表使用 GPG key 进行签名

```
git config --global user.name "<你的 github username>"
```

:::

:::tip My tip

根据每次代码提交的内容确定
如果在 Mac 操作系统下 commit 遇到 gpg: signing failed: Inappropriate ioctl for device 的报错， 
请先执行

```
export GPG_TTY=$(tty)
```
再重新运行 

```
git commit -S -m ""
```
:::

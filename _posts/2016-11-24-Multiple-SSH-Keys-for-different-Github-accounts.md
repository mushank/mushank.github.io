---
layout: post
title: "Multiple SSH Keys for different Github accounts"
subtitle: ""
author: "Jack"
date: 2016-11-24
tag: [Git, Usage, Github]
header-img: "img/post-img/post-bg-utils.jpg"
---



### 前言

这两天在帮朋友管理Github账户时（假设朋友账户为Cathy），遇到了***在同一台电脑上使用SSH Key获取多个Github账户权限***的问题，所以有了这篇文章进行记录。

### 发现问题

我本地电脑已经有了一对SSH密钥供自己的Github账户使用（假设自己账户为Jack），但是由于同一个SSH公钥无法添加到不同的Github账户中，所以需要生成一对新的SSH密钥供Cathy的账户使用：

```
$ ssh-keygen -t rsa -C 'cathy@email.com'
```

将生成的公钥添加至Cathy的Github账户内：

```
登录GitHub，Account Settings -> SSH Keys -> Add SSH Key，复制id_rsa_cathy.pub文件内容添加新的SSH Key
```

将私钥添加至本地电脑的SSH-Agent：

```
ssh-add -K ~/.ssh/id_rsa_cathy
eval "$(ssh-agent -s)"
```

随后验证SSH授权是否成功：

```
ssh -T git@github.com:Cathy/repoName
```

结果提示信息如下：

> Permission denied (publickey).

拒绝授权哈哈哈！

### 分析问题

使用`ssh-add -l`命令查看SSH-Agent中所有的密钥：

> 2048 SHA256:wJv5ADoixLWNecfJ+wF8gCy3AiC8yIj/1nBI802sO9o id_rsa_jack (RSA)
> 2048 SHA256:TnzWgz+5gvwl85PqlwiWLQgigN6oacAqrCje77F8EV4 id_rsa_cathy (RSA)

发现`id_rsa_cathy`已成功在列，但是马上又产生了一个新的疑问，这两个密钥目前想要获取授权的host都是`github.com`，系统怎么知道什么时候该用哪一个？

如果用对应`jackRepo`的`id_rsa_jack`去验证`cathyRepo`肯定是会被拒绝的！所以我们需要告诉系统，当我访问`cathyRepo`时请使用`id_rsa_cathy`进行验证，当我访问`jackRepo`时才使用`id_rsa_jack`去验证。

### 解决方法

为了告知系统针对同一个host分情况使用不同的`id_rsa`，我们需要对`~/.ssh/config`文件进行修改（如果你的该目录下没有此文件，那么请创建一个），将该文件内容修改如下：

```
Host jack
    HostName github.com
    User git
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_jack

Host cathy
    HostName github.com
    User git
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_cathy
```

可以看到，我们新创建了两个hosts：`jack`和`cathy`，他们都指向github.com，关键是指定了不同的`IdentityFile`。

此时我们在进行SSH验证测试：

```
$ ssh -T jack
$ ssh -T cathy
```

返回提示信息分别如下：

> Hi Jack! You've successfully authenticated, but GitHub does not provide shell access.
>
> Hi Cathy! You've successfully authenticated, but GitHub does not provide shell access.

全部通过授权！！！

然后尝试对不同Github账户下的repo进行clone操作，也都显示成功。

```
git clone git@jack:jack/repoName
git clone git@cathy:Cathy/repoName
```

***注：clone操作时请将`github.com`替换为你创建host时写入的对应名称***




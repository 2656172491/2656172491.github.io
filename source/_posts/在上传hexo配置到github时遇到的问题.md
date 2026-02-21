---
title: 在上传hexo配置到github时遇到的问题
tags: hexo故障
summary: >-
  这里是言寺AI，这篇文章主要介绍了作者在将Hexo博客配置上传到GitHub时遇到的一系列问题及解决过程，首先上传时出现访问权限和仓库不存在的报错，作者先检查了Git全局配置中的用户名和邮箱设置，接着检查GitHub上SSH密钥并尝试通过22端口连接GitHub，出现22端口连接超时报错，改用443端口连接仍报错，经网上查询发现问题出在DNS解析上，通过在~/.ssh/config文件里修改配置，将Host设为github.com，HostName设为ssh.github.com，User设为git，Port设为443，PreferredAuthentications设为publickey，IdentityFile设为~/.ssh/id_rsa，成功解决问题，Hexo
  d命令也恢复正常，报错原因是DNS被墙致GitHub.com无法正常访问，但ssh.github.com子域名可正常访问，修改配置手动解析SSH得以解决。
date: 2025-01-19 22:58:54
---

# 故障现象

在今天对博客页面进行一定美化后打算上传配置到github仓库时出现报错信息，其主要内容是：

```
Please make sure you have the correct access rights
and the repository exists.
--------------------------------------
请确保您拥有正确的访问权限
并且存储库存在。
```

# 解决过程

第一时间进行检查git上的全局配置

```
git config --global user.name 
git config --global user.email
```

通过这2行命令进行检查自己的用户名和邮箱设置是否正常。

于是检查自己在github上面设置的ssh密钥是否正确

并且尝试连接

```
ssh -T git@github.com
```

发现运行后出现报错，报错内容是22端口连接超时

```
ssh: connect to host github.com port 22: Connection timed out
```

于是尝试用443端口进行访问

```
ssh -T -p 443 git@github.com
```

还是出现相同的报错信息，于是上网百度

发现问题出现在于dns解析上面

通过在~/.ssh/config上进行修改

```
Host github.com
HostName ssh.github.com
User git
Port 443
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
```

成功解决问题，并且尝试连接没有发生问题。

hexo d命令也恢复正常

# 报错原因

网上的说法是dns被墙了，github.com无法被正常的访问，但是我们要连接的ssh.github.com子域名是可以正常访问的。于是修改配置，手动对ssh的进行解析。

于是问题成功解决。
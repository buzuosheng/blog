---
title: git push timeout Port 22报错
date: 2020-10-12 20:16:44
tags: 报错
description:
categories: github
---

前些天在一次push的时候，突然给我报这个错，对于这种**以前没问题，突然出问题**的情况，就很烦。

```
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

但是烦归烦，该解决的问题还是要解决的。

先来理性分析一波，是因为**地区问题**。因为在前两天在甘肃push没问题，刚到了新疆吐鲁番，push没成功，我完全有理由怀疑。

但是分析归分析，现在来解决问题。端口22超时，其中的一种方式是换一个端口。

我是win10的操作系统，解决方法是在Git的安装目录下更改了ssh的配置。

进入`git/etc/ssh`目录下，打开`ssh_config`文件，在最后加入以下代码

```
Ciphers +aes128-cbc,3des-cbc,aes256-cbc,aes192-cbc
Host github.com
User git
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

将22端口替换为443端口后，push成功。

但是在几天后又一次出现了超时的问题，这次无论是挂上梯子还是换端口都没有解决这个问题。

过了几天后，我返回甘肃，再次push的时候，成功，没有问题。

应该是不同地区对github的支持不同？

总的来说，算是发现了一个问题，记录下来。
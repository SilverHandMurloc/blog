---
title: 本地GitHub多账号切换
layout: post
categories: [Solutions]
description: "本地GitHub多账号切换"
---

> Secrets are things we give to others to keep for us. 
* random line to make it work. This will be removed.
{:toc}

## 问题

如果有多个Github账户，如何在本地优雅的切换，使用且不借助第三方工具？

## 步骤

1. 创建SSH密钥
2. 将密钥与对应的GitHub绑定
3. 创建config文件进行管理
4. 确认SSH服务开启
5. 配置config文件来管理key
6. 更新存储的key
7. 使用方式

### SSH密钥
``` shell
cd ~/.ssh
ssh-keygen -t rsa -C "your_mail_address"
ssh-keygen -t rsa -C "another_mail_address"
```

**注意**：Enter file in which to save the key时修改默认文件名为`id_rsa_<anything>`

### config文件

- 在 ~/.ssh/ 目录下创建一个 config 文件
- 写入
```
# first account
Host first
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_first

# second account
Host second
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_second
```

### 更新

- 管理员权限下清除当前认证
``` shell
ssh-add -D
```
- 增加key
```shell
ssh-add id_rsa_first
ssh-add id_rsa_second
```
- 验证：看到"Hi \<your Github account\>! You've successfully authenticated, but GitHub does not provide shell access."说明成功
``` shell
ssh -T personal
```

### 使用

<span class = "alert y">Careful.</span>

一般情况下，我们是这样用的
``` shell
git clone git@github.com:SilverHandMurloc/blog.git
```

在多账户的情况下，我们应该这么用，first是config文件中配置好的，切记first要与对应账户绑定
``` shell
git clone git@first:SilverHandMurloc/blog.git
```

其他命令同理

## 参考
- [Quick Tip: How to Work with GitHub and Multiple Accounts](https://code.tutsplus.com/tutorials/quick-tip-how-to-work-with-github-and-multiple-accounts--net-22574)
- [如何在本地管理和切换多个 github 账号？](https://juejin.cn/post/6844903831000596488)

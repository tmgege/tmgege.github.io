---
layout: post
title:  "多个git账户生成多份rsa密钥实现多个账户同时使用"
date:   2015-07-24 10:48:54
categories: Git
tags: cocos2dx c++ vs
excerpt: 多个git账户生成多份rsa秘钥实现多个账户同时使用配置例子了，这个例子非常的好用对于有多个git的朋友有不小的帮助
---

* content
{:toc}

## 生成密钥

使用过git的应该对id_rsa密钥不陌生，生成id_rsa命令如下（windows,linux,macos）：
<pre><code>$ ssh-keygen -t rsa -C "your-email"
</code></pre>

默认情况下，这个密钥会在你账户下的.ssh目录生成id_rsa文件，并对应一个id_rsa.pub公钥文件。

密钥是跟email地址绑定的，你可以在任何用该email地址认证的地方使用id_rsa.pub公钥校验你的私钥，因为系统默认在校验的时候就会读取你的id_rsa私钥文件。

## 多个密钥

有时我们会需要多个密钥来登录不同的git仓库（当然，多个也可以用同一个，这只是为了举例说明），或者不同的人共用一台机器，但需要不同的帐户来登录同一个git仓库，这时我们就需要生成多个不同的密钥文件了。

生成命令还是如上所示，但在出现`Enter file inwhick to save the key`提示的时候要敲入你要生成的私钥的名称(如：id_rsa_1)。

生成好以后，需要配置一下ssh让系统可以找到id_rsa_1这个密钥，即执行add命令将私钥加入ssh：
<pre><code>$ ssh-add ~/.ssh/id_rsa_1
</code></pre>

如果密钥设置了密码则会提示你输入相应的密码。
添加成功后，执行如下命令可以查看当前成功加入ssh的密钥信息：
<pre><code>$ ssh-add -l
</code></pre>
<pre><code>$ ssh-add -L
</code></pre>

好了，如果对应的git服务端已经将你的公钥加入到git用户的authorized_keys文件中，你可以用如下命令查看是否验证成功：
<pre><code>$ ssh -T "git服务器地址" //如：git@github.com
Welcome to GitLab, 谭铭
</code></pre>

如出现welcome之类的提示，那么恭喜你，你可以开始克隆了 : )
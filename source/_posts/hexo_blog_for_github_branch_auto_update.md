---
title: "hexo 博客利用 github 分支同步源文件"
date: 2017-03-06 14:00:00
tags: ['hexo', 'github']
description: "hexo 是一个优秀的静态博客工具，唯一的不足就是源文件无法同步，让人几乎只能在一台电脑上写博客，为了解决这个问题，我们可以使用 Github 来管理我们的 hexo 源文件，具体思路就是：在我们博客的远程仓库中新建一个分支，用这个分支来存储博客的源文件，这样我们每次在更新博客并部署之后可以顺手多执行两条命令将源文件同步到远程分支中去，不需要做任何环境切换的操作，还可以将部署和同步操作写成一个命令脚本，自动执行以上命令。"

---
[原文转自imweb社区：](http://imweb.io/topic/5848d4259be501ba17b10a9a)

---

hexo 是一个优秀的静态博客工具，唯一的不足就是源文件无法同步，让人几乎只能在一台电脑上写博客，为了解决这个问题，我们可以使用 Github 来管理我们的 hexo 源文件，具体思路就是：在我们博客的远程仓库中新建一个分支，用这个分支来存储博客的源文件，这样我们每次在更新博客并部署之后可以顺手多执行两条命令将源文件同步到远程分支中去，不需要做任何环境切换的操作，还可以将部署和同步操作写成一个命令脚本，自动执行以上命令。建立同步的过程很简单：

## 初始化版本库&建立仓库关联（已与远程仓库关联的可忽略这一步）

  一般根据 hexo 教程一步步建立起来的博客都没有跟自己的远程仓库建立关联，查看是否关联的方法是输入 git remote 看是否有关联的远程仓库：

```js
cd blog
git remote
// origin
```

可以看到我这里有一个名叫 origin 的仓库与我本地的文件夹关联，如果当前还没有关联，先用 git init 命令将你的本地文件夹初始化成一个版本库，然后使用 git remote add origin 你的github仓库地址 命令来建立与远程仓库的关联，这里的 origin 是你定义的远程仓库在本地的名字，你也可以叫别的，一般命名成 origin，这样就建立好了关联了，使用刚刚的 git remote 命令检测会看到你关联的仓库。
```js
git init
// Initialized empty Git repository in ****/blog/.git/
git remote add origin git@github.com:lulua87/lulua87.github.io.git

git remote
// orgin
```

## 提交文件

  像正常提交文件那样使用 git add 、 git commit 和 git push 命令提交文件，但这里在 push 的时候要注意新建一个分支去存你要提交的源文件，具体命令是 git push -u origin HEAD:分支名，这里的分支名自己取，HEAD 是版本库的头指针的意思，代表本地版本库里面的最新版本，origin 是刚刚你自己添加远程关联时候的名字，如果你的不是叫 origin 就写成自己定义的名字， -u 参数是为了建立本地分支与远程分支的关联，以后 push 的时候直接输入 git push 就可以了，所以这整个命令的意思就是：把本地最新版本的代码提交到远程仓库的某个分支上去，如果远程仓库还没有这个分支，就在远程仓库里新建一个分支，然后将它跟本地当前分支关联起来。提交之后你就会发现自己的 github 仓库多了一条分支，就是你刚刚提交的那个分支。   至于这里为什么不先在 github 上面手动建立分支，然后再在本地建立关联，是因为如果是远程手动建立分支会自动以 master 分支为模板建立一份一模一样的文件，而我们仓库里面 master 分支存的都是经过 hexo 编译的文件，跟源文件完全不一样，新建这样一个分支之后还要手动把里面的文件删掉，另一个原因是如果在远程手动建分支，你在本地还得手动用 git fetch origin 拉取远程分支的更新，然后再手动建立与分支的关联，比较麻烦，当然如果你是刚开始部署 hexo，github 仓库里面还一点东西都没有的话这些问题都不存在，那就随意。

```js
git add .
git commit -m 'submit sth'
git push -u origin HEAD:rawblog
```

## 设置默认分支

  最后我们需要把你新建的那个分支设置成 github 的默认分支，这样做的原因是为了你以后在别的机器上拉取代码的时候能够直接拉取源文件，不用再指定分支。

在这里选择好默认分支之后，update就行了。现在你就可以使用 github 来同步自己的 hexo 博客源文件啦~

原文章结束－－－－－－－－－－－－

## 遇到的问题总结

#### Mac下配置多个SSH-Key

附上：Mac下配置多个SSH-Key，为了同时在同一台电脑上使用ssh公钥和私钥进行公司日常工作和业余时间进行github代码上传。
因为在同一台电脑上，我们不能指望通过ssh-keygen生成的一对密钥对，既能访问github又能访问公司私有的gitlab。

注意： 这里已经事先配好了用于公司工作提交的第一个ssh公钥和私钥。过程忽略～

我们进行第二个ssh－key的创建注意事项：

1. cd ~/.ssh
2. ssh-keygen -t -rsa -C '你的github绑定的私人邮箱完整格式' -f id_rsa_lulua87
3. ssh -T git@github.com
出现如下信息，表明创建成功：
Hi lulua87/lulua87.github.io! You've successfully authenticated, but GitHub does not provide shell access.
4. cat ~/.ssh/id_rsa_lulua87.pub | pbcopy
把为了github生成的公钥上到你的github的blog仓库对应的设置里的最后一项Deploy keys，点击 add Deploy keys 并复制；
5. vi ~/.ssh/config
然后在~/.ssh 目录下创建config文件，该文件用于配置私钥对应的服务器：

```js
# icode
Host ****
HostName **＊*
User ＊＊工作邮箱＊＊
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa

# github
Host github.com
HostName github.com
User git
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_lulua87
```

PS：编辑完成之后，在英文输入法下， 键入esc, :wq 保存并退出。

当链接到公司内部上的仓库时，会使用id_ras验证远端的公钥是否匹配。而链接到github.com上的仓库时，则使用私钥id_ras_lulua87验证远端（github.com）的公钥(id_rsa_lulua87.pub)是否匹配。

#### hexo安装之后使用hexo指令命令行报错

```js
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }  
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }  
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```

解决办法on google
google之后发现被这个问题困扰的小伙伴还真不少，网上最靠谱的解决办法是：

```js
npm install hexo --no-optional
```

但在我的环境下方法无效：

```js
npm uninstall hexo
npm install hexo --no-optional
```

可是依旧没有效果。
其实hexo暂时并没有用到dtrace-prodider，仅仅是报错而已，hexo的命令还是能执行，但对于强迫症来说，简直无法忍受。

我的解决办法
重装hexo-cli:

```js
npm uninstall hexo-cli -g
npm install hexo-cli -g
```

——————————————————————
2018-04-09补充
遇到的问题：
hexo本地测试运行重启后页面空白,提示 : WARN No layout: index.html?
解决：
我的情况是next目录下是空白的，重新克隆了next仓库（git clone https://github.com/theme-next/hexo-theme-next themes/next）），然后hexo generate; 之后就OK了。

新换的电脑，想把远程库关联到本机

```js
B0000000111581:lulua87.github.io gaolu04$ sudo ssh -T git@github.com
The authenticity of host 'github.com (192.30.255.113)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.255.113' (RSA) to the list of known hosts.
Permission denied (publickey).
B0000000111581:lulua87.github.io gaolu04$ ssh-keygen -t rsa -C "962605247@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/baidu/.ssh/id_rsa): 
/Users/baidu/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/baidu/.ssh/id_rsa.
Your public key has been saved in /Users/baidu/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:uZNNz2Bytvk6eLOtmq35L6NUaDatHqNv794b7gj6gcM 962605247@qq.com
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|                 |
|                 |
|         +       |
|        S O      |
|     . + & *     |
|      E @.+ +    |
|       *oX** .   |
|      o+XX@@O.   |
+----[SHA256]-----+
B0000000111581:lulua87.github.io gaolu04$ ls
_config.yml	node_modules	public		source
db.json		package.json	scaffolds	themes
B0000000111581:lulua87.github.io gaolu04$ cat /Users/baidu/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDcOdd22TpIErmEoW6SiNbaOx7HpQDiG1D66ao95GBmhvSqtK7cQnwJemHJiPnMc2dyv8ZejvGIbzGuqY2fGnbgYeWX7UJXGCmqmzHJi//5fPHpgHHnqZQA1tOqd93Z04ChmP5Pgp9ynBpVV2OxtUhF2Xo68gEHS23sTVa0bANTw95+3ltpvPfq+lGV6sHq+lHWJ+n3dDRjrlit7Ij1Rs09FjeVmhu1+wbHy2qQv1yBreCSVq+hoDCt2SrnpFABeHTaqFBr1e8z6lkXvV5n7WgZLSX5Twwldp7AkrLT3HU2Cj33NhIvgGm20Kp76N4WGV9Mqn3wgPkrhoWm5Yi5ONSJ 962605247@qq.com
B0000000111581:lulua87.github.io gaolu04$
```


---
date: 2023-12-17T18:17:28+08:00
title: "基于Arch Linux搭建hugo博客"
draft: false
url: "/learn/3-1"
keywords:
- hugo博客
categories:
- 学
tags:
- hugo博客
---

# 1.本地搭建hugo

本篇基于的环境是arch linux。

安装go语言：

```bash
yay -S go
```

安装hugo：

```bash
yay -S gohugo-extended-bin
```

创建博客文件：

```bash
hugo new site myblog
```

该操作会创建一个博客文件夹，并在里面生成博客文件

`myblog` 可以替换成你想要的博客文件夹的名字。

进入**博客文件夹的根目录**里面。

```bash
cd myblog
```

下载博客主题，这里选用的是`stack` 主题。

```bash
git clone https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```

将博客主题中的的`exampleSite` 复制到博客的根目录。

```bash
cp -r themes/hugo-theme-stack/exampleSite/* ./
```

将博客原来的配置文件`hugo.toml`删除。

```bash
rm -r hugo.toml
```

主题中的`exampleSite`已经给我们提供了一个配置文件`config.yaml`，接下来我们配置config.yaml文件。可以参考官方文档与其他人的教程来配置。

**渲染博客**

```bash
hugo
```

这个命令会将你更新的文章与配置文件，还有修改的前端文件，在public文件夹里面生成相应的网页文件。最后别人看到的博客其实就是public里面的文件。

**预览博客**

```bash
hugo server
```

![](https://img.0pt.icu/learn/build-hugo/1.png)

现在，打开本地浏览器打开`http://localhost:1313` ，即可看见你的博客。

![](https://img.0pt.icu/learn/build-hugo/2.png)

至此本地搭建hugo完成。

在本地更新的文章与配置文件，还有修改的前端文件，之后查看效果。确认无误后，运行`hugo` 命令渲染生成网页文件后，我们可以开始把博客推送到托管平台上面。

# 2.配置git

先注册一个[github](https://github.com)

生成密钥，在本地运行，(中间需要输入的可以直接回车即可)：

```bash
ssh-keygen -t rsa
```

![](https://img.0pt.icu/learn/build-hugo/5.png)

然后根据他的默认路径（对于我来说是`/home/opeth/.shh`）,找到`id_rsa.pub`

文件。

![](https://img.0pt.icu/learn/build-hugo/6.png)

打开id_rsa.pub文件，将其中的内容全部复制。

来到[github](https://github.com/)页面登录后。

![](https://img.0pt.icu/learn/build-hugo/7.png)

点击头像。

![](https://img.0pt.icu/learn/build-hugo/8.png)

点击设置。

![](https://img.0pt.icu/learn/build-hugo/9.png)

点击`SSH and GPG keys` 。

![](https://img.0pt.icu/learn/build-hugo/10.png)

点击新增SSH密钥。

![](https://img.0pt.icu/learn/build-hugo/11.png)

![](https://img.0pt.icu/learn/build-hugo/12.png)

依次点击。创建一个新的仓库。

![](https://img.0pt.icu/learn/build-hugo/13.png)

![](https://img.0pt.icu/learn/build-hugo/14.png)

创建成功。

![](https://img.0pt.icu/learn/build-hugo/15.png)

点击这里的SSH。

![](https://img.0pt.icu/learn/build-hugo/16.png)

这里的这一串链接既为你的`ssh连接命令` 。先记下来备用。

现在转战本地。

本地如果没有`git`就用**包管理器**安装`git`

```bash
yay -S bash
```

打开终端，运行：

```bash
git config --global user.name yourName
git config --global user.email yourEmail
```

请将`yourName`和`yourEmail` 替换成你自己的github用户名和注册github的邮箱。

进入你本地的博客文件夹的根目录。

(不知道博客文件夹的根目录是什么的。请从上文中的 1.本地部署hugo的 创建博客文件 到 进入**博客文件夹的根目录**  部分里面自行理解推断。)

初始化博客文件夹为git仓库。

```bash
git init
```

![](https://img.0pt.icu/learn/build-hugo/3.png)

将本地的`git`分支从`master`分支转变为`main`分支。

```bash
git branch -m master main
```

![](https://img.0pt.icu/learn/build-hugo/4.png)

将本地的git仓库与我们刚才在github上面创建的那个仓库连接起来。

```bash
git remote add myblog git@github.com:0pt12/myblog.git
```

这里的`myblog` 是刚才我们在github上面创建的仓库的名字，请替换成你自己创建的仓库名字。

这里的`git@github.com:0pt12/myblog.git` 是刚才我们在github上面创建的仓库的`ssh连接命令`，也请替换成你自己的仓库的`ssh连接命令`。

至此，配置git完成。

# 3.将本地的博客文件推送到github仓库

操作都在本地的博客文件夹的根目录下完成。

一般来说，先在本地对博客进行更改，之后先运行：

```bash
hugo
```

将博客的更改内容渲染生成网页文件到public文件夹里面。

![](https://img.0pt.icu/learn/build-hugo/17.png)

然后执行：

```bash
git add .
#注意后面有一个点
git commit -m "Your commit message"
#这里引号里面的内容相当于注释，可以写这次更新的内容
git push -f myblog main
# myblog 是我的刚才创建的github上面仓库的名字，请自行替换
```

![](https://img.0pt.icu/learn/build-hugo/18.png)

![](https://img.0pt.icu/learn/build-hugo/19.png)

推送完成，我们去github上面的这个仓库看看。

![](https://img.0pt.icu/learn/build-hugo/20.png)

刚才空空如也的仓库里面出现了我们本地推送上去的东西。

# 4.将我们博客的github仓库与托管平台连接

这里，我们选用的托管平台是[4everland](https://www.4everland.org/)

点开这个连接[4everland](https://www.4everland.org/)

![](https://img.0pt.icu/learn/build-hugo/21.png)

![](https://img.0pt.icu/learn/build-hugo/22.png)

这是用你的github账号去注册。

![](https://img.0pt.icu/learn/build-hugo/23.png)

![](https://img.0pt.icu/learn/build-hugo/24.png)

这里刚注册就叫你花钱升级，直接给他放弃。

![](https://img.0pt.icu/learn/build-hugo/25.png)

![](https://img.0pt.icu/learn/build-hugo/26.png)

![](https://img.0pt.icu/learn/build-hugo/27.png)

然后4everland就可以识别你的github里面的仓库了。选择我们刚才创建的博客的仓库，点击导入。

![](https://img.0pt.icu/learn/build-hugo/28.png)

![](https://img.0pt.icu/learn/build-hugo/29.png)

![](https://img.0pt.icu/learn/build-hugo/30.png)

![](https://img.0pt.icu/learn/build-hugo/31.png)

![](https://img.0pt.icu/learn/build-hugo/32.png)

等待部署完成。

![](https://img.0pt.icu/learn/build-hugo/33.png)

这里提示我们部署成功了。

![](https://img.0pt.icu/learn/build-hugo/34.png)

点击访问详情页。

![](https://img.0pt.icu/learn/build-hugo/35.png)

# 5.添加域名（需要你有域名）

![](https://img.0pt.icu/learn/build-hugo/36.png)

![](https://img.0pt.icu/learn/build-hugo/37.png)

![](https://img.0pt.icu/learn/build-hugo/38.png)

这里提示我们域名未生效，需要添加一下CNAME记录，目标也给出了。

转到我们托管直接域名的平台，添加一个CNAME记录。我使用的是Cloudflare，其他平台也同理。

![](https://img.0pt.icu/learn/build-hugo/39.png)

![](https://img.0pt.icu/learn/build-hugo/40.png)

添加完成，返回4everland。

![](https://img.0pt.icu/learn/build-hugo/41.png)

等待一会。

![](https://img.0pt.icu/learn/build-hugo/42.png)

这里域名也可以了，现在可以通过访问https://myblog.0pt.icu来访问博客了。

[myblog]([https://myblog.0pt.icu](https://myblog.0pt.icu))

![](https://img.0pt.icu/learn/build-hugo/43.png)

成功！

# 6.后续

之后你每次更新博客，现在本地更改，通过：

```bash
hugo server -D
```

访问 http://localhost:1313 查看效果。

然后运行：

```bash
hugo
```

渲染成网页文件到public文件夹。

再通过：

```bash
git add .
git commit -m "Your commit message"
git push -f myblog main
```

将本地更新内容推送到github仓库上。

当github仓库发生变化时，托管平台会自动检测到变化，并自动重新部署，将更新内容同步到网络上，让其他人可以看到。

# 7.鸣谢

感谢[Yon Zilch](yon.im) 提供的方案。

感谢[Purkit](https://purkit.lockey.icu/) 提供的[教程]([Ubuntu平台下使用Vercel+Hugo搭建静态博客 | Purkit's Blog](https://purkit.lockey.icu/2022/11/11/hello-hugo/)) 。

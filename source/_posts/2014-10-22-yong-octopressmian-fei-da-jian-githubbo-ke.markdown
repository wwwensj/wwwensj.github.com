---
layout: post
title: "用Octopress免费搭建Github博客"
date: 2014-10-22 22:30:17 +0800
comments: true
categories: Github
---
之前使用 [Octopress][1] 搭建的 [Github][2] 博客只写了几篇文章，后来一直没用，最近重新用起来时，发现有不少问题，又要重新找网上文章去搭建，这里我记录一下搭建的过程。

### 简介
[Octopress][1] 利用 [Jekyll][3] 博客引擎开发的博客系统，生成静态的 Html 页面，以一个项目的形式上传至有约定名称的 [Github][2] 上，使其在 `Github` 上以网页的形式展示，由于 `Github` 公开项目托管是免费的，因此相当于免费搭建了一个博客，那这个过程是怎样的？要安装什么软件？用什么形式写博客？怎样绑定自己的域名？下面都能解决你的问题。
 
### 软件安装
由于我使用的是苹果的 `OS X` 系统，因此这里说的也是针对苹果系统说的。

*  1、**安装 Ruby(可选)**  
查看系统已经安装 `Ruby`,可在 `Terminal` 使用以下命令

```
ruby --version // 查看 ruby 的版本
```

如果发现已经安装，则直接进入第 `2` 步即可。

Octopress 依赖 Ruby 环境，我们需要先安装 RVM(Ruby Version Manager,用于安装和管理 Ruby 的)，命令如下：

```
curl -L https://get.rvm.io | bash -s stable --ruby
```

安装后，则安装 Ruby 2.0.0(目前最新是这个版本，网上好多文章是让安装 1.9.3 的)，命令如下：

```
rvm install 2.0.0
rvm use 2.0.0
rvm rubygems latest
```

安装完成后，使用以下如命令查看是否安装完成:

```
ruby --version
```

Octopress 官网也有介绍安装 Ruby的，[猛戳传送门][4]。

*  2、**安装 Octopress**  

使用 `git` 命令把 `octopress` 从 `Github` clone 到本机，命令如下：

```
git clone git://github.com/imathis/octopress.git octopress
cd octopress //进入 octopress 目录
```

clone 完后，安装依赖项:

```
sudo gem install bundler
rbenv rehash    # If you use rbenv, rehash to be able to run the bundle command
bundle install
```

完成后，安装 Octopress 主题，**“如果Ruby版本高于0.9.2时，运行rake时需要有前面加上 bundle exec”**

```
rake insall 
或
bundle exec rake install
```

**“这里 bundle install 可能会报错，例如什么 RedCloth 4.2.9 安装出错，Native Extension 安装出问题等，极有可能是因此没安装 xcode-select”**，那么要安装完 `xcode-select` 后再进行 `bundle install` 的安装，命令如下：

```
xcode-select --install
```

以上的安装过程，`Octopress` 官方也有指导，[猛戳传送门][5]。

*  3、**配置Octopress**  

配置可直接看官方教程 [Configuring Octopress][6]，其中还有点小技巧，可参考大神 `唐巧` 的文章 [象写程序一样写博客：搭建基于github的博客][7]。

*  4、**部署到Github**  

Github的 [Page Service][8] 可免费托管代码，还可以绑定自己的域名。

1、**创建仓库，命名有约定**。例如我的 Github 的 `username` 为 `examplename` ，则仓库名为 `examplename.github.com`

2、**配置仓库的 page**。 在 `Terminal` 运行：

```
rake setup_github_pages
或
bundle exec rake setup_github_pages
```

按上述命令运行后的提示，输入正确的 Url。完成这些操作后，会有本地创建一个 `_deploy` 的目录，就是所谓的发布目录，后面全把目录内容上传到上述仓库 `examplename.github.com` 的 `master` 中。
下面就可以生成博客内容并拷贝至 `_deploy` 目录，并 `commit` 和 `push` 到 `master` 分支了，操作如下：

```
rake generate 或 bundle exec rake generate
rake deploy 或 bundle exec rake deploy
```

博客的 `source` 需要单独提交到仓库的 `source` 分支上，操作如下：

```
$ git add .
$ git commit -m 'Initial source commit'
$ git push origin source
```

至此，博客部署完成了，`Github` 一般会有几分钟的延时才会生效，生效后，用浏览器访问 `http://examplename.github.com` 即可。

我们也可以在本机先睹为快，在部署前，可以先预览博客的模样，命令如下：

```
rake preview
或
bundle exec rake preview
```

在浏览器打开 `http://127.0.0.1:4000`，是不是 amazing。


*  5、**如何添加新的文章**  

Octopress 已经提供了一些方式来创建博文和页面，目录存储和命令规范已经帮我们想好。博文放在 `source/_posts` 下，命令规范为 `YYYY-MM-DD-post-title.markdown` ，名称会当成博文 url 的一部分，并且名称的时间可能用来区分博文。

创建名为 `用Octopress免费搭建Github博客` 的博文，命令如下：

```
rake new_post["用Octopress免费搭建Github博客"]
或
bundle exec rake new_post["用Octopress免费搭建Github博客"]
```

查看 `source/_posts`，会发现生成了 `2014-10-22-yong-octopressmian-fei-da-jian-githubbo-ke.markdown` 的文件，打开，内容如下：

```
---
layout: post
title: "用Octopress免费搭建Github博客"
date: 2014-10-22 22:30:17 +0800
comments: true
categories: Github //编码文件时加的，开始生成没有
---
```

之后就是在上述内容的下方，用 `markdown` 标签语言进行写博文了，建议使用 `LightPaper` 这个软件，网上好多说 `Mou` 不错，但这是以前，现在已经没落了，也已经卖出去了，记得在高的 `OS X` 版本下还有 Bug，支持也不好。

写完后保存，生成再部署，命令如下：

```
$ rake generate 或 bundle exec rake generate
$ rake preview 或 bundle exec rake preview //可选，本地预览
$ git add .
$ git commit -am "提交摘要说明" 
$ git push origin source
$ rake deploy 或 bundle exec rake deploy
```

至此，发博文已经没问题了。

*  5、**如何绑定域名**  

如何你有自己的域名，可以进行绑定的，以我在 [godaddy.com][9] 购买域名举例来说，操作如下：

1、**购买域名**。例如我的为 `www.wensj.net`

2、**配置DNS**。

登录 godaddy.com 后，在已经购买域名的 DNS 设置项目(`DNS Zone file`)中，在 `A(host)` 中添加或修改一条记录，如 `Host 为 @, Points to 为 examplename.github.com对应的 ip 地址`;

在 `CName (Alias)` 中添加或个性一条记录，如 `Host 为 www，Points to 为 examplename.github.com`。

3、**给Page指定域名**。

在 `source` 目录下新建名为 `CNAME` 的文件，里面写上你的域名就行了，例如我的为 `www.wensj.net` ，然后把文件 `commit` 和 `push` 到 github 上。

完成上述配置后，慢慢等它生效就是了。

*  5、**如何添加评论模块**  

添加评论模块其它也挺简单，这里就不说了，我是用 [https://disqus.com][10] 的，注册一下，然后在 Octopress 配置一下，就差不多了，祝你好运。


[1]: http://octopress.org/
[2]: https://github.com
[3]: http://github.com/mojombo/jekyll
[4]: http://octopress.org/docs/setup/rvm/
[5]: http://octopress.org/docs/setup/
[6]: http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/
[7]: http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/
[8]: http://pages.github.com/
[9]: https://www.godaddy.com/
[10]: https://disqus.com/
---
layout: post
title:  "jekyll的安装 || Mac终端socks5代理"
date:   2016-04-17 20:10:08 +0800
categories: jekyll Mac proxy
---

### 前言

由于国情，一些很方便的包和库管理工具在大陆使用总会遇到timeout的问题。jetkyll是基于ruby的静态博客生成程序，可以托管在Git Pages上，支持程序员上传.md格式的博文。

### 1. Mac的依赖包管理Homebrew

安装Homebrew管理Mac软件包：

Install Homebrew.

``` bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Homebrew将软件包安装在自有目录下，通过symlink连接到/usr/local。

Homebrew installs packages to their own directory and then symlinks their files into /usr/local .

查看已安装的包：

Fox example:

``` bash
$ cd /usr/local
$ find Cellar
Cellar/wget/1.16.1
Cellar/wget/1.16.1/bin/wget
Cellar/wget/1.16.1/share/man/man1/wget.1

$ ls -l bin
bin/wget -> ../Cellar/wget/1.16.1/bin/wget
```

如果无法在线安装brew，请跳过本文前往 'https://github.com/rofl0r/proxychains-ng' 寻找离线安装方法。

### 2. 为Mac终端添加socks5代理

``` bash
brew install proxychains-ng
```

修改proxychains-ng默认代理设置：

``` bash
cd /usr/local/Cellar
vim proxyvchains-ng/X.XX/etc/proxychains.conf
```

X.XX为安装的proxyvchains-ng版本号。

或者使用atom编辑proxychains.conf（已安装atom shell）：

``` bash
atom /usr/local/Cellar/proxyvchains-ng/X.XX/etc/proxychains.conf
```

将 'socks4 127.0.0.1 9095' 改为本地ss代理，如： '127.0.0.1 8080' 。

安装完成，在需要代理的命令前加上'proxychains4'即可。

### 3. 安装/更新ruby

``` bash
brew install ruby
```

### 4. 安装jekyll

``` bash
proxychains4 gem install jekyll
```

### 5. 用jekyll创建博客吧

``` bash
jekyll new my-blog
cd my-blog
jekyll serve
```

---
title: 初识hexo
date: 2021-11-02 21:25:15
categories:
- 技术
tags:
---

## 1、新建博客

通过命令新建博客的方式有两种。

<!--more-->

命令1：

```shell
hexo new "blog name"
```

命令2：

```shell
 hexo new page categories
```

命令1新建的博客位置在/source/_posts/下，而命令2会首先在/source目录下新建一个名字叫categories的目录，在categories这个目录下会新建一个默认的博客index。实际上，命令2是新建了一个页面，因此需要对文章进行分类的话，可以用命令2新建一个目录，里面存放类别一样的文章。

## 2、其他命令

```shell
# 清理命令，执行其他命令之前执行
hexo clean
# 修改了配置，新建了博客，都可以运行下这个命令
hexo g
# 启动本地服务器，访问地址为：localhost:4000
hexo s
# 将本地的代码上传到github上面
hexo d
```

***注意：1和2中的所有命令都是在根目录下执行的。***
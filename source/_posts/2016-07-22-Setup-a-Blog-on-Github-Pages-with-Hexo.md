---
title: 使用Hexo静态博客引擎在GitHub pages上搭建博客
tags:
  - blog
date: 2016-07-22 21:52:20
---

> 前提要求: [Git](https://git-scm.com/), [Node](https://nodejs.org/en/)

### 参考[Hexo](https://hexo.io/zh-cn/docs/)文档初始化一个博客目录

假设名为`yourname`

```bash
$ hexo init yourname
$ cd yourname
$ npm i
$ git init
```

<!-- more -->

### (可选) 安装并启用一个主题

[NexT](http://theme-next.iissnan.com/getting-started.html)是个不错的选择

```bash
$ git submodule add https://github.com/iissnan/hexo-theme-next themes/next
```
> 别忘了修改配置启用主题

> 以后只要该主题有更新，定位至主题目录pull即可

### 创建一个GitHub Repo

在[Github](https://github.com/new)上创建一个新的仓库，名为*username.github.io*，其中*username*是你的GitHub用户名

### 部署

安装[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)

```bash
# 在你的博客根目录中
$ npm i hexo-deployer-git --S

```

修改站点的`_config.yml`配置，新增`deploy`节：

```yaml
deploy:
  type: git
  repo: {your-git-repo-url}
  branch: master
  message: "Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }}"
```

部署：

```bash
# 也许先写点什么
$ hexo new {postname}
# ...
$ hexo g -d
```

### (可选)推送源代码到GitHub

*hexo-deployer-git*推送编译好的静态博客，为了更好维护源码，创建一个新的分支推送到远程仓库中

```bash
$ git remote add origin {your-git-repo-url}
$ git checkout -b source 
# add and commit...
$ git push origin source
```

> 你以后可以留在`source`进行开发，使用`hexo g -d`推送博客到`master`
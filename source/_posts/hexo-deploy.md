---
title: Hexo deploy 遇到的那些坑
date: 2018-08-27 22:49:23
tags: Hexo
categories: Blog
---
### 简介
- 通过使用 Hexo + NexT + GitHub Page 可以快速构建一个个人博客

<!-- more -->

### Deploy
- 修改主站配置文件
``` yaml
url: //justforfun2c.github.io/slash_ins
root: /slash_ins/
permalink: :year/:month/:day/:title/
permalink_defaults:

deploy:
  type: git
  repo: git@github.com:JustForFun2C/slash_ins.git
  branch: gh-pages
```

- deploy
``` bash
hexo clean
hexo generate
hexo deploy
```

### GitHub Page
- 源代码和部署代码必须分开在两个不同的 branch, 若放在同一个，则部署后会打乱源代码，会报错
- 必须建立 branch : gh-pages 来存放部署后的代码，gh-pages branch 才能被 GitHub Pages 的设置中识别

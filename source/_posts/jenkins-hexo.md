---
title: Docker + Jenkins + GitHub + Hexo
tags: [Hexo,Jenkins,Docker]
categories: Deploy

---

### 简介

- 使用 Docker + Jenkins + GitHub 实现 push 到 GitHub 自动触发 Jenkins 部署 Hexo Blog

<!-- more -->

#### 安装 Docker

- todo

#### 使用 docker 镜像 certbot 生成证书

#### 定制 Jenkins ssl + Git 镜像

- 如何使用 GitHub + DockerHub 自动部署定制镜像 todo

- 使用命令 

  ```zsh
  docker run --name myjenkins -it -d -p 9000:8443 -v /etc/localtime:/etc/localtime:ro -v /var/jenkins_home/certs:/certs slashins/docker-jenkin-ssl:latest
  ```

- 查看 docker 镜像是否启动成功

  ```zsh
  docker ps
  ```

  ![docker_jenkins](http://pez5ww4dd.bkt.clouddn.com/blog/2018-09-13-docker_jenkins.png)

- 访问 https://你的域名(或域名):9000 即可访问你的 Jenkins

#### 在 Jenkins 插件安装里面安装 Nodejs 插件

- 配置安装 Nodejs 版本
- 安装 Hero-cli

#### Github 上设置 webhook 触发 Jenkins

#### 新建 Job 进行配置

#### 测试


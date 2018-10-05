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

- Repository - setting - webhooks​ - add webhook     ![webhooks](http://pez5ww4dd.bkt.clouddn.com/blog/2018-10-05-webhook.png)

- 填写相应信息

  - payload url 填写 Jenkins 部署的地址 + GitHub-webhook

  ```
  https://Jenkins地址/github-webhook/
  ```

  - Content type : application / json

  - SSL verification 因为我们使用的私人证书，可以先 disable

  - 触发方式：Just the push event

    ![add_webhook](http://pez5ww4dd.bkt.clouddn.com/blog/2018-10-05-add_webhook.png)

#### 在 Github 上获取 Personal access tokens

- Settings - Developer settings - Personal access tokens
- 需要 admin:repo_hook, repo 权限，注意：生成后要手动保存该 tokens

#### 新建 Job 进行配置

- 其中 secret text 配置的是由 Github 授权的 tokens

  ![jenkins_config](http://pez5ww4dd.bkt.clouddn.com/blog/2018-10-05-jenkins_configure.png)

#### 测试

- 对文章进行修改，并上传到 Github 即可出发 Jenkins build 生成相应的博客文章
- 经测试，可成功触发


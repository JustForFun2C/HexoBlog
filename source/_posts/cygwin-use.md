---
title: Cygwin64 推荐安装插件 
tags: Tools
categories: Tools

---

### 简介

- 使用 Docker + Jenkins + GitHub 实现 push 到 GitHub 自动触发 Jenkins 部署 Hexo Blog

<!-- more -->

### 推荐安装插件
- Vim
- Wget
- Curl
- Git
- lynx (可用来安装 apt-cyg)
- gcc
	- gcc-core
	- gcc-g++
	- make
	- gdb
	- binutils
- inetutils (telnet, 也可安装完 apt-cyg 后安装 apt-cyg install inetutils)

### 安装 apt-cyg
```
git clone https://github.com/transcode-open/apt-cyg.git
cd apt-cyg /*切换到apt-cyg源码目录*/
install apt-cyg /bin /*将apt-cyg安装到/bin目录下*/
```
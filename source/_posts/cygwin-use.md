---
title: Cygwin64 推荐安装插件 
tags: Tools
categories: Tools
---

### 简介

- Cygwin是一个在windows平台上运行的类UNIX模拟环境,它对于学习UNIX/Linux操作环境，或者从UNIX到Windows的应用程序移植，或者进行某些特殊的开发工作，尤其是使用GNU工具集在Windows上进行嵌入式系统开发，非常有用。

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

```zsh
git clone https://github.com/transcode-open/apt-cyg.git
cd apt-cyg /*切换到apt-cyg源码目录*/
install apt-cyg /bin /*将apt-cyg安装到/bin目录下*/
```

### 安装 zsh / oh-my-zsh

```zsh
apt-get install -y zsh
```

### 自动启用 zsh
- 在 .bashrc 文件最后添加
```zsh
exec /bin/zsh
```
- 重新载入配置文件
```zsh
source ~/.bashrc
```

### 安装 oh-my-zsh

```zsh
wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O - | sh
```

- 修改主题
```zsh
vim ~/.zshrc
```

- 重新载入配置文件
```zsh
source ~/.zshrc
```
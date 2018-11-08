---
title: IntelliJ IDEA 注册码 
tags: Tools
categories: Tools
---

### 简介

- 详情可查看 http://idea.lanyus.com/

<!-- more -->

- 打开IDEA的JVM配置文件，一般会在C:\Users\用户名\.IntelliJIdea2018.1\config下的idea64.exe.vmoptions文件,如果找不到可以在IDEA中点击”Help-> “Edit Custom VM Options …”自动打开

- 在该文件最后添加一句:-javaagent:{你刚刚下载的补丁的路径} 

```
javaagent:/Users/{你的账户}/Downloads/JetbrainsCrack.jar
```
- 重启IDEA,在激活对话框中选Activation code 随便输入 点击OK 
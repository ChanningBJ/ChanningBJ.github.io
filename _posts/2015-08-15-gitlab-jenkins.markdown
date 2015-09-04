---
layout: post
category: CICD
title: "gitlab与jenkins协同工作"
date: "2015-08-15"
---
介绍如何在配置gitlab在commit以后自动触发jenkins job进行build，并且在成功后添加git tag

<!--more-->


# gitlab push 触发 Jenkins Job

配置后的效果：

- 向分支push后自动触发jenkins job
- jenkins job 成功执行后为当前的commit添加一个标签，方便以后进行线上回滚


需要安装的插件：

- Gitlab Hook Plugin
- GitLab Plugin


## Jenkins Job 的设置

### 设置源代码仓库

首先在Jenkins创建一个job，在“源码管理”中指定git并设置URL。
在 Branch Specifier 中填写 `*/dev*` 表示只有dev开头的分支有push操作的时候才会触发这个job。
![](/pic/2015-09-04T08:15:15.864Z.png)
设置中的“Repository URL”是gitlab中SSH方式的URL，如下：
![](/pic/2015-08-15T07:55:12.689Z.png)

设置触发器：
![](/pic/2015-09-04T08:18:28.742Z.png)
这里 Build when a change is pushed to GitLab. GitLab CI Service URL 的值后面需要填写到gitlab的push hook中

在“Credentials”中添加一个用户的认证方式，这里我们选择ssh key的方式：
![](/pic/2015-08-15T07:57:23.624Z.png)

### 配置build成功以后在gitlab上打标签

首先在源代码管理中为这个仓库起一个名字：
![](/pic/2015-08-15T08:05:49.934Z.png)
创建一个构建后的操作“GIT Publisher”, 在 Target remote name 中填写仓库的名字。TAG的名称可以引用Jenkins的环境变量，如 B$BUILD_NUMBER
![](/pic/2015-08-15T08:04:23.731Z.png)

## GitLab 设置

### 设置jeknins用户的权限
在gitlab项目中，将jenkins的公钥添加到当前项目master用户中。如果不需要构建成功后打标签的话可以在deploy key 中添加jenkins的公钥。
![](/pic/2015-08-15T08:11:58.573Z.png)

jenkins在给git分支打标签的时候会进行push操作，而deploy key只有可读的权限。

### 设置push hook

![](/pic/2015-09-04T08:21:04.153Z.png)

在这里添加一个push的webhook，URL为jenkins job触发器配置中 `Build when a change is pushed to GitLab. GitLab CI Service URL` 对应的值

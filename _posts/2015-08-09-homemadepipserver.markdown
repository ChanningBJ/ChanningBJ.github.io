---
layout: post
category: python
title: "搭建自己的pip server"
date: "2015-08-09"
---

<!--more-->

## step1: set up the directories

    mkdir -p ~/pypi/packages
    cd ~/pypi

## step2: set up the virtualenv and activate it

    virtualenv venv
    . ./venv/bin/activate

## step3: install and start payload service

    pip install pypiserver
    pypi-server -p xxx ~/pypi/packages

## step4: 发布软件包
安装之后需要把软件源码打包成tar.gz并且按照下面的这个方式命名：  pacakgename-version.tar.gz (例如 nginxparser-1.0.tar.gz) ，然后复制到目录 ~/pypi/packages

## step5: 最后，使用pip就可以安装了：

    sudo pip install -i http://xxxxxxxx/simple/ nginxparser

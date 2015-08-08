---
layout: post
category: tools
title: "开始使用 Jeckll 写博客"
date: "2015-08-08"
---

<!--more-->

# atom jeckll 插件

[插件下载地址](https://atom.io/packages/jekyll)

可以使用 `new post` 命令创建一个新的页面。如果需要修改模版，可以更改源码
`~/.atom/packages/jekyll/lib/new-post-view.coffee` :

    fileContents: (title, dateString) ->
        [
          '---'
          'layout: post'
          'category: default'
          "title: \"#{title}\""
          "date: \"#{dateString}\""
          '---'
          'Short desc'
          ''
          '<!--more-->'
        ].join(os.EOL)

# 设置表格边框

在文件 `assets/themes/Snail/css/main.css` 中加入：


    table {
        border-collapse:collapse;
        border-spacing:0;
        border:1px solid black;
    }

    td {
        text-align:left;
        font-weight:normal;
        vertical-align:middle;
        border:1px solid black;
        padding:5px;
    }

    th {border: 1px solid black;
        padding:5px;}


得到下面的效果：

| page items | dd | dd |
|:-----------|:---|:---|
| dd         | dd | dd |
| dd         | dd | dd |

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

# 添加页面访问量

使用了[不蒜子](http://ibruce.info/2015/04/04/busuanzi/)的页面访问统计工具。
首先在 `_includes/themes/Snail/default.html` 添加

    <script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>

然后在 `_includes/themes/Snail/post.html` 模版中自己需要的位置添加上显示访问统计的脚本，例如：

    <HR style="border:1 dashed #987cb9" width="80%" color=#987cb9 SIZE=1>
	  <div class="journal-date">发布时间 {{ page.date | date_to_long_string }} </br>
    <span id="busuanzi_container_page_pv">访问次数 <span id="busuanzi_value_page_pv"></span></span>

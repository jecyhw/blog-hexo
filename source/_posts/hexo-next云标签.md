---
title: hexo next云标签
tags:
  - hexo
  - hexo-tag-cloud
categories:
  - hexo
abbrlink: 2867892032
date: 2018-04-07 11:33:29
---
hexo next添加hexo-tag-cloud云标签
<!-- more -->
### hexo-tag-cloud插件安装
``` 
npm i hexo-tag-cloud --save
```

### 修改next/layout/_custom/sidebar.swig
加入以下内容
``` 
{% if site.tags.length > 1 %}
<script type="text/javascript" charset="utf-8" src="/js/tagcloud.js"></script>
<script type="text/javascript" charset="utf-8" src="/js/tagcanvas.js"></script>
<div class="widget-wrap">
    <h3 class="widget-title">Tag Cloud</h3>
    <div id="myCanvasContainer" class="widget tagcloud">
        <canvas width="250" height="250" id="resCanvas" style="width=100%">
            {{ list_tags() }}
        </canvas>
    </div>
</div>
{% endif %}
```

### 完成安装和显示
``` 
hexo clean && hexo g && hexo s
```
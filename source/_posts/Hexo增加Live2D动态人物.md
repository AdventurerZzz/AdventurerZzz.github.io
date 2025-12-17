---
title: Hexo增加Live2D动态人物
date: 2024-09-18 15:30:52
tags:
---

# 一、安装 hexo-helper-live2d 插件

使用<code>npm</code>安装<code>hexo-helper-live2d</code>模块

**[hexo-helper-live2d 官方中文文档](https://github.com/EYHN/hexo-helper-live2d/blob/master/README.zh-CN.md)**

```bash
npm install --save hexo-helper-live2d
```

---

# 二、添加配置文件

在 hexo 博客到\_config.yml 文件添加以下配置 建议在根目录下的\_config.yml 配置，这样以后换了主题就不用重新配置了 当然也可以在 themes 下的\_config.yml 配置但是要注意模型目录要填写正确

```markdown
live2d:
enable: true # 是否启动
scriptFrom: local # 默认
pluginRootPath: live2dw/ # 插件在站点上的根目录(相对路径)
pluginJsPath: lib/ # 脚本文件相对与插件根目录路径
pluginModelPath: assets/ # 模型文件相对与插件根目录路径
tagMode: false # 标签模式, 是否仅替换 live2d tag 标签而非插入到所有页面中
debug: false # 调试, 是否在控制台输出日志
model:
use: live2d-widget ## 模型文件
display:
position: right # 定位方向 left right top bottom
width: 150 # 小人宽度
height: 300 # 小人高度
hOffset: 0 # 向 偏移
vOffset: -15 # 像 偏移
mobile:
show: true # 手机端是否显示
react:
opacity: 0.8 # 模型透明度
```

其中的模型需要根据自己的需求下载相应的依赖

---

# 三、安装喜欢的模型依赖

通过 npm install npm install --save live2d-widget-model-xxx 来安装你喜欢的模型 比方说作者喜欢的是 koharu 这个那就使用 npm install npm install --save live2d-widget-model-tororo 进行安装

最后我们打开添加的配置文件，找到这一行

```markdown
model:
use: live2d-widget ## 模型文件
```

把 use 后面改成我们安装好的依赖的名称，这样我们就完成了修改使用 hexo d || hexo d 来部署，然后清除一下浏览器缓存，打开后就可以发现我们添加的小人已经在屏幕右下角看着你了。

# 四、模型预览

**[点击这里](https://blog.csdn.net/wang_123_zy/article/details/87181892)**

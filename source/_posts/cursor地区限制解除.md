---
title: "cursor地区限制解除"
date: 2025-12-17 11:32:32
tags:
---

本文用于记录解决 cursor 地区限制解除的方法

# 问题背景

大概在 7 月份的时候，国外有很多大模型在国内被禁用，在 cursor 中使用这些模型的时候会提示，Model not available
This model provider doesn't serve your region.下面提供一些方法来解决这个问题

# 解决方法

## 设置代理

**路径**：**文件**-> **首选项** -> **设置** -> **搜索 proxy**
**操作**： 找到**Http: Proxy** ，在其中填入你的魔法工具的代理地址
![代理设置](/img/proxyuntill.png "代理设置")

## 禁用 HTTP2.0

1. 在 cursor 的设置页面中找到 Network
2. 找到 HTTP Compatibility Mode ,然后选择 HTTP1.1
3. 打开魔法工具，打开 TUN 模式

![cursor设置](/img/cursorSetting.png "cursor设置")

## End

好了，现在就能正常使用限制地区的模型了
![cursordemo](/img/askdemo.png "cursordemo")

---
title: "Github Pages 绑定自定义域名"
date: 2026-04-30 14:32:18
categories:
  - Github Pages
tags:
  - Github Pages
  - 自定义域名
---

使用 GitHub Pages 部署网站后，想要加快国内的访问速度，所以就决定使用自定义域名，然后使用cloudflare的CDN加速。

## 前置条件

你首先需要有一个域名，这里我是在GoDaddy购买的域名,几块钱一年。

# 步骤

## 1. 验证自定义域名

在GitHub Pages的设置中，提供了验证自定义域名的功能，该功能能够防止你的域名被他人恶意使用，所以要先完成对域名的验证。

- 在GitHub页面，点击自己的头像，点击进入 <code style="color: red;">Settings</code>。
- 在左侧栏找到 <code style="color: red;">Pages</code>。
  ![verifien_domain](/img/verifien_domain.png "verifien_domain")
- 点击 <code style="color: red;">Add domain</code>。复制这里的<code style="color: red;"> hostName</code> 和 <code style="color: red;">code</code>。
  ![add_domain](/img/add_domain.png "add_domain")

- 在GoDaddy域名管理页面中，找到DNS设置，点击进入DNS设置页面。
- 在DNS设置页面中，点击添加记录。
- 在添加记录页面中，选择类型为TXT记录，将你刚才复制的 name 和 value 填入，主机记录为<code style="color: red;"> hostName</code>，记录值为<code style="color: red;"> code</code>。
  ![verity_goDaddy](/img/verity_goDaddy.png "verity_goDaddy")

## 2. 配置域名

- 还是点击 Add New Record。然后在弹出的配置框中进行配置，其中 Type 选择 A，Name 填写 @，Value 填写四个由 Github 提供的 IP 地址（这四个 IP 地址大家都一样，具体见 Github 官方文档），如下所示。TTL 默认，最后点击 Save按钮

```bash
@  185.199.108.153
@  185.199.109.153
@  185.199.110.153
@  185.199.111.153
```

![add_type-A](/img/add_type-A.png "add_type-A")

- 在添加记录页面中，选择类型为CNAME记录，主机记录为www，记录值为你的GitHub Pages的域名<code style="color: red;"> username.github.io</code>。
  ![add_www](/img/add_www.png "add_www")

- 到此，在域名服务器的DNS配置就完成，上面的内容需要花一点实际在网络的各个DNS服务器上面传播，接着我们回到GitHub的设置页面。
- 在username.github的仓库中，点击<code style="color: red;">Settings</code>，然后点击<code style="color: red;">Pages</code>。
- 在Custom domain中，输入你购买的域名，然后点击<code style="color: red;">Save</code>，等待一段时间后就能通过域名看到内容了。
  ![add_customDomain](/img/add_customDomain.png "add_customDomain")

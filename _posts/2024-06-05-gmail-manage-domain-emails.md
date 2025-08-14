---
layout: post
title: 如何用 Gmail 统一管理 99 个域名客服邮箱？
tags:
  - 域名邮箱
  - 出海
categories:
  - 折腾不止
date: 2024-06-05 19:41:00 +0800
---

基本每一个出海产品都需要配置一个独立的客服邮箱，项目多了邮件管理就成了问题，今天分享一个能统一管理、互相隔离且体验接近 Gmail 的方案。

#### 实现效果

- ✅ 一个 Gmail 邮箱统一收发件
- ✅ 回复邮件使用一致的域名邮箱
- ✅ 支持任意别名 + 任意自有域名
- ✅ 通过 SPF、DKIM、DMARC 身份验证
- ✅ 发信不暴露 Gmail 邮箱
- ✅ 无缝使用 Gmail App
- ✅ 支持第三方邮件客户端

#### 实现原理

其实很简单，就是支持多域名的邮箱服务 + 邮件路由 + Gmail SMTP 发信。

#### 配置过程

##### 域名邮箱配置

1. 注册一个 Zoho Mail 账号，开通 $12 一年的付费版本。
   https://mail.zoho.com/signup

2. 进入 Zoho Mail 超级管理员后台，进入 Domains 页面，按提示新增域名以及 DNS 记录。
   https://mailadmin.zoho.com/

3. 同时增加 _dmarc TXT 记录（mailto 改一下）。
   ```
   v=DMARC1; p=reject; adkim=s; aspf=s; rua=mailto:postmaster@youremail; ruf=mailto:postmaster@youremail pct=100; fo=1;
   ```

4. 进入 Users 页面，找到 Mailbox Settings 添加需要的 Email Alias。

5. 进入 Zoho Mail 点击发信，这时应该能选择刚才添加的域名邮箱。
   https://mail.zoho.com/

##### 邮件路由配置

1. 进入 Zoho 后台 Mail Settings -> Mail Routing 页面，点击 Add 添加邮件路由。
   https://mailadmin.zoho.com/cpanel/home.do#mailSettings/routing/configuration/list

2. Destination host 填写 gmail.com

3. Basic Settings -> Email Delivery type 选择 Dual Delivery

4. Advanced Settings -> 打开 Recipient username change 选项，Username part for recipient emails 填写 Gmail 客服邮箱前缀。

5. 现在 Zoho Mail 的邮件会按规则自动转发到 Gmail 邮箱。

##### Gmail 邮箱配置

1. 注册个人 Gmail 作为客服管理邮箱，推荐与私人 Gmail 分开。

2. 进入 Gmail 邮箱「设置」，「账号和导入」

3. 在「用这个地址发送邮件」逐个添加你需要使用的域名邮箱，SMTP 服务器使用 smtppro.zoho.com，端口 587，开启 TLS。

4. 「回复邮件时：」勾选「用此相同地址回复」

最后就是检查收发邮件是否正常，查看原始邮件检查身份验证是否通过。

#### 限制

- Zoho Mail 最多设置 30 个域名邮箱。
- Gmail 最多添加 99 个发送邮箱。

所以单个 Gmail 最多可以搭配 4 个 Zoho Mail，实现统一管理 99 个客服邮箱。

#### 成本

最低 $12 一年 + 10 分钟配置

#### 已知缺点

第三方邮件客户端可以添加 Gmail，但发信不支持选域名邮箱。当然，第三方邮件客户端也是可以直接添加域名邮箱的。

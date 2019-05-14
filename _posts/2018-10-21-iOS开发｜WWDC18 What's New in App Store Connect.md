---
layout: post
title: "iOS开发｜WWDC18 What's New in App Store Connect"
subtitle: "WWDC2018 - App Store Connect API"
author: "Jack"
date: 2018-10-21
tag: [iOS, WWDC2018, App Store Connect API]
header-img: "img/post-img/post-bg-ios.jpg"
---

[官方视频请点此处](https://developer.apple.com/videos/play/wwdc2018/301/)

⚠️***本文只关注该视频内讲述的App Store Connet API的部分内容。***

## 概述

从App的生命周期看App Store Connet API支持哪些流程：

1. 设计与开发（不支持）
2. 服务配置（Provison）（支持）
3. 用户管理（支持）
4. 交付（支持）
5. 测试（支持）
6. 准备商店数据（Prepare App Store Metadata）（不支持）

7. 发布（不支持）
8. 数据分析（支持）

API可以自动化以上支持部分。

### 服务配置文件（Provisioning）

通过API可以有更多的控制，可以不用登录Apple Developer网站即可进行生成签署App所需的资源。

API将会使服务配置流程更加自动化：

- 生成服务配置文件（Generate provisioning profiles）

- 创建和撤销签名证书（Create and revoke signing cer）

- 管理设备与bundle ID（manager devices and bundle IDs）

### 用户管理（Manager Users）

- 邀请新用户（Invite new users）

- 修改App权限（用户可以看到哪个App）（Modify app access）

- 管理用户角色（Manager user roles）

- 更新用户个人信息（Update user profiles）

### 交付（Deliver）

- **Transporter**
  - 一个命令行工具，可以将编译版本传送到Apple系统中
  - 原来只支持MacOS，现在也支持Linux了

- 交付前的确认工作（Validation before delivery）

- TOKEN验证方式（除了用户名密码之外的另一种验证方式）

### 测试（Beta Test，TestFlight）

当前，testflight邀请测试用户需要邮箱地址，现在你可以通过公共链接。

- TestFlight公共链接（TestFlight Public Link）
  - 统一不变的URL
  - 可以在任何地方分享
  - 使用该链接任何人可以成为测试人员
  - 最多1w名测试人员（可自由设定上限，但最高不能超过1w）
  - 可以随时禁用该链接

这部分工作都可以使用API进行：

- 创建测试用户群组
- 为群组添加构建版本
- 管理公共链接
- 添加或删除测试用户
- 更新测试信息

### 发布（Distribution）

- 无信息

### 数据分析（Analyze）

- 报告下载
  - 财务报告
  - 销售报告


















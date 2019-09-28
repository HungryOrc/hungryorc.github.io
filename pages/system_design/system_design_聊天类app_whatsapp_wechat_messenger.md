---
title: "System Design 聊天类App: WhatsApp, WeChat, Messenger"
tags: [system_design]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: system_design_聊天类app_whatsapp_wechat_messenger.html
folder: system_design
toc: false
---

## 需求分析

### 地基功能
* 用户注册，登录（用数据库来存）
* Contacts（用数据库来存）

### 核心功能
* 一对一聊天
  * 即时收发消息
  * 一方不在线的收发消息
* 群聊

### 进阶功能
* 历史消息记录
  * 是否后端存储。一般都是
    * 能否多机读取同一个账户的历史消息。微信就不行，换了手机就无法看到聊天记录
* 用户在线状态
* 多机登录 multi devices
  * A机登录，其他BCD机就必须立刻强制下线。微信就是如此
  * 可以多机同时登录一个账户，同时收发消息。FB 就是如此

## 系统承载能力 Scenario

### 



## Reference
* [文章标题 [Youtube]](网址放在这里)

{% include links.html %}

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

### 日活跃用户 (DAU)，月活跃用户 (MAU)
* Monthly Active User 如果是 2 Million，那么我们可以设一个 Daily Active User 和它的比例，比如设为 DAU = MAU * 50%，则 DAU = 1 Million

### Query per Second (QPS)
* 要考虑这个App是一个读更多的App，还是一个写更多的App。一般来说 聊天类App的读比写多很多，比如多10倍
* 如果每个用户每天做20次读写，那么一天就是 1 Million 用户 * 20 queries = 20 M queries，再除以 86400 秒，就是 Query per Second，或者说是 Average QPS
* Peak QPS 可以认为是 Average QPS 的几倍，比如说是 5倍
* 根据 Average QPS 和 Peak QPS 就可以计算需要多少的服务器资源

### Storage
* 假设每个用户每天发送 10条 新消息，那么一天就是 10 M 条新消息，每个消息平均 100个 letter 的话（每个letter 占用 1 Byte），就需要 1 GB / 天 的存储资源





## Reference
* [文章标题 [Youtube]](网址放在这里)

{% include links.html %}

---
title: 零元购自建RSS订阅服务
published: 2026-02-06
description: 如何白嫖免费的云主机构建一套RSS订阅系统
tags: [RSS, 自建]
category: Tech
draft: false
---

# 前言
这篇博客记录了我如何在**零元购条件下**花两天时间搭建出一套稳定可行的**RSS订阅云服务**。

## RSS适合哪些人？
* *厌倦了算法每天喂给你的铺天盖地的大粪，想要主动去过滤信息和夺回自己注意力的人*
* 愿意花时间倒腾的人

## 为什么要自建？
许多网站都会提供RSS订阅服务(例如Github，Twitter等)。对于这些网站，我们可以在客户端上进行直接订阅。

但也有很多网站为了将流量锁在APP里，选择掐断了这项服务(尤其是国内的知乎等)。
对于这些没有RSS订阅服务的网站，我们只能通过RSSHub这样的中间件去爬取它的内容，在生成RSS订阅链接。

RSSHub本身是有公共实例的，但公共实例使用人数多，带宽容易被挤占，导致响应速度不稳定。

幸运的是，RSSHub是开源的，因此我们可以通过自部署来建立自己的RSS订阅云服务。

# Final Plan
* Domain NameServer: Cloudflare
* Reader: FlentReader/FeedFlow
* RSSHub: Hugging Face Docker Container
* FreshRSS: Hugging Face Docker Container
* PostgreSQL: Supabase Free Plan
* Crontab: [cron-job](cron-job.org)

CF部署的各种细节

BiliBili和Zhihu Cookie

Direct Connection和Session Pool

FreshRSS API的坑和登录的区别

设置Trigger: `cron-job.org -> fresh-rss.kokoro2336.site -> rsshub.kokoro2336.site & supabase`

# RSS Reader Selection
本人筛选出了几款能用的 RSS Reader :

| RSS Reader          | 功能                                                     | 刷新频率                       | APP内阅读体验               | UI               | 获取       |
| ------------------- | ------------------------------------------------------ | -------------------------- | ---------------------- | ---------------- | -------- |
| `Fluent Reader`(最优) | 简洁但十分强大；移动端`Fluen Reader Lite`功能较少，仅支持 Server 订阅，无直接订阅 | 高，并且能设置刷新频率                | 好                      | 一般，but who cares | 开源且免费    |
| `FeedFlow`          | 少                                                      | 可实时刷新，但只能设置进入 APP 时刷新和手动刷新 | 网页阅读体验差，且知乎等部分网站抓取不到全文 | 简洁               | 免费       |
| `Folo`              | 丰富，有AI集成(~~虽然没格调用~~)                                   | 低，并且频率非常迷惑                 | 好                      | 好                | Pro 需要收费 |
# FreshRSS Envs
```bash
# FRESHRSS_INSTALL
--default-user= --base-url= --language= --db-type= --db-host= --db-user= --db-password= --db-base= --api-enabled --auth-type=form --db-prefix

# FRESHRSS_USER
--user= --password= --api-password=--email= --language=
```

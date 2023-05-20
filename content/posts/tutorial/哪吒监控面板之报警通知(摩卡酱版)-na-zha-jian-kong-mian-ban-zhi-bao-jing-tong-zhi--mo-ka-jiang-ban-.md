---
title: 哪吒监控面板之报警通知(摩卡酱版)
date: 2022-07-01 15:15:38.0
updated: 2022-07-20 15:15:57.952
url: /p=9
categories:
- tutorial
tags: 
---

## 初始化TG账号
1. 打开 Telegram 点击 @moka233_bot 并启用
2. 发送 /reg 命令初始化账号 (拿到Token)

## 在哪吒面板配置报警项
- text的简单示例 - 配置好即可
    - 名称(自定义即可)：摩卡酱 - TG
    - URL：https://moka.sage.run/api/send?token=你的Token&text=#NEZHA#
    - 请求方式: GET
    - 请求类型: JSON
- 离线通知
    - 名称：离线通知
    - 规则：`[{"type":"offline","duration":3600}]`(离线3600秒)
- 月流量监控
    - 名称：月流量
    - 规则：`[{"type":"transfer_out_cycle","max":549755813888,"cycle_start":"2022-01-01T00:00:00+08:00","cycle_interval":1,"cycle_unit":"month"}]`(月初到月底，最高512G流量监控)

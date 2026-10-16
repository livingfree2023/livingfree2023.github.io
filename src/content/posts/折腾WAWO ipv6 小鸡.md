---
title: 折腾WAWO ipv6 小鸡
category: Tech
tags:
  - VPS
  - XTLS
  - WARP
published: 2026-10-15T20:56:51-07:00
image: https://image.heavenroad.org/default_cover.webp
slug: slug20261015205718
upload: false
---

## 基本情况

简单记录下折腾流程和阶段性结论

**WAWO IPv6 Only 0.5G 500G ¥27/年**

只有 ipv6，但是国内移动过去单线程 500Mbps，非常不错。ipv6 本身解锁 Youtube。

1. Youtube 本机直出 其他默认走 Warp 双栈
	-  **结论**：Youtube 速度 3 万，其他正常
2. 想用 xray 直连 wireguard
	- **结论**： 失败，原因不明
3. 用 Warp 搭建本地 socks，默认走 socks
	1. 避坑：Socks 本身不支持 udp DNS，所以必须用 port 53 做路由直出才能访问。
	2. 避坑：现在的 warp 官方客户端版本（Cloudflare WARP Client 2024.6 之后的版本）底层已经从 WireGuard 协议彻底迁移到了 Cloudflare 自研的 **MASQUE** （基于 QUIC / HTTP/3）协议。**无法用 xray wireguard 直连，勿折腾**。
	3. **结论**： 正常，Youtube 走 Warp 速度**33 万**
4. 用 warp-go 搭建 v4 tun，默认走 warp-go
	1. 结论：youtube 速度变回 3w，延迟 236 ms，但是缓慢能升到 11w，怀疑是 warp-go 还在使用 wireguard 协议，而 CF 把资源倾斜到新协议？
5. 用 realm 搭建到 HK Zouter V6 的隧道
	1. 结论：延迟 1.5ms，Youtube 速度**20w+**
6. 用warp-go添加双栈tun
	1. 结论：youtube 速度3w

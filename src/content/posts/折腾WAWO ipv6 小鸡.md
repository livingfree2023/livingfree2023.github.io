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

只有ipv6，但是国内移动过去单线程500Mbps，非常不错。ipv6本身解锁Youtube。

1. Youtube 本机直出 其他默认走Warp 双栈
	-  **结论**：Youtube速度3万，其他正常
2. 想用xray直连wireguard
	- **结论**： 失败，原因不明
3. 用Warp搭建本地socks，默认走socks
	1. 避坑：Socks本身不支持udp DNS，所以必须用port 53做路由直出才能访问。
	2. 避坑：现在的warp官方客户端版本（Cloudflare WARP Client 2024.6 之后的版本）底层已经从 WireGuard 协议彻底迁移到了 Cloudflare 自研的 **MASQUE** （基于 QUIC / HTTP/3）协议。**无法用xray wireguard直连，勿折腾**。
	3. **结论**： 正常，Youtube走Warp速度33万
4. 用warp-go搭建v4 tun，默认走warp-go
	1. 结论：速度变回4w，怀疑是warp-go还在使用wireguard协议，


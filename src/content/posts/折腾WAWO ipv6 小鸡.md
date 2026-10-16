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
	1. **结论**：youtube 速度变回 3w，延迟 236 ms，但是缓慢能升到 11w，怀疑是 warp-go 还在使用 wireguard 协议，而 CF 把资源倾斜到新协议？
5. 用 realm 搭建到 HK Zouter V6 的隧道
	1. **结论**：延迟 1.5ms，Youtube 速度**20w+**
6. 用 warp-go 添加双栈 tun
	1. **结论**：youtube 速度 3w
7. 无法获取 warp-ip
	1. **结论**：反复尝试 warp 后被禁用了，不折腾了
8. 活了，又死了
	1. **warp 官方客户端直接双栈**： 复活，fast 测速 1G，youtube 20w
	2. **避坑**：犹豫还想搞清楚 warp 的 v6 如果设成默认会不会比本身的更快，把 v6 默认路由写成了 warp，然后机器就失联了，因为我 ssh 上去的是原 v6，而 warp v6 只出不进
	```bash
	ip -6 route replace default dev CloudflareWARP
Read from remote host [原 v6] Operation timed out
Connection to [原 v6] closed.
client_loop: send disconnect: Broken pipe
	```
	3. **求助vnc**：复活
	```bash
	sudo ip -6 route add [原 v6 网段]/64 dev eth0
	sudo ip -6 route replace default via [原 v6 网关] dev eth0
	```
	4. **结论**：warp 的 v6 地址无用，因为没有路由，如果让路由走原 v6，那对性能毫无意义。不要瞎折腾。

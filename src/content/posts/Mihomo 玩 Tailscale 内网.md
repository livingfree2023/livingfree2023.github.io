---
title: Mihomo 玩 Tailscale 内网
category: Tech
tags:
  - Clash
  - Tailscale
  - Mihomo
  - Synology
published: 2026-09-21T21:26:44+08:00
image: https://image.heavenroad.org/default_cover.webp
slug: slug20260921212644
upload: false
---

起因：
懒的每次打开 sunpanel 导航都要输密码

解决方案：
1. 把 sunpanel 的用户设为 public，则不需要登陆了
2. 把 sunpanel 的服务绑定在 tailscale 上，不暴露公网
3. 在 tailscale 官网后台，新建一个 service 叫 sunpanel，endpoint 为 tcp:443
4. 在服务器上运行 `tailscale serve --service=svc:sunpanel --https 443 127.0.0.1:13002` 假设服务在 13002 端口
5. 然后你就可以浏览器访问 https://sunpanel.xxxxxx.ts.net 了，ssl 证书自动搞定
6. 如果不想开 tailscale，还有个办法就是在 mihomo 中添加一个 node，填入自己 tailscale 的 auth-key，然后做好分流规则（ip 和 domain 都要），再加上一条 dns 规则
7. 于是只要开着 mihomo，就能无缝访问 tailscale 内网了。

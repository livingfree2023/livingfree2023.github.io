---
title: Clash进阶-自建结合机场订阅统一落地
category: Tech
tags:
  - Clash
  - Mihomo
  - 机场
  - 落地
  - 链式代理
published: 2026-11-02T18:30:41-08:00
image: https://image.heavenroad.org/default_cover.webp
slug: slug20261102183111
upload: false
---

本文面对有模板基础的 Clash/Mihomo 用户，介绍统一落地出口，结合自建和机场，打造自动优选路由的玩法

本文聚焦两个核心设计模式：

1. **单订阅全量载入，在 `proxy-groups` 层实施多区域按需正则过滤**（避免维护冗余 provider）。

2. **基于 `dialer-proxy` 的机场中继链式落地**，以及**自建节点与机场同地域 `url-test` 容灾池**。

## 拓扑设计

```

```

## 一、架构设计：单 Provider 多 Group 动态过滤

在 `proxy-providers` 保留全量节点；下沉至各个区域策略组内通过 `filter` 匹配关键字。这样只需维持一次订阅拉取任务与单份本地缓存文件，即可供给全区域路由。

```YAML
proxy-providers:
  机场1:
    type: http
    url: "https://subscribe.example.com/api/v1/client/subscribe?token=xxxx"
    path: ./providers/airport.yaml
    interval: 86400
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300
```

## 二、场景落地

### 1. 区域 URL-Test 自动聚合与自建容灾

将自建节点直接写入策略组的 `proxies` 列表，同时通过 `use` 挂载全量 provider 并结合 `filter` 抓取对应地域。

`url-test` 会同时对自建节点与匹配出来的机场节点发起基准测速，配合 `tolerance` 参数防止网络抖动导致的连接震荡。

```YAML
proxies:
  - name: "自建线路HK1"
    type: vless
  - name: "自建线路HK2"
    type: ss
  - name: "自建线路HK3"
    type: anytls


proxy-groups:
  # 香港：自建 + 机场，自动容灾
  - name: "HK-Auto"
    type: url-test # 如果希望优先自建，这里可以改成fallback，如果希望流量分摊，这里填 load-balance
    url: http://落地鸡的web服务/generate_204 # 这里可以改写落地的服务器generate_204
    interval: 300
    tolerance: 50
    proxies:
      - "自建线路HK1"
	  - "自建线路HK2"
      - "自建线路HK3"
    use:
      - 机场1
	  - 机场2
      - 机场3
    filter: "(?i)港|hk|hongkong"
```

### 2. 链式代理：机场专线前置 + 自建落地 (Dialer-Proxy)

利用商业机场优质的国内入口与跨境专线作为 Relay，以自建 VPS 的纯净住宅/机房 IP 作为 Exit。

在 Mihomo / Meta 核心下，无需使用老旧且难以分流的 `relay` 组类型，直接在自建节点配置中使用 `dialer-proxy` 绑定一个上游策略组（例如直接挂载上述配置好的 `HK-Auto`）。

YAML

```
proxies:
  # 出口端：自建纯净落地节点
  # 流量流向：Client -> HK-Auto(机场前置节点) -> Self-HK-Chain(自建出口) -> Target
  - name: "亚太自建落地"
    type: ss
    【其他省略】
    dialer-proxy: "HK-Auto"

```

## 三、自建generate_204提供真实测速

默认使用 Google 的 `generate_204` 测试的是“节点直连 Google CDN 边缘节点”的时延，无法真实反映到目标业务服务器或自建落地的端到端质量。

通过在自建落地机上搭建轻量 HTTP 204 端点，并将 `proxy-groups` 内的 `url` 改向该服务，能精确衡量“客户端 $\to$ 机场中继 $\to$ 落地机”整条链路的真实可用性与往返延迟。

### 1. Nginx 配置示例

```
server {
    listen 56780;
    listen [::]:56780;

    location /generate_204 {
        # 禁用缓存，确保每次探测触发真实网络请求
        add_header Cache-Control "no-cache, no-store, must-revalidate";
        add_header Pragma "no-cache";
        expires 0;
        
        # 核心指令：直接响应 204 且不携带 body
        return 204;
    }

    # 其余路径直接阻断，降低被扫描利用风险
    location / {
        return 404;
    }
}
```

### 2. Caddy 配置示例

```
:56780 {
    respond /generate_204 204
    respond 404
}
```
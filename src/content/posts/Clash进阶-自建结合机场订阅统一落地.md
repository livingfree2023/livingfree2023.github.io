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



本文面对y针对统一落地出口，结合自建和机场，打造自动优选路由的高级玩法

  

本文聚焦两个核心设计模式：

  

1. **单订阅全量载入，在 `proxy-groups` 层实施多区域按需正则过滤**（避免维护冗余 provider）。
    
      
    
2. **基于 `dialer-proxy` 的机场中继链式落地**，以及**自建节点与机场同地域 `url-test` 容灾池**。
    
      
    

## 拓扑设计

Code snippet

![](data:image/png;base64,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)

```
flowchart TD
    subgraph "上游数据源"
        P1["Proxy Provider (单机场全量节点)"]
        SelfNode["Proxies (自建纯净 VPS)"]
    end

    subgraph "Proxy Groups 策略路由层"
        HK["HK-Auto (url-test)
        filter: 港|HK"]
        JP["JP-Auto (url-test)
        filter: 日|JP"]
        US["US-Auto (url-test)
        filter: 美|US"]
        SG["SG-Auto (url-test)
        filter: 狮城|SG"]

        Chain["Chain-Out (Select)
        dialer-proxy: HK-Auto"]
    end

    P1 -.->|全量注入| HK
    P1 -.->|全量注入| JP
    P1 -.->|全量注入| US
    P1 -.->|全量注入| SG

    SelfNode -.->|显式挂载| HK
    HK -->|作为前置中继| Chain
    SelfNode -->|作为出口端| Chain
```

## 一、架构设计：单 Provider 多 Group 动态过滤

在 `proxy-providers` 声明阶段不做任何 `filter`，保留全量节点；下沉至各个区域策略组内通过 `filter` 匹配关键字。这样只需维持一个订阅拉取任务与单份本地缓存文件，即可供给全区域路由。

  

YAML

```
proxy-providers:
  Airport-All:
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

  

YAML

```
proxies:
  - name: "Self-HK-Direct"
    type: trojan
    server: 1.2.3.4
    port: 443
    password: "TrojanPassword"
    sni: hk.yourdomain.com

proxy-groups:
  # 香港：自建 + 机场，自动容灾
  - name: "HK-Auto"
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    proxies:
      - "Self-HK-Direct"
    use:
      - Airport-All
    filter: "(?i)港|hk|hongkong"

  # 日本：仅机场提取
  - name: "JP-Auto"
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    use:
      - Airport-All
    filter: "(?i)日|jp|japan"

  # 美国：仅机场提取
  - name: "US-Auto"
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    use:
      - Airport-All
    filter: "(?i)美|us|united states"

  # 新加坡：仅机场提取
  - name: "SG-Auto"
    type: url-test
    url: https://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50
    use:
      - Airport-All
    filter: "(?i)坡|sg|singapore"
```

### 2. 链式代理：机场专线前置 + 自建落地 (Dialer-Proxy)

利用商业机场优质的国内入口与跨境专线作为 Relay，以自建 VPS 的纯净住宅/机房 IP 作为 Exit。

  

在 Mihomo / Meta 核心下，无需使用老旧且难以分流的 `relay` 组类型，直接在自建节点配置中使用 `dialer-proxy` 绑定一个上游策略组（例如直接挂载上述配置好的 `HK-Auto`）。

  

YAML

```
proxies:
  # 出口端：自建纯净落地节点
  # 流量流向：Client -> HK-Auto(机场前置节点) -> Self-HK-Chain(自建出口) -> Target
  - name: "Self-HK-Chain"
    type: ss
    server: 1.2.3.4
    port: 8388
    cipher: aes-256-gcm
    password: "Password"
    udp: true
    dialer-proxy: "HK-Auto"

proxy-groups:
  # 业务主出口：聚合各区域自动组与纯净链式出口
  - name: "FINAL-OUT"
    type: select
    proxies:
      - "Self-HK-Chain"  # 专走纯净链式出口
      - "HK-Auto"
      - "JP-Auto"
      - "US-Auto"
      - "SG-Auto"
      - "DIRECT"
```

## 三、工程实现关键点

- **生命周期与解析开销**：单 Provider 多 Group 过滤模式下，内存中仅常驻一份 Provider 节点实体列表，各 Group 仅维护指向匹配实体的引用索引，相比实例化多个 Provider 更省内存与健康检查探针开销。
    
      
    
- **UDP 与 DNS 寻址**：使用 `dialer-proxy` 时，自建节点的 `server` 字段若是域名，其域名解析将通过 `dialer-proxy` 前置节点发起。若依赖精准出口 CDN 分流，需注意 Fake-IP 模式与目标端远程解析行为。
    
      
    
- **容错与回退**：若机场节点命名规范不稳定（如临时增减前后缀），可使用管道符拓展正则边界，例如 `(?i)(港|hk|hong\s*kong|hkg)`。
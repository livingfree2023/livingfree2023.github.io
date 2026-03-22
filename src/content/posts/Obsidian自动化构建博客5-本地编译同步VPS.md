---
title: Obsidian自动化构建博客5-本地编译同步VPS
category: Blog
description: ""
tags:
  - GitBlog
  - Obsidian
published: 2026-03-20T12:00:00+08:00
draft: false
#slug: "20260319010005"
---

![](https://image.heavenroad.org/resources/Pasted%20image%2020260322210022.png)


> 走过的弯路
> [Obsidian自动化构建博客1-梦开始的地方](Obsidian自动化构建博客1-梦开始的地方.md)
> [Obsidian自动化构建博客2-迁移Astro](Obsidian自动化构建博客2-迁移Astro.md)
> [Obsidian自动化构建博客3-小鸡编译](Obsidian自动化构建博客3-小鸡编译.md)
> [Obsidian自动化构建博客4-小鸡监控仓库](Obsidian自动化构建博客4-小鸡监控仓库.md)
> [Obsidian自动化构建博客5-本地编译同步VPS](Obsidian自动化构建博客5-本地编译同步VPS.md)
> [Obsidian自动化构建博客6-插入图片](Obsidian自动化构建博客6-插入图片.md)
> [Obsidian自动化构建博客7-总结和插件配置](Obsidian自动化构建博客7-总结和插件配置.md)
> [Obsidian自动化构建博客8-最终章](Obsidian自动化构建博客8-最终章.md)


> [!TIP]
> 我的整个 vault 有私人信息。如果在 vps 直接拉取我的 vault，万一这台被黑了，我整个就炸了。不行不行。还有个问题，如果我修改 Vault 内其他文件，总是会触发 Github Action，这也太麻烦了。
> 换个方案，在本地编译，速度不但快，而且能直接看到错误，把编译好的内容推到 VPS 就行了，也省的在 VPS 上监控 Github 更新，好像是个办法，开始折腾

> [!NOTE]
> Obsidian Shell Command - Build && Rsync VPS - Caddy - Cloudflare Tunnel，发布过程就一个操作！而且不会因为编辑其他文件而触发。VPS 上只有静态文件，安全满分！！

懒得整理过程了，倒是可以记录几个 特别方便的 Obsidian 的设置
1. Template 中可以用 `"260321220355"` 自动生成一个时间戳作为 slug，避免 url 中出现中文转码贼长的问题
2. Template 中 **published** 字段精确到分钟的写法 `2026-03-22T10:12:43+08:00`
3. **shell command** 的脚本（自行替换 **username** 和 **vps**）：
```bash
source ~/.zshrc && \
# 1. 进入工厂目录
cd /Users/username/static-blog-source && \
# 2. 彻底清理旧产物并重新编译
pnpm run build && \
# 3. 只有在编译成功(dist/index.html 存在)时才同步到 VPS
[ -f "./dist/index.html" ] && \
rsync -avz --delete ./dist/ vps:/var/www/html/blog/
```

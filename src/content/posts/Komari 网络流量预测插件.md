---
title: Komari 网络流量预测插件
category: Blog
tags:
  - GitBlog
  - AI
  - VPS
published: 2026-10-08T11:35:08+08:00
image: https://github.com/livingfree2023/Komari-Plugin-NetForecast/raw/main/assets/preview.svg
slug: slug20261008113509
upload: false
---

今天花了大约两三个小时，借助 Gemini 把我的 Komari 网络流量预测插件（[NetForecast](https://github.com/livingfree2023/Komari-Plugin-NetForecast)）彻底打磨完善并顺利完成了提交上架。

整个协作过程高效且顺畅：起初插件首次加载偏慢，我与 Gemini 一起分析瓶颈，不仅加入了带超时保护的并发机制，还实现了一套带有分步状态指示的流光加载进度条；针对原本当日柱状图空白的问题，通过比对昨日基准与网卡实时计数器，精准实现了当日截至当前时间点的实时流量呈现。随后，我们将账单重置日与到期时间逻辑完全对齐，并重写了更加直观、带有高保真效果图的 README 文档。

最后，在 Gemini 的协助下，我们建立了自动打包发布的 GitHub Actions 工作流，成功发布了 `v26.10.08` 正式版本，并严格按照规范向 Komari 官方插件市场提交了上架申请。从性能排查、细节修复到最终开源分发，几个小时内一气呵成完成了高质量闭环。

---

以上是 gemini 自吹自擂的总结，其实大差不差，只是我描述完需求它直接动手干完了，我让它生成一个 html 的 mockup 才发现交互设计的问题，在 mockup 上调试妥当了让它写代码的时候，又擅作主张的把 mockup 的数据给写进去了，害我浪费了 20 分钟排查 bug。

在 github 的上传和 action 的编写方面的确非常丝滑。

因为刚刚做完，实际效果其实我还不知道，要等几天才能看到数据。

---
title: Obsidian自动化构建博客7-总结和插件配置
description: ""
category: Blog
tags:
  - Obsidian
  - GitBlog
image: 
published: 2026-03-20T14:37:00+08:00
draft: false
slug: "20260320143700"
---
![](https://image.heavenroad.org/resources/Pasted%20image%2020260322205756.png)


## 走过的弯路
[Obsidian自动化构建博客1-梦开始的地方](Obsidian自动化构建博客1-梦开始的地方.md)
[Obsidian自动化构建博客2-迁移Astro](Obsidian自动化构建博客2-迁移Astro.md)
[Obsidian自动化构建博客3-小鸡编译](Obsidian自动化构建博客3-小鸡编译.md)
[Obsidian自动化构建博客4-小鸡监控仓库](Obsidian自动化构建博客4-小鸡监控仓库.md)
[Obsidian自动化构建博客5-本地编译同步VPS](Obsidian自动化构建博客5-本地编译同步VPS.md)
[Obsidian自动化构建博客6-插入图片](Obsidian自动化构建博客6-插入图片.md)
[Obsidian自动化构建博客7-总结和插件配置](Obsidian自动化构建博客7-总结和插件配置.md)
[Obsidian自动化构建博客8-最终章](Obsidian自动化构建博客8-最终章.md)


### 目录环境

1. 找一个目录 `git clone` obsidian-vault 和 fuwari 目录，这两个目录应该同级别
2. 把 `fuwari/src/content/posts` 目录删除，用 `ln -s ../../../obsidian-vault/blog fuwari/src/content/posts` 建立软链接
3. 在 fuwari 中安装 pnpm 并执行 `pnpm install`
4. 然后可以用 `pnpm dev` 本地测试， `pnpm build` 本地编译
5. 设置好远程 VPS 的 `ssh key` 和 `.ssh/config`

### 在 Obsidian 中的流程

1. 编写 markdown，图片可以直接粘贴进来，图片会暂存在自文件夹的 resource 目录，没关系也不用上传（可以考虑关闭 git 自动 commit）
2. 执行 `Image Upload Toolkit: Publish Page` 会自动把图片上传 R2，同时更新文章内的链接，但是不会自动删除本地文件
3. 执行 `File Cleaner Dux: Clean files` , 因为 resource 下粘贴的文件不再被任何文件引用，可以用这个插件删除（尽量在这个步骤之后执行 git 同步）

### 关键设置
![](https://image.heavenroad.org/Pasted%20image%2020260321230001.png)

这个设置影响所有的图片路径，图片上传 R2 之后会在 bucket 里面自动建一个 resource 目录，未来所有的链接都会包含 resource，不要轻易改动。


### Templater

- 建一个目录放模板
- 新建一个笔记：`Blog-Front-Matter`:
```
---
title: Post Front-matter
category:
description: ""
tags:
  - GitBlog
image: ""
published: 260322T05:59:35+08:00
slug: "260322050309"
draft: false
---
```
- 配置一个 **folder template**, 指定 blog 目录对应的 template 为 `Blog-Front-Matter`

### Git
![](https://image.heavenroad.org/Pasted%20image%2020260321222728.png)
![](https://image.heavenroad.org/Pasted%20image%2020260321222807.png)
![](https://image.heavenroad.org/Pasted%20image%2020260321222918.png)

### File Cleaner Redux
![](https://image.heavenroad.org/Pasted%20image%2020260321222954.png)
![](https://image.heavenroad.org/Pasted%20image%2020260321223014.png)

### Image Upload Toolkit
![](https://image.heavenroad.org/Pasted%20image%2020260321223104.png)
![](https://image.heavenroad.org/Pasted%20image%2020260321223122.png)

#### Cloudflare R2 bucket
- Enable public access
- Expose your bucket as a custom domain under your control.
- Expose your bucket using a Cloudflare-managed `https://r2.dev` subdomain for non-production use cases.

### Linter
- Quote 这个开关和 templater 里面的模板冲突，必须关掉
![](https://image.heavenroad.org/Pasted%20image%2020260321223329.png)
- 这个必须开启，否则中英文混合的 markdown 会出错！
![](https://image.heavenroad.org/Pasted%20image%2020260321223534.png)

### Shell Command

- 编译和同步的脚本
```bash
source ~/.zshrc && \
# 1. 进入工厂目录 lint 一下内容
cd ~/static-blog-source && \
# 2. 彻底清理旧产物并重新编译
pnpm run build && \
# 3. 只有在编译成功(dist/index.html 存在)时才同步到 VPS
[ -f "./dist/index.html" ] && \
rsync -avz --delete ./dist/ hklife:/var/www/html/blog/

```

- Output - stdout - ignore
- Output - stderr - ask after execution （可以方便的看结果，熟练后无所谓）

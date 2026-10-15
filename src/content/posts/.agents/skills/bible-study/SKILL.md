---
name: bible-study
description: Create Obsidian-ready Bible book study guides with BibleProject Chinese posters and videos, and add ebible.org links to scripture references in requested Markdown articles in this blog directory. Use for book overviews, backgrounds, key verses, or scripture-linking requests.
---

# Bible study

Use the part of this workflow that matches the request: write a new book overview, link references in an existing article, or do both. Work only on files the user identifies. Keep the original `link-ebible-references` skill separate; this skill contains the complete linking instructions needed to work on its own.

## Book overview

Produce a complete, standalone Markdown study guide for the named Bible book. If the user asks for chat output, return the document itself without conversational framing, truncation, or placeholders. If the user asks for a file, save it at the requested location. For a new article in this blog directory, use the frontmatter and media placement exemplified by `玛拉基书.md`; for an existing article, preserve its unrelated metadata.

Use Simplified Chinese by default throughout headings, analysis, and scripture quotations. Follow an explicit language request throughout the whole document. Do not alternate Chinese and English prose or use slash-separated bilingual labels unless the user requests bilingual text. Use CUVS for Chinese scripture quotations and NASB for English quotations; verify the exact wording against an authorized source and identify the translation. Never reconstruct a quotation from memory. If the requested translation cannot be verified or reproduced within applicable quote limits, cite and paraphrase accurately rather than inventing text.

### BibleProject poster and video

For a study guide about a specific Bible book, look up that book on `https://bibleproject.com/zh-hans/`. Match the Chinese book title and check whether BibleProject presents one video, multiple parts, or a combined-book video. Use the site's **海报** link for the `image` field and its **下载** link for the video source. Use the direct poster image URL and direct MP4 stream URL, not the site's homepage or a YouTube page. A download-only query string may be removed from the MP4 URL so the `<video>` element can play it. Do not invent, hardcode for other books, or reuse another book's media URLs.

For a new blog article, model the YAML header on `玛拉基书.md`: `title` is the book name, `category` is `宗教`, `tags` include the book name and study tags, `published` is the actual creation time in a valid ISO timestamp, `image` is the verified BibleProject poster URL, `slug` is a unique value following the directory convention, and `upload` is `false`; keep `aliases` when useful. Do not copy Malachi's timestamp, slug, poster, or video into another book. For an existing article, update only the relevant `image` field and media block unless the user asks to revise other metadata.

Place the video immediately after the YAML header and before the book heading, using the same HTML form as `玛拉基书.md`:

```html
<video width="100%" height="468" controls>
  <source src="VERIFIED_MP4_URL" type="video/mp4">
  您的浏览器不支持 video 标签。
</video>
```

Translate the fallback sentence for an English guide. For a book with multiple video parts, place one block per part in BibleProject's order; use the book's matching poster in `image`. If the site offers only a combined-book video, use it only when it clearly covers the requested book. If a poster or playable MP4 is unavailable, omit the missing field or block rather than insert a placeholder; preserve any existing unrelated image. Check that each selected URL belongs to the requested book and that the Markdown contains valid YAML and video HTML.

Default document structure (translate the headings and labels into natural English for an English guide; use the blog header above for new articles in this directory):

```markdown
---
title: 中文书卷名
category: 宗教
tags:
  - 中文书卷名
  - bible-study
  - old-testament OR new-testament
  - appropriate-genre-tag
aliases:
  - 中文书卷名
  - English Book Name
published: ACTUAL_ISO_TIMESTAMP
image: VERIFIED_BIBLEPROJECT_POSTER_URL
slug: UNIQUE_BLOG_SLUG
upload: false
---

<video width="100%" height="468" controls>
  <source src="VERIFIED_MP4_URL" type="video/mp4">
  您的浏览器不支持 video 标签。
</video>

# 中文书卷名

## 1. 历史与文化背景

- **作者** ：传统归属、文本所说的作者，以及主要学术争议
- **写作时间与地点** ：时代、可能地点与证据
- **原始受众** ：最初读者及其处境
- **历史背景与写作目的** ：历史、文化、核心问题与写作目的

## 2. 全书概要与结构

### 核心主题

用 1 至 2 个自然段概括全书的信息与神学意义。

### 结构大纲

按全书主要分段列出章节范围，并说明思想脉络与核心内容。

## 3. 核心经文

选 3 至 5 处代表全书主旨或转折的经文。每处使用：

### 书卷名 章:节

> 「经文原文」（CUVS）

- **背景与意义** ：解释该处在全书论证中的作用、历史与神学意涵，并避免断章取义。
```

Choose the actual testament and genre tag, such as `pentateuch`, `history`, `poetry`, `major-prophets`, `minor-prophets`, `gospels`, `pauline-epistles`, `general-epistles`, or `apocalyptic`. Cover the whole book in the outline. Distinguish explicit biblical statements, historical reconstruction, and church tradition. Verify chapter and verse numbers, especially where Hebrew and English/Chinese versification differs. Do not use Markdown tables unless requested.

For Chinese typography, place one space between Han characters and English letters or numbers; use full-width Chinese punctuation and 「」 for primary quotations, 『』 for quotations within quotations. In English, use English punctuation. Put a space on both sides of inline Markdown emphasis and links when they occur within running text; keep Markdown syntax valid in headings, lists, and blockquotes.

## ebible.org links

Add links to explicit scripture citations in new guides and, when requested, existing Markdown articles. A named book with a chapter, verse, or range is a citation. An unqualified chapter/verse reference is a citation when its book is clear from context. A whole-book mention counts when it clearly cites that book as a source; an incidental mention does not.

- Preserve the visible citation, punctuation, emphasis, frontmatter, and other existing content. Wrap only the citation text as `[text](url)`. Do not nest or duplicate links.
- Use an existing ebible.org study link in the article as the model for query order and translation settings. Otherwise use `https://ebible.org/study/?v2=BOOKCHAPTER_VERSE&t2=local%3Aeng-web&w2=bible&w1=bible&t1=local%3Acmn-cu89s&v1=BOOKCHAPTER_VERSE`.
- Set `v1` and `v2` to the cited book and first verse of the range. A chapter-only citation starts at verse 1; a whole-book citation starts at chapter 1 verse 1. Keep the full range in the visible text. Where versification differs, retain the original citation and link to its corresponding verse in the URL's translation.
- Verify book identifiers from `https://ebible.org/study/content/texts/eng-web/index.html` or another authoritative ebible.org page rather than guessing. Examples: Genesis `GN`, Numbers `NU`, Psalms `PS`, Obadiah `OB`, Malachi `ML`, Romans `RM`, Hebrews `HB`.
- Check each URL's book and start verse against its citation. For existing files, review the diff for accidental prose changes and confirm no requested reference remains unlinked.

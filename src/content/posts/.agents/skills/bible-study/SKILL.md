---
name: bible-study
description: Create Obsidian-ready introductory study guides for Bible books and add ebible.org links to scripture references in requested Markdown articles in this blog directory. Use for book overviews, backgrounds, key verses, or scripture-linking requests.
---

# Bible study

Use the part of this workflow that matches the request: write a new book overview, link references in an existing article, or do both. Work only on files the user identifies. Keep the original `link-ebible-references` skill separate; this skill contains the complete linking instructions needed to work on its own.

## Book overview

Produce a complete, standalone Markdown study guide for the named Bible book. If the user asks for chat output, return the document itself without conversational framing, truncation, or placeholders. If the user asks for a file, save the document at the requested location; when writing into this blog directory, follow an established article's frontmatter if its publishing fields are required, while retaining the study metadata below where compatible.

Use Simplified Chinese by default throughout headings, analysis, and scripture quotations. Follow an explicit language request throughout the whole document. Do not alternate Chinese and English prose or use slash-separated bilingual labels unless the user requests bilingual text. Use CUVS for Chinese scripture quotations and NASB for English quotations; verify the exact wording against an authorized source and identify the translation. Never reconstruct a quotation from memory. If the requested translation cannot be verified or reproduced within applicable quote limits, cite and paraphrase accurately rather than inventing text.

Default document structure (translate the headings and labels into natural English for an English guide):

```markdown
---
tags:
  - bible-study
  - old-testament OR new-testament
  - appropriate-genre-tag
aliases:
  - 中文书卷名
  - English Book Name
---

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

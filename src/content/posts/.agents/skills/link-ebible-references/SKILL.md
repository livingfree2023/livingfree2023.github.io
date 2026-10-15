---
name: link-ebible-references
description: Add ebible.org Markdown links to Bible references in Markdown articles in this blog directory. Use when asked to link, complete, or normalize scripture citations in one or more .md files.
---

# Link Bible references in blog articles

Work only on the Markdown files the user identifies within this blog directory. Identify explicit Bible references in prose and headings, including a named book with a chapter, verse, chapter range, or verse range. A whole-book mention counts when the text clearly cites that book as a source; an incidental book name does not.

- Keep the visible citation text, surrounding punctuation, emphasis, frontmatter, and other article content unchanged. Wrap only the citation text in `[text](url)`.
- Preserve existing Markdown links and avoid nested or duplicate links. Use an existing ebible.org study link in the article as the model for its query parameter order, translations, and pane layout. If none exists, use the two-pane pattern below.
- Set both `v1` and `v2` to the cited book and **first verse** of the cited range. For a chapter without a verse, use verse 1. For a whole book, use chapter 1 verse 1. The displayed citation retains the full range; the URL opens at its start because this study URL takes a single verse position.
- Use ebible.org's book identifiers, not guessed English abbreviations. Confirm unfamiliar identifiers from the book links at `https://ebible.org/study/content/texts/eng-web/index.html` or another authoritative ebible.org page. For example, Genesis is `GN`, Numbers `NU`, Psalms `PS`, Obadiah `OB`, Malachi `ML`, Romans `RM`, and Hebrews `HB`.
- Default URL: `https://ebible.org/study/?v2=BOOKCHAPTER_VERSE&t2=local%3Aeng-web&w2=bible&w1=bible&t1=local%3Acmn-cu89s&v1=BOOKCHAPTER_VERSE`. Replace both positions with values such as `GN25_27`.
- Check every added URL's book and starting verse against its visible citation. Review the diff for accidental prose or formatting changes and confirm no requested citation remains unlinked.

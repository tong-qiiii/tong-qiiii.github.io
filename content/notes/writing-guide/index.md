---
title: "博客写作指南 · 从第一篇题解开始"
description: 用 Markdown 发布题解、展示公式和代码，并整理自己的学习资料。
date: 2026-10-14T09:00:00+08:00
slug: writing-guide
categories: [学习笔记]
tags: [博客使用, Markdown]
math: true
toc: true
---

## 发布一篇题解

在仓库创建 `content/solutions/你的题目名/index.md`。可以复制 [题解模板](/templates/solution.md)，填好标题、日期、标签与正文，最后将 `draft` 改为 `false`。

```yaml
---
title: "比赛名 · A 题题解"
description: "一句话总结核心思路"
date: 2026-10-14T12:00:00+08:00
slug: contest-a
draft: false
categories: [ICPC 题解]
tags: [动态规划, C++]
math: true
toc: true
---
```

提交到 `main` 后，GitHub Actions 会自动构建并发布。可以在仓库的 [Actions 页面](https://github.com/tong-qiiii/tong-qiiii.github.io/actions) 查看发布结果。

也可以直接把原题链接、你的代码和思路发给 Codex，让它帮你整理为文章并更新仓库。

## 推荐的题解结构

1. **题目信息**：比赛、题号、原题链接。
2. **题意与约束**：影响解法的数据范围和边界情况。
3. **思路推导**：为什么想到这个做法，如何改进朴素算法。
4. **正确性证明**：证明关键转移、贪心选择或算法不变量。
5. **复杂度**：说明时间与空间消耗。
6. **代码与复盘**：完整实现、易错点和其他解法。

## 数学公式

行内公式用 `$...$`，例如 `$O(n \log n)$` 会显示为 $O(n \log n)$。

独立公式用两行 `$$` 包住：

```text
$$
dp_i = \max(dp_{i-1}, dp_{i-2} + a_i)
$$
```

## 代码、图片与附件

代码块使用三个反引号并标注语言，例如 `cpp`、`python`。页面支持代码高亮与复制。

把 `diagram.png` 或 `notes.pdf` 放到文章的 `index.md` 旁边，然后写：

```markdown
![思路示意图](diagram.png)
[下载笔记](notes.pdf)
```

通用附件放在 `static/materials/`，页面引用时使用 `/materials/文件名`，不需要加 `static`。

## 整理学习笔记

在 `content/notes/主题名/index.md` 创建文章，可使用 [学习笔记模板](/templates/note.md)。记录概念、自己的解释、例子、参考资料与尚未解决的问题。

## 草稿与检索

- `draft: true` 的文章不会在正式站点发布。
- `tags` 用于具体算法标签，`categories` 用于大类。
- 搜索支持匹配标题与文章正文。
- 侧栏可以切换浅色与深色，首次访问默认跟随系统。

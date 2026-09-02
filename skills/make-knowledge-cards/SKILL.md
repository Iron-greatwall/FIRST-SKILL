---
name: make-knowledge-cards
description: Convert a local Markdown or TXT article into 5–8 knowledge cards (each with title, core knowledge, simple explanation, and an example or self-question). Use when the user provides a local .md or .txt file and asks to distill it into cards, "知识卡片", "卡片", "card notes", or similar compression tasks.
agent_created: true
---

# Make Knowledge Cards

## Purpose

Transform a local article file (Markdown `.md` or plain text `.txt`) into a set of 5–8 self-contained knowledge cards. Each card captures exactly one high-value knowledge point extracted directly from the article.

## When to Use This Skill

Trigger this skill when **all** of the following are true:

- The user provides an absolute or relative path to a local file.
- The file extension is `.md` or `.txt`.
- The user asks to turn it into "知识卡片", "卡片", "card notes", "extract knowledge", "distill the article", or any equivalent compression request.

Do **not** trigger this skill when:

- The source is a URL, PDF, EPUB, image, or scanned document (this skill does not fetch or parse these).
- The user wants an Anki `.apkg` / `.csv` export or a graphical / visual card layout (this skill outputs plain Markdown only).
- The input is not a local `.md` or `.txt` file.

## Workflow

1. Read the file at the path the user gave.
2. Identify the article's topic and the most important, non-overlapping knowledge points worth keeping. Aim for **5–8 cards**.
3. For each selected point, compose a card with exactly four fields:
   - **标题 (Title)** — 4–12 字，概括该卡承载的动作或结论。
   - **核心知识点 (Core Knowledge)** — 1–2 句，高度凝练原文要点，**严格忠于原文**，不得补写原文没有的事实、数字或来源。
   - **通俗解释 (Simple Explanation)** — 1–3 句，用更生活化的语言重述，让没读过原文的人也能抓住要点；少用黑话。
   - **举例 / 自问 (Example or Self-Question)** — 一个具体例子，或一个能触发回忆的自问自答式问题，二选一。
4. Merge points that the article repeats from different angles; drop filler and transitional sentences.
5. Render the cards as a single Markdown document in the exact format below.

## Output Format

Produce one Markdown document with the cards separated by `---`. No extra prose before or between cards:

```markdown
# 知识卡片：<文章主题>

> 来源：<原文件名>
> 卡片数：N（5 ≤ N ≤ 8）

---

### 卡片 1：<标题>
- **核心知识点**：...
- **通俗解释**：...
- **举例 / 自问**：...

---

### 卡片 2：<标题>
- **核心知识点**：...
- **通俗解释**：...
- **举例 / 自问**：...

...（持续到第 N 张）
```

## Strict Constraints

- **忠于原文 (Faithful to source)** — The "核心知识点" of every card must trace back to a statement actually present in the source article. Never add facts, sources, statistics, or examples that the article does not contain.
- **一卡一意 (One point per card)** — Each card carries exactly one independent knowledge point. Do not bundle two ideas.
- **去重去冗 (Deduplicate)** — If the article makes the same point from multiple angles, merge into a single card. Drop filler.
- **不说糊话 (Clear and unambiguous)** — The "通俗解释" must let a reader who has not seen the article still grasp the point. Avoid jargon unless the article itself defines it.
- **数量约束 (Count constraint)** — Maximum 8 cards, minimum 5. If the article is genuinely too short to support 5 distinct points (under ~200 字), tell the user explicitly that the source is too thin and output whatever cards can honestly be extracted (allowing fewer than 5 in that case).
- **不做越界活 (No scope creep)** — Do not silently start fetching URLs, parsing PDFs, building Anki decks, or producing GUI mockups. If the user asks for any of these, state the limitation and stop.

## After Generating

- Print the full Markdown card output inline in the chat so the user can copy it directly.
- Only write to disk if the user explicitly requests a file path. Default save path when asked: same directory as the source file, named `<原文件名>.cards.md`.
- Never initialize git, commit, or push as part of this skill.
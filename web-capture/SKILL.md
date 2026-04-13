---
name: web-capture
description: Capture a web article or URL into the Obsidian Ideaverse vault as a properly structured knowledge note. Use this skill whenever the user shares a URL, link, or web page and wants to save it, capture it, take notes on it, or add it to their knowledge base. Also use when the user says things like "save this article", "add this to my vault", "capture this page", or pastes a URL and asks to process it. This skill handles the full workflow — fetching, extracting, structuring, tagging, and placing the note in the correct ACE folder.
---

# Web Capture

Capture a web URL into the Ideaverse vault as a structured knowledge note following ACE conventions.

## Workflow

### Step 1: Fetch and Extract

1. Use `WebFetch` to retrieve the page content
2. Extract the following:
   - **Title** — the article/page title (clean it up if needed, remove site names like " | Medium")
   - **Author** — if available
   - **Date published** — if available
   - **Core content** — the main body text, stripped of navigation, ads, footers
   - **Key images** — note any images that are important for context (describe them, reference their original URLs in markdown image syntax). Do NOT download images to the vault unless the user explicitly asks.
   - **Source type** — classify as one of: `article`, `tool`, `person`, `book`, `paper`, `speech`, `video`, `course`, `recipe`, or `reference`

### Step 2: Determine Vault Placement

Based on the source type and content, decide where the note belongs:

| Content type | Folder | `in` property |
|---|---|---|
| Article / blog post / essay | `Atlas/Dots/` | Relevant MOC(s) from `Atlas/Maps/` |
| Tool / product / service | `Atlas/Dots/Things/` | Relevant MOC(s) |
| Person / bio | `Atlas/Dots/People/` | `- "[[People Map]]"` |
| Book | `Atlas/Dots/` | `- "[[Books]]"` and `- "[[Sources]]"` |
| Paper / academic | `Atlas/Dots/` | `- "[[Papers]]"` and `- "[[Sources]]"` |
| Video / speech | `Atlas/Dots/` | `- "[[Speeches]]"` or relevant MOC, and `- "[[Sources]]"` |
| Course | `Atlas/Dots/` | `- "[[Courses]]"` and `- "[[Sources]]"` |
| Recipe / how-to | `Atlas/Dots/` | Relevant MOC(s) |

If the content relates to an active Effort, consider placing it under `Efforts/` instead and linking to the relevant effort note.

### Step 3: Scan Existing Tags

Before assigning tags, scan the vault for existing tags:

```
grep -r "^  - " --include="*.md" Atlas/ Efforts/ Calendar/ | grep "tags:" -A 20 | sort -u
```

Or use the Grep tool to search for tag patterns in frontmatter across the vault. Reuse existing tags that fit. Only create 1-2 new tags if nothing existing is appropriate. Keep tags lowercase, use hyphens for multi-word tags (e.g., `machine-learning`).

### Step 4: Identify the Right MOC(s)

Read `Atlas/Maps/Library.md` to understand the LYT Classification System (000-900). Match the content to the appropriate category and find relevant MOCs. Common MOCs to consider:

- Technology/AI content → look for AI MOC, Systems MOC, or relevant 600-level MOCs
- Psychology/philosophy → Psychology MOC, Philosophy MOC (200-level)
- People → People Map (300-level)
- Business/social → 300-level MOCs
- Language/communication → Language MOC (400-level)
- Science → 500-level MOCs
- Art/recreation → 700-level MOCs
- History/geography → Places MOC, 900-level MOCs

If no specific MOC exists yet, use the broadest fitting parent map and consider whether this content warrants a new MOC (mention this to the user but don't create one automatically).

### Step 5: Build the Note

Structure the note with this format:

```markdown
---
up:
  - "[[<parent map or Home>]]"
related: []
created: <today's date YYYY-MM-DD>
in:
  - "[[<relevant MOC>]]"
tags:
  - <tag1>
  - <tag2>
source: "<original URL>"
author: "<author if known>"
year: <publication year if known>
---

<Concise, well-structured summary of the content. Not a copy-paste — distill the key ideas, arguments, and insights. Use the author's core points but rewrite for clarity and brevity.>

## Key Takeaways

<3-7 bullet points capturing the most important ideas>

## Notes

<Any additional context, connections to other ideas in the vault, or your own observations. Include relevant images using markdown syntax if they add context.>

---

Source: [<title>](<url>)
```

### Writing Style Guidelines

- **Be concise** — distill, don't transcribe. The user wants the essence, not a copy of the article.
- **Preserve key data** — numbers, dates, names, specific claims should be accurate
- **Use wikilinks** — whenever you mention a concept, person, or topic that exists (or should exist) in the vault, use `[[wikilinks]]`. Check existing notes before linking.
- **Structure with headers** — use H2 for major sections, H3 for subsections
- **Tables for comparisons** — if the source compares things, use markdown tables
- **Code blocks for technical content** — preserve code snippets, commands, configs

### Step 6: Confirm with User

Before writing the file, present:
1. The proposed **file name** and **location**
2. The proposed **frontmatter** (tags, MOC links, properties)
3. A brief preview of the note structure

Ask if the user wants any adjustments. Then write the file.

## Edge Cases

- **Paywalled content**: If WebFetch can't access the full article, tell the user and ask them to paste the content instead. Then switch to the create-knowledge workflow.
- **Non-article URLs** (GitHub repos, product pages, documentation): Adapt the note structure — use a "Features" or "Overview" section instead of "Key Takeaways".
- **Multiple URLs**: Process one at a time, confirming each before moving to the next.
- **URL is a PDF**: Use the Read tool if possible, or ask the user to paste key sections.

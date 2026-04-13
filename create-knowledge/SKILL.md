---
name: create-knowledge
description: Create a structured knowledge note in the Obsidian Ideaverse vault from any input — pasted text, rough notes, Notion exports, Apple Notes, OneNote content, clipboard dumps, meeting notes, or freeform context the user provides. Use this skill whenever the user wants to add knowledge to their vault, asks to "create a note", "save this", "add this to my knowledge base", imports content from another app, or provides raw information that should become a vault note. Also triggers when the user pastes a block of text and wants it structured, or says things like "turn this into a note", "clean this up for my vault", or "I learned something about X".
---

# Create Knowledge

Transform any user-provided content into a properly structured Ideaverse vault note following ACE conventions.

## Input Types This Skill Handles

| Input | How to handle |
|---|---|
| **Pasted text / raw notes** | Structure and distill directly |
| **Notion export** (markdown or plain text) | Strip Notion artifacts (database properties rendered as text, toggle syntax, etc.), restructure |
| **Apple Notes paste** | Clean up formatting (often loses structure), rebuild with headers |
| **OneNote paste** | Handle nested bullet madness, flatten and restructure |
| **Meeting notes / transcript** | Extract decisions, action items, key points |
| **Freeform "I learned about X"** | Research the topic if needed, build a proper knowledge note |
| **Multiple fragments** | Ask user if they want one combined note or separate notes |

## Workflow

### Step 1: Understand the Content

Read the user's input carefully. Identify:
- **Subject matter** — what is this about?
- **Note type** — is this a concept (Dot), a source (book/article), an effort (project/task), a log entry (Calendar), or a quick capture (+)?
- **Completeness** — is this ready to be a full note, or does the user want you to expand/research further?

If anything is ambiguous, ask the user before proceeding. Common clarifying questions:
- "Should this be a standalone knowledge note in Atlas, or is it related to one of your Efforts?"
- "Do you want me to keep this concise or expand on the topic?"
- "I see this mentions [X] — should I create separate notes for [X] or keep everything in one note?"

### Step 2: Determine Note Type and Placement

| Note type | Folder | Template basis |
|---|---|---|
| **Concept / idea / knowledge** | `Atlas/Dots/` | Base template |
| **Person** | `Atlas/Dots/People/` | Base template + person fields |
| **Source** (book, paper, video) | `Atlas/Dots/` | Source template |
| **Effort / project** | `Efforts/<intensity>/` | Effort template |
| **Daily / timestamped** | `Calendar/` | Daily Note template |
| **Quick capture / unprocessed** | `+/` | Minimal frontmatter |

### Step 3: Scan Tags and MOCs

Before assigning metadata, scan the vault for context:

1. **Existing tags** — Search frontmatter across the vault for tags. Reuse what fits. Only create 1-2 new tags if nothing existing covers the topic. Tags should be lowercase, hyphenated for multi-word.

2. **Relevant MOCs** — Read `Atlas/Maps/Library.md` for the LYT Classification System. Find the best-fit MOC(s) for the `in` property. If no MOC fits well, use the broadest parent and mention to the user that a new MOC might be warranted as this area grows.

3. **Existing related notes** — Search the vault for notes on similar topics. These become `related` links and `[[wikilink]]` candidates in the body.

### Step 4: Build the Note

Use the appropriate frontmatter template:

**For knowledge notes (Dots):**
```markdown
---
up:
  - "[[<parent map>]]"
related:
  - "[[<related note if any>]]"
created: <today YYYY-MM-DD>
in:
  - "[[<relevant MOC>]]"
tags:
  - <tag1>
  - <tag2>
---

<Well-structured content here>
```

**For source notes:**
```markdown
---
up:
  - "[[<parent map>]]"
related: []
year: <year if known>
encountered: <today YYYY-MM-DD>
in:
  - "[[<collection, e.g. Books, Sources>]]"
tags:
  - <tag1>
source: "<URL or reference if available>"
author: "<author>"
---

<Structured summary and notes>
```

**For effort notes:**
```markdown
---
up:
  - "[[<parent effort or Efforts map>]]"
related: []
created: <today YYYY-MM-DD>
rank: <suggest a rank or ask user>
tags:
  - <tag1>
---

<Project/task content>
```

### Step 5: Structure the Content

Regardless of how messy the input is, the output note should be clean and follow these principles:

- **Distill, don't dump** — rewrite for clarity. Remove filler, redundancy, and fluff from the source material.
- **Lead with the essence** — the first paragraph should capture what this note is about and why it matters. Someone skimming should get the point immediately.
- **Use headers** — H2 for major sections, H3 for subsections. A flat wall of text is never acceptable.
- **Bullet points for lists** — if the content has enumerable items, use bullets or numbered lists.
- **Tables for structured data** — comparisons, specifications, timelines → use tables.
- **Wikilinks generously** — whenever mentioning a concept, person, or topic that exists or could exist in the vault, use `[[wikilinks]]`. Check existing notes before linking to avoid broken links or duplicates.
- **Preserve specifics** — don't lose important data: numbers, dates, names, quotes, code. Summarize the narrative, but keep the facts precise.

### Step 6: Handle Import-Specific Cleanup

**From Notion:**
- Strip database property blocks that render as plain text at the top
- Convert Notion toggle syntax to Obsidian callouts: `> [!note]+ Title`
- Fix internal links (Notion uses UUIDs) — replace with `[[wikilinks]]` or plain text
- Notion's `/code` blocks translate directly to markdown code blocks

**From Apple Notes:**
- Rebuild heading hierarchy (Apple Notes pastes often lose all formatting)
- Convert checklists to markdown `- [ ]` syntax
- Embedded images won't transfer — note them as `[Image: description]` and ask user if they want to add them manually

**From OneNote:**
- Flatten deeply nested bullet hierarchies — OneNote encourages 5+ levels of nesting which is unreadable
- Convert OneNote's tag system to vault-compatible tags
- Strip formatting artifacts (OneNote pastes often include invisible formatting)

**General cleanup:**
- Fix broken links
- Normalize heading levels (don't start with H4)
- Remove trailing whitespace and excessive blank lines
- Ensure consistent list formatting

### Step 7: Confirm with User

Before writing the file, present:
1. **Proposed file name** — a clean, descriptive title
2. **Proposed location** — the folder path
3. **Frontmatter preview** — tags, MOC links, properties
4. **Brief content preview** — first paragraph + section headers

Ask for adjustments. Then write the file.

## Handling Multiple Notes

If the user provides a large dump of content that clearly covers multiple distinct topics:
1. Identify the separate topics
2. Propose splitting into individual notes (with file names and locations)
3. Get user approval on the split
4. Create notes one at a time, linking them to each other via `related` properties and `[[wikilinks]]`

## When the User Says "I Learned About X"

If the user provides a topic but not much content:
1. Ask what specifically they learned or want captured
2. If they want you to build out a knowledge note on the topic, do light research (WebSearch if needed) and draft a note
3. Clearly distinguish between the user's original input and any content you've added, so they can verify accuracy

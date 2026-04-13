# Obsidian ACE Method Skills for Claude Code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of three specialized Claude Code skills designed for managing an [Obsidian](https://obsidian.md) vault structured with Nick Milo's **ACE Method** from the [Ideaverse](https://www.linkingyourthinking.com/).

These skills enable efficient knowledge capture, creation, and maintenance while following the Access-Control-Express (ACE) organizational framework and Linking Your Thinking (LYT) principles.

---

## 📚 What is This?

This repository contains three custom skills that transform Claude Code into a powerful knowledge management assistant for your Obsidian vault:

1. **`web-capture`** — Capture web articles and URLs into properly structured knowledge notes
2. **`create-knowledge`** — Transform any input (notes, imports, rough ideas) into structured vault notes
3. **`vault-review`** — Audit and maintain vault health (orphaned notes, missing metadata, connection opportunities)

---

## 🎯 The ACE Method

The **ACE Method** (Access-Control-Express) is a folder structure system created by Nick Milo for organizing personal knowledge:

- **Atlas/** — Your knowledge base (Maps and Dots)
  - **Maps/** — Maps of Content (MOCs) that organize and link related notes
  - **Dots/** — Individual knowledge notes (concepts, people, sources, things)
- **Calendar/** — Time-based notes and logs
- **Efforts/** — Active projects and initiatives, organized by intensity (On, Ongoing, Simmering, Sleeping)
- **+/** — Quick capture inbox for unprocessed notes
- **Extras/** — Templates, attachments, and supporting files

These skills work within this structure, automatically placing notes in the correct folders and creating appropriate metadata following the [Linking Your Thinking](https://www.linkingyourthinking.com/) framework.

---

## ⚡ Quick Start

### Prerequisites

- [Claude Code](https://claude.ai/claude-code) CLI installed
- An Obsidian vault structured using the ACE Method (Get Ideaverse Light or Pro from https://www.linkingyourthinking.com)
- `CLAUDE.md` and `CONTEXT.md` files in your vault root explaining your structure (see Setup Guide below)

### Installation

1. Clone this repository or download the skills:
   ```bash
   git clone https://github.com/yanickfischer/obsidian-ACE-method-skills.git
   ```

2. Copy the skill folders to your Claude Code skills directory:
   ```bash
   cp -r obsidian-ACE-method-skills/* ~/.claude/skills/
   ```

3. Navigate to your Obsidian vault:
   ```bash
   cd /path/to/your/obsidian/vault
   ```

4. Start using the skills:
   ```bash
   claude /web-capture https://example.com/article
   claude /create-knowledge
   claude /vault-review
   ```

---

## 🛠️ Skills Overview

### 1. `/web-capture`

**Purpose:** Capture web content into your vault as structured knowledge notes.

**Use when:**
- You find an interesting article, blog post, or webpage
- You want to save reference material with proper context
- You need to extract and distill web content into your knowledge system

**What it does:**
- Fetches and extracts content from URLs
- Determines the appropriate vault location (Atlas/Dots, People, Things, etc.)
- Scans existing tags and MOCs for consistency
- Creates properly structured notes with complete frontmatter
- Links to related notes and concepts in your vault

**Example:**
```bash
claude /web-capture https://example.com/article-about-ai
```

---

### 2. `/create-knowledge`

**Purpose:** Transform any raw input into a structured vault note following ACE conventions.

**Use when:**
- Importing content from Notion, Apple Notes, OneNote, or other apps
- You have rough notes or meeting transcripts to process
- Cleaning up pasted text or unstructured information
- Creating notes from "I learned about X" moments

**What it does:**
- Handles multiple input formats (Notion exports, Apple Notes, meeting notes, raw text)
- Cleans up formatting and removes artifacts from other apps
- Determines note type and appropriate folder placement
- Scans for existing tags and MOCs to maintain consistency
- Structures content with headers, bullets, tables, and wikilinks
- Creates or updates frontmatter with proper metadata

**Supported input types:**
- Pasted text / raw notes
- Notion exports (markdown or plain text)
- Apple Notes content
- OneNote dumps
- Meeting notes and transcripts
- Freeform learning captures

---

### 3. `/vault-review`

**Purpose:** Audit and maintain the health of your Obsidian vault.

**Use when:**
- You want to check vault organization and health
- Looking for orphaned or disconnected notes
- Processing items in your inbox (`+/` folder)
- Finding opportunities to link related notes
- Checking for missing metadata or incomplete frontmatter
- Reviewing effort priorities and project status

**What it checks:**
1. **Inbox processing** — Unprocessed captures in `+/`
2. **Orphaned notes** — Notes without upward links or MOC placement
3. **Incomplete frontmatter** — Missing required properties (tags, dates, etc.)
4. **Effort intensity** — Projects that might need status updates
5. **Connection opportunities** — Notes that could be linked but aren't
6. **Tag consistency** — Duplicate or inconsistent tag usage

**Example output:**
```
# Vault Review — 2026-04-13

## Summary
- Inbox: 3 items to process
- Orphaned notes: 2
- Connection opportunities: 5

## Priority Actions
1. Process inbox items
2. Link "Machine Learning" with "AI Ethics" (shared tags)
3. Add MOC links to orphaned notes
```

---

## 📖 Setup Guide

To get the most out of these skills, set up your Obsidian vault with supporting files that help Claude understand your structure:

### 1. Create `CLAUDE.md` in your vault root

This file tells Claude about your vault structure, naming conventions, and organizational principles. Example:

```markdown
# Vault Structure

This is an Ideaverse vault following the ACE Method.

## Folder Organization
- Atlas/Maps/ — Maps of Content (MOCs)
- Atlas/Dots/ — Knowledge notes (concepts, people, sources)
- Efforts/ — Projects organized by intensity
- Calendar/ — Daily notes and time-based logs
- +/ — Inbox for quick captures

## Conventions
- Use lowercase-hyphenated tags
- Always include frontmatter with up, created, in, and tags
- Use wikilinks [[like this]] to connect notes
- Follow the LYT Classification system for MOCs (see Atlas/Maps/Library.md)
```

### 2. Create `CONTEXT.md` in your vault root

Provide additional context about your specific knowledge domains, active projects, and how you prefer to organize information.

### 3. Maintain a Library.md MOC

Keep a `Atlas/Maps/Library.md` file that explains your LYT Classification System (the 000-900 organization categories). This helps the skills place notes correctly.

---

## 🙏 Credits

This project builds on the excellent work of:

### Jake Van Clief — File-System Architecture with Claude
Jake pioneered best practices for using Claude Code with file-based knowledge systems. His approach to structuring repositories and using `CLAUDE.md` files forms the foundation of these skills.

- **Video:** [File-System Architecture with Claude](https://www.youtube.com/watch?v=MkN-ss2Nl10&t=6s)
- **Community:** [Quantum Quill Lyceum on Skool](https://www.skool.com/quantum-quill-lyceum-1116)

### Nick Milo — Ideaverse and the ACE Method
Nick created the ACE Method and the Linking Your Thinking framework, which provides the organizational structure these skills implement.

- **Video:** [The ACE Method Introduction](https://www.youtube.com/watch?v=z4AbijUCoKU&t=392s)
- **Website:** [Linking Your Thinking](https://www.linkingyourthinking.com/)

---

## 💡 Tips for Effective Use

1. **Start with `/web-capture`** when you find something worth saving online
2. **Use `/create-knowledge`** to process rough notes or imports from other systems
3. **Run `/vault-review`** weekly to maintain vault health and discover connections
4. **Let Claude explore your vault first** — it will learn your existing tags, MOCs, and organizational patterns
5. **Always review and confirm** before Claude makes changes — you know your knowledge system best

---

## 🤝 Contributing

Found a bug? Have an improvement? Feel free to:
- Open an issue describing the problem or enhancement
- Submit a pull request with your changes
- Share your experience using these skills

---

## 📄 License

MIT License - feel free to use, modify, and distribute these skills as needed.

---

## 🔗 Related Resources

- [Obsidian](https://obsidian.md) — The knowledge base app
- [Linking Your Thinking](https://www.linkingyourthinking.com/) — Nick Milo's LYT framework
- [Claude Code](https://claude.ai/claude-code) — Anthropic's CLI for Claude
- [Quantum Quill Lyceum](https://www.skool.com/quantum-quill-lyceum-1116) — Community for Claude-powered knowledge work

---

**Happy knowledge building! 🌱**

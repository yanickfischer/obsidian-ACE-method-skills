# Ideaverse — Claude Configuration

This is a personal knowledge management (PKM) vault in Obsidian, built on **Nick Milo's ACE Folder Framework** and **Linking Your Thinking (LYT)** methodology.

## Vault Architecture (ACE)

```
Home.md              ← Main dashboard and launchpad
+/                   ← Inbox: quick captures before processing
Atlas/               ← Knowledge base
  Maps/              ← Maps of Content (MOCs) — navigational hubs
  Dots/              ← Atomic knowledge notes
    People/          ← Notes on people
    Statements/      ← Ideas, claims, opinions, propositions
    Things/          ← Reference material, guides, how-tos, tools
    X/               ← Archive / older collections
  Utilities/Images/  ← Atlas-specific images
Calendar/            ← Time-stamped notes (daily notes, logs, meetings)
Efforts/             ← Projects & tasks, sorted by intensity
  On/                ← Active, high-priority work
  Ongoing/           ← Steady, recurring efforts
  Simmering/         ← Background, low-priority
  Sleeping/          ← Paused / archived
  Notes/             ← Effort-related reference notes
x/                   ← System toolbox (templates, images, excalidraw)
  Templates/         ← Note templates with property presets
```

## Navigation — Use the Maps

When you need to find or understand content in this vault, **read the Maps of Content** in `Atlas/Maps/`. These are the primary navigation layer:

- **Start here**: `Home.md` — links to all major maps
- **All maps index**: `Atlas/Maps/Maps.md` — master list of all MOCs
- **Key maps**:
  - `Library.md` — LYT Classification System (000-900 Dewey-like categories)
  - `Sources.md` — Books, movies, papers, and other source material
  - `Thinking Map.md` — Concepts, mental models, thinking frameworks
  - `People Map.md` — Notes on people
  - `Life Map.md` — Personal grounding and values
  - `Efforts.md` — Current projects and priorities
  - `Study Dashboard.md` — custom-built dashboard for university courses (weekly lecture notes aggregator)
  - `Concepts.md` — Core concept notes
  - `Meta PKM.md` — Best practices for managing this system

Do NOT maintain a separate reference index. The Maps are the reference system.

## Frontmatter Conventions

All notes use YAML frontmatter with these properties:

| Property   | Purpose                          | Example                        |
|------------|----------------------------------|--------------------------------|
| `up`       | Parent note (hierarchy)          | `- "[[Home]]"`                 |
| `down`     | Child notes                      | `- "[[Relate]]"`               |
| `related`  | Lateral connections              | `- "[[Concepts]]"`             |
| `in`       | Collection membership            | `- "[[Maps]]"` or `"[[Books]]"` |
| `created`  | Creation date                    | `2024-09-02`                   |
| `rank`     | Priority (for Efforts)           | numeric value                  |
| `tags`     | Obsidian tags                    | `- x/readme`                   |
| `year`     | Year (for Sources)               | `2020`                         |
| `encountered` | When a source was encountered | date                           |
| `aliases`  | Alternative names                | `- Sources Map`                |

## Templates

Templates live in `x/Templates/` and follow the naming pattern `Template, Properties, [Type].md`:

- **Base** — minimal: `up`, `related`, `created`
- **Effort (Kit)** — adds `rank` for priority sorting
- **Source (Kit)** — adds `year`, `encountered`, `in` for collection tracking
- **Daily Note (Kit)** — sections: Freewrite, Big Things Today, Log
- **Course Weekly (Kit EN)** — weekly lecture note (English): `course`, `semester`, `week`, `status`, sections for takeaways, learning objectives, guiding questions, summary, glossary, quiz, exercises
- **Kurs Weekly (Kit DE)** — same as above in German

## Study System

Weekly lecture notes are managed through a dedicated study pipeline:

- **Dashboard MOC**: `Atlas/Maps/Study Dashboard.md` — Dataview views by course, status, and flat index
- **Weekly notes location**: `Efforts/Notes/{Institution}/{Semester}/{Course}/` (e.g., `Efforts/Notes/MyUni/Spring2026/CS101/`)
- **Frontmatter**: weekly notes use `up`/`in` → `[[Study Dashboard]]`, plus `course`, `semester`, `week`, `status`
- **Status values** (case-sensitive): `"Not Started"` | `"In Progress"` | `"Done"` | `"To Review"`
- **Tags**: `{institution}`, `{course_tag}`, `lecture`, `week-{n}`

## Dataview Queries

Many MOCs use Dataview queries to dynamically list content. These queries pull from:
- Folder paths (e.g., `FROM "Efforts/On"`)
- The `in` property (e.g., `WHERE contains(in, this.file.link)`)
- Tags (e.g., `FROM -#x/readme` to exclude system notes)

Claude cannot execute Dataview — when you need to know what a dynamic list contains, search the relevant folder or property directly.

## Skills (`.claude/skills/`)

This vault has custom skills for common knowledge management workflows. Use them whenever the user's request matches — don't wait for them to invoke by name.

| Skill | Trigger | What it does |
|-------|---------|--------------|
| `web-capture` | User shares a URL or says "save/capture this article" | Fetches URL → structured vault note with frontmatter, tags, MOC placement |
| `create-knowledge` | User pastes content, imports from Notion/Apple Notes/OneNote, or describes something to capture | Any input → structured vault note with ACE conventions applied |
| `vault-review` | User asks "review my vault", "what needs attention", "clean up" | Runs 6 diagnostic checks: inbox, orphans, frontmatter, efforts, connections, tags |

**Key behaviors shared across skills:**
- Always scan existing tags before creating new ones — reuse first
- Always consult `Atlas/Maps/Library.md` to find the right MOC for the `in` property
- Always confirm file name, location, and frontmatter with the user before writing
- Distill content — never copy-paste raw source material into a note

## Rules for Working in This Vault

1. **Respect the ACE structure** — place notes in the correct folder based on their nature
2. **Always add frontmatter** — use the appropriate template properties
3. **Link liberally** — use `[[wikilinks]]` to connect ideas (this is the core of LYT)
4. **Use `up` for hierarchy** — every note should link up to its parent map or Home
5. **Use `in` for collections** — to make notes appear in Dataview-powered MOCs
6. **New MOCs go in `Atlas/Maps/`** — and should have `in: - "[[Maps]]"` in frontmatter
7. **New knowledge notes go in `Atlas/Dots/`** — organized in subfolders by type
8. **Quick captures go in `+/`** — to be processed later via the Add workflow
9. **Efforts use intensity folders** — move files between On/Ongoing/Simmering/Sleeping as priority changes
10. **System files stay in `x/`** — templates, images, config
11. **Use skills proactively** — if the user's request matches a skill, use it without being asked

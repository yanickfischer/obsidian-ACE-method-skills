---
name: vault-review
description: Review and maintain the Obsidian Ideaverse vault — find orphaned notes, missing frontmatter, unprocessed inbox items, broken links, and suggest new connections between notes. Use this skill when the user asks to "review my vault", "clean up my notes", "find orphans", "check my inbox", "what needs attention", "audit my vault", or any request about vault health, maintenance, hygiene, or organization. Also use when the user asks things like "what's in my inbox", "are there notes that need linking", or "help me process my captures".
---

# Vault Review

Audit the Ideaverse vault for health, completeness, and connection opportunities. This is the "gardening" skill — it keeps the knowledge base healthy as it grows.

## Review Modes

The user might ask for a full review or a specific check. Adapt based on their request. If they just say "review my vault" or "what needs attention", run the full review.

### Full Review

Run all checks below and present a summary report with actionable items, organized by priority.

### Targeted Reviews

If the user asks about a specific concern, run only the relevant check(s).

---

## Check 1: Inbox Processing (`+/` folder)

Scan the `+/` folder for unprocessed captures.

For each note found:
1. Read its contents
2. Suggest where it should go (Atlas/Dots, Efforts, Calendar)
3. Suggest tags and MOC links
4. Ask the user if they want you to process it (move + add frontmatter + restructure)

**Report format:**
```
## Inbox (`+/`)
- X note(s) waiting to be processed
  - "Note Title" → suggest: Atlas/Dots/, tags: [x, y], in: [[MOC Name]]
  - ...
```

## Check 2: Orphaned Notes

Find notes that are disconnected from the vault's navigation structure. A note is "orphaned" if it has:
- No `up` property (or `up: []`)
- No `in` property
- Is not linked from any MOC in `Atlas/Maps/`

**How to check:**
1. Scan all `.md` files outside of `Atlas/Maps/`, `x/`, and `+/`
2. Read their frontmatter
3. Flag any with empty or missing `up` and `in` properties
4. For flagged notes, suggest an appropriate `up` link and `in` MOC

**Report format:**
```
## Orphaned Notes
- X note(s) with no upward links
  - "Note Title" (Efforts/Ongoing/...) → suggest: up: [[Efforts]], in: [[Maps]]
  - ...
```

## Check 3: Incomplete Frontmatter

Check all notes for missing required properties. The minimum expected frontmatter depends on note type:

| Note type | Required properties |
|---|---|
| Atlas/Dots notes | `up`, `created`, `tags` |
| Atlas/Maps notes | `up`, `created`, `in` (should include `[[Maps]]`) |
| Effort notes | `created`, `tags`, `rank` |
| Calendar notes | `created` |
| Source notes | `up`, `in`, `year` or `encountered` |

**How to check:**
1. Scan all `.md` files
2. Parse frontmatter
3. Flag notes missing required properties for their type
4. Suggest values for the missing fields

**Report format:**
```
## Incomplete Frontmatter
- X note(s) with missing properties
  - "Note Title" — missing: `up`, `tags` → suggest: up: [[Home]], tags: [topic]
  - ...
```

## Check 4: Effort Intensity Check

Review the Efforts folders to help the user keep priorities current:

1. List all efforts in `Efforts/On/` — are these still actively being worked on?
2. List all efforts in `Efforts/Ongoing/` — should any move to Simmering or On?
3. Check if any efforts in `Efforts/Simmering/` should be archived to `Sleeping/`
4. Flag efforts without a `rank` property

Present a quick status overview and ask the user if anything needs to move.

**Report format:**
```
## Efforts Status
### On (active)
- "Effort Name" (rank: X) — still active?

### Ongoing
- "Effort Name" (rank: X)
- ...

### Simmering
- "Effort Name" — archive to Sleeping?
```

## Check 5: Connection Opportunities

This is the highest-value check. Look for notes that could be linked but aren't.

**How to find connections:**
1. For each note in `Atlas/Dots/`, read its content and tags
2. Search for other notes with overlapping tags or topics
3. Check if they already link to each other via `related` or body `[[wikilinks]]`
4. If not, suggest the connection

Also look for:
- Notes that mention concepts/people by name but don't use `[[wikilinks]]`
- Notes with the same tags that aren't linked
- Notes under the same MOC that might benefit from cross-references

**Report format:**
```
## Connection Opportunities
- "Note A" and "Note B" share tags [x, y] but aren't linked → suggest adding to `related`
- "Note C" mentions "Concept Name" in body text but doesn't wikilink it
- ...
```

## Check 6: Tag Consistency

Review the vault's tag usage for consistency:
- Find tags used only once (might be typos or could be consolidated)
- Find near-duplicate tags (e.g., `ai` vs `AI` vs `artificial-intelligence`)
- Suggest tag consolidation where appropriate

**Report format:**
```
## Tag Health
- X unique tags in use
- Tags used only once: [list] — consider consolidating or removing
- Potential duplicates: `ai` / `artificial-intelligence` → suggest standardizing
```

---

## Output: The Review Report

After running the checks, present a single consolidated report:

```
# Vault Review — <today's date>

## Summary
- Inbox: X items to process
- Orphaned notes: X
- Incomplete frontmatter: X notes
- Efforts needing attention: X
- Connection opportunities: X
- Tag issues: X

## Priority Actions
1. [Most impactful item]
2. [Second most impactful]
3. ...

## Details
[Expand each check's findings here]
```

After presenting the report, ask the user which items they want to address. Then work through them one at a time, confirming each change before making it.

## Important: Don't Auto-Fix

This skill is diagnostic first. Present findings and get user approval before modifying any files. The user knows their vault better than you do — a note that looks "orphaned" might be intentionally standalone, and a "missing" tag might not be needed.

The exception: if the user says "fix everything you find" or "just clean it all up", then proceed with changes, but still summarize what you changed afterward.

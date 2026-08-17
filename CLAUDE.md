# CLAUDE.md — {{PLAYBOOK_TITLE}}

> **Fill in every `{{PLACEHOLDER}}` in this file before you start.** This file is what Claude Code reads automatically whenever you work in this repo, so the more accurate it is, the less you have to re-explain each session. See `resources/SETUP_GUIDE.md` if you haven't set this project up yet.

## PROJECT OVERVIEW

**Title**: {{PLAYBOOK_TITLE}} (e.g. "Hidden Realities in [Your Field]: A Practitioner's Playbook")

**Purpose**: Document real, specific work experiences to build a practical, sellable guide for {{TARGET_AUDIENCE}}.

**Author**: {{YOUR_NAME}}, {{YOUR_ROLE_OR_CREDENTIAL}}

**Timeline**: {{CAPTURE_START_MONTH}} (capture) → {{LAUNCH_MONTH}} (launch)

**Target Audience**: {{TARGET_AUDIENCE_DETAIL}}

**Target Revenue**: {{REVENUE_GOAL}} (optional — delete if you're not selling this)

---

## TECHNICAL ARCHITECTURE

### Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Story Capture** | Markdown (git-tracked) | Lightweight, version-controlled story files |
| **Source Control** | Git + GitHub | Cross-device sync, public portfolio, audience building |
| **Audience Building** | LinkedIn | Weekly teaser posts from real stories |
| **Sales** | {{SALES_CHANNEL}} (e.g. direct email + Gumroad) | Distribution once launched |

### Repo Structure

```
{{PLAYBOOK_SLUG}}/
├── README.md
├── CONTRIBUTING.md
├── LICENSE
├── stories/                     # Weekly story captures
├── chapters/                    # Playbook drafts (start once you have 15-20 stories)
├── teaser-posts/                # LinkedIn post drafts + published/ archive
└── resources/
    ├── story-template.md
    ├── SETUP_GUIDE.md
    ├── ROUTINE_PROMPTS.md
    └── PUBLORA_SETUP.md
```

---

## CORE WORKFLOWS

### WORKFLOW 1: Weekly Story Capture (15-20 min)

As real moments come up in your work: copy `resources/story-template.md` into `stories/YYYY-MM-DD_story-slug.md`, fill in the 7 sections, commit and push.

**Trigger events worth capturing**:
- Discovered something standard guidance doesn't mention
- A task took far longer than expected
- Stakeholder resistance or surprise
- Had to redo something
- Solved a problem creatively
- Framework/textbook guidance didn't match reality

### WORKFLOW 2: Weekly Automated LinkedIn Post

Two automated cloud routines (set up per `resources/SETUP_GUIDE.md`) run every Monday:
1. **{{PLAYBOOK_ROUTINE_NAME}}** — reads `/stories`, picks the strongest not-yet-teased story, drafts a LinkedIn post, commits it to `/teaser-posts`.
2. **{{BRAND_ROUTINE_NAME}}** (optional second stream) — searches the last 7 days of news in {{FIELD}}, drafts a thought-leadership post tied to your broader personal brand, commits to a separate `drafts/` folder in your brand repo if you keep one.

**Neither routine calls Publora directly** — that network path is blocked from Anthropic's cloud sandbox by policy. Drafts get scheduled via Publora from your own local Claude Code session (see `resources/PUBLORA_SETUP.md`). This is a structural fact of how Claude Code cloud routines work, not a bug — don't try to route around it by embedding API keys in the routine prompt.

### WORKFLOW 3: Quarterly Playbook Synthesis (once you have 15-20+ stories)

Ask Claude Code (with this repo open): "Group my stories into chapters, outline the structure, draft chapter 1."

---

## VOICE RULES (apply to every generated post)

- Zero em dashes, en dashes, or double-dashes — use a period or comma instead
- No generic AI vocabulary: leverage, utilize, delve, unlock, harness, foster, robust, seamless, holistic, landscape, ecosystem, paradigm, game-changer, fundamentally
- No "What do you think?" or "Thoughts?" as a closer — end on a specific question or a direct invite (e.g. "DM me")
- 900-1300 characters (LinkedIn's algorithmic sweet spot), short paragraphs, blank line between each idea
- 3-4 hashtags, never fewer than 3, never 5+ (5+ is a documented negative/spam-pattern signal on LinkedIn as of 2026)
- No raw URLs in the post body — use a soft pointer like "links in the comments," and prepare the actual first-comment text separately since Publora can't post it until after the post is live
- At least one specific number or concrete detail per post, never fabricated — pull only from what's actually in the source story

---

## KEY TECHNICAL DECISIONS (reference, adapt or delete as needed)

### Why GitHub over a doc/spreadsheet
Version control, public portfolio effect, cross-device sync, free.

### Why LinkedIn over a website (for now)
Network effects, built-in audience, zero hosting cost. Revisit once you have 400+ engaged followers.

### Why the cloud routine never calls Publora directly
Anthropic's cloud sandbox environment blocks arbitrary outbound API calls by policy — this was confirmed by repeated failures, not assumed. Scheduling happens from a local session instead. If you find old instructions anywhere suggesting the routine should call Publora directly, they're wrong — don't try it again.

---

## VERSION HISTORY

| Date | Update |
|------|--------|
| {{TODAY}} | Initial setup from practitioner-playbook-kit template |

---

## NEXT IMMEDIATE ACTIONS

1. Finish `resources/SETUP_GUIDE.md` end to end
2. Capture your first real story
3. Set up both weekly routines
4. Set up Publora and schedule your first post manually to confirm the pipeline works end to end before trusting it unattended

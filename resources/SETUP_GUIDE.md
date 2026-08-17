# Setup Guide — Start Here

This gets you from "cloned this repo" to "first story captured and weekly LinkedIn automation running" in about 30-45 minutes, spread across a few sessions if you like.

## What you need before starting

- [ ] A GitHub account (free) — [github.com/signup](https://github.com/signup) if you don't have one
- [ ] A Claude Pro, Max, Team, or Enterprise plan — **the free Claude tier does not include Claude Code at all**, and the weekly automated routines specifically require a paid plan. If you're testing this on free, you can still capture stories and draft posts manually with Claude Code's normal chat, you just won't get the unattended weekly automation.
- [ ] A Publora account (free tier: 15 LinkedIn posts/month) — [app.publora.com/signup](https://app.publora.com/signup)
- [ ] A LinkedIn account you're comfortable posting from

---

## Step 1: Make this repo yours (5 min)

```bash
# Clone this template
git clone https://github.com/{{TEMPLATE_OWNER}}/practitioner-playbook-kit.git {{PLAYBOOK_SLUG}}
cd {{PLAYBOOK_SLUG}}

# Point it at your own new GitHub repo instead of the template's
# (create an empty repo first at github.com/new — don't initialize it with a README)
git remote remove origin
git remote add origin https://github.com/{{YOUR_GITHUB_USERNAME}}/{{PLAYBOOK_SLUG}}.git
git branch -M main
git push -u origin main
```

## Step 2: Fill in the placeholders (10 min)

Two files need every `{{PLACEHOLDER}}` replaced with your real details — don't skip the second one, it's easy to miss and it's what actually gets pasted into your routine later:

1. **`CLAUDE.md`** — your name, your field, your target audience, your revenue goal (or delete that line if you're not selling anything). This file is what Claude Code reads automatically every time you work in this folder.
2. **`resources/ROUTINE_PROMPTS.md`** — same details plus your credential/positioning and a signature hashtag. In Step 5 you'll copy these prompts directly into your routine setup, so any `{{PLACEHOLDER}}` left unfilled here gets pasted literally into your live routine.

Also update this file's own placeholders (`{{TEMPLATE_OWNER}}`, `{{PLAYBOOK_SLUG}}`, `{{YOUR_GITHUB_USERNAME}}`) once you've picked names for things.

Commit and push:
```bash
git add CLAUDE.md resources/ROUTINE_PROMPTS.md resources/SETUP_GUIDE.md
git commit -m "Fill in project details"
git push
```

## Step 3: Capture your first story (15-20 min)

```bash
cp resources/story-template.md stories/$(date +%Y-%m-%d)_my-first-story.md
```

Open it, fill in the 7 sections with something real from your own work. Rough is fine. Commit and push.

## Step 4: Set up Publora (5 min)

1. Sign up free at [app.publora.com/signup](https://app.publora.com/signup)
2. In Publora, go to **Channels → Add Channel** and connect your LinkedIn account
3. Once connected, note your **platform ID** — it looks like `linkedin-xxxxxxxxxx` and is shown on the Channels page next to your connected account
4. Go to the **API** section in the sidebar and copy your API key — it looks like `sk_...`
5. Save both somewhere you can paste them into Claude Code when asked (a password manager, not a plaintext file)

**Do not commit these to the repo, ever.** They go directly into a Claude Code conversation or your shell environment, never into a tracked file.

## Step 5: Set up the two weekly cloud routines (10 min)

Open Claude Code (a normal terminal session, on your own machine, logged into your own Claude account), and say:

> "I want to set up a scheduled cloud routine using the /schedule skill. Read resources/ROUTINE_PROMPTS.md in this repo for the two prompts I need, use my real GitHub repo URL, and ask me for anything else you need."

Claude Code will walk you through picking a schedule (the template defaults to Monday mornings), which repo to point each routine at, and will create them under your own Claude account. This has to happen in *your* session, not by copying someone else's routine — routines are tied to whoever's account creates them.

## Step 6: Test the full pipeline once, manually (10 min)

Before trusting the automation unattended, run it through once by hand:

1. Ask Claude Code to draft a post from your first story
2. Give it your Publora API key and platform ID (paste them directly in chat, or `export` them in your shell first)
3. Ask it to schedule the post via Publora, and to **verify** the result with a `get-post` call afterward, not just trust the initial response — Publora's `create-post` endpoint needs an explicit `"status": "scheduled"` field or it silently saves as a draft even when you set a `scheduledTime`. This is a real, confirmed quirk, not a hypothetical — check for it.

Once that works end to end, the weekly routines should run unattended, with two known limits worth understanding upfront:

- **The cloud routine can't call Publora itself** — Anthropic's cloud sandbox blocks that outbound call by policy. The routine drafts and commits only; scheduling happens from your local session (see `resources/PUBLORA_SETUP.md` for exactly how, including a working example script).
- **Reminders and unattended weekly scheduling need your terminal session to stay open** — Claude Code's session-local scheduling (used for warm-up/engagement reminders) doesn't survive closing the terminal. The cloud routines themselves keep drafting regardless; it's specifically the "remind me to engage" and "auto-schedule this week's draft" pieces that need a live session. If that's not workable for you, the fallback is simply asking Claude Code to check and schedule manually once a week, which takes under a minute.

---

## Ongoing rhythm

**Weekly**: Capture 1 story (15-20 min) whenever something real happens worth writing down.

**Every Monday**: Both routines draft automatically. Review, then schedule via Publora (or ask Claude Code to do it).

**Quarterly** (once you have 15-20+ stories): Ask Claude Code to group them into chapters and start drafting the actual playbook.

---

## If something breaks

- **Routine drafted a generic "meta" post instead of a real story**: you've run out of untapped stories. Capture a new one before the next Monday.
- **Post didn't actually schedule despite Claude Code saying it did**: always ask for a `get-post` verification, not just the `create-post` response — see the note in Step 6.
- **Cloud routine tried to call Publora and failed**: expected, per the note in Step 6. Update the routine prompt to stop attempting it if you see this — it should only draft and commit.

# Routine Prompts

Two prompt templates for the `/schedule` skill. Fill in every `{{PLACEHOLDER}}` before creating the routine, and give the finished prompt to Claude Code when it asks what the routine should do.

Both routines **draft and commit only — neither should ever attempt to call Publora directly.** Anthropic's cloud sandbox blocks that outbound network call by policy (confirmed by repeated real failures during this template's development, not a hypothetical). If a routine's own response claims it scheduled something via Publora, don't trust it without an independent `get-post` check from your local session — it's much more likely the call silently failed or was never actually reachable.

---

## Routine 1: Weekly Story Teaser Post

**Suggested schedule**: Monday mornings, your local time.

**Repo**: your playbook repo (this one).

```
You are drafting a LinkedIn teaser post for {{YOUR_NAME}}'s upcoming book "{{PLAYBOOK_TITLE}}"
(real stories from {{YOUR_NAME}}'s work in {{FIELD}}, positioned as practical, not theoretical,
honest about the hidden work that never makes it into official guidance).

IMPORTANT: This routine only drafts and commits. It does NOT call any external API to schedule
or publish the post. Scheduling is handled separately from a local Claude Code session. Your job
ends when the draft is committed and pushed.

DRAFTING TASK:
1. Read every file in /stories to see all captured stories.
2. Read every file in /teaser-posts (including /teaser-posts/published/ if it exists) to see
   which stories/themes have already been turned into posts. Do not repeat a story or angle
   that's already been covered.
3. Pick the strongest not-yet-teased story. If there are zero untapped stories, instead draft a
   "meta" post: a progress update, a value-prop post, or a behind-the-scenes post about the
   capture process -- and clearly flag in your final response that new stories are needed.
4. Draft ONE complete, ready-to-post LinkedIn post using this formula:
   - Hook (1-2 sentences): the false assumption / standard guidance in {{FIELD}}
   - Reality (2-3 sentences): what actually happened, with a specific detail (never fabricate
     anything not in the source story)
   - Lesson (1-2 sentences): the practical takeaway
   - CTA: invite people to DM {{YOUR_NAME}} if they're working through something similar. Vary
     the phrasing week to week, never reuse verbatim.
   Length: 900-1300 characters, short paragraphs, blank line between each idea.
   Rotate the post type week to week: Hidden Reality (assumption vs reality), Lesson Learned,
   Process Insight, Time Investment Reality, Stakeholder Moment, Toolkit Deep Dive, Quick Tip --
   don't repeat the type used in the last 2 posts.
5. Voice rules, no exceptions:
   - Zero em dashes, en dashes, or double-dashes -- use a period or comma instead
   - No generic AI vocabulary (leverage, utilize, delve, unlock, harness, foster, robust,
     seamless, holistic, landscape, ecosystem, paradigm, game-changer, fundamentally)
   - No "What do you think?" or "Thoughts?" as a closer
   - No raw URLs in the post body -- use a soft pointer like "links in the comments"
6. Hashtags: 3-4 total, never fewer than 3, never 5+ (a documented negative/spam-pattern signal
   on LinkedIn). Rotate a different subset each week from relevant tags in {{FIELD}}, always
   including {{SIGNATURE_HASHTAG}}.
7. Produce 1-2 backup angle ideas (one-line hooks, not full posts).
8. Prepare a FIRST-COMMENT text block pointing to {{YOUR_LINKS}} (e.g. portfolio site, product
   page). This is NOT auto-posted -- you post it manually once the main post is live.

REPO RECORD TASK:
9. Write the full draft plus backups plus the first-comment block to
   teaser-posts/YYYY-MM-DD-weekly-teaser.md (today's date). Label it "READY TO POST -- schedule
   via Publora from a local session". Include the intended target slot.
10. Commit and push to main.
11. In your final response, output the complete draft in full, which story it's based on, and
    the backup hooks.
```

---

## Routine 2: Weekly Thought-Leadership Post (optional second stream)

**Suggested schedule**: A different day than Routine 1 (e.g. Monday if Routine 1 is Wednesday), same account, avoids same-day cannibalization on LinkedIn.

**Repo**: your general brand/content repo, if you keep one separate from the playbook repo.

```
You are drafting a ready-to-post LinkedIn post for {{YOUR_NAME}}, {{YOUR_CREDENTIAL}}, positioning
them as {{POSITIONING_STATEMENT}}.

IMPORTANT: This routine only drafts and commits. It does NOT call any external API to schedule
or publish. Scheduling happens from a local Claude Code session afterward.

BRAND CONTEXT:
{{BRAND_CONTEXT}} -- describe your pillars, what you've built/shipped, your voice (direct vs
warm, contrarian vs consensus-building, technical vs plain-language, etc).

DRAFTING TASK:
1. Search the web for news/discussion from the last 7 days relevant to {{FIELD}}.
2. Pick the single strongest angle and draft ONE complete, ready-to-post LinkedIn post. Hard
   rules, no exceptions:
   - Zero em dashes
   - 900-1300 characters, short paragraphs
   - First line is a complete, standalone hook, no throat-clearing
   - At least one specific number and one named real entity (something you've actually built,
     worked on, or a real employer/certification)
   - One moment of genuine vulnerability or a mistake, not a humble-brag
   - Ends on a specific question or a DM invite, never "Thoughts?"
   - No raw URLs in the body
   - No generic AI vocabulary (same list as Routine 1)
3. Hashtags: 3-4, rotating, from tags relevant to {{FIELD}}.
4. 1-2 backup angle ideas.
5. Prepare a FIRST-COMMENT block pointing to {{YOUR_LINKS}}.

REPO RECORD TASK:
6. Write the draft plus backups plus first-comment block to drafts/YYYY-MM-DD-weekly-draft.md.
   Label it "READY TO POST -- schedule via Publora from a local session".
7. Commit and push.
8. Output the full draft, backup hooks, and first-comment text in your final response.
```

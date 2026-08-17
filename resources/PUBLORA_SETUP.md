# Publora Setup & Scheduling Reference

Publora is the service that actually posts to LinkedIn on a schedule. This doc covers account setup and three real, confirmed quirks worth knowing before you rely on it unattended.

## Account setup

1. Sign up free at [app.publora.com/signup](https://app.publora.com/signup) (free tier: 15 LinkedIn posts/month)
2. **Channels → Add Channel** → connect LinkedIn
3. Note your **platform ID** from the Channels page — format: `linkedin-xxxxxxxxxx`
4. **API** section in the sidebar → copy your key — format: `sk_...`

Never commit either value to a repo. Paste them directly into a Claude Code chat, or export them in your own shell:

```bash
export PUBLORA_API_KEY="sk_..."
export LINKEDIN_PLATFORM_ID="linkedin-..."
```

## Three real quirks, confirmed by actual failures during this template's development

### 1. `status: "scheduled"` is required, undocumented at the main endpoint

`POST /api/v1/create-post` will return `"success": true` even if you omit `status`, but the post silently saves as a **draft**, not scheduled — despite a `scheduledTime` being set. This isn't documented on the `create-post` endpoint itself, only mentioned in passing on the `update-post` docs. Always include it explicitly:

```json
{
  "content": "...",
  "platforms": ["linkedin-xxxxxxxxxx"],
  "status": "scheduled",
  "scheduledTime": "2026-08-25T04:00:00.000Z"
}
```

### 2. Always verify with `get-post`, never trust the `create-post` response alone

```bash
curl -s https://api.publora.com/api/v1/get-post/<postGroupId> \
  -H "x-publora-key: $PUBLORA_API_KEY"
```

Check the `status` field is exactly `"scheduled"`. If it's `"draft"`, force it with:

```bash
curl -s -X PUT https://api.publora.com/api/v1/update-post/<postGroupId> \
  -H "x-publora-key: $PUBLORA_API_KEY" -H "Content-Type: application/json" \
  -d '{"status":"scheduled","scheduledTime":"2026-08-25T04:00:00.000Z"}'
```

### 3. Bare Python `urllib`/`requests` calls can get blocked; `curl` works

A request with no real User-Agent header (Python's default) got a 403 from Publora's own layer during testing, while the identical request via `curl` succeeded immediately. If scripting this in Python, either shell out to `curl`, or set a real User-Agent header. The working pattern:

```bash
python3 - << 'PYEOF'
import json
content = open("post.txt").read()
payload = {
    "content": content,
    "platforms": ["linkedin-xxxxxxxxxx"],
    "status": "scheduled",
    "scheduledTime": "2026-08-25T04:00:00.000Z"
}
with open("/tmp/payload.json", "w") as f:
    json.dump(payload, f)
PYEOF

curl -s -X POST "https://api.publora.com/api/v1/create-post" \
  -H "x-publora-key: $PUBLORA_API_KEY" -H "Content-Type: application/json" \
  --data @/tmp/payload.json
```

## Why the cloud routine can't just do this itself

Anthropic's Claude Code cloud sandbox (the environment scheduled routines run in) blocks outbound calls to `api.publora.com` at the network/proxy layer, confirmed by repeated real failures, not assumed. This is a platform-level policy, not something fixable from inside a routine's prompt. The working architecture is:

1. Cloud routine drafts the post, commits it to the repo. No API key involved, nothing to leak.
2. You (or Claude Code from your own local terminal session) read the committed draft and schedule it via Publora, using the pattern above.

If you want this to happen without manually asking each week, Claude Code's session-local `CronCreate` scheduling can do it automatically — but that requires your terminal session to stay open. It's a real limitation, not a guarantee: if you close the terminal, nothing fires. The safe fallback is just asking Claude Code to check and schedule once a week, which takes under a minute once the routine's draft already exists.

## Getting the full account state (no per-post ID needed)

```bash
curl -s "https://api.publora.com/api/v1/list-posts?limit=100&sortBy=scheduledTime&sortOrder=asc" \
  -H "x-publora-key: $PUBLORA_API_KEY"
```

## Checking your LinkedIn connection is still healthy

```bash
curl -s "https://api.publora.com/api/v1/platform-connections" \
  -H "x-publora-key: $PUBLORA_API_KEY"
```

Check `tokenStatus` is `"valid"` and `lastError` is `null`.

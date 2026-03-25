---
name: content-crosspost-ops
description: Evidence-first crossposting workflow for Hermes. Use when adapting posts, threads, demos, videos, or articles across LinkedIn, Threads, Bluesky, Farcaster, and YouTube Community while keeping per-platform copy distinct and verified.
metadata:
  hermes:
    tags: [generated, content, crosspost, workflow, verification]
---

# Content Crosspost Ops

Use this when the user wants Hermes to crosspost or repurpose content across multiple platforms, especially from Telegram-driven publishing requests.

## Skill Stack

Pull these imported skills into the workflow when relevant:
- `content-engine` for platform-native rewrites
- `crosspost` for sequencing and destination-specific adaptation
- `article-writing` when the source asset is long-form
- `video-editing` or `fal-ai-media` when the post should lead with a clip, frame, or visual
- `search-first` before claiming a platform or API supports a format
- `eval-harness` mindset for publish verification and status reporting

## When To Use

- user says `crosspost`, `post everywhere`, `put this on linkedin too`, or similar
- the source asset is an X post/thread, quote tweet, article, demo video, screenshot, or YouTube post
- the destination is a community thread or showcase channel like Discord's `built-with-claude`
- the user asks whether a new destination or post type is supported

## Workflow

1. Read the real source asset and any destination rules first. Do not draft from memory.
   - if the user pasted thread requirements, comply with those requirements before drafting
2. If the request depends on platform capability, API support, or quota behavior, verify it before answering.
   - if the user asks whether PostBridge can handle a destination or format, inspect the real wrapper, configs, or recent publish logs before promising support
   - if the destination is unsupported, say `blocked by unsupported capability` and give the next viable path
3. Extract one core idea and a few specifics. Split multiple ideas into separate posts.
4. Write native variants instead of reusing the same copy:
   - X: fast hook, minimal framing
   - LinkedIn: strong first line, short paragraphs, explicit lesson or takeaway
   - Threads, Bluesky, Farcaster: shorter, conversational, clearly distinct wording
   - YouTube Community: lead with the result or takeaway, keep it media-friendly
5. Prefer native media when the user wants engagement:
   - for quote tweets, articles, or external links, prefer screenshots or media over a bare outbound link when the platform rewards native assets
   - if the user says the demo itself should lead, use the video or a frame from it instead of a generic screenshot
   - for community showcase threads, prefer the strongest demo clip or screenshot pair the user explicitly pointed to
6. Use link placement intentionally:
   - put external links in comments or replies when engagement is the goal and the platform supports it
   - otherwise use a platform-native CTA such as `comment for link` only when it matches the user's instruction
7. Resolve account and auth blockers early for browser-only destinations:
   - for Discord or other browser-only community shares, verify the active account and whether the destination is reachable before spending more turns on extra asset hunting or copy polish
   - verify the active account before typing into a community or social composer
   - if login is blocked by MFA or a missing verification code, use the checked-in helper path instead of ad hoc inline scripting and do at most one focused resend plus one fresh helper check
   - if that still returns no matching code, stop and report `blocked on missing MFA code`
8. Execute in order:
   - post the primary platform first
   - stagger secondary destinations when requested, defaulting to 4 hours apart unless the user overrides it
   - prefer PostBridge for supported platforms, browser flows only when required
9. Verify before claiming completion:
   - capture a returned post ID, URL, API response, or an updated verification log
   - when the user asks `did you do it?`, answer with the exact status for each platform: posted, queued, drafted, uploaded-only, blocked, or awaiting verification
   - record every attempt with `/Users/affoon/.hermes/workspace/content/log_crosspost.py` or `/Users/affoon/.hermes/workspace/content/postbridge_publish.py`
   - if the state is only drafted, uploaded-only, queued, blocked, or pending manual action, report that exact status

## Pitfalls

- do not post identical copy cross-platform
- do not assume platform support without checking
- do not ignore thread rules or platform-specific showcase requirements
- do not call a draft, composer state, or upload step `posted`
- do not keep searching unrelated systems after a login or MFA blocker is already the limiting step
- do not keep refining copy or looking for better assets once auth is the only blocker on a browser-only publish
- do not answer a support question with a guess when the wrapper, logs, or API response can settle it
- do not ignore the user's preference for screenshots or native media over raw links

## Verification

- `/Users/affoon/.hermes/workspace/content/crosspost-verification-latest.md` reflects the latest attempts
- each destination has an ID, URL, or explicit failure reason
- the copy and media logged match what was actually sent

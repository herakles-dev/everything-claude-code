---
name: knowledge-ops
description: Evidence-first memory and context retrieval workflow for Hermes. Use when the user asks what Hermes remembers, points to OpenClaw or Hermes memory, or wants context recovered from a compacted session without re-reading already loaded files.
origin: Hermes
---

# Knowledge Ops

Use this when the user asks Hermes to remember something, recover an older conversation, pull context from a compacted session, or find information that "should be in memory somewhere."

## Skill Stack

Pull these companion skills into the workflow when relevant:
- `continuous-learning-v2` for evidence-backed pattern capture and cross-session learning
- `search-first` before inventing a new lookup path or assuming a store is empty
- `eval-harness` mindset for exact source attribution and negative-search reporting

## When To Use

- user says `do you remember`, `it was in memory`, `it was in openclaw`, `find the old session`, or similar
- the prompt contains a compaction summary or `[Files already read ... do NOT re-read these]`
- the answer depends on Hermes workspace memory, Supermemory, session logs, or the historical knowledge base

## Workflow

1. Start from the evidence already in the prompt:
   - treat compaction summaries and `do NOT re-read` markers as usable context
   - do not waste turns re-reading the same files unless the summary is clearly insufficient
2. Search in a fixed order before saying `not found`:
   - `mcp_supermemory_recall` with a targeted query
   - grep `/Users/affoon/.hermes/workspace/memory/`
   - grep `/Users/affoon/.hermes/workspace/` more broadly
   - `session_search` for recent Hermes conversations
   - grep `/Users/affoon/GitHub/affaans_knowledge_base/` or the OpenClaw archive for historical context
3. If the user says the answer is in a specific memory store, pivot there immediately:
   - `openclaw memory` means favor the historical knowledge base or OpenClaw archive
   - `not in this session` means stop digging through the current thread and move to persistent stores
4. Keep the search narrow and evidence-led:
   - reuse names, dates, channels, account names, or quoted phrases from the user
   - search the most likely store first instead of spraying generic queries everywhere
5. Report findings with source evidence:
   - give the file path, session id, date, or memory store
   - distinguish between a direct hit, a likely match, and an inference
6. If nothing turns up, say which sources were checked and what to try next. Do not say `not found` after a single failed search.

## Pitfalls

- do not ignore a compaction summary and start over from zero
- do not keep re-reading files the prompt says are already loaded
- do not answer from vague memory without a source path, date, or session reference
- do not stop after one failed memory source when others remain

## Verification

- the response names the source store or file
- the response separates direct evidence from inference
- failed lookups list the sources checked, not just a bare `not found`

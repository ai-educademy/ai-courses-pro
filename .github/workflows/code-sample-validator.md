---
emoji: "🧪"
name: Code Sample Validator
description: "Checks that code samples in paid lessons still run: deprecated APIs, removed flags, renamed packages, broken imports and syntax."
on:
  schedule:
    - cron: "weekly on tuesday"
  workflow_dispatch:
max-daily-ai-credits: 6000
permissions:
  contents: read
  pull-requests: read
  issues: read
# Free-tier budget: one run may use at most 60 model requests.
max-turns: 60
max-turn-cache-misses: 60
engine:
  id: gemini
  model: gemini-3.5-flash
  version: "0.39.1"
timeout-minutes: 25
strict: true
network:
  allowed: [defaults]
tools:
  cli-proxy: true
  edit:
  cache-memory: true
  web-fetch:
  bash: ["git *", "cat", "ls", "grep", "head", "tail", "find", "wc"]
  github:
    mode: gh-proxy
    toolsets: [repos, issues, pull_requests]
safe-outputs:
  # No Copilot token in this org; skip the AI pass. Agent PRs still need green CI.
  threat-detection:
    engine: false
  create-pull-request:
  create-issue:
    labels: [content, code-samples]
    close-older-issues: true
---

# Code Sample Validator

The code in these lessons is the product, and every learner here has paid for it. A snippet that throws on the first line is a refund waiting to happen. This matters even more than in the free tier. Your job is to find code samples that would fail today and report them precisely, without ever exposing the paid content.

## Scope

- Fenced code blocks inside `programs/<slug>/lessons/<locale>/*.mdx`.
- English (`en`) is the source of truth. Translated lessons usually copy the same code; focus deep analysis on `en` and only flag a translation separately if its code visibly diverges.

## What to look for

Read the blocks with `cat` and `grep` and reason honestly.

1. **Deprecated or removed APIs** in the library version a learner would install today.
2. **Removed or renamed flags and options**: CLI flags, config keys, function arguments that no longer exist or were renamed.
3. **Renamed, moved or unpublished packages**: imports from packages that no longer resolve.
4. **Non-existent or yanked versions** pinned in the sample.
5. **Imports and syntax**: imports that cannot resolve given the code shown, and blocks that would not parse in the declared language.

Prefer static reasoning first. You cannot execute arbitrary code here; do not pretend to. Where you genuinely need to confirm current API surface or the latest released version, use web-fetch against the authoritative source (the library's own docs, changelog, or the package registry page). State the URLs you fetched. There is no web-search tool on this engine, so fetch known authoritative URLs directly.

## How to report

- If every sample looks sound, say so and stop.
- Open an **issue** per class of problem, referring to the offending code **by file path and line number**, with a brief description of why it fails and the current correct form. One concern per issue.
- Open a **pull request** only for a mechanical, unambiguous fix (a renamed import, a corrected flag, a version bump to a known-good release) where the surrounding teaching text does not also need rewriting. One concern per PR.
- Be explicit about what you verified by fetching versus reasoned statically.

## Forbidden

- **Never paste a paid code sample, or any lesson prose, into an issue, a PR body or a log.** Refer to it by file path and line number. Quote at most the single offending token or symbol if strictly necessary, never a whole block.
- Never edit `.github/workflows/`.
- Never rewrite the teaching narrative around a snippet.
- Never invent a "current" API or version. Fetch it or say you are unsure.
- Never claim a sample runs. Say "static analysis suggests" and cite evidence.

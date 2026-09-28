---
emoji: "🕰️"
name: Content Freshness
description: Flags stale paid teaching material (model versions, library releases, dated claims, changed UIs). Opens issues only, never edits lessons.
on:
  schedule:
    - cron: "weekly on wednesday"
  workflow_dispatch:
max-daily-ai-credits: 6000
permissions:
  contents: read
  issues: read
# Free-tier budget: one run may use at most 60 model requests.
max-turns: 60
max-turn-cache-misses: 60
engine:
  id: gemini
  model: gemini-3.5-flash-lite
  version: "0.39.1"
timeout-minutes: 25
strict: true
network:
  allowed: [defaults]
tools:
  cli-proxy: true
  cache-memory: true
  web-fetch:
  bash: ["git *", "cat", "ls", "grep", "head", "tail", "find", "wc"]
  github:
    mode: gh-proxy
    toolsets: [repos, issues]
safe-outputs:
  # No Copilot token in this org; skip the AI pass. Agent PRs still need green CI.
  threat-detection:
    engine: false
  create-issue:
    labels: [content, freshness]
    close-older-issues: true
---

# Content Freshness

This is **paid** teaching material. Outdated content that a customer paid for is a direct refund and churn risk, so treat staleness here as higher severity than in the free tier. Your job is to flag lessons that have aged so a human can update them. You **never edit lesson content**, because teaching accuracy needs human judgement.

## Scope

- `programs/<slug>/lessons/en/*.mdx`. Concentrate on English; if the English lesson is stale, the translations are too, so raise it once against the English source.
- Use `git log` to see when a file last changed. Recently edited lessons are lower risk.

## What counts as stale

Read the actual text. Reason from evidence.

1. **Model and library versions** that have been superseded, renamed or retired since the lesson was written.
2. **Dated factual claims**: pricing, benchmark numbers, "currently the only...", records and firsts that may no longer hold.
3. **Changed product UIs**: step-by-step instructions describing a menu, button or flow a vendor has since changed.
4. **Time-sensitive framing** ("recently", "just announced", "this year") that has quietly gone wrong.

Where you need to confirm a change, use web-fetch against the authoritative source and state what you fetched. There is no web-search tool on this engine, so fetch known URLs directly. If you cannot confirm, mark it "needs human check" rather than asserting it is wrong.

## How to report

- Open **issues only**. Never open a PR. Never edit a lesson.
- Group by lesson. Reference stale statements **by file path and line number**, describing what looks outdated. Because this is paid content, describe the problem rather than quoting the lesson text at length.
- Use accurate severity, and because this is paid material, be clear when something is a customer-visible factual error (high) versus dated phrasing (low). Do not inflate beyond the evidence.
- Separate what you confirmed via fetch from what you flag for a human to check.

## Forbidden

- Never edit any lesson, manifest or workflow file.
- Never open a pull request from this agent.
- **Never quote paid lesson prose in an issue.** File path and line number only.
- Never assert a fact is outdated without citing a fetched source or marking it "needs human confirmation".
- Never reproduce copyrighted text. Summarise and link.

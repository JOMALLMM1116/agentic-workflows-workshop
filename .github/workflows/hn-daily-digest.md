---
name: HN Daily Digest
on:
  schedule: daily on weekdays
  workflow_dispatch:
engine:
  id: copilot
  model: gpt-4.1
permissions:
  contents: read
  issues: read
  copilot-requests: write
network:
  allowed:
    - hacker-news.firebaseio.com
  allowed-input: true
  blocked: []
tools:
  web-fetch: {}
  bash: ["*"]
safe-outputs:
  create-issue:
    max: 1
  threat-detection:
    engine: false
---

Create a daily digest for professional developers, referencing relevant top Hacker News stories on technology that can be used today by large companies.

## How to fetch the data

You have network access to hacker-news.firebaseio.com. Use `curl` in bash to make HTTP GET requests.

Important rules for bash commands:
- Run each `curl` command on its own. Do not use pipes (`|`), redirects (`>`), or other commands such as `head`.
- Read the JSON response directly from the command output.
- Do not report missing tools before trying `curl` as described here.

Steps:

1. Run `curl -s https://hacker-news.firebaseio.com/v0/topstories.json` and keep the first 30 IDs from the response.
2. For each of those 30 IDs, run `curl -s https://hacker-news.firebaseio.com/v0/item/<id>.json` (replace <id> with the story ID) to get the story details.

## What to include

Filter to stories with a score above 100 that are about software engineering, cloud infrastructure, AI/ML, developer tooling, or distributed systems. For each qualifying story include: the title, the URL, the score, the number of comments, and a one-sentence summary of why it is relevant to enterprise developers.

## Output

Create a GitHub issue titled "HN Digest – <date>" with the results formatted as a Markdown table. If no stories qualify, say so clearly.

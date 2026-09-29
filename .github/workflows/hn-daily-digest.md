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
tools:
  web-fetch: {}
safe-outputs:
  create-issue:
    max: 1
  threat-detection:
    engine: false
---

Create a daily digest for professional developers, referencing relevant top Hacker News stories on technology that can be used today by large companies. Fetch the top 30 stories from the Hacker News API (https://hacker-news.firebaseio.com/v0/topstories.json) and get each story's details from https://hacker-news.firebaseio.com/v0/item/<id>.json. Filter to stories with a score above 100 that are about software engineering, cloud infrastructure, AI/ML, developer tooling, or distributed systems. For each qualifying story include: the title, the URL, the score, the number of comments, and a one-sentence summary of why it is relevant to enterprise developers. Create a GitHub issue titled "HN Digest – <date>" with the results formatted as a Markdown table. If no stories qualify, say so clearly.

---
name: HN Sentiment Analysis
on:
  issue_comment:
    types: [created]
if: startsWith(github.event.comment.body, '/hn-sentiment')
engine:
  id: copilot
  model: gpt-4.1
  args: ["--allow-url=hacker-news.firebaseio.com"]
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
  bash: ["curl:*", "jq:*"]
safe-outputs:
  add-comment:
    max: 1
  allowed-domains:
    - news.ycombinator.com
  threat-detection:
    engine: false
---

The triggering issue comment is:

"${{ steps.sanitized.outputs.text }}"

When a user posts a comment on a GitHub issue that starts with "/hn-sentiment <url>", where <url> is a Hacker News story URL (e.g. https://news.ycombinator.com/item?id=12345), do the following:

1) Extract the Hacker News item ID from the URL in the triggering comment above.
2) Get the story with curl -s https://hacker-news.firebaseio.com/v0/item/<id>.json.
   Its "kids" field contains the IDs of the top-level comments.
3) Fetch up to 50 top-level comments, one at a time, with
   curl -s https://hacker-news.firebaseio.com/v0/item/<comment-id>.json.
   Run each curl command on its own, without pipes or redirects.
4) Perform sentiment analysis on the comment text, classifying each
   comment as Positive, Negative, or Neutral.
5) Produce a summary that shows: the story title, the overall sentiment
   (with count and percentage breakdown in a Markdown table), the top 3
   most positive comments (with excerpt), and the top 3 most negative
   comments (with excerpt).
6) Reply to the original issue comment with the analysis formatted in Markdown.

If no URL is provided or the URL is not a valid Hacker News item,
reply with a helpful error message.

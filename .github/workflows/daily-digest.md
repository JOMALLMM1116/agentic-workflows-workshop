---
name: Daily Digest
on:
  schedule: daily on weekdays
  workflow_dispatch:
engine: copilot
model: auto
permissions:
  contents: read
  issues: read
  pull-requests: read
  copilot-requests: write
safe-outputs:
  create-issue:
    max: 1
---

Create a GitHub issue titled "Daily Digest – <today's date>" summarizing all open issues and open pull requests in this repository.

Requirements:
- Fetch all open issues and all open pull requests in the repository.
- Group all items by label. If an item has no label, place it under "Unlabeled".
- Include a short summary at the top with the total number of open items.
- For each item, include:
  - title
  - author
  - age in days, calculated from when it was opened until today
  - label group
- Sort the groups by label name and the items within each group by oldest first.
- If there are no open issues or pull requests, clearly state that there are no open issues or pull requests.
- Format the final issue as Markdown with clear sections for each label group.
- Create only one issue per run.

The issue body should be easy to scan in GitHub and should highlight the total counts, grouped content, and any empty state clearly.

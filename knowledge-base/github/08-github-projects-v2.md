# GitHub Projects v2

> Populated during Module 12 — GitHub Advanced
> Released: 2022. Replaced the old Projects board with a real PM tool.

## What's New vs Classic Projects

| Feature | Classic | Projects v2 |
|---------|---------|-------------|
| Custom fields | No | Yes (text, number, date, single-select, iteration) |
| Multiple views | No | Yes (board, table, roadmap) |
| Cross-repo | No | Yes |
| Automation | Basic | Full (built-in + Actions) |
| Filtering/grouping | No | Yes |

---

## Views

Three view types per project:
- **Table**: Spreadsheet-style. Great for prioritization and bulk editing.
- **Board**: Kanban-style. Great for sprint tracking.
- **Roadmap**: Gantt-style timeline. Great for planning (requires date fields).

---

## Custom Fields

Add fields via project settings:
- **Text**: free-form notes
- **Number**: story points, effort
- **Date**: deadlines
- **Single select**: status, priority, team
- **Iteration**: sprints with fixed durations

---

## Automation

Built-in automations (no code):
- Auto-add items when opened in repo
- Auto-archive items when closed
- Auto-set status when PR merged

Custom automation via GitHub Actions:

```yaml
# .github/workflows/add-to-project.yml
name: Add new issues to project
on:
  issues:
    types: [opened]
jobs:
  add-to-project:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/add-to-project@v1
        with:
          project-url: https://github.com/orgs/myorg/projects/1
          github-token: ${{ secrets.PROJECT_TOKEN }}
```

---

## gh CLI for Projects

```bash
gh project list                     # list projects
gh project view 1                   # view project 1
gh project item-add 1 --url <issue-url>   # add item
gh project item-list 1              # list items
```

---

## Notes & Gotchas

<!-- Add your own notes here during Module 12 -->

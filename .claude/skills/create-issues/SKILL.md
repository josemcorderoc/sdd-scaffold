---
name: create-issues
description: Create GitHub issues from the task breakdown and add them to a GitHub project
disable-model-invocation: true
argument-hint: "[feature-name]"
---

## Create GitHub Issues from Tasks

Read the task breakdown from `.specs/$ARGUMENTS/tasks.md` and create GitHub issues for each task.

### Steps

1. **Read the tasks file** at `.specs/$ARGUMENTS/tasks.md`
2. **Create labels** for each phase (e.g., `phase:1-bootstrap`, `phase:2-domain`)
3. **For each task**, create a GitHub issue:
   - Title: task ID and title (e.g., "T-1a: Initialize repository and Docker Compose")
   - Label: the phase label
   - Body: phase, dependencies, description summary, and acceptance criteria as checkboxes
   - Use `gh issue create --title "..." --label "..." --body "..."`
4. **Add to project** (if a GitHub project exists):
   - List projects: `gh project list --owner OWNER --format json`
   - Add each issue: `gh project item-add PROJECT_NUMBER --owner OWNER --url ISSUE_URL`
5. **Report** the list of created issues with their numbers

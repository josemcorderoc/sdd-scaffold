---
name: create-issues
description: Create GitHub issues from the task breakdown with traceability labels
disable-model-invocation: true
argument-hint: "[feature-name]"
---

## Create GitHub Issues from Tasks

Read the task breakdown and specs to create fully-traced GitHub issues with traceability labels.

### Steps

1. **Read the specs:**
   - `.specs/$ARGUMENTS/tasks.md` — task list
   - `.specs/$ARGUMENTS/requirements.md` — FR/NFR/EC IDs
   - `.specs/$ARGUMENTS/design.md` — DD IDs

2. **Create labels** (if they don't exist):
   - Phase labels: `phase:1-bootstrap`, `phase:2-domain`, etc.
   - Requirement labels: `req:FR-001`, `req:FR-002`, etc. (one per requirement referenced by any task)
   - Design decision labels: `dd:DD-001`, `dd:DD-002`, etc.
   - NFR labels: `nfr:NFR-001`, etc.
   - Edge case labels: `ec:EC-1`, etc.
   - Use `gh label create "<label>" --force` for each

3. **For each task**, create a GitHub issue:
   - Title: `T-XX: <title>`
   - Labels: phase label + all applicable `req:`, `dd:`, `nfr:`, `ec:` labels
   - Body:
     ```
     **Phase:** <phase name>
     **Dependencies:** <task IDs or "none">

     ## Description
     <task description summary>

     ## Acceptance Criteria
     - [ ] ...
     ```
   - Use `gh issue create --title "..." --label "phase:...,req:FR-001,dd:DD-003,..." --body "..."`

4. **Add to project** (if a GitHub project exists):
   - List projects: `gh project list --owner OWNER --format json`
   - Add each issue: `gh project item-add PROJECT_NUMBER --owner OWNER --url ISSUE_URL`

5. **Report** the list of created issues with their numbers and labels

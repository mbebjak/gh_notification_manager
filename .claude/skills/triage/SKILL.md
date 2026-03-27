---
name: triage
description: Triage GitHub notifications — apply auto-done rules and present remaining for review.
---

# Triage Notifications

## Steps

1. Fetch unread notifications:
   ```bash
   gh api '/notifications?per_page=100' --paginate
   ```

2. Read `rules.md` from the project root to load the auto-done rules.

3. For each notification, evaluate all rules. Some rules require fetching extra data (PR details, reviews, labels) via `subject.url`. Batch these efficiently.

4. Show results in three sections:

   **Merged PRs** — PRs that were merged. Show a condensed summary for each (extract what was done from the PR body, skip checklists/testing/screenshots/boilerplate):
   ```
   ## Merged PRs

   - **LiveAgent#21996** refactor: Remove legacy La_Db_CannedMessage — by @author
     Removed deprecated CannedMessage classes, migrated to new template system.
   ```

   **Auto-done** — other notifications that matched a rule, grouped by rule name:
   ```
   ## Auto-done

   ### Deploy PRs
   - deploy(la-server/dev): 5.63.3.20260327... (QuCloud-provisioning)

   ### Draft PRs
   - Fix CrmIntegration extension (LiveAgent)
   ```

   **Needs attention** — remaining notifications:
   ```
   ## Needs attention

   | # | Repo | Title | Reason |
   |---|------|-------|--------|
   | 1 | LiveAgent | Add Instant message UI... | review_requested |
   ```

5. Ask: "Mark auto-done notifications as done?" Before marking, wait for confirmation.

$ARGUMENTS

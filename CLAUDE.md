# Notifications Manager

Automation for managing GitHub notifications for `mbebjak`.

## Goal

Fetch all GitHub notifications that are not marked as done, apply rules to auto-mark some as done, and present the rest for manual review.

## Technical Details

### Auth

- Uses `gh` CLI (authenticated as `mbebjak`)
- Token must have `notifications` scope (in addition to `repo`, etc.)
- Check with: `gh auth status`

### API Commands

**Fetch unread notifications (default, matches blue dot in UI):**
```bash
gh api '/notifications?per_page=100' --paginate
```

**Fetch all notifications including read:**
```bash
gh api '/notifications?all=true&per_page=100' --paginate
```
Note: `all=true` returns both read and unread notifications. This returns more than what the GitHub web UI inbox shows — it includes old notifications that were never explicitly marked as done. There is no API filter for "inbox only". For practical purposes, prefer fetching unread only.

**Mark a notification as done (removes from inbox):**
```bash
gh api --method DELETE /notifications/threads/{thread_id}
```
Returns `204 No Content` on success.

**Mark a notification as read (keeps in inbox, removes blue dot):**
```bash
gh api --method PATCH /notifications/threads/{thread_id}
```

### Notification Fields

Each notification has:
- `id` — thread ID (used in API calls)
- `reason` — why you got it: `review_requested`, `author`, `team_mention`, `mention`, `subscribed`, `comment`, `ci_activity`
- `unread` — boolean
- `subject.title` — PR/issue title
- `subject.type` — `PullRequest`, `Issue`, `CheckSuite`
- `subject.url` — API URL (extract PR/issue number from last path segment)
- `repository.full_name` — e.g. `QualityUnit/LiveAgent`

### Triage Behavior

- **Always re-fetch PR details** when evaluating rules — never reuse data from earlier in the conversation. PR state (draft, labels, reviewers) changes frequently.

### Auto-Done Rules

Rules are defined in natural language in `rules.md`. Read that file and apply each rule to every notification. If any rule matches, mark the notification as done.

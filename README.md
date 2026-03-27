# GitHub Notifications Manager

CLI automation for triaging GitHub notifications. Fetches all not-done notifications, auto-marks noise as done based on configurable rules, and presents the rest for review.

## Prerequisites

- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated
- Token must include the `notifications` scope:
  ```bash
  gh auth refresh -h github.com -s notifications
  ```

## How it works

1. Fetches all notifications not marked as done via the GitHub API
2. Applies rules from `rules.json` in order — first match wins
3. Notifications matching a `"done"` rule are automatically removed from inbox
4. Remaining notifications are presented for manual review

## Rules

Rules are defined in [`rules.json`](rules.json). Each rule has:

| Field | Description |
|-------|-------------|
| `name` | Human-readable rule name |
| `description` | Why this rule exists |
| `action` | What to do: `"done"` removes from inbox |
| `match` | Object of field/regex pairs to match against notification fields |

### Available match fields

- `subject.title` — PR/issue title (regex)
- `subject.type` — `PullRequest`, `Issue`, `CheckSuite`
- `reason` — `review_requested`, `author`, `team_mention`, `mention`, `subscribed`, `comment`, `ci_activity`
- `repository.full_name` — e.g. `QualityUnit/LiveAgent`

### Default rules

| Rule | Matches | Action |
|------|---------|--------|
| Deploy PRs | Title starts with `deploy(` | done |
| CI activity | Type is `CheckSuite` | done |
| CI activity by reason | Reason is `ci_activity` | done |
| Subscribed only | Reason is `subscribed` | done |

## Usage

Run with Claude Code from this directory:

```
claude
```

Then ask to process notifications, e.g.:
- "fetch and triage my notifications"
- "apply rules and show me what's left"
- "mark all deploy PRs as done"

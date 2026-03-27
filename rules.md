# Auto-Done Rules

Mark a notification as done if ANY of the following rules match.

## Draft PRs

If the notification is about a PullRequest that is currently in draft state, mark it as done — unless the reason is `mention` or `team_mention` (i.e., you were explicitly mentioned). To check draft status, fetch the PR details from `subject.url`.

## PRs past review stage

If a PR has labels `testing:ready` or `integration-test:ready`, it has moved past code review. Mark the notification as done — unless the reason is `mention` (someone explicitly mentioned me). My review is already done on these; I don't care about further rebases, test results, or status changes.

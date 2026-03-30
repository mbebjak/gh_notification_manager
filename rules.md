# Auto-Done Rules

Mark a notification as done if ANY of the following rules match.

## Draft PRs

If the notification is about a PullRequest that is currently in draft state, mark it as done — unless the reason is `mention` or `team_mention` (i.e., you were explicitly mentioned). To check draft status, fetch the PR details from `subject.url`.

## Deploy PRs

Routine automated deploy PRs — title starts with `deploy(`. Always mark as done.

## Dependency bump PRs

Automated dependency update PRs — title starts with `build(deps)`. Always mark as done — unless the reason is `mention` or `team_mention` (i.e., explicitly asked to look).

## Merged PRs

If a PR has been merged (`merged` is true), mark the notification as done. Before marking, collect these PRs and show a summary with: repo, PR title, author, and a condensed description of what was done (extract from PR body — skip checklists, testing steps, screenshots, and boilerplate).

## PRs past review stage

If a PR has labels `testing:ready` or `integration-test:ready`, it has moved past code review. Mark the notification as done — unless the reason is `mention` (someone explicitly mentioned me). My review is already done on these; I don't care about further rebases, test results, or status changes.

## Already covered by aharsani

If `aharsani` has already submitted a review on the PR (check `/pulls/{number}/reviews`), and I (`mbebjak`) am NOT in `requested_reviewers`, mark as done. Being only in `requested_reviewers` without a submitted review doesn't count — I still want to see those.

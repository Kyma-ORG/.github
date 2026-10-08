# Contributing — repository & branch policy

This applies to **every repository** in the organization.

## Two-branch model

Every repository has **at most two branches**:

| Branch | Role |
|---|---|
| `main` | Default branch. Always stable and verified. The only permanent branch. |
| `dev` | The single working branch. **Fixed name**: `dev` — never `develop`, never a per-task name. |

A repository is **main-only** when no work is in progress. `dev` exists only while it is needed, and is not mandatory.

## Rules

1. `main` is the default branch **everywhere**. `master` is not allowed — rename it to `main`.
2. All code work happens on `dev`.
3. **As soon as the code is good and verified, merge `dev` → `main` immediately.** No deferred merges.
4. If `dev` stays open, it is **continued** under the same name for the next change; it is closed only when the work is finished.
5. **No per-task branches.** `feat/…`, `fix/…`, `issue-number/…` branches must be merged and deleted **within the same task**. Never leave a branch open past the work that created it.
6. A branch opened by a tool (e.g. `dependabot/…`) is removed as soon as its pull request is merged or closed.
7. A branch is deleted **only after** its work is in `main` (or in `dev`). Never delete unmerged work.
8. Changes are proposed with a **Pull Request** into `main`; the PR is merged, then its branch is deleted.

## Why

One stable branch keeps the default branch honest and deployable. One working branch keeps parallel work from fragmenting into dozens of stale branches. Short-lived branches mean `main` always reflects reality.

# Contributing — repository & branch policy

This applies to **every repository** in the organization.

## Two-branch model

Every repository has **at most two live branches**:

| Branch | Role |
|---|---|
| `main` | Default branch. Always stable and verified. The only permanent branch. Work reaches it **only by merge**. |
| `issue-<NN>-<slug>` | The working branch **of issue `#NN`**. One branch per issue, and the **same name** in every repository the issue touches (e.g. `issue-46-github-hardening`). |

A repository with no work in progress is **main-only**. There is **no fixed working branch**:
`dev` is not allowed — the working branch *is* the issue.

## Rules

1. `main` is the default branch **everywhere**. `master` is not allowed — rename it to `main`.
2. **No direct pushes to `main`.** Work reaches `main` only through a pull request merge.
3. **As soon as the issue branch is born, its pull request is born** (`gh pr create --draft`).
4. All work happens on the issue branch `issue-<NN>-<slug>`; a branch is deleted **only after** its work is in `main` (PR merged with `--delete-branch`).
5. **Closing the pull request closes the issue**: merge the PR → close the issue → set the board Status to `Done`.
6. A multi-repo issue has **one branch and one PR per repository**, all with the same `issue-<NN>-<slug>` name; all PRs are closed, then the issue.
7. Work in progress is limited to **one issue at a time** (WIP = 1): the next issue's branch is not opened until the current PR is closed.
8. A branch opened by a tool (e.g. `dependabot/…`) is removed as soon as its pull request is merged or closed.
9. Never delete a branch with unmerged work, and never merge with a red check.

## Why

One stable branch keeps the default branch honest and deployable. One branch per issue makes it
obvious at a glance what is being worked on, and the branch disappears together with the issue it
belongs to. Short-lived branches mean `main` always reflects reality.

Full process (Italian): `Origami-WEC/organization` → `docs/processi/lavoro.md` §1.

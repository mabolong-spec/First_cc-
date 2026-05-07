---
name: rewind
description: >-
  Reverts or rolls back recent edits and agent-driven changes using git or
  explicit file restore, with a short impact summary. Use when the user types
  /rewind, says "rewind", "undo the last changes", "restore before the last
  edit", or wants to go back to a known-good state after agent work.
disable-model-invocation: true
---

# /rewind

## Goal

Move the working tree (or named paths) back toward a previous state. Prefer **verifiable, reversible** steps. Do not delete or rewrite history the user has not agreed to.

## When `/rewind` is underspecified

1. **Scope**: whole repo, current file only, or a list the user names.
2. **Target state**: "last commit", "before this chat", "as in `main`", or a **commit hash** if they have git.
3. If still unclear, ask one short question; if they say "just undo everything you did", treat that as **revert uncommitted changes** in the current session’s touched files (discover via `git status` / `git diff` when available).

## Git available (check with `git rev-parse --is-inside-work-tree` or try `git status`)

1. Show **what would change** first: `git status` and, for the relevant paths, `git diff` (or `git diff --stat`).
2. **Uncommitted reversions** (most common):
   - Single file or folder: `git restore <paths>` (or `git checkout -- <paths>` on older git).
   - All tracked files: only after explicit user confirmation; then `git restore .` or restore per path list.
3. **Committed work** the user wants dropped: never `reset --hard` without **explicit** confirmation naming the risk. Prefer `git revert <commit>` for shared branches, or document consequences before `git reset`.
4. **Untracked files** the agent created: list with `git status -u`; remove only with confirmation (`git clean` is destructive—warn first).

## No git or non-git files

- Use the editor’s **Local History** / timeline if the environment supports it (describe steps for the user if the agent cannot invoke it).
- If only one file matters and content was in an earlier message, recover from the transcript only if the user asks and copyright permits.

## Safety

- **Never** run `git clean -fd`, `git reset --hard`, or branch-delete without the user clearly opting in.
- After operations, give a **brief summary**: paths affected, whether anything remains dirty, and suggested next check (e.g. run tests).

## If the user later specifies custom /rewind rules

Follow any **verbatim** instructions they give in the same request or in a project requirements file; those override the defaults above for that session.

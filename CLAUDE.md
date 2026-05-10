# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A single-file Pomodoro timer (番茄钟) web app — `pomodoro.html`. No build step, no dependencies, no package manager.

## How to open

```powershell
Start-Process "E:\Cursor\First_cc\pomodoro.html"
```

## Architecture

Everything lives in `pomodoro.html` — inline CSS (custom properties for theming, dark palette) and vanilla JS (no framework).

- **Modes**: focus (25min), short break (5min), long break (15min) — defined in the `MODES` object.
- **Timer**: `setInterval`-based countdown, SVG ring progress indicator. `tick()` decrements `remainingSeconds`, `onTimerComplete()` plays a chime and increments session stats on focus completion.
- **Persistence**: `localStorage` keys — `pomo_sessions`, `pomo_focus_min`, `pomo_tasks`, `pomo_app_opacity_pct`.
- **Tasks**: Simple todo list embedded below the timer — add/toggle/delete with localStorage backing.
- **Keyboard shortcuts** (when not focused on an input): `Space` toggle timer, `R` reset, `1`/`2`/`3` switch modes.
- **Notifications**: Web Audio API — three-note ascending chime (C5-E5-G5 sine waves).

No routing, no state management library — all state is module-scoped variables at the top of the `<script>` block.


<!-- BEGIN BEADS INTEGRATION v:1 profile:minimal hash:7510c1e2 -->
## Beads Issue Tracker

This project uses **bd (beads)** for issue tracking. Run `bd prime` to see full workflow context and commands.

### Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work
bd close <id>         # Complete work
```

### Rules

- Use `bd` for ALL task tracking — do NOT use TodoWrite, TaskCreate, or markdown TODO lists
- Run `bd prime` for detailed command reference and session close protocol
- Use `bd remember` for persistent knowledge — do NOT use MEMORY.md files

**Architecture in one line:** issues live in a local Dolt DB; sync uses `refs/dolt/data` on your git remote; `.beads/issues.jsonl` is a passive export. See https://github.com/gastownhall/beads/blob/main/docs/SYNC_CONCEPTS.md for details and anti-patterns.

## Session Completion

**When ending a work session**, you MUST complete ALL steps below. Work is NOT complete until `git push` succeeds.

**MANDATORY WORKFLOW:**

1. **File issues for remaining work** - Create issues for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **PUSH TO REMOTE** - This is MANDATORY:
   ```bash
   git pull --rebase
   git push
   git status  # MUST show "up to date with origin"
   ```
5. **Clean up** - Clear stashes, prune remote branches
6. **Verify** - All changes committed AND pushed
7. **Hand off** - Provide context for next session

**CRITICAL RULES:**
- Work is NOT complete until `git push` succeeds
- NEVER stop before pushing - that leaves work stranded locally
- NEVER say "ready to push when you are" - YOU must push
- If push fails, resolve and retry until it succeeds
<!-- END BEADS INTEGRATION -->

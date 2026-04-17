# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

`wt` is a zsh function for managing git worktrees with minimal friction. The entire implementation is a single zsh function in the `wt` file. It uses the same worktree conventions as Claude Code for interoperability.

## Testing Changes

There is no test suite. To test changes:
1. Source the modified function: `source ./wt`
2. Test in a git repository with existing worktrees

## Architecture

The `wt` file contains a single zsh function with nested helper functions:

- `_wt_random_name` - Generates random adjective-animal names for fallback naming
- `_wt_find_by_branch` - Locates worktree directory by branch name (uses awk)
- `_wt_get_branch` - Gets branch name for a worktree path (uses awk)
- `_wt_sorted_paths` - Returns real worktree paths under `$wt_base` that are on a branch, sorted oldest-first by mtime
- `_wt_resolve_target` - Resolves a target (name, number, branch) to a worktree path
- `_wt_go` - Core navigation logic for switching between worktrees
- `_wt_usage` - Prints usage information

## Key Conventions

- Worktrees are stored in `<repo>/.claude/worktrees/<name>/`
- Branch naming: `worktree-<name>` (e.g., `wt -b foo` creates branch `worktree-foo`)
- Numeric indices (`wt 1`, `wt 2`) come from `_wt_sorted_paths`: real worktrees under `.claude/worktrees/` that are on a branch, sorted by directory mtime ascending (oldest = `[1]`). New worktrees always get the next unused number; creation never shifts existing numbers. Removal shifts down numbers above the removed one.
- Listing displays home `[0]` at the top, then worktrees in ascending number order.
- Detached-HEAD worktrees and stale directories (not registered with git) are filtered out of both listing and numeric resolution. They are still reachable by name or branch.
- `wt HEAD` uses `git worktree add --force` to create a worktree sharing the current branch with the caller's directory.
- `$_WT_PREV` global tracks previous worktree for `wt -` navigation

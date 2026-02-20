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
- `_wt_resolve_target` - Resolves a target (name, number, branch) to a worktree path
- `_wt_go` - Core navigation logic for switching between worktrees
- `_wt_usage` - Prints usage information

## Key Conventions

- Worktrees are stored in `<repo>/.claude/worktrees/<name>/`
- Branch naming: `worktree-<name>` (e.g., `wt new foo` creates branch `worktree-foo`)
- Numeric indices (`wt 1`, `wt 2`) are assigned by alphabetically sorting worktree directory names
- `$_WT_PREV` global tracks previous worktree for `wt -` navigation

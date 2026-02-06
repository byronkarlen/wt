# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

`wt` is a zsh function for managing git worktrees with minimal friction. The entire implementation is a single zsh function in the `wt` file.

## Testing Changes

There is no test suite. To test changes:
1. Source the modified function: `source ./wt`
2. Test in a git repository with existing worktrees

## Architecture

The `wt` file contains a single zsh function with nested helper functions:

- `_wt_config_section` - Parses `.worktree` config file sections
- `_wt_copy_files` - Copies files to new worktrees based on `[copy]` config
- `_wt_run_hooks` - Executes commands from `[setup]` and `[init]` config sections
- `_wt_next_dir` - Finds next available worktree directory number
- `_wt_find_by_branch` - Locates worktree directory by branch name (uses awk)
- `_wt_get_branch` - Gets branch name for a worktree path (uses awk)
- `_wt_go` - Core navigation logic for switching between worktrees
- `_wt_usage` - Prints usage information

## Key Conventions

- Worktrees are stored in `<repo-parent>/<repo-name>-worktrees/<repo-name>-worktree-N/`
- `$_WT_PREV` global tracks previous worktree for `wt -` navigation
- The `.worktree` config file uses INI-style sections: `[copy]`, `[setup]`, `[init]`
- `$WT_DIR` is substituted in hook commands with the actual worktree path

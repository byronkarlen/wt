# wt

A zsh function for managing git worktrees with minimal friction.

## Features

- Short numeric aliases for worktrees (`wt 1`, `wt 2`, etc.)
- `wt 0` or `wt home` to return to main repo
- `wt -` to jump to previous worktree
- Auto-creates worktree when navigating to an existing branch
- Configurable file copying and hooks via `.worktree` config
- `wt status` overview of all branches and worktrees

## Recommended Installation

1. Copy `wt` to your zsh functions directory:
   ```bash
   cp wt ~/.zsh/functions/
   ```

2. Source it in your `.zshrc`:
   ```bash
   source ~/.zsh/functions/wt
   ```

## Usage

```
wt [go] <#|branch>   go to worktree (creates if branch exists)
wt [go] 0|home       go to main repo
wt [go] -            go to previous worktree
wt new <branch>      create new branch and worktree
wt rm <#|branch>     remove worktree
wt rm -b <#|branch>  remove worktree and delete branch
wt list              list worktrees (raw)
wt status            show branches and worktree status
```

### Examples

```bash
wt new feature-x     # Create new branch and worktree
wt 1                 # Go to worktree 1
wt feature-x         # Go to worktree by branch name
wt 0                 # Go back to main repo
wt -                 # Go to previous worktree
wt rm 1              # Remove worktree 1
wt rm -b feature-x   # Remove worktree and delete branch
wt status            # Show all branches and their worktree numbers
```

## Directory Structure

```
parent/
  project/                    # Main repo (home, [0])
  project-worktrees/
    project-worktree-1/       # [1]
    project-worktree-2/       # [2]
```

## Configuration

Create a `.worktree` file in your repo root:

```ini
[copy]
# Files to copy when creating a new worktree
.env
**/.claude/settings.local.json

[setup]
# Commands run from parent dir after worktree creation
# $WT_DIR is the path to the new worktree
mise trust "$WT_DIR"

[init]
# Commands run inside the new worktree after creation
npm install
```

### Sections

- **[copy]** - Glob patterns for files to copy from home repo to new worktrees. Supports `**` for recursive matching.
- **[setup]** - Commands run from the parent directory after `git worktree add`. Use `$WT_DIR` to reference the new worktree path. Useful for `mise trust`, `direnv allow`, etc.
- **[init]** - Commands run inside the new worktree. Useful for `npm install`, `bundle install`, etc.

> **Note:** You can globally ignore `.worktree` files by adding them to `~/.config/git/ignore`.

## Requirements

- zsh
- git

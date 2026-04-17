# wt

A zsh function for managing git worktrees with minimal friction. Compatible with [Claude Code](https://claude.ai/code) worktree conventions.

## Features

- Named worktrees stored in `.claude/worktrees/<name>/`
- Short numeric aliases for worktrees (`wt 1`, `wt 2`, etc.)
- `wt 0` or `wt home` to return to main repo
- `wt -` to jump to previous worktree
- Auto-creates worktree when navigating to an existing local or remote branch
- `wt HEAD` creates a worktree sharing the current branch with your current directory

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
wt                        list worktrees
wt <#|name|branch|HEAD>   go to worktree (create if missing; HEAD shares current branch)
wt -b <name>              create worktree at .claude/worktrees/<name>/ on branch worktree-<name>
wt -                      go to previous worktree
wt rm <#|name|branch>     remove worktree
wt rm -b <#|name|branch>  remove worktree and delete branch
```

### Examples

```bash
wt -b feature-x      # Create .claude/worktrees/feature-x/ with branch worktree-feature-x
wt 1                 # Go to worktree numbered [1]
wt feature-x         # Go to worktree by name (auto-creates from local or remote branch)
wt origin-branch     # Creates worktree tracking origin/origin-branch if it exists
wt HEAD              # Create worktree for current branch (shared with current dir)
wt 0                 # Go back to main repo
wt -                 # Go to previous worktree
wt rm 1              # Remove worktree numbered [1]
wt rm -b feature-x   # Remove worktree and delete branch
```

## Directory Structure

```
project/                          # Main repo (home, always [0])
  .claude/
    worktrees/
      feature-x/                  # (branch: worktree-feature-x)
      fix-bug/                    # (branch: worktree-fix-bug)
```

Numeric indices are assigned by creation time (oldest = `[1]`, newer = higher numbers), so a new worktree never changes existing numbers. The listing shows home (`[0]`) at the top, then worktrees in ascending number order. Removing a worktree shifts down the numbers above it; the relative creation order is preserved.

## Claude Code Interop

`wt` uses the same directory layout as Claude Code's built-in worktree support (`claude -w`):

- Worktrees live in `.claude/worktrees/<name>/`
- Branches are prefixed `worktree-<name>`

Worktrees created by either tool are visible to both.

## Requirements

- zsh
- git

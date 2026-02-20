# wt

A zsh function for managing git worktrees with minimal friction. Compatible with [Claude Code](https://claude.ai/code) worktree conventions.

## Features

- Named worktrees stored in `.claude/worktrees/<name>/`
- Short numeric aliases for worktrees (`wt 1`, `wt 2`, etc.)
- `wt 0` or `wt home` to return to main repo
- `wt -` to jump to previous worktree
- Auto-creates worktree when navigating to an existing branch
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
wt [go] <#|name|branch>   go to worktree (creates if branch exists)
wt [go] 0|home            go to main repo
wt [go] -                 go to previous worktree
wt new [name]             create new worktree (branch: worktree-<name>)
                            (no name: move current branch from home)
wt rm <#|name|branch>     remove worktree
wt rm -b <#|name|branch>  remove worktree and delete branch
wt list                   list worktrees (raw)
wt status                 show branches and worktree status
```

### Examples

```bash
wt new feature-x     # Create .claude/worktrees/feature-x/ with branch worktree-feature-x
wt 1                 # Go to first worktree (alphabetical order)
wt feature-x         # Go to worktree by name
wt 0                 # Go back to main repo
wt -                 # Go to previous worktree
wt rm 1              # Remove first worktree
wt rm -b feature-x   # Remove worktree and delete branch
wt status            # Show all branches and their worktree numbers
```

## Directory Structure

```
project/                          # Main repo (home)
  .claude/
    worktrees/
      feature-x/                  # [1:feature-x]  (branch: worktree-feature-x)
      fix-bug/                    # [2:fix-bug]    (branch: worktree-fix-bug)
```

Numeric indices are assigned alphabetically: `feature-x` sorts before `fix-bug`, so `wt 1` goes to `feature-x` and `wt 2` goes to `fix-bug`.

## Claude Code Interop

`wt` uses the same directory layout as Claude Code's built-in worktree support (`claude -w`):

- Worktrees live in `.claude/worktrees/<name>/`
- Branches are prefixed `worktree-<name>`

Worktrees created by either tool are visible to both.

## Requirements

- zsh
- git

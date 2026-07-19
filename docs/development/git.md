# Git Cheat Sheet

> **Applies to:** Current Git command-line workflows
> **Last reviewed:** 2026-07-14

A practical reference for repository setup, branches, commits, remotes, inspection, undo operations, rebasing, and recovery.

> [!WARNING]
> Commands such as `reset --hard`, `clean`, forced pushes, and branch deletion can permanently remove uncommitted or unreferenced work. Inspect state and create a backup branch before destructive recovery.

## Identify the current repository state

```bash
git status --short --branch
git branch --show-current
git remote -v
git log --oneline --decorate --graph -20
```

## Configure identity

```bash
git config --global user.name '<name>'
git config --global user.email '<email>'
git config --global init.defaultBranch main
git config --list --show-origin
```

Use repository-local configuration when work and personal identities differ:

```bash
git config user.name '<name>'
git config user.email '<email>'
```

## Create or clone a repository

```bash
git init
git clone <repository-url>
git clone --branch <branch> <repository-url>
```

## Inspect changes

```bash
git status
git diff
git diff --staged
git diff <base>...<head>
git log --oneline --decorate --graph --all
git show <commit>
git blame <file>
```

## Stage and commit

```bash
git add <file>
git add --patch
git restore --staged <file>
git commit -m '<message>'
git commit --amend
```

Use `git add --patch` to review each hunk and avoid accidentally committing unrelated changes.

## Branches and switching

```bash
git branch
git branch --all
git switch -c <new-branch>
git switch <branch>
git switch -
git branch -d <branch>
```

Force-delete a local branch only after confirming its commits are preserved elsewhere:

```bash
git branch -D <branch>
```

## Remotes

```bash
git remote -v
git remote add <name> <url>
git remote set-url <name> <url>
git fetch --all --prune
git remote show origin
```

## Pull, merge, and push

```bash
git fetch origin
git merge origin/<branch>
git pull --ff-only
git push -u origin <branch>
git push
```

Prefer `git pull --ff-only` when you do not want Git to create an implicit merge commit or rebase.

## Merge a branch

```bash
git switch <target-branch>
git fetch origin
git merge --ff-only origin/<target-branch>
git merge <source-branch>
```

Resolve conflicts, stage the resolved files, and complete the merge:

```bash
git status
git add <resolved-file>
git commit
```

Abort an in-progress merge:

```bash
git merge --abort
```

## Rebase

```bash
git fetch origin
git rebase origin/main
git rebase --continue
git rebase --skip
git rebase --abort
```

Interactive cleanup:

```bash
git rebase -i <base-commit>
```

> [!CAUTION]
> Rebase rewrites commit IDs. Avoid rebasing shared branches unless the collaboration workflow explicitly expects it.

## Stash

```bash
git stash push -m '<description>'
git stash push --include-untracked -m '<description>'
git stash list
git stash show --patch stash@{0}
git stash apply stash@{0}
git stash pop
git stash drop stash@{0}
```

Use `apply` instead of `pop` when you want to keep the stash until the result is verified.

## Undo uncommitted changes

Restore one file from the index:

```bash
git restore <file>
```

Restore both staged and working-tree content from `HEAD`:

```bash
git restore --source=HEAD --staged --worktree <file>
```

Remove untracked files only after a dry run:

```bash
git clean -nd
git clean -nfd
git clean -fd
```

> [!DANGER]
> `git clean -fd` deletes untracked files and directories. Ignored files require additional flags and can include build artifacts, local configuration, or secrets.

## Undo committed changes safely

Create a new commit that reverses an earlier commit:

```bash
git revert <commit>
git revert <oldest-commit>^..<newest-commit>
```

Revert is generally safer for shared branches because it preserves history.

## Reset local history

Inspect first:

```bash
git status
git log --oneline --decorate -10
git branch backup/<description>
```

Reset modes:

```bash
git reset --soft <commit>
git reset --mixed <commit>
git reset --hard <commit>
```

| Mode | Branch pointer | Index | Working tree |
|---|---|---|---|
| `--soft` | Moves | Preserved | Preserved |
| `--mixed` | Moves | Reset | Preserved |
| `--hard` | Moves | Reset | Reset |

> [!DANGER]
> `git reset --hard` discards tracked working-tree changes. Create a backup branch and verify the target commit before running it.

## Cherry-pick

```bash
git cherry-pick <commit>
git cherry-pick --continue
git cherry-pick --abort
```

Apply a commit without immediately committing it:

```bash
git cherry-pick --no-commit <commit>
```

## Tags

```bash
git tag
git tag -a <tag> -m '<message>'
git show <tag>
git push origin <tag>
git push origin --tags
```

## Find a regression with bisect

```bash
git bisect start
git bisect bad
git bisect good <known-good-commit>
```

Test each selected commit and mark it:

```bash
git bisect good
git bisect bad
git bisect reset
```

Automate with a test command:

```bash
git bisect run <test-command>
```

## Recover lost commits

```bash
git reflog
git show <reflog-commit>
git branch recovery/<description> <reflog-commit>
```

The reflog is local and expires over time. Create a branch as soon as the desired commit is found.

## Submodules

```bash
git submodule status
git submodule update --init --recursive
git submodule sync --recursive
git clone --recurse-submodules <repository-url>
```

## Worktrees

```bash
git worktree list
git worktree add ../<directory> <branch>
git worktree add -b <new-branch> ../<directory> <base>
git worktree remove ../<directory>
git worktree prune
```

Worktrees are useful when multiple branches must be checked out simultaneously without repeated stashing.

## Safer force push

```bash
git fetch origin
git push --force-with-lease origin <branch>
```

> [!CAUTION]
> `--force-with-lease` is safer than `--force`, but it still rewrites remote history. Confirm that the branch is intended to be rewritten and that no collaborator work will be lost.

## Useful aliases

```bash
git config --global alias.st 'status --short --branch'
git config --global alias.lg 'log --oneline --decorate --graph --all'
git config --global alias.unstage 'restore --staged --'
```

## Troubleshooting checklist

1. Run `git status --short --branch`.
2. Confirm the current branch and remote.
3. Fetch before comparing local and remote history.
4. Inspect staged and unstaged diffs separately.
5. Use `git reflog` before assuming a commit is lost.
6. Create a backup branch before reset, rebase, or force-push recovery.
7. Prefer revert for changes already shared with others.

## References

- [Git documentation](https://git-scm.com/docs)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [Git glossary](https://git-scm.com/docs/gitglossary)

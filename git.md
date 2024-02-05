# Git Command Cheat Sheet

This cheat sheet is intended for developers, IT professionals, and anyone using Git for version control. Git is a distributed version control system that's widely used for tracking changes in source code during software development.

Here, you'll find a collection of basic and advanced Git commands, categorized for ease of reference. This guide is meant to assist with common version control tasks and scenarios encountered in everyday development workflows.


## Basic Repository Setup

- **git init**
  - Initializes a new Git repository.

- **git clone [url]**
  - Clones a repository into a new directory.

## Local Changes

- **git status**
  - Shows the working tree status.

- **git add [file]**
  - Adds a file to the staging area.

- **git commit -m "[message]"**
  - Records file snapshots in the version history.

## Branching and Merging

- **git branch**
  - Lists all local branches.

- **git branch [branch_name]**
  - Creates a new branch.

- **git checkout [branch_name]**
  - Switches to the specified branch.

- **git merge [branch]**
  - Merges the specified branch into the current branch.

## Remote Repositories

- **git remote -v**
  - Lists all remote repositories.

- **git push [remote] [branch]**
  - Pushes the branch to the remote repository.

- **git pull [remote]**
  - Fetches from and integrates with another repository or a local branch.

## Advanced Git

- **git rebase [branch]**
  - Reapplies commits on top of another base tip.

- **git stash**
  - Temporarily stores all modified tracked files.

- **git stash pop**
  - Restores the most recently stashed files.

- **git log**
  - Shows commit logs.

- **git fetch [remote]**
  - Downloads objects and refs from another repository.

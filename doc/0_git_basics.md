# Git basics

**Table of contents**

- [About](#about)
- [What is Git?](#what-is-git)
- [Key concepts](#key-concepts)
  - [Repository](#repository)
  - [The three areas](#the-three-areas)
  - [Commit](#commit)
  - [Branch](#branch)
  - [Checkout](#checkout)
  - [Remote](#remote)
  - [Merge](#merge)
  - [Tag](#tag)
- [A typical workflow](#a-typical-workflow)
- [Everyday commands](#everyday-commands)
- [References](#references)

## About

This document introduces the core ideas and vocabulary of [git](https://git-scm.com)
so that the rest of the documentation - [setup](1_setup.md) and
[use-cases](2_use_cases.md) - makes sense.

It is meant to be read first, before doing any actual work.

## What is Git?

Git is a **distributed version control system**. It records the history of changes
to a set of files (usually source code) so you can:

- see *what* changed, *when*, *why*, and by *whom*,
- go back to any earlier state,
- work on several things in parallel without them interfering,
- collaborate with others and combine everyone's work.

*Distributed* means that every copy of a project (every *clone*) is a complete
repository with the full history - not just a working copy. You can commit, view
history and create branches entirely offline, and only synchronise with others
when you choose to.

> **Note:** Git (the tool) is not the same as [GitHub](https://github.com),
> [Bitbucket](https://bitbucket.org) or [GitLab](https://gitlab.com). Those are
> *hosting services* for git repositories. Git works perfectly well without any
> of them.

## Key concepts

### Repository

A **repository** (or *repo*) is the project together with its entire history.
Physically it is your project folder plus a hidden `.git` directory that stores
all the history and metadata.

You create a repository in one of two ways:

```
git init              # start a brand new repository in the current folder
git clone <url>       # copy an existing repository (with full history) from a remote
```

### The three areas

Understanding these three areas explains almost every everyday command:

| Area | What it is |
| --- | --- |
| **Working directory** | The actual files you see and edit on disk. |
| **Staging area** (the *index*) | A holding space for the changes you want in your *next* commit. |
| **Repository** (`.git`) | The committed history - permanently recorded snapshots. |

Changes flow from one area to the next:

```
working directory  --(git add)-->  staging area  --(git commit)-->  repository
```

- `git add` moves changes from the working directory into the staging area.
- `git commit` records everything staged into the repository as a new commit.

Check where things are at any time with:

```
git status
```

### Commit

A **commit** is a snapshot of your staged changes at a point in time, together
with a message describing them. Each commit has a unique identifier (a *hash*,
e.g. `9ca0219`) and points to its parent commit(s), forming the history.

```
git add .                        # stage all current changes
git commit -m "Describe the change"
```

Good commits are small, focused, and have a clear message. The history is only
as useful as the messages you write.

> **Note:** As described in [setup](1_setup.md#signingkey-config), a commit can be
> **signed** by adding the `-S` flag, e.g. `git commit -S -m "..."`.

### Branch

A **branch** is a movable pointer to a commit - effectively an independent line
of work. It lets you develop a feature or fix in isolation without disturbing the
main line of code. The default branch is usually called `main`.

```
git branch                       # list branches (the current one is marked with *)
git branch fb_workitem001        # create a new branch from the current commit
git branch -d fb_workitem001     # delete a branch once it is no longer needed
```

Creating a branch is cheap and fast - it is just a new pointer, not a copy of
your files.

### Checkout

**Checking out** switches your working directory to a different branch or commit -
i.e. it changes *what you are currently looking at and working on*.

```
git checkout develop             # switch to the 'develop' branch
git checkout -b fb_workitem001   # create a new branch AND switch to it in one step
```

> **Note:** Newer versions of git also offer `git switch` (to change branch) and
> `git restore` (to discard changes), which split the many jobs of `checkout`
> into clearer commands. `git checkout` still works and is used throughout the
> [use-cases](2_use_cases.md).

### Remote

A **remote** is a version of your repository hosted elsewhere (typically on
GitHub/Bitbucket) that you synchronise with. The default remote is usually named
`origin`.

```
git remote add origin <url>      # connect a local repo to a remote
git push                         # send your commits to the remote
git pull                         # fetch commits from the remote and merge them in
```

### Merge

**Merging** combines the work from one branch into another. You switch to the
branch you want to merge *into*, then merge the other branch:

```
git checkout develop             # the branch to merge INTO
git merge fb_workitem001         # bring the feature branch's work into develop
```

If the same lines were changed on both branches, git reports a **merge conflict**
that you resolve by hand before completing the merge.

### Tag

A **tag** is a fixed, named label on a specific commit - commonly used to mark
releases (e.g. `18_06`). Unlike a branch, a tag does not move.

```
git tag 18_06                    # mark the current commit as release 18_06
git tag                          # list tags
```

## A typical workflow

Putting the concepts together, a common day-to-day cycle looks like this:

```
git checkout -b fb_myfeature     # 1. branch off for a new piece of work
# ... edit files ...
git status                       # 2. see what changed
git add .                        # 3. stage the changes
git commit -m "Add my feature"   # 4. record a snapshot
git checkout develop             # 5. switch back to the main line
git merge fb_myfeature           # 6. merge the work in
git push                         # 7. share it with the remote (when ready)
```

See [use-cases](2_use_cases.md) for fuller, branch-based examples.

## Everyday commands

| Command | What it does |
| --- | --- |
| `git init` | Create a new repository in the current folder. |
| `git clone <url>` | Copy an existing remote repository locally. |
| `git status` | Show what is changed, staged, or untracked. |
| `git add <file>` | Stage changes for the next commit (`.` for everything). |
| `git commit -m "msg"` | Record staged changes as a new commit. |
| `git log` | Show the commit history. |
| `git branch` | List, create or delete branches. |
| `git checkout <ref>` | Switch branch or restore files. |
| `git merge <branch>` | Merge another branch into the current one. |
| `git pull` / `git push` | Sync commits with a remote. |
| `git tag <name>` | Label a commit (e.g. a release). |
| `git --help` | Get help - always close at hand. |

## References

* [Git - official site](https://git-scm.com)
* [Pro Git book (free)](https://git-scm.com/book/en/v2)
* [Git reference documentation](https://git-scm.com/docs)
* [GitHub Docs](https://docs.github.com/en)

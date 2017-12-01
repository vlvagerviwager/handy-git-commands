# Handy dandy Git commands

These are commands which either accomplish basic tasks in the Git version control system, or helped me out of sticky situations when I was learning how to use Git (with Gerrit). I used to look at this list a lot, but ever since I started using GitHub at work (January 2016), I’ve stopped. I probably figured some of these out myself, but the majority come from the Internet, specifically Stack Overflow.

Commands/tips are in no particular order.

Text preceded by a `#` are comments.

Text between angle brackets (`< >`) needs to be replaced based on your circumstances.

---

## View branches

```sh
git branch
```

## Get information/help on a git command

```sh
git -h
```

## Simulate what would happen if a command is run

```sh
git --dry-run
```

## Clean up unindexed files

```sh
git clean -h
```

## “Your branch is ahead of origin (remote branch) by x commits”

```sh
git push origin HEAD:refs/heads/branch-name
```

## Amend a commit message 

```sh
git commit --amend # Will amend the previous commit
```

OR

```sh
git rebase -i
```

## Pulling conflicting changes

```sh
git pull --rebase # commit unpushed changes

git add

git rebase --continue

git push origin HEAD:refs/for/master
```

## Updating a feature branch with master

```sh
git rebase origin/master
```

## Resetting to a particular state

```sh
git reflog my-branch # shows pointers

git reset --hard HEAD~2
```

OR

```sh
git reflog git reset --hard HEAD@{x}
```

## Squashing commits

### You are on a branch which has a bunch of commits 

```sh
git checkout -b mergeBranch # save those commits to this branch
```

Resolve the conflicts. Check out master and pull. Check out mergeBranch.

```sh
git rebase origin/master # the commits should be on mergeBranch now

git merge --squash mergeBranch
```

If the commits are beside each other on the branch (check with “git log”), just do:

```sh
git rebase -i
```

### Squashing most recent 2 commits on a local branch

```sh
git rebase -i HEAD~2 # git rebase -i looks on the remote for unpushed commits
```

### Squash last 2 commits and write new commit message

```sh
git reset --soft HEAD~2 && git commit
```

## Diff

```sh
git diff --stat
```

## Delete a local branch

```sh
git branch -d my-local-branch
```

## Rename current branch

```sh
git branch -m newName
```

## Something messed up - commits stuck/can’t amend/want to squash

```sh
git fetch git rebase -i HEAD~2 # work with the last 2 commits
```

See the options for squashing (replace “pick” on the bottom commit with “s”). Edit the commit message and push.

## Search past commands in Bash

Ctrl + R in the terminal

## When rebasing on multiple commits, remember solved merge conflicts

```sh
git rerere
```

## Reset one file (uncommitted)

```sh
git checkout --
```

## Fix merge conflict (GitHub)

```sh
git fetch origin

git merge master # Fix conflicts; HEAD = my changes

git add git commit
```

## Replace my-branch completely with master (or any other branch)

```sh
git checkout master

git merge -s ours my-branch

git checkout my-branch

git merge master
```

## Gerrit-related (but can probably be adapted)

### Amend a commit on Gerrit

```sh
git checkout master # or the branch that the Gerrit commit was on

git checkout -b amendChange # or any other temp name

git fetch # get from Gerrit - Download > cherry pick > clipboard

git cherry-pick pushedCommit # the branch that the change was committed to

git rebase -i HEAD~2

git push origin HEAD:refs/for/master
```

### Merge commit feature branch

```sh
git fetch

git merge origin/master

git push origin HEAD:refs/heads/ # "/heads/" will skip gerrit for merge commit

git push origin HEAD:refs/for/master # Go through Gerrit
```

---

# TODO

- Add TOC (https://github.com/jonschlinkert/markdown-toc)

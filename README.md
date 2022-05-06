# Handy-Dandy Git Commands

These are commands which accomplish basic tasks in the Git version control system, and helped me out of sticky situations when I was learning how to use Git (with Gerrit). 

I used to look at this list a lot, but ever since I started using GitHub at work (January 2016), I’ve stopped. I probably figured some of these out myself, but the majority come from the Internet, specifically [Stack Overflow](https://stackoverflow.com/).

Commands/tips are in no particular order.

Text preceded by a `#` are comments.

Text between angle brackets (`< >`) needs to be replaced based on your circumstances.

---

## View branches

```sh
git branch
```

## View remotes 

Remotes are where you are pushing to and pulling from.

```sh
git remote -vv
```

## Get information/help on a git command

```sh
git -h
```

## Simulate what would happen if a command is run

```sh
# use the --dry-run flag, e.g. before running "git add .":
git add . --dry-run
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
git push origin HEAD:refs/for/main
```

## Updating a feature branch with main

```sh
git rebase origin/main
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

Resolve the conflicts. Check out `main` and pull. Check out `mergeBranch`.

```sh
git rebase origin/main # the commits should be on mergeBranch now
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

## Fix merge conflict 

```sh
git fetch origin
git merge main # Fix conflicts; HEAD = my changes
git add .
git commit
```

## Replace my-branch completely with main (or any other branch)

```sh
git checkout main
git merge -s ours my-branch
git checkout my-branch
git merge main
```

## Reset your local main branch (e.g. if it's irreparably messed up)

```sh
git branch -D main # delete your local main branch
git checkout -b main remotes/upstream/main # pull it back down from the remote repo
```

## Revert all uncommitted changes including files and folders (e.g. from git merge)

Use with caution.

```sh
git clean -fd
```

## Remove lots of accidental commits pulled into PR

https://stackoverflow.com/questions/41955765/git-remove-all-commits-from-pr 

## Remove commits that have been pushed

```sh
git reset --hard HEAD~3
git push -f origin HEAD 
```

## Replace the contents of one branch with another, erasing commit history

In this example, you have a QA branch named `qa` which is missing commits from main or otherwise misaligned, and you want to refresh it with the contents of main. This will not preserve the commit history of `qa` 

```sh
git checkout main
git pull origin main
git branch -D qa
git checkout -b qa
git push origin qa -f 
```

## Push a feature branch

```sh
git fetch
git checkout qa
git merge origin/bug/JIRA-123-fix-this # origin/topic/branch
git push origin qa
```

## Can’t rebase on GitHub / GitLab

```sh
git checkout master
git pull
git checkout pr-branch
git rebase master
git push origin pr-branch
```

## Rogue commits in branch

```sh
git rebase HEAD~n
<edit the commits and drop them>
git rebase --continue
git push -f origin pr-branch
```

## The source branch is n commits behind the target branch

Don't rebase!

```sh
git checkout master
git pull
git checkout pr-branch
git merge master
```

---

## Working with forks

###  Checkout a branch on someone else’s fork

https://stackoverflow.com/questions/9153598/how-do-i-fetch-a-branch-on-someone-elses-fork-on-github/9153737 

---

## Working with Gerrit

I used these specifically in the context of a Gerrit workflow, but they can be adapted.

### Amend a commit on Gerrit

```sh
git checkout main # or the branch that the Gerrit commit was on
git checkout -b amendChange # or any other temp name
git fetch # get from Gerrit - Download > cherry pick > clipboard
git cherry-pick pushedCommit # the branch that the change was committed to
git rebase -i HEAD~2
git push origin HEAD:refs/for/main
```

### Merge commit feature branch

```sh
git fetch
git merge origin/main
git push origin HEAD:refs/heads/ # "/heads/" will skip gerrit for merge commit
git push origin HEAD:refs/for/main # Go through Gerrit
```

---

# TODO

- Add a table of contents

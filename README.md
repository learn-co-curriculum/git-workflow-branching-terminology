# Git: Working with Remote Branches

## Learning Goals

- Identify local and remote branches
- Demonstrate getting updates from remote
- Demonstrate merging branches
- Demonstrate deleting branches

## Introduction

Many developers and organizations use Git to keep track of their work; it’s pretty
important that we understand how to work with Git not just on a local computer,
but utilizing Git's remote features that allow for backups and collaborative work.

## Identify Local and Remote Branches

We've gone through the steps of pushing our branches to GitHub, so let's clarify
some terminology around different types of branches and related commands before
proceeding.

### Local Branches

So far, we've seen examples of local branch operations. The git branch command
also works on remote branches. A local branch is a branch that only we (the
local user) can see. It exists only on _your_ computer.

we can see a list of all local branches on your computer with `git branch`.

### Remote Branches

A remote branch is a branch on a remote repository (in most cases origin). When
we push our local branch `new-branch-name` to `origin`, we and other users can
track it.

We can see a list of all remotes with `git remote` (it will most likely only be
`origin`). More importantly, we can see a list of all remote branches of the
repository with `git branch -a`.

### Remote Tracking Branches

A _remote tracking branch_ is a local copy of a _remote_ branch. When
`new-branch-name` is pushed to origin using the command, a remote tracking
branch named `origin/new-branch-name` is created on your machine. This remote
tracking branch tracks the remote branch `new-branch-name` on origin. we can
update your remote tracking branch to be in sync, or up-to-date, with the remote
branch using `git fetch` or `git pull`. We will go more in depth a little later
on these commands.

### Local Tracking Branches

A _local tracking branch_ is a local branch that is tracking another local
branch. This is so that we can push/pull commits to/from the other branch. Local
tracking branches, in most cases, track a remote tracking branch.

When we push a local branch to `origin` using the `git push` command with a `-u`
option (as shown previously), we set up the local branch `new-branch-name` to
track the remote tracking branch `origin/new-branch-name`.

Otherwise, we will see this when we try `git push`:

```bash
fatal: The current branch new-branch-name has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin myNenew-branch-namewBranch
```

**Note:** This is needed to use `git push` and `git pull` without specifying an
upstream to push to or pull from.

## Demonstrate Getting Updates From Remote

Getting updates to branches come into play when there are branches that live on
a remote repository. These branches can be ones that we are tracking locally, or
ones that have been created and pushed up from other computers.

Previously, we mentioned that we can use commands the commands `git fetch` or
`get pull` to sync local tracking branches with remote tracking branches. On the
local tracking branch that we want to sync, either of these commands are used to
download new data from a remote repository. One of these actions must be taken
to do so. Your local tracking branch is only as up-to-date as the last time we
**explicitly** downloaded the data from the remote.

What is the difference between fetch and pull?

!["git fetch"](https://curriculum-content.s3.amazonaws.com/prework/git-workflow/git%20fetch.gif)
!["git pull"](https://curriculum-content.s3.amazonaws.com/prework/git-workflow/git%20pull.gif)

- `git fetch` **only** downloads new data from a remote repository. Fetch will
  never change anything on your local repository. Fetching is what you do when
  you want to great for seeing all the changes that happened in a remote
  repository since your last sync.
- `git pull` brings a local tracking branch up-to-date with its remote version,
  while also updating your other remote tracking branches. Pull not only
  downloads new data&mdash;it also directly integrates it into your current
  working copy files, so we should not have any uncommitted local changes before
  we pull.

  **Note:** `git pull` performs a `git fetch` and a `git merge`. We will go into
  depth of what a merge is next.

## Demonstrate Merging Branches

Merging allows us to take our branches and integrate them back into a single
branch. We can use `git merge other-branch-name` to integrate the changes from
one branch to the branch to the receiving branch (the branch we are currently
on).

So, if we want to take the changes we created on `new-branch-name` and merge
them back into the `master` branch now that we've confirmed the changes are safe
to integrate, we do so by using these commands:

```bash
git checkout master # This switches us back to the master branch
git merge new-branch-name # This integrates our new branch, new-branch-name, and its changes into master
```

As long as the `git` histories are still in sync, you will see no _merge
conflicts_, and now all the changes that were made on the branch
`new-branch-name` will be integrated into `master`. With work on this branch
completed and merged, we no longer need it. Any additional work moving forward
can be done on a new branch.

## Demonstrate Deleting Branches

Since we've merged our changes into master, we can safely delete our local version
of `new-branch-name`. A branch can be deleted by providing the `-d`/`–D` options
with the `git branch` command. Before deleting the `new-branch-name`, we should
be on another branch. Currently, we're still on `master`.

The `-d` option stands for `--delete`, which would delete the local branch only
if you have already _pushed_ and _merged_ it with your remote branches.

The `-D` option stands for `--delete --force`, which deletes the branch
regardless of its push and merge status. Be careful with the usage of this one,
however, this is useful when you have a branch with changes that you don't want
to merge (maybe you experimented with things here and decided to throw them
out).

To delete an obsolete local branch (that has been pushed and merged), we type
`git branch -d <branch-to-delete>`. To make sure this branch was successfully
deleted, we type `git branch`. It should no longer exist in the list of local
branches.

<gif of git branch -d, git branch -D and git branch being used>

To delete the remote tracking branch, we can use `git push <remote_name (most
likely origin)> --delete <branch-name>`. To list all remaining remotes, again,
we can type `git branch -a`. It should no longer exist in the list of remote
branches.

## Conclusion

While branching is a very useful tool for working on your own code, in a
collaborative environment, it is common for several developers to share and work
on the same source code. Retrieving branches from remote repositories allows us
to pick up where we left off, or add onto someone's work. Some developers will
be fixing bugs, others will be implementing new features, etc. It allows us to
stay organized, work more freely, collaboratively, and better avoid set backs
from making hard-to-undo mistakes.

## Resources

* [What is the difference between ‘git pull’ and ‘git fetch’?](https://www.javacodegeeks.com/2018/09/git-pull-git-fetch.html)
* [What's the difference between git fetch and git pull?](https://www.git-tower.com/learn/git/faq/difference-between-git-fetch-git-pull)
* [git fetch](https://www.atlassian.com/git/tutorials/syncing/git-fetch)
* [Delete a local and a remote GIT branch](https://koukia.ca/delete-a-local-and-a-remote-git-branch-61df0b10d323)

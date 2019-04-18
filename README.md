# Git: Working with Remote Branches

## Learning Goals

- Identify local branch
- Identify remote branches
- Identify remote tracking branches
- Identify local tracking branches
- Demonstrate getting updates from remote
- Demonstrate merging branches
- Demonstrate deleting branches
- Demonstrate updating remotes and merging changes in one step

## Introduction

Many developers and organizations use Git branches to keep track of their work.
It’s important that we understand how to work with branches on both our local
machine _as well as_ on a shared resource, like GitHub. Being able to share
branches means that all the advantages of branch-based development are
available both for individuals ***and*** organizations.

## Review Terminology

We've gone through the steps of pushing our branches to GitHub, so let's clarify
some terminology around different types of branches and related commands before
proceeding.

### Identify Local Branch

A **local branch** is a branch that only we (the local user) can see. It exists
only on _our_ computer. The `git branch` command can show us our local
branches:

```bash
$ git branch
  * master
  larry-branch
  michelle-branch
  curly-branch
```

### Identify Remote Branches

A **remote branch** is a branch on a remote repository. Remote repositories
have a short name or nickname. By default, when we `git clone` the remote is
called `origin`. Many Git commands assume that if you don't specify a repo
explicitly, you mean `origin`.  When we push our local branch `new-branch-name`
to `origin`, we and other users can track it. That is, we can retrieve those
contents. So we don't have to haul computers around, we can simply grab the
code from the remote repository and pick up where we left off!

We can see a list of all the remotes' shortnames with `git remote`. Usually 
there is only one, `origin`, the default. As you get more skilled you might
share your code to multiple remotes.

We can see a list of all ***remote branches*** &mdash; that is all the branches
at all the remotes &mdash; that Git knows about with `git branch -a`.

### Identify Remote Tracking Branches

A _remote tracking branch_ is a _local_ copy of a _remote_ branch.

When a locally-created branch like `new-branch-name` is pushed to `origin`
using the `git push -u origin new-branch-name` command, a remote tracking
branch named `origin/new-branch-name` is created on your machine.

In reverse, when you clone a repository you're given remote tracking branches
for all the remote branches. Amazingly, by doing so Git allows you to work with
all the remote branches on all the remotes **offline**. Provided you "freshen"
your local repository with `git fetch` right before you go into the wilderness,
you can have _years_ of software development on your laptop (provided you
explicitly freshen with `git fetch` right before, more on that below!).

This remote tracking branch "follows" or "tracks" the remote branch
`new-branch-name` on `origin`. We can update your remote tracking branch to be
in sync, or up-to-date, with the remote branch using `git fetch`.

### Identify Local Tracking Branches

A _local tracking branch_ is a local branch that is tracking another local
branch. Most often, the local branch is tracking a remote tracking branch that,
in turn, is tracking a remote branch.

This sounds confusing and this is an area that can be really frustrating with
Git. Let's suppose  you want to work on a branch that _someone else_ pushed up
to your remote. After you clone the repo, you want to work on
`my-friends-branch`.

If you do `git checkout origin/my-friends-branch` Git will:

1. Establish a remote tracking branch called (`remotes/origin/my-friends-branch`) that points to `origin/my-friends-branch`
2. Establish a local tracking branch (`my-friends-branch`) that tracks (`remotes/origin/my-friends-branch`)
3. Put you "on" the local tracking branch (`my-friends-branch`)

Because Git can trace how to get from your local tracking branch to the remote
tracking branch _back_ to the original remote branch, if you make a commit on
this branch and then type `git push`, Git will send your change to the remote
repository so that other collaborators can get your change and build on it!
This is the heart of collaboration in Git.

## Demonstrate Getting Updates From Remote

Remember we mentioned going into the wilderness &mdash; no Wi-Fi, no Instagram,
etc.? If you wanted to cache all the information about all the branches on all
your remotes, you need to use `git fetch` to update the cache for each remote
name.

Let's suppose a colleague has created a new branch and pushed it to a location
that you _both_ use as your `origin` remote. To freshen your local repository
issue:

```bash
$ git fetch
```

Git, yet again, assumes you mean `origin`, but if you had multiple remotes, you
might provide a name like `my-startup`.


```bash
$ git fetch my-startup
```

This synchronizes your remote tracking branches with what's going on on the
remotes. Because of this, your remote tracking branches update. Go into the
heart of the desert and you will have ***all*** the commits for all the
branches. You'll have their histories tracing back to the initial commit.

`git fetch` **only** downloads new data from a remote repository. Fetch will
never change anything on your local branch. Fetching is what you do when you
want to fetch all the changes that happened in a remote repository since your
last sync.

> **IMPORTANT**: Your local tracking branch is only as up-to-date as the last
> time we **explicitly** downloaded the data from the remote.

Here, the `git fetch` command is being used.

!["git fetch"](https://curriculum-content.s3.amazonaws.com/prework/git-workflow/git%20fetch.gif)

## Demonstrate Merging Branches

Merging allows us to take branches and integrate their content into another
branch. From Git's point of view, it doesn't care whether the branch is local
or remote or remote-tracking. It finds the difference between the branch you're
on and the branch you're merging in and weaves them in together.

We can use `git merge other-branch-name` to integrate the changes from one
branch to the branch to the branch we are currently on. Here are some examples

Assuming we're on `master`:

```bash
$ git merge other-branch
$ git merge origin/idea-my-friend-had
$ git merge my-startup/time-travel-engine
```

So, if we want to take the changes we created on `new-branch-name` and merge
them back into the `master` branch, now that we've confirmed the changes are
safe to integrate, we do so by using these commands:

```bash
git checkout master # This switches us back to the master branch
git merge new-branch-name # This integrates our new branch, new-branch-name, and its changes into master
```

Now all the changes that were made on the branch `new-branch-name` are
integrated into `master`. With work on this branch completed and merged, we no
longer need it. Any additional work moving forward can be done on a new branch.

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

With `git branch -d <branch name>` we get a little warning before it's deleted.
IF it has not been pushed and merged, it will reject the command. With
`git branch -D <branch name>`, it will force-delete the branch without warnings.

!["git branch -d vs. git branch -D"](https://curriculum-content.s3.amazonaws.com/prework/git-workflow/git%20branch%20delete.gif)

To delete the remote tracking branch, we can use `git push <remote_name (most
likely origin)> --delete <branch-name>`. To list all remaining remotes, again,
we can type `git branch -a`. It should no longer exist in the list of remote
branches.

## Demonstrate Fetching and Updating the Local Branch

You might think that fetching (to update the cache) and then merging (to pull
the changes into the local tracking branch you're on) is an unnecessarily two-step long process.
If you're on a local tracking branch, you can issue `git pull`. This will:

1. Run `git fetch` and update all the remote tracking branches
2. Bring in the changes, if any, from the remote tracking branch to your local
   tracking branch

The following are equivalent (assume we're on `new-idea` local tracking branch):

```bash
$ git fetch origin
$ git merge origin/new-idea
```

```bash
$ git pull origin/new-idea
```

Since we're on a local tracking branch, Git will assume you mean "the branch of
the same name" at `origin`, if you've been following the code patterns we've
given to you.

```bash
$ git pull
```

## Conclusion

While branching is a very useful tool for working on your own code and sharing
code with others.  It is common for several developers to share and work on the
same source code by referring to the same shared "remote," typically called
`origin`.

Retrieving branches from remote repositories allows us to pick up where we left
off, or add onto someone's work. Some developers will be fixing bugs, others
will be implementing new features, etc. Branch-based development allows us to
stay organized and work more freely and collaboratively.

## Resources

* [What is the difference between ‘git pull’ and ‘git fetch’?](https://www.javacodegeeks.com/2018/09/git-pull-git-fetch.html)
* [What's the difference between git fetch and git pull?](https://www.git-tower.com/learn/git/faq/difference-between-git-fetch-git-pull)
* [git fetch](https://www.atlassian.com/git/tutorials/syncing/git-fetch)
* [Delete a local and a remote GIT branch](https://koukia.ca/delete-a-local-and-a-remote-git-branch-61df0b10d323)

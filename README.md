# Git: Branching Terminology

## Learning Goals

- Identify local branch
- Identify remote branches
- Identify remote tracking branches
- Identify local tracking branches

## Introduction

Many developers and organizations use Git branches to keep track of their work.
It’s important that we understand how to work with branches on both our local
machine _as well as_ on a shared resource, like GitHub. Being able to share
branches means that all the advantages of branch-based development are
available both for individuals ***and*** organizations.

We've gone through the steps of pushing our branches to GitHub, so let's clarify
some terminology around different types of branches and related commands.

## Identify Local Branch

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

## Identify Remote Branches

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

## Identify Remote Tracking Branches

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

## Identify Local Tracking Branches

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

## Conclusion

Branching is a very useful tool for working on your own code and sharing
code with others. It is common for several developers to share and work on the
same source code by referring to the same shared "remote," typically called
`origin`.

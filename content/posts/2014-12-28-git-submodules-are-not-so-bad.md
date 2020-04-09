---
layout: post
title: git submodules are not so bad
date: 2014-12-28 23:25:51 +0000
comments: true
tags:
- git
---

I see a lot of complaints about git submodules, [people suggesting alternatives](https://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/), [complaints about merging](https://codingkilledthecat.wordpress.com/2012/04/28/why-your-company-shouldnt-use-git-submodules/) or other bits and pieces.

Git submodules *have their place*. Yes they are not ideal for all situations but they are ideal for the typical use case I'm about to outline.

## Example use case

In my work I have a *master* project which contains multiple submodules. Each submodule is also cloned into a separate *development* repository sitting near by. I use the *master* repository to synchronise pushes to the deployment server (in this case an isolated computer sitting on a mountain in Chile!)

Admittedly I have to train myself to follow certain procedures:

1. *Never* perform code alterations or general development in the submodule inside the *master* repository.

2. Only use the *master* repository for synchronisation.

## Git submodule advantages

For my use case I find it very good for the *master* repository to track a single *commit*, as this bakes in the state of the submodules for each commit of the *master* repository. I only need to record the git sha of the *master* to be able to recreate the entire source code checkout of the individual packages.

Development on the individual packages is also easier as they're modularised into separate small, testable directories, and only combined in the *master* during a "deployment".

I can add tests to the *master* project that assess the integration between submodules. In the case of an error I can then check out a specific revision in my submodules at will to re-run tests and find breaking changes, perhaps with `git bisect`.

## Git submodule disadvantages

The paradigm shift of using submodules is a pain. They require extra, very strange commands to synchronise.

I have a few git aliases to help me with this:

```
su = submodule update --init --recursive
```

This command initialises and synchronises the submodules in whatever state they started in, either not existing - so a clone takes place - or a previous commit. It also synchronises nested submodules, which I'll admit I would probably never do.

```
upsub = submodule foreach 'git checkout master && git pull'
```

A very useful command, `git submodule foreach` runs a command over every submodule specified in the current project. In this case it's used to update the master branch to `origin`.

A minor annoyance is a submodule with code changes will appear to a git status in the *master* as an untracked file, showing the whole submodule as *modified*. I personally like this feature as it reminds me that the submodule in the *master* is not in a proper state:

```
-Subproject commit c74dd1e56794962d7ff7661a7f8ae84cafa96b4f
+Subproject commit c74dd1e56794962d7ff7661a7f8ae84cafa96b4f-dirty
```

and therefore shouts at me for forgetting rule 1. above. It can be disabled via: `git status --ignore-submodules=dirty` but I do not recommend this. Consider it a *feature*.

## Git subtree

A common suggested alternative is [git subtree](https://git-scm.com/book/en/v1/Git-Tools-Subtree-Merging) which I have tried, though briefly. Perhaps learning submodules from an early stage has ingrained their process in my brain too hard for other alternatives to really show their benefits. As far as I could tell `git subtree` creates a huge merge commit in the master repository as a single static blob. I didn't look into coding in the subtrees but I feel the separation of projects afforded by submodules fits my needs better. Perhaps I just have to read the linked article above a bit more.

## Hints and tips

Some hints and tips I always have to google myself:

### Remove a submodule
[source](https://stackoverflow.com/a/16162000/56711)

```
git submodule deinit asubmodule
git rm asubmodule
```

## Summary

* Separate projects and use a *master* project to coordinate commits,
* *Never* develop in the *master*s copy of the repository,
* Use shortcuts to make managing submodules easier.

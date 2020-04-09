---
title: git rebase --skip is fine
date: 2014-08-27 22:53:48
comments: true
Tags:
- git
Category: Tech
---

So `git rebase` is a powerful tool, able to change history itself. With this power however requires great care to avoid needing to `git push --force`.

Git rebase comes with very user friendly ways to cancel out of a rebase if something goes wrong or if you become confused:

    git rebase --abort

This returns your working tree back to the state before the rebase was started.

One thing that has always made me nervous when using rebase was when I rebased and a conflict occured, so I only kept changes from the HEAD commit which caused the following message:

    Applying: change
    No changes - did you forget to use 'git add'?
    If there is nothing left to stage, chances are that something else
    already introduced the same changes; you might want to skip this patch.
    
    When you have resolved this problem, run "git rebase --continue".
    If you prefer to skip this patch, run "git rebase --skip" instead.
    To check out the original branch and stop rebasing, run "git rebase --abort".

Now I don't like error messages or messages that look like error messages so this result is quite off-putting. In reality the second line of advice, running `git rebase --skip` is not a hacky way out of this situation, it's the right answer. skipping a commit when it only includes changes from the HEAD commit is perfectly normal. Once I learned this my time with git was much easier.


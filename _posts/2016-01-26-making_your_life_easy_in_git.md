---
title: Making Your Life Easy In Git
layout: post
published: false
---

Git is a very powerful and full-featured VCS that can either be your friend or your greatest foe.  There are countless ways to use git and some really quick ways to get yourself out of bad situations.  Often times you will find that most of these "quick fixes" will cause someone a lot of pain and should never be done unless absolutely necessary.  Git makes your coding life easier and more manageable if you follow some general guidelines.

## Commit often

It is a prominent and very bad habit for developers to write code in a box for hours and days at a time.  It causes large commits, merge conflicts, slacking in reviews, overlooked code problems, etc.  Your commits should be small and often.  If you find yourself in the "EUREKA!" state that we all love, commit.  Don't wait.  Don't say "let me just..." Just commit.  It doesn't matter if you have some refactoring to do or some comments to remove, etc.  If it works and doesn't break someone else's workflow, commit.  You don't have to push, just commit.  This makes merge conflicts minimal, sparse, and rare.  It also creates a rollback position so that *when* you break everything during your refactoring and cleanup stages you always have something to rollback to.

Features may take a few iterations to complete, but you should be working in small, deployable increments which can be tested individually.  If your task is to write a command line application you could write, test, and deploy every time you implement a new flag.  You should not wait until the whole application is completed to merge.  You want testing to be done incrementally, throughout the sprint and not all at once in the end.  You should be writing code and testing simultaneously.  This is crucial to feature branching because it makes merges quick and painless.

## Review your commit

**Stop using "add -A ." and "commit -a"** - The problem with using these commands is that it irrationally assumes that everything in the cwd is shippable.  Chances are you have some extraneous code that you didn't remember to remove.  You probably left a "break" or a few "console.log" statements.  You might be referencing some random local library.  You may have even written your personal password in a configuration file for testing purposes.  You might have forgotten to remove a log.txt file, etc.  The rational assumption is that there is something you need to remove so use a different method.

The better way to commit code is to use "git add -p" or a GUI tool like [SourceTree](https://www.sourcetreeapp.com/).  Look through your changes and **only add what you need**.  Once you're finished adding files/changes make sure you didn't miss any untracked files and then you want to commit **without** the `-a` flag.  The `-a` flag means "all changes of tracked files" so using this will ignore all of the effort you spent manually adding blocks to commit.

## Don't Ramble in your messages

```
Hey! Do you like reading rambling paragraphs on one long line? No? Then why are you writing your commit messages like that? If you need to scroll back and forth to read the output of git log, you're doing it wrong.  Keep your message to 50 characters or less.  If you have more to say than that, put it in additional paragraphs wrapped to 72 characters.
```

[source](http://stopwritingramblingcommitmessages.com)

Commit messages should follow the 50/72 Formatting.  Your first line should be 50 characters or less and anything else should be on separate lines split at 72 characters or less.  Github and many other git based solutions will truncate your message anyway.  When we review git logs we should be able to scroll through your messages and identify which commits do what.  Read more on this [here](http://chris.beams.io/posts/git-commit/).  TL;DR: be very terse and concise.  **Hint:** It is very difficult to write small commit messages when your commits span 500 lines of code.  See "Commit often" above.

## Pull REBASE

We don't all need to know every time each developer pulls code from remote so there's no reason to mark it in the history.  When you pull without rebase you are creating a merge commit on your feature branch without even realizing it.  This happens because if git detects that remote and local have diverged then it will create a merge commit when you pull and pollute the history making it unreadable and annoying to traverse.  The only merge commits in history should be intentional merges.  Luckily there is a quick command you can run that will default to this.

```
$ git config --global pull.rebase true
```

## Merge NO FASTFARD

Merges should be merges.  I know it sounds redundant, but it's not always clear when someone does a merge.  By default, git will apply a fast-forward if it finds that the branch you are merging does not conflict with your branch.  This is confusing in some cases.  For example, if you're merging an integration branch into master you want to know when that specific merge happened.  You ***want*** a merge commit.  To prevent git from automatically converting your merges you need to apply the following configuration change.

```
[master] $ git config --global merge.ff false
```

## Rebase non-master branches by default

Merging is great for flagging your transactions between branches.  It's also mostly excessive.  Git has another way of bring two branches together and it's called rebase.  Merges take two branches and create a merge point.  Merges show that two branches were working simultaneously and one branch was pulled back into the other branch.  This is what really happened, but does it really matter in most cases?  Not really.  A rebase essentially rewrites history to make the work appear as if it was all done on the same branch.  Initially this feels like it conceals something that's necessary, but it's really not.  Does it matter if I worked on two different branches locally before I created a feature branch? No.  Does it matter that 7 feature branches were created before deciding that a single feature branch was necessary? No. In cases where the history of branch decisions doesn't matter then you should rebase.  There is one time when you want to make sure that a merge is used over a rebase and that's when bring features into master.  You want to see these things happen historically.  You want to be able to track down things like production bugs that started after a specific release.

## Merge in before you merge out

Before your merge a branch into master you should first merge master back into your branch. The goal is to keep any possible cross pollination problems out of master. If you encounter merge conflicts it's best to resolve them and re-test before you deploy. If you merge master into your branch and find that there are no conflicts then it's safe to say you can merge right back into master. Here's an example.

```
[master] $ git checkout -b feature-a
# do some awesome code work
[feature-a] $ git commit -m 'fix final issue with feature'
[feature-a] $ git pull # --rebase
# resolve any problems
[feature-a] $ git push
[feature-a] $ git fetch --all # make sure master is latest
[feature-a] $ git rebase master # put the master commits into your branch as if they were always there
# no conflicts
[feature-a] $ git checkout master
[master] $ git merge feature-a
```


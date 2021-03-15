---
layout: blog
comments: true
istop: false
code: true
title: "Use Rebase to Merge Multiple Commits"
background-image: https://i.loli.net/2021/03/15/FjLbcWCemYy4lAu.png
date:  2020-11-25 14:45:56
category: code
tags:
- git
- rebase
---

The first two commits are local commits. 

View git log:

```
$ git log
```

```sh
 yi@C02D37L9MD6R  ~/code/git/test-git   master  git status
On branch master
Your branch is up to date with 'origin/master'.

commit 5ff173eadd1090b28f97b80b2a8ec1bcc74eb21b (HEAD -> master)
Author: zhaoyi 
Date:   Thu Nov 26 11:58:52 2020 +0800

    change 2

commit b0d9c417ac5f860bcebcbd8d7cff24c3e25ecb29
Author: zhaoyi 
Date:   Thu Nov 26 11:58:38 2020 +0800

    change 1

commit 62e7bba8eb4a2e37860c34d594dd64f26093d74c (origin/master)
Author: zhaoyi 
Date:   Mon Nov 9 20:11:39 2020 +0800

    c3

commit 9537547f347f90ebba512dde619e0f502867936a
Author: zhaoyi 
Date:   Mon Nov 9 20:10:30 2020 +0800

    c1
```

Now we want to merge those two commits into a single commit.

Command:

```
$ git rebase -i HEAD~2
```

Pick the commit to be select as the final commit, use `s`  to mark those commits which are enrolled in the final commit.

```
pick b0d9c41 change 1
s 5ff173e change 2

# Rebase 62e7bba..5ff173e onto 62e7bba (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
```

edit commit message:

```
# This is a combination of 2 commits.
# This is the 1st commit message:

change 1

# This is the commit message #2:

change 2

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Thu Nov 26 11:58:38 2020 +0800
#
# interactive rebase in progress; onto 62e7bba
# Last commands done (2 commands done):
#    pick b0d9c41 change 1
#    squash 5ff173e change 2
# No commands remaining.
# You are currently rebasing branch 'master' on '62e7bba'.
#
# Changes to be committed:
#       modified:   README.md
#
```

View commits again:

```sh
commit 00dfb031b21c9820abfcb8ec936d544f5dc50919 (HEAD -> master)
Author: zhaoyi 
Date:   Thu Nov 26 11:58:38 2020 +0800

    change 1

    change 2

commit 62e7bba8eb4a2e37860c34d594dd64f26093d74c (origin/master)
Author: zhaoyi 
Date:   Mon Nov 9 20:11:39 2020 +0800

    c3

commit 9537547f347f90ebba512dde619e0f502867936a
Author: zhaoyi 
Date:   Mon Nov 9 20:10:30 2020 +0800

    c1

commit 2c0c7a703d180791e65d82c65192ed1e7ef03efe
Author: zhaoyi 
Date:   Mon Nov 9 20:09:02 2020 +0800
```


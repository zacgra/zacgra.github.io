---
title: "Git Cheatsheet"
author: zacgra
layout: post
categories: Code
tags: [git, version control, cheatsheet]
---

```sh
# Clone remote repository
git clone https://github.com/zacgra/zacgra.github.io.git

# Add changes to local repository
git add filename    # add specific file
git add .           # add all files/file changes in directory
git add -A          # add all files/file changes in directory, and remove deleted files

# Stash current changes
git stash           # moves all changes to stash, returning code state to last commit
git stash list      # view stashed entries
git stash pop       # moves stash files/files changes back to HEAD

git stash show -p stash@    # view the difference between stash and working tree
git diff stash@ master      # view differenece between stash and HEAD

git stash branch branchname stash@1 # if a git conflict occurs, create a new branch for the stash to examine

git stash drop      # delete stash entries one at at time
git stash clear     # remove all entries in stash
```

---
title: "Solve Merge Conflict during Pull Request"
tags: [git]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: merge_conflict.html
folder: git
toc: false
---

Senario: when suubmitting PR from `dev` origin branch to `master` upstream branch, and found that there's a merge conflict (Github will notify you that "Auto-merge cannot be done" in this case).

## Method 1

Merge from upstream `master` to local `dev`, solve the conflicts on local `dev`, then push to origin `dev` by force.

* In command line, git checkout to the `dev` branch

* `git fetch upstream master` (when PR destination is upstream master branch) or `git fetch origin master` (when PR destination is origin master branch)

* `git merge upstream/master` (when PR destination is upstream master branch) or `git merge master` (when PR destination is origin master branch)

  This will invoke the merge conflict onto the local machine, and the command line will show something like this:
  ```
  ... $ git merge upstream/master
  Auto-merging README.md
  CONFLICT (content): Merge conflict in README.md
  Automatic merge failed; fix conflicts and then commit the result.
  ```
  
  Essentially, `git fetch upstream master` + `git merge upstream/master` = `git pull upstream master`.

* Slove the merge conflicts on the local machine

* Run `git add`, `git commit`

* Push by force! Run `git push [origin dev] -f`. If all the conflicts were fixed correctly, then Github will be updated and show that Auto-merge is available now.

## Method 2, essentially the same as Method 1

Merge from upstream `master` to local `master`, go to local `dev`, rebase local `dev` to local `master`, solve the conflicts on local `dev`, push to orogin `dev` by force.

* `git checkout master`
* `git pull upstream master`
* `git checkout dev`
* `git rebase -i master`, 把 local `dev` 上的改动合并到 local `master` 上去
* Solve the conflicts on local `dev`
* `git push -f`, push to origin `dev` by force

## Reference

[Resolving a merge conflict on GitHub [GitHub Help]](https://help.github.com/articles/resolving-a-merge-conflict-on-github/)


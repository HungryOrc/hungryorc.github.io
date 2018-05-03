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

## Steps

* Submit PR from `dev` branch to `master` branch, and found that there's a merge conflict (Github will notify you that "Auto-merge cannot be done" in this case)

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
  
  Essentially, `fetch` + `merge` means `pull`.

* Slove the merge conflicts on the local machine

* Run `git add`, `git commit`, `git push`. If all the conflicts were fixed correctly, then Github will be updated and show that Auto-merge is available now.

## Reference

{% include links.html %}

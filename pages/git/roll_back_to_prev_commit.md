---
title: "Roll back to a previous commit"
tags: [git]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: roll_back_to_prev_commit.html
folder: git
toc: false
---

## Roll back to the latest commit

This will abandon all the current changes that haven't been staged/committed yet.

```
$ git reset --hard
...
$ git clean -fd
...
```

## Roll back to a certain previous commit

This will abandon all the current changes that haven't been committed yet, and all the commits/pushes before the designated commit. It's by force, very violent and efficient. I love it so much. No regret accepted.

```
$ git reset --hard <commidId>
...
$ git clean -f
...
```

Don't do this if you're sharing your branch with other people who have copies of the old commits.


{% include links.html %}

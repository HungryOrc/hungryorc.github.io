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

It's by force, very violent and efficient.

```
git reset --hard <commidId> && git clean -f
```

Don't do this if you're sharing your branch with other people who have copies of the old commits



{% include links.html %}

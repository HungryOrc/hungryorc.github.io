---
title: "Roll back to a certain commit"
tags: [git]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: roll_back_to_certain_commit.html
folder: git
toc: false
---

This will abandon all the current changes that haven't been committed yet, and all the commits/pushes before the designated commit. It's by force, very violent and efficient. I love it so much. No regret accepted.

```
git reset --hard <commidId> && git clean -f
```

Don't do this if you're sharing your branch with other people who have copies of the old commits.



{% include links.html %}

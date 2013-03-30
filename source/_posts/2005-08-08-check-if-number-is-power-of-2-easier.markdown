---
layout: post
title: "Check if a number is a power of 2 the easier way"
date: 2005-08-08T17:29:00+05:30
comments: true
---

This one-liner does the trick.

```
((x > 1) && !(x & (x - 1)))
```

**Clue**: "every power of 2 has only one bit set in the binary representation"

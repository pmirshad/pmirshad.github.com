---
layout: post
title: "Web Server from Any Directory in Linux"
date: 2013-07-03 17:57
comments: true
categories: ["linux", "python", "web server"]
keywords: "simple web server, instant web server, web server from any directory"
description: "instant web server from any directory in linux"
---

If you want to quickly test a website under development or serve a couple of
webpages from a local directory without the hassle of moving/linking the
directory to a web server root, the following python command helps:

<!-- more -->

Run the following command from the intended directory substituting `[port]` with
the required value. It defaults to `8000` if nothing is specified.

For Python2,

```python
python -m SimpleHTTPServer [port]
```

For Python3,

```python
python -m http.server [port]
```

---
layout: post
title: "Remove Redundant /blog Prefix from Octopress website"
date: 2013-04-14 21:56
comments: true
categories: ["octopress", "theme"]
---

Octopress, by default, generates static websites with a `/blog` prefix attached
to categories, archives and post URLs. This is particularly annoying when the
blog is hosted at `blog.yourdomain.tld` and have funny URLs like
`blog.yourdomain.tld/blog/archives`.

To get rid of this, I've created a clone of the `classic` Octopress theme with the necessary changes. It also renames `Blog` to `Home` in the navigation bar.

<!--more-->

To install and use `octopress-classic-no-blog` theme:
```bash
$ cd octopress
$ git clone git@github.com:pmirshad/octopress-classic-no-blog.git .themes/octopress-classic-no-blog
$ rake install['octopress-classic-no-blog']
```

Changes are also required in `_config.yml`. `permalink`, `category_dir`,
`pagination_dir` properties have to be modified to remove the `blog` prefix

```yml
#permalink: /blog/:year/:month/:day/:title/
permalink: /:year/:month/:day/:title/

#category_dir: blog/categories
category_dir: categories

#pagination_dir: blog  # Directory base for pagination URLs eg. /blog/page/2/
pagination_dir:       # Directory base for pagination URLs eg. /page/2/
```

After installing the theme and fixing `_config.yml`, generate the blog.

```bash
$ rake generate
```

### DIY for other themes

To achieve this for other themes, the following modifications should suffice.
[_Assumption_: The theme is already installed and you are familiar with _diffs_]

```bash
cd octopress/
```

Move `archives` folder from `source/blog` to `source/`.

```bash
mv -v source/blog/archives source/
```

Edit `source/_includes/custom/navigation.html` to fix navigation URLs.

```diff
-  <li><a href="{{ root_url }}/">Blog</a></li>
-  <li><a href="{{ root_url }}/blog/archives">Archives</a></li>
+  <li><a href="{{ root_url }}/">Home</a></li>
+  <li><a href="{{ root_url }}/archives">Archives</a></li>
```

Edit `source/index.html` to fix the _Archives_ page URL.

```diff
-    <a href="/blog/archives">Blog Archives</a>
+    <a href="/archives">Archives</a>
```

A theme in the `.themes` folder can be modified similarly, but will have to be
reinstalled with the `rake install['theme-name']` command afterwards.

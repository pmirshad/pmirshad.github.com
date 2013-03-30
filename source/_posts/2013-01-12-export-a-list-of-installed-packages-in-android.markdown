---
layout: post
title: "Export a List of Installed Packages in Android"
date: 2013-01-12 15:51
comments: true
categories: 
---

Before switching to a new ROM, I wanted to keep a list of currently installed
packages on my phone for reference. There are two ways to get this done. Both
require [__Android Debug Bridge (adb)__](http://developer.android.com/tools/help/adb.html) to be installed on the system.

<!--more-->

* Use __adb__ to get a list of installed packages and then dump the output to a
  file.

  ```adb shell pm list packages > ~/installed_packages.txt```
* The file `packages.xml` located at `/data/system/` contains all information
  about the installed packages. You can backup this to a safe location

  ```adb pull /data/system/packages.xml ~/```

This
[thread](http://android.stackexchange.com/questions/9824/how-can-i-export-a-list-of-currently-installed-applications-to-a-file) at android.stackexchange.com saved me.

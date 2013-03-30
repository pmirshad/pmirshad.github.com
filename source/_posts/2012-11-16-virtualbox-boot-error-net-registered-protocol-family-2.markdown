---
layout: post
title: "VirtualBox Boot Error - \"NET: Registered protocol family 2\""
date: 2012-11-16T21:30:00+05:30
categories:
 - Linux
---

I was trying to install Ubuntu 6.10 in VirtualBox 4.1.22 and it would get stuck during boot at the following stage,

```
"NET: Registered protocol family 2"
```

Enabling *'IO APIC'* as mentioned at
[Cloudera VirtualBox boot error NET: Registered protocol family 2](http://www.deepakkapoor.net/cloudera-virtualbox-boot-error-net-registered-protocol-family-2/) did the trick. Thanks Deepak

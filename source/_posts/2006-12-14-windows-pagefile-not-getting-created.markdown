---
layout: post
title: "PageFile not getting created @ the specified location"
date: 2006-12-14T17:35:00+05:30
categories:
 - Technology
---

There was no free space left on my windows installation drive [C:] . So I decided to migrate the page file from C: drive to D: drive.

<!--more-->

I opened up the Virtual Memory settings in the My Computer properties page and changed the page file location from C: to D:, setting a minimum size of 1000MB and a maximum size of 1024MB. Then i restarted my PC. But windows popped up an error saying that there was no page file and a page file will be created temporarily. It got created on the C: drive itself. All the registry settings pointed to the new location and the size was also correct. But still the page file never got created on the D: drive.

{% img center http://bp2.blogger.com/_1UAzLLUZnSw/RYFDho61kuI/AAAAAAAAAAM/eT7nD1GcSW4/s1600/D.Drive.jpg %}

I did eventually find the problem. The D: drive did not have permissions for SYSTEM group. Adding this group to the security list for D: drive with full permissions did the trick. On the next restart, windows created a new page file on D: drive. I deleted the old one from C: drive. It freed up 1.5GB of space.

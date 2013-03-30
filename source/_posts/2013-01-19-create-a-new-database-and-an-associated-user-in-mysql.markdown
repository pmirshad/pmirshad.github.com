---
layout: post
title: "Create a new database and an associated user in MySQL"
date: 2013-01-19 20:45
comments: true
categories: 
---

To create a new database and a user who has all privileges to that database in
MySQL, use the following sequence of commands

* Login to the MySQL instance as the admin user (root in my case). This will
  prompt for the password to be entered and drop you to the `mysql` shell
  afterwards.
```
mysql -u root -p
```

<!--more-->

* Create a new database. Don't forget the `;` at the end of commands.
```
create database my_db;
```

* Create a new user `testuser` with password as `testpasswd`
```
grant usage on *.* to testuser@localhost identified by 'testpasswd';
```

* Grant all privileges for testuser on my_db
```
grant all privileges on my_db.* to testuser@localhost;
```

* Check if the database was added. This should show `my_db` in the list
```
show databases;
```

* Exit the mysql shell and try connecting to my_db as the created user
```
mysql -u testuser -p my_db
```

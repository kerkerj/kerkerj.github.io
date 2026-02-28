---
title: "Emoji data in MySQL"
description: "How to store Emoji in MySQL: change the character set of your database, table, and columns to utf8mb4 (supports 4 bytes), and run SET NAMES utf8mb4 before each operation."
date: 2014-08-06
slug: emoji-data-in-mysql
tags: ['mysql']
---


Supported MySQL 5.5, 5.6.

First, MySQL should set DB and table to utf8mb4.

utf8 - 3 byte characters  

utf8mb4 - 4 byte characters

```sql
ALTER DATABASE my_database DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

ALTER TABLE my_table DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

ALTER TABLE my_column CHANGE my_column my_column TEXT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL;
```

Then, when inserting data or selecting data, execute this statement first:

```sql
--Execute this statement first
SET NAMES utf8mb4;

--then insert or select
SELECT * FROM my_table
```

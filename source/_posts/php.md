---
title: php
author: bro wen
date: 2018-02-19 11:01:12
tags: [PHP]
---
###  prevent SQL injection

1. convert param strings:
$db->real_escape_string();
$db->escape_string();
addslashes();

2. db prepare statement:
$stmt = $conn->prepare($sql);
$stmt->bind_param("sss", ...args);
$stmt->execute();
$stmt->close();

3. or use PDO


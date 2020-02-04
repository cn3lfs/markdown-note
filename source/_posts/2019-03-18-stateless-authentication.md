---
title: stateless_authentication
author: bro wen
date: 2019-03-18 08:18:51
tags: [Authentication]
---

## diagram

in traditional web, we use session to validate the user. but in stateless authentication, we use restful api, we don't care the connected state. we use token. token is long string in server, it has a very short amout of time. You can store it in LocalStorage. token is used to validate user. when you send request to get resource from server, you should use token.
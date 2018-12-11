---
title: 更新npm
date: 2017-07-04 08:50:46
tags:
    - Node.js
    - npm
categories: Node.js
---

更新npm版本...
<!--more-->

```bat
Set-ExectutionPolicy Unrestricted -Scope CurrentUser -Force

npm i -global --production npm-windows-upgrade

npm-windows-upgrade

npm -v
```

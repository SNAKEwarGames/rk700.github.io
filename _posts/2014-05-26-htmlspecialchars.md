---
title: htmlspecialchars
author: rk700
layout: post
categories:
  - writeup
tags:
  - wechall
  - XSS
---
<a href="http://www.wechall.net/challenge/htmlspecialchars/index.php" target="_blank">http://www.wechall.net/challenge/htmlspecialchars/index.php</a>  
单引号也需要encode，否则可以添加事件如 `onclick=alert(1)`  
所以需要给 `htmlspecialchars` 加上 `ENT_QUOTES`
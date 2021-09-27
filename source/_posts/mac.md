---
title: mac系统遇到的各种问题
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: DarkStrand.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-08-26 16:27:40
comments: true
tags: 
 - mac
keywords: mac
description: mac
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---

## brew启动mongodb时报错

> Error: Formula `mongodb-community` has not implemented #plist, #service or installed a locatable service file

1. 首先使用`brew update`更新brew
2. 如果这时候意外按下`control+z`，再次执行`brew update`会提示
  ```
    Error: Another active Homebrew update process is already in progress.Please wait for it to finish or terminate it to continue.
  ```

  解决办法
  ```
    rm -rf /usr/local/var/homebrew/locks
  ```
3. 再次更新，并启动发现成功了

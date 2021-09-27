---
title: Docker必须使用sudo操作的解决方案
date: 2021-09-27 15:20:46
tags: 
  - docker 
  - sudo
categories:
  - docker
---

创建docker组：sudo groupadd docker
将当前用户加入docker组：sudo gpasswd -a ${USER} docker
重启服务：sudo service docker restart
刷新docker成员：newgrp - docker

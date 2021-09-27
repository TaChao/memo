---
title: pass val to to tomcat container
date: 2021-09-27 15:21:00
tags:
categories: ['docker','tomcat']
---

解决方法
```
ENV CATALINA_OPTS=”-Dkey=value”
```
例子：
```
FROM tomcat:8.0-jre8
ENV spring.profiles.active=dev 
ENV CATALINA_OPTS="-Dkey=value"
ADD myWar.war /usr/local/tomcat/webapps/
CMD ["catalina.sh", "run"]
```
多个参数：
```
ENV CATALINA_OPTS="-DDB_URL=${DB_URL} -DUSR=${DB_USR} -DPASS=${DB_PW}"
```

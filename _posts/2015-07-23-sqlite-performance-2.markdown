---
layout: post
title:  "SQLite performance solutions"
date:   2015-07-23 13:21:00
categories: pixus sqlite
comments: true
---

A few days ago I wrote about some [performance problems with SQLite] which were causing me major headaches. The solution was to simply add 1 new index which covered both of the foreign keys instead of just having an index on each. With this in place the performance anomalies disappear. Sadly I'm still not sure why this causes them to occur in the first place.


[performance problems with SQLite]: {% post_url 2015-07-21-sqlite-performance %}
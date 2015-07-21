---
layout: post
title:  "SQLite performance problems"
date:   2015-07-21 13:21:00
categories: pixus sqlite
comments: true
---

One of the key requirements of pixus is a high performance database capable of handling hundreds of thousands of pieces of content along with a similar number of related meta data in other tables. Given the need for performance I decided on SQLite as I thought it would give me the best ability to perform complex queries. In general the performance blows something like core data out of the water but every now and again the whole thing falls apart and the query performance drops by a few orders of magnitude. I've found ways to work around these issues but every time I change a query I risk introducing huge performance issues.

The 2 tables at the heart of the problem:

`create table "journey_item" ("id" SERIAL NOT NULL PRIMARY KEY,"position" INTEGER NOT NULL,"last_update" BIGINT NOT NULL,"rank" DOUBLE PRECISION NOT NULL,"skipped" BOOLEAN NOT NULL,"item_id" INTEGER NOT NULL,"journey_id" INTEGER NOT NULL);`

`create table "content_items" ("id" SERIAL NOT NULL PRIMARY KEY,"full_id" VARCHAR(32) NOT NULL,"title" VARCHAR(508),"timestamp" BIGINT NOT NULL,"item_size" INTEGER NOT NULL,"http_link" VARCHAR(254),"local_url" VARCHAR(254),"creator_id" INTEGER NOT NULL,"from_id" INTEGER,"location_id" INTEGER);`

There are indexes on all primary and foreign keys.

A content_item represents a single photo and journey_items are used to rank photos in order of importance. The problem is that certain queries between these 2 objects can have massive performance differences. For the purposes of testing I've imported data into these tables. I have 15000 content_items and 29500 journey_items. Here's some indications of query times

`SELECT * FROM content_items` 76 ms

`SELECT * from journey_item` 113 ms

This proves that the amount of data being read isn't causing a major issue. Scanning a whole table isn't too slow with this amount of data.

`SELECT * FROM content_items ci INNER JOIN journey_item ji ON ji.item_id = ci.id` 266 ms

`SELECT * FROM content_items ci LEFT OUTER JOIN journey_item ji ON ji.item_id = ci.id` 267 ms

`SELECT * FROM journey_item ji INNER JOIN content_items ci ON ji.item_id = ci.id` 296 ms

Joins are a little slower but still fairly sensible. Now here's where things get a little strange.

`SELECT * FROM journey_item ji INNER JOIN content_items ci ON ji.item_id = ci.id ORDER by ji.id` 335 ms

`SELECT * FROM content_items ci INNER JOIN journey_item ji ON ji.item_id = ci.id ORDER by ji.id` 338 ms

`SELECT * FROM content_items ci LEFT OUTER JOIN journey_item ji ON ji.item_id = ci.id ORDER by ji.id` 669 ms

The order by is a little slower but the last query almost doubles in time. The only difference between 2 & 3 is the type of join which had no effect before but now doubles the query time.

`SELECT * FROM content_items ci INNER JOIN journey_item ji ON ji.item_id = ci.id WHERE ji.journey_id = 1` 167 ms

`SELECT * FROM content_items ci LEFT OUTER JOIN journey_item ji ON ji.item_id = ci.id WHERE ji.journey_id = 1` 210002 ms

Now I'm just completely lost. Nothing I know can explain these results. Filtering should reduce the number of rows returned and make the query faster as demonstrated in the first query. But the second takes ~1200 times longer. I'm really hoping someone out there can give me an answer and that my next blog post can be titled SQLite performance solutions. 



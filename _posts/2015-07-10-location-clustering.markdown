---
layout: post
title:  "Location clustering"
date:   2015-07-10 11:48:00
categories: pixus
comments: true
---

![Location Filter]({{ site.url }}/assets/IMG_0094.jpg)

One of the important aspects of pixus is the ability to search for images by location. There's already a location filter in place but it feels a bit boring. What this screen needs is a way to see how many photos there are for a particular location. I don't want to just display each location on the map because that will look awful so I need an algorithm which can take a list of points and group them sensibly.

k-means
---------------------------

My first thought was to use the [k-means clustering] algorithm that I had read about once before. After some investigation I decided against it because many implementations do not let you specify the distance formula and picking a good value for the number of clusters was going to be difficult.

Google to the rescue
------------------------

After some more searching I found a [well written blog post] which seemed to solve my problem exactly. All I had to do was get my list of locations, put them into a quad tree and let then let the algorithm I had do the clustering. The location list was easy: `SELECT loc.* FROM location loc INNER JOIN content_items ci on ci.location_id = loc.id`. To create the quad tree I just followed along with what the original codebase did and removed the parts that I didn't need. With that done everything was looking fine:

![Location Filter]({{ site.url }}/assets/IMG_0095.jpg)

But this highlighted another problem in my code; While the marker said 3 only a single item was being returned in the search. The culprit seemed like it was going to be a previous hack to filter locations in code rather than at the database level. Now I've spent the last 3 days trying to work out why sqlite performance dies with certain queries so the prospect of adding a distance formula into a where statement wasn't one I was looking forward to. 

Dealing with Sqlite
-----------------------

As expected I hit a roadblock pretty quickly with sqlite. It doesn't have many inbuilt functions, in particular it lacks `SQRT`, `POW` and `COS` functions. I could live without the first 2 but not without `COS`. So now I was at a point where I had to choose between getting custom functions working in sqlite or using [SpatialLite]; I found a [post on the subject of creating a distance function in sqlite] so I decided to try that out before switching to a new database. This actually caused far less issues than I was expecting and now my location filtering is done at the database level without any noticeable performance degradation. 

Finishing up
------------------------

With everything working it was just a matter of adding some animations and making sure the map reloads as new data comes in. Suddenly the app feels like location is an important feature as opposed to a side note. A good way to end the week.


[k-means clustering]:      https://en.wikipedia.org/wiki/K-means_clustering
[well written blog post]: https://robots.thoughtbot.com/how-to-handle-large-amounts-of-data-on-maps
[SpatialLite]: https://en.wikipedia.org/wiki/SpatiaLite
[post on the subject of creating a distance function in sqlite]: http://www.thismuchiknow.co.uk/?p=71

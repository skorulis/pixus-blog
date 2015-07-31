---
layout: post
title:  "Content Freshness"
date:   2015-07-31 00:23:00
categories: pixus ranking technical
comments: true
---

Yesterday I explained the concept behind [image journeys]. In order for this to work the app needs to constantly update content to keep it fresh and well targeted to what the user wants to see. In a traditional centralised app, a server would create various lists of content for consumption and the client would merely fetch whichever of those lists it was interested in. In pixus the client has to take care of this itself and because of the distributed nature of the content this becomes slightly more complicated. There are 3 separate pieces involved to make this system work

1. Connecting to users who have the best content
1. Sending information in fetch requests to retrieve the desired content
1. Ranking the retrieved content so the best images are quickly surfaced


The biggest issue with each of these is that it can't be done for an entire set of available data due to space and time constraints. Currently the number of users and amount of content is fairly small so 3 is the highest priority.


Ranking Content
--------------------------

In order to rank content, every image the app finds is given a score. This score is based off of many factors like age, popularity and tags. In fact an image will most likely have multiple scores calculated but one of these is special. This special score is called the `baseScore`. This is a baseline calculation that is not linked to any journey and is used at a later stage of the process. This way when a journey needs to find better content it doesn't have to search through the entire image set blindly but can use the `baseScore` as a guide. This `baseScore` is also used to decide what to send when another user requests images.

BaseScore Freshness
---------

In order for the `baseScore` to be useful it needs to be kept up to date so items will likely be scored multiple times. The first ranking occurs when a new image is received so that every image is guaranteed to have a score. After that the item will be periodically involved in 1 of 2 ranking passes. The first takes the top unseen images by score and recalculates the rank. This is important because it bumps images down from the top of the list quickly if required. The second gradually checks every image in order of when it was last checked. This is important so that every image is periodically touched so nothing can ever sink to the bottom of the list and get stuck there forever.

Journey Freshness
-----------

With a `baseScore` in place the app can now intelligently score images within a journey. There is no need in this case to score every single image as this would potentially create an unmanageable number of entries. There are still 2 other passes that need to be done but they are slightly different from the base scoring. The first is to take the top items from the journey that have not been seen and calculate the score. This once again stops any incorrectly scored items staying at the top of the list. The second pass takes the top items by `baseScore` that have not yet been ranked within this journey. 

Which Journey to Rank
--------

Since a user can have many journeys it would be too time consuming to do this process for every journey all the time so there needs to be a system to pick which journey is most in need to attention. The first thing to do here is to define what would classify a journey as needing to be ranked:

* It's currently being viewed by the user
* The user has already consumed all the photos in the particular journey
* The next items to be consumed have low ratings
* It was recently viewed
* It hasn't been ranked recently

The most important thing here is that the algorithm doesn't get stuck in a loop of constantly picking 1 journey. The last point should help after some fine tuning on the weight of each input. Realistically this is never going to be perfect but as long as it's good enough then the things it will keep the app running.

And that's all the pieces in place for the time being. The app is getting very close to being usable outside a testing environment. I have a feeling the next post will cover setting up a production server.




[image journeys]: {% post_url 2015-07-29-image-journeys %}

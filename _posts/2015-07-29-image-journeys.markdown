---
layout: post
title:  "Image Journeys"
date:   2015-07-29 04:29:00
categories: pixus journey
comments: true
---

I've been using the word "journey" when describing the way users consume content in pixus a lot lately so I thought I would take a bit of time to describe exactly what I mean when I use this term. The concept of a journey is the laid back companion of search. When you do a search you know what you want before you start where as with a journey you start viewing content and then refine what it is you want to see as you go. With the amount of new content constantly being created on the Internet I think there is a real need for this kind system. 

While this idea is similar to traditional content aggregation, existing services provide predefined filters and don't create a journey that is fully tailored to an individual user. 

Current design
----

![Current design of the journey list]({{ site.url }}/assets/IMG_0464.jpg){: .center-image }

This is the top level of the journey system. This screen lets the user see previously created journey's as well create new ones or delete unwanted ones. The icons here give an indication of how many photos have been viewed as well as some hints to what kind of time or location filters have been used. Clicking on a journey takes the user into the photo view.

![Current design of the journey stream]({{ site.url }}/assets/IMG_0466.jpg){: .center-image }

This is the main guts of the app where users interact with images. Each journey is a fixed list of photos that the user has looked at and a constantly adjusting list of photos yet to be viewed. As the user votes or new content comes in the best content will be put on the top of the unviewed list. The icons at the top let the user get more information about a photo and provide interaction.


Thoughts for the future
-----

One thing that needs some thought is how a journey can change over time. Right now when a journey is created it is a fixed set of filters that adjusts ratings based on previous actions. But I can see various times when the user may want to adjust the filters for a journey. For now the solution of creating a new journey works but I think there's a solid idea around a journey not just being comprised of the images being looked at but also actions that significantly change the journey. 
---
layout: post
title:  "What is pixus anyway?"
date:   2015-07-07 11:48:00
categories: pixus
comments: true
---

I've had an idea rattling around my brain for a long time of an app based around peer to peer content delivery. Pixus is my attempt to make this idea a reality. The initial idea was fairly generic in what that content would be, for pixus I've chosen to start with images as they have a few favourable properties:

* There's a constant new stream of content being produced
* All platforms display images without any effort
* There's no need for streaming
* There's less of a copyright issue compared to music or video


Why peer to peer?
---------------------

Most of the web is built around centralised servers with clients being essentially nothing more than terminals to view and edit the information on the server. This model is nice because you have a central source of truth and trust, but you also have a single point of failure and as demand grows the server has to expand to keep up which takes both technical effort and recurring costs. As a lone developer with an idea of questionable value (aren't they all?) I'm not prepared to setup and pay for the infrastructure to handle a large number of users. 

Luckily, with p2p the bulk of the work is pushed onto the clients so the app can support a large number of users without having to worry too much about the performance and scalability of the server. Being primarily an iOS developer this greatly appeals to me. I can spend more time working on my client and less time on the server. If I ever get pixus working it will be in a unique position to provide similar functionality to traditional social networks at a fraction of the cost.


What's in it for the user?
---------------------

This is the million (billion?) dollar question. The technical challenge appeals to me and business aspects of pixus open up some interesting opportunities, but without something to appeal to the end user it means nothing. So what can a p2p network offer users that traditional server models can't? On one hand the answer is sadly nothing. Anything that p2p can do can be done using a central server. But certain things are cost prohibitive so while they can be done, it may not be possible to offer as a free service. In the case of pixus I'm hoping to use p2p to deliver high quality photo recommendations in a way that would be infeasible for a centralised service. Every time a user votes on a photo the following photos are reranked to give the user a constantly updated feed. 



How does it all fit together
----------------------------

So far I've only brushed the surface of what pixus is and why it's interesting but haven't discussed any specifics of what pixus will do. So here's the plan for the first version:

* Lets users share their photo library
* Users can search for photos by location or time
* The system learns what the user is interested in and find photos appropriately
* The network learns what users have similar tastes and pairs them up

This is enough for a proof of concept to validate if my idea is even possible. There's a lot of other features I've got in mind but getting the core experience right is much more important. 





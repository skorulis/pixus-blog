---
layout: post
title:  "Journey filters"
date:   2015-08-04 04:43:00
categories: pixus journey
comments: true
---

In the rush to get an alpha version of pixus out many parts of the app are getting a much needed rework. I built a lot of screens to give me the functionality I needed to test my ideas without worrying too much about how they looked or felt. One such screen is the journey setup screen which is rather difficult to understand right now.

![The awful looking journey setup screen]({{ site.url }}/assets/IMG_0471.PNG){: .center-image }

The top two icons are to set the location and time filters and the next 3 switches are for various options. The bottom arrow starts the journey and goes to the first photo. None of it is easy to understand and use and on top of that I know that I will want to add more things to this screen in the future which is just going to make it more cluttered. There's not even a title to tell you what you're doing. It took me a while looking at this screen to decide exactly how I could solve all my problems but then the answer seemed obvious. Rather than trying to place buttons all over the screen, each filter option should be a row which can be selected. In that way new options can be easily added, each row will have enough space to make the text a bit more explanatory and it will bring some much needed consistency to the screen.

![A little better]({{ site.url }}/assets/IMG_0472.PNG){: .center-image }

It's already looking better just by changing it to a list. A little bit of padding adjustment, a few text changes and a change to the bottom button and it's all finished. I'll probably come back and make a few changes later on but for the alpha version it will work nicely.

![A little better]({{ site.url }}/assets/IMG_0473.PNG){: .center-image }
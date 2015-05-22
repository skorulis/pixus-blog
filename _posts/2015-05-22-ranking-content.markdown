---
layout: post
title:  "Ranking content"
date:   2015-05-22 11:48:00
categories: pixus ranking
---

One of the main elements I want in pixus is to be able to easily customise your image browsing experience via simple sliders. The goal here is to take away exact values so the user can just give an indication of what they are interested in. Below is a rough  example of wanting new content nearby without caring about how popular it is

![Ranking v1]({{ site.url }}/assets/IMG_0340.PNG)

First lets define the inputs to the algorithm. In my system I have 3 separate pieces of information; Location, Newness and Popularity which I will refer to as `L`, `N` and `P`. The final rating of a piece of content is then `Rank = L + N + P` with higher values being better. The user assigns weights to each of these variables, I will call these `Lw`,`Nw` and `Pw`. Each of these has a value `0..1` which is simply the value of the slider.

It's important now to decide how to interpret the weighting values. 0 is easy, this means that the contribution will be zero and should be ignored. Let's say 0.5 means 'take this moderately into account' and 1 means 'this should dominate the rating'. From this definition there is a much bigger gap between 0.5 and 1 than simply doubling the value which points to a non linear rating scheme. 

While higher weights should contribute more they should also be more localised. So with `Lw` at 0.5 the drop off radius will be much larger than at 1. This does mean that for some inputs it will be possible that choosing a higher weight will actually result in a lower output which I don't see as causing any problems. In order to allow this to be tweaked I decided to specify 3 values.

* `max` is the largest useful value. Above this the rating will be 0
* `min` is the smallest possible value. Below this the rating will be 1
* `mid` is used in conjunction with `min` to generate `pivot`. This value essentially replaces min as the weight drops.

At this point I think I'm ready to write my code and see how the results turn out. Overall the rankings are much better than they were previously with simple linear rankings but I'm still not convinced that this is the best way to do things. Right now it's really hard to tell because the app depends on a large number of users to contribute meta data which I just don't have right now but at least when I use the app myself the photos I'm seeing make sense.


Here's the final code I ended up with in case anyone wants to use it.

{% highlight objc %}

@import Foundation;

@interface ValueRanker : NSObject

@property (nonatomic,readonly) double maxPoint;
@property (nonatomic,readonly) double minPoint;
@property (nonatomic, readonly) double midPoint;

@property (nonatomic,readonly) double weight;

- (instancetype) initWithMax:(double)max mid:(double)mid min:(double)min weight:(double)weight;

- (double) rank:(double)value;

+ (ValueRanker*) locationRanker:(double)weight;
+ (ValueRanker*) timeRanker:(double)weight;
+ (ValueRanker*) popularityRanker:(double)weight;

@end


{% endhighlight %}

{% highlight objc %}

#import "ValueRanker.h"

static const double kLocationMax = 1000; //1000 km
static const double kLocationMid = 10; //10 km
static const double kLocationMin = 1.5; //1.5 km

static const double kTimeMax = 60 * 60 * 24 * 100; //100 days
static const double kTimeMid = 10 * 60 * 60; //10 hours
static const double kTimeMin = 1 * 60 * 60; //1 hours

static const double kPopularityMax = 1;
static const double kPopularityMid = 0.5;
static const double kPopularityMin = 0.01;


@interface ValueRanker ()

@property (nonatomic, readonly) double pivot;

@end

@implementation ValueRanker

- (instancetype) initWithMax:(double)max mid:(double)mid min:(double)min weight:(double)weight {
    self = [super init];
    NSParameterAssert(max >= mid);
    NSParameterAssert(mid >= min);
    NSParameterAssert(min > 0);
    _maxPoint = max;
    _weight = weight;
    _minPoint = min;
    _midPoint = mid;
    _pivot = weight * min + (1 - weight) * mid;
    return self;
}

- (double) rank:(double)value {
    if(_weight == 0) { return 0; }
    if(value >= _maxPoint) { return 0;}
    
    if(value <= _pivot) {
        return _weight*_weight;
    }
    
    double r1 = _pivot / value;
    
    return sqrt(r1) * _weight*_weight;
}

+ (ValueRanker*) locationRanker:(double)weight {
    return [[ValueRanker alloc] initWithMax:kLocationMax mid:kLocationMid min:kLocationMin weight:weight];
}

+ (ValueRanker*) timeRanker:(double)weight {
    return [[ValueRanker alloc] initWithMax:kTimeMax mid:kTimeMid min:kTimeMin weight:weight];
}

+ (ValueRanker*) popularityRanker:(double)weight {
    return [[ValueRanker alloc] initWithMax:kPopularityMax mid:kPopularityMid min:kPopularityMin weight:weight];
}

@end

{% endhighlight %}


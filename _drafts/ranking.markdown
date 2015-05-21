---
layout: post
title:  "Ranking content"
date:   2015-05-12 12:00:00
categories: pixus ranking
---

One of the main elements I want in pixus is to be able to easily customise your image browsing experience via simple sliders. The goal here is to take away exact values so the user can just give an indication of what they are interested in. Below is a rough  example of wanting new content nearby without caring about how popular it is

![Ranking v1]({{ site.url }}/assets/IMG_0340.PNG)

First lets define the inputs to the algorithm. In my system I have 3 separate pieces of information; Location, Newness and Popularity which I will refer to as `L`, `N` and `P`. The final rating of a piece of content is then `R = L + N + P` with higher values being better. The user assigns weights to each of these variables, I will call these `Lw`,`Nw` and `Pw`. Each of these has a value `0..1` which is simply the value of the slider. When talking about a rating piece generically I will refer to it as `V`. Input values are designated as `Vi`.

It's important now to decide how to interpret the weighting values. 0 is easy, this means that the contribution will be zero and should be ignored. Let's say 0.5 means 'take this moderately into account' and 1 means 'this should dominate the rating'. From this definition there is a much bigger gap between 0.5 and 1 than simply doubling the value which points to a non linear rating scheme.

With the input worked out now it's time for the output. Originally I was going to rank each piece `0..1` with the ranking being `0..3` but this feels a bit constrictive. Having location at 1 could be outweighed by the other 2 categories at 0.5 and 0.51 which doesn't feel right. To get around this I'm going to break my ratings into 2 parts, `Vlow` and `Vhigh`. `Vlow` is going to be `0..1` and on a linear scale. `Vhigh` is going to be `0..x` on a non linear scale where `x` is a tweaking variable. The purpose of low is the give a baseline while high is meant to work on the top part of the scale. As the user reduces `Vw` the value of `Vhigh` should rapidly diminish. Also the range on which `Vhigh` operates should simultaneously increase.

So what does this really all mean. It means that the difference between a photo being 10km away and 50km away should be huge when `Lw` is 1 but only minor when `Lw` is 0.5. It also means that when `Lw` is 1 the difference between 5km and 10km should be huge while the difference between 20km and 25km should be minor. While at 0.5 the difference may be quite similar.

This requires some new variables to be declared. `Vmax` and `Vmid`. `Vmax` is the maximum value that I care about, above `Vmax` the result should always be 0. `Vmid` is the point where `Vhigh` starts to kick in. `Vmid` approaches `Vmax` as `Vw` approaches 0.

At this point I think I'm ready to write my code and see how the results turn out.

{% highlight objc %}

@import Foundation;

FOUNDATION_EXPORT const double kLowValueRank;
FOUNDATION_EXPORT const double kHighValueRank;

@interface ValueRanker : NSObject

@property (nonatomic) double maxPoint;
@property (nonatomic) double midPoint;

@property (nonatomic) double weight;

- (instancetype) initWithMax:(double)max mid:(double)mid weight:(double)weight;

- (double) rank:(double)value;

@end


{% endhighlight %}

{% highlight objc %}

#import "ValueRanker.h"

const double kLowValueRank = 1;
const double kHighValueRank = 5;


static const double kMidMovementPower = 3;
static const double kHighMaxPower = 3;

@implementation ValueRanker

- (instancetype) initWithMax:(double)max mid:(double)mid weight:(double)weight {
    self = [super init];
    _maxPoint = max;
    _weight = weight;
    _midPoint = [self calculateMid:mid];
    return self;
}

- (double) calculateMid:(double)fullMid {
    double gap = (_maxPoint - fullMid);
    double mult =  pow((1 - _weight),kMidMovementPower);
    
    return fullMid + mult * gap;
}

- (double) rank:(double)value {
    if(_weight == 0) { return 0; }
    if(value >= _maxPoint) { return 0;}
    
    double rankLow = (_maxPoint - value)/_maxPoint;
    double rankHigh = 0;
    if(value < _midPoint) {
        rankHigh = pow((_midPoint - value)/_midPoint, kHighMaxPower*_weight);
    }
    
    return (rankLow*kLowValueRank + rankHigh*kHighValueRank) * _weight;
}

@end

{% endhighlight %}


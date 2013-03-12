---
layout: post
title: "Analyzing Repeated-Measures Data in R"
date: 2012-04-18 19:00
comments: true
categories: [R, lme4, repeated-measures]
published: false
---
## Overview
{% blockquote Judd, McClelland, & Ryan http://www.amazon.com/Data-Analysis-Comparison-Approach-Edition/dp/0805833889/ref=sr_1_1?ie=UTF8&qid=1333736761&sr=8-1 Data Analysis %}
We have encountered students who have bludgeoned their data to fit an orthogonal 
analysis of variance with equal sample sizes because it is the only test they 
know.
{% endblockquote %}

## Mixed-Effect Model
``` R Analyzing Repeated-Measures using lme4
require(lme4)
data()
fm1 <- 
```

## You Want Multiple Comparisons Too??
``` R Using multcomp
require(multcomp)

```

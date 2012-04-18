---
layout: post
title: "Graphing Error Bars for Repeated-Measures Variables with ggplot2"
date: 2012-03-07 10:37
comments: true
categories: [R, ggplot2, repeated-measures]
published: true
---
When presenting data, confidence intervals and error bars let the audience know the amount of uncertainty in the data, and
see how much of the variance is explained by the reported effect of an experiment.  While this is straightforward for
between-subject variables, it's less clear for mixed- and repeated-measures designs.

Consider the following.  When running an ANOVA, the test accounts for three sources of variance:  1) the fixed effect of the condition, 2) the 
ability of the participants, and 3) the random error, as data = model + error.  Plotting the repeated-measures without taking the
different sources of variance into consideration would result in overlapping error bars that include between-subject variability, confusing the presentation's audience.
While the ANOVA partials out the differences between the participants and allow you to assess the effect of the 
repeated-measure, computing a regular confidence interval by 
multiplying the standard error and the F-statistic doesn't work in this way.

Winston Chang has developed a set of R functions based on Morey (2008) and Cousineau (2005) on his wiki that help deal with this problem, where the sample variance is 
computed for the normalized data, and then multiplied by the sample variances in each condition by M(M-1), where M is the 
number of within-subject conditions.

[See his wiki here](http://goo.gl/9rD3P) for more info.
<!-- more -->
### References
Morey, R. D. (2008). *Confidence Intervals from Normalized Data:  
A correction to Cousineau (2005).*

Cousineau,D. (2005). *Confidence intervals in within‐subject
designs:A simpler solution to Loftus and Masson’s
method.*

Loftus, G.R., & Masson, M.E.J. (1994). *Using confidence
intervals in within‐subject designs.*

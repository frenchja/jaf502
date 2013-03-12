---
layout: page
title: "Multiple Regression in R"
date: 2013-03-11 18:50
comments: true
sharing: true
footer: true
---

This R Guide for multiple regression hypothesis testing was created by [Jason A. French](mailto:frenchja@u.northwestern.edu).  Email him with any comments and suggestions.

Part 1
-------

A political scientist is interested in factors influencing judgments of political candidates. To explore these issues, he uses some national survey data collected from a randomly drawn sample of 989 voting age citizens. Data on each of the following five variables are available from each of these survey respondents:

CAND  : The respondent’s judgment of the liberalness/conservativeness of a particular candidate . Respondents make their judgments of the candidate on a seven point scale, with 1 defined as “very conservative” and 7 defined as “very liberal.”

SELF :  Respondent’s judgment of his or her own liberalness/conservativeness on the same seven point scale as for CAND.

LIKE  :  Respondent’s judgment of liking for the candidate. Judgments are on a 100 point scale with 0 indicating the respondent very much does not like the candidate and 100 indicating the respondent very much likes the candidate.

TRUST  :  Respondent’s judgment of the candidate’s trustworthiness on a seven-point scale where 1 indicates “very untrustworthy” and 7 indicates “very trustworthy.”

Know  :  A composite measure of the respondent’s political knowledge, ranging from 0 to 10, with a score of 10 indicating that the respondent is very knowledgeable about political issues and a score of 0 indicating that the respondent knows nothing about politics.

Based on these data, the researcher wishes to conduct analyses to answer each of the following questions. For each question, indicate the compact and augmented models that should be compared to answer the question and state the null hypothesis being tested. In addition, indicated the degrees of freedom associated with the F* that would be used to evaluate each hypothesis. Some of these questions may require that you construct new variables out of the five that were measured.

## 1.1
Are judgments of liking for the candidate predictable from the respondent’s self judgment of liberalness/conservativeness?

Model C:  $Like_i = \beta_0 + \epsilon_i$

Model A:  $Like_i = \beta_0 + \beta_1 Self_i + \epsilon_i$

$F^*_{1,987} = H_0: \beta_1 = 0$

## 1.2
Are judgments of the candidate’s trustworthiness related to candidate evaluation even when controlling for the respondent’s own political persuasion?

Model C:  $Like_i = \beta_0 + \beta_1 Self_i + \epsilon_i$

Model A:  $Like_i = \beta_0 + \beta_1 Self_i + \beta_2 Trust_i + \epsilon_i$

$F^*_{1,986} = H_0: \beta_2 = 0$

## 1.3
Are judgments of how the candidate is evaluated predictable from trustworthiness judgments less strongly among politically knowledgeable respondents?

Model C:  $Like_i = \beta_0 + \beta_1 Trust_i + \beta_2 Know_i + \epsilon_i$

Model A:  $Like_i = \beta_0 + \beta_1 Trust_i + \beta_2 Know_i + \beta_3 (Know_i)(Trust_i) + \epsilon_i$

$F^*_{1,985} = H_0: \beta_3 = 0$

## 1.4
Do those respondents whose political views are relatively extreme in either a liberal or conservative direction, like the candidate less well than those who are more moderate, even controlling for the fact that liberals prefer the candidate to those who are more conservative? (Hint: Draw a graph of this hypothesis.)

Model C:  $Like_i = \beta_0 + \beta_1 Self_i + \epsilon_i$

Model A:  $Like_i = \beta_0 + \beta_1 Self_i + \beta_2 Self^2_i + \epsilon_i$

$F^*_{1,986} = H_0: \beta_2 = 0$

## 1.5
On average, do respondents assimilate judgments of the candidate’s position to their own position? That is, the more liberal respondents are, the more they judge the candidate to be liberal. The more conservative respondents are, the more conservative they judge the candidate to be.

Model C:  $Cand_i = \beta_0 + \epsilon_i$

Model A:  $Cand_i = \beta_0 + \beta_1 Self_i + \epsilon_i$

$F^*_{1,987} = H_0: \beta_1 = 0$

## 1.6
Does the extent to which respondents assimilate judgments of the candidate’s position to their own depend on the degree to which they like or dislike the candidate? Those who relatively like the candidate assimilate the candidate’s position to their own more strongly than those who like the candidate less well.

Model C:  $Cand_i = \beta_0 + \beta_1 Self_i + \beta_2 Like_i + \epsilon_i$

Model A:  $Cand_i = \beta_0 + \beta_1 Self_i + \beta_2 Like_i + \beta_3 (Self_i)(Like_i) + \epsilon_i$

$F^*_{1,985} = H_0: \beta_3 = 0$

## 1.7
Is liking for the candidate predictable from knowledge of the absolute difference between the respondent’s judgment of his or her own political position and his or her judgment of the candidate’s political position?
First, create a new variable:  $Diff_i = |Self_i - Cand_i|$

Model C:  $Cand_i = \beta_0 + Error_i$

Model A:  $Cand_i = \beta_0 + \beta_1 Diff_i + \epsilon_i$

$F^*_{1,987} = H_0: \beta_1 = 0$

## 2.1
See if there is an interaction between Books and Classes in the prediction of Grade. Interpret your results. Draw a graph to display the interaction, whether or not it is significant.


```r
library(psych)
# Since Excel files are garbage, convert it to a .csv file
books.data <- read.csv(file = "~/Desktop/Statslearninteractionexamp.csv", header = TRUE)
# Now center the Attend and Book variables
books.data$Attend <- scale(books.data$Attend, center = TRUE, scale = FALSE)
books.data$Books <- scale(books.data$Books, center = TRUE, scale = FALSE)
books.lm <- lm(Grade ~ Books + Attend + Books * Attend, data = books.data)
summary(books.lm)
```

```
## 
## Call:
## lm(formula = Grade ~ Books + Attend + Books * Attend, data = books.data)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -24.84 -11.85   1.47   8.16  34.20 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)    61.602      2.319   26.57   <2e-16 ***
## Books           4.155      1.678    2.48    0.018 *  
## Attend          1.333      0.562    2.37    0.023 *  
## Books:Attend    0.735      0.349    2.10    0.042 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
## 
## Residual standard error: 13.4 on 36 degrees of freedom
## Multiple R-squared: 0.402,	Adjusted R-squared: 0.352 
## F-statistic: 8.07 on 3 and 36 DF,  p-value: 0.000306
```

```r
anova(books.lm)
```

```
## Analysis of Variance Table
## 
## Response: Grade
##              Df Sum Sq Mean Sq F value  Pr(>F)    
## Books         1   2634    2634   14.57 0.00051 ***
## Attend        1    944     944    5.22 0.02827 *  
## Books:Attend  1    800     800    4.43 0.04246 *  
## Residuals    36   6506     181                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


Graphs: Draw the three representative line graphs of Grade by the centered Books at the centered Attend = average (0), 1sd below from the average (-4.278), and 1sd above from the average (4.278) using the “simple” regression equation:

$\hat{Grade} = (61.60 + 1.333Attend_0) + (4.155*(0.735*Attend_0))Books_0$

If we plug in three representative values of Attend0...

Attend0 = -4.278

$\hat{Grade} = = (61.60 + 1.333*-4.278) + (4.155 + (0.735*-4.278))*Books_0$

Attend0 =0

$\hat{Grade} = (61.60 + 1.333*0) +(4.155+(0.735*0))*Books_0$

Attend0 = 4.278

$\hat{Grade} = (61.60 + 1.333*4.278) + (4.155 + (0.735*4.278))*Books_0$

For the people who attended fewer classes than average, the number of textbooks they read was rather weakly related with their grade.  On the other hand, for the people who attended more classes than average, the number of textbooks they read had a strong positive relationship with their grade. 

```r
library(ggplot2)
p <- qplot(x = Books, y = Grade, data = books.data, main = "Grade ~ Books", 
    legend = TRUE)
p + geom_abline(intercept = (61.6 + 1.333 * -4.278), slope = (4.155 + (0.735 * 
    -4.278)), aes(colour = "1")) + geom_abline(intercept = (61.6 + 1.333 * 0), 
    slope = (4.155 + (0.735 * 0)), aes(colour = "2")) + geom_abline(intercept = (61.6 + 
    1.333 * 4.278), slope = (4.155 + (0.735 * 4.278)), aes(colour = "3"))
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


## 2.2
See if either Books or Classes has a curvilinear (quadratic) effect in the prediction of Grade. Interpret your results. Provide a graph for one of these results.

### Let's do Grades ~ Attend first
$\hat{Grade} = b_0 + b_1 Attend + b_2 Attend^2$


```r
attend.lm <- lm(Grade ~ Attend + I(Attend^2), data = books.data)
summary(attend.lm)
```

```
## 
## Call:
## lm(formula = Grade ~ Attend + I(Attend^2), data = books.data)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -28.58 -10.09   2.09   9.93  33.84 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   61.353      3.610   17.00   <2e-16 ***
## Attend         1.892      0.558    3.39   0.0017 ** 
## I(Attend^2)    0.123      0.153    0.80   0.4270    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
## 
## Residual standard error: 14.9 on 37 degrees of freedom
## Multiple R-squared: 0.246,	Adjusted R-squared: 0.205 
## F-statistic: 6.02 on 2 and 37 DF,  p-value: 0.00543
```

```r
anova(attend.lm)
```

```
## Analysis of Variance Table
## 
## Response: Grade
##             Df Sum Sq Mean Sq F value Pr(>F)   
## Attend       1   2531    2531   11.40 0.0017 **
## I(Attend^2)  1    143     143    0.65 0.4270   
## Residuals   37   8210     222                  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
p <- qplot(x = Attend, y = Grade, data = books.data, main = "Polynomial Fit of Grade ~ Attend")
p + layer(data = books.data, mapping = aes(x = Attend, y = Grade), stat = "smooth", 
    stat_params = list(method = "glm", formula = y ~ poly(x, 2)))
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

*Attendance* does not have a curvilinear relationship with Grades, $p = n.s.$

### Now we'll test Grades ~ Books

$\hat{Grade} = b_0 + b_1 Books + b_2 Books^2$

```r
books.lm2 <- lm(Grade ~ Books + I(Books^2), data = books.data)
summary(books.lm2)
```

```
## 
## Call:
## lm(formula = Grade ~ Books + I(Books^2), data = books.data)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -27.61 -12.97  -1.49  10.84  33.04 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   63.961      3.678   17.39   <2e-16 ***
## Books          5.738      1.669    3.44   0.0015 ** 
## I(Books^2)    -0.205      1.411   -0.15   0.8850    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
## 
## Residual standard error: 14.9 on 37 degrees of freedom
## Multiple R-squared: 0.242,	Adjusted R-squared: 0.201 
## F-statistic: 5.92 on 2 and 37 DF,  p-value: 0.00588
```

```r
anova(books.lm2)
```

```
## Analysis of Variance Table
## 
## Response: Grade
##            Df Sum Sq Mean Sq F value Pr(>F)   
## Books       1   2634    2634   11.82 0.0015 **
## I(Books^2)  1      5       5    0.02 0.8850   
## Residuals  37   8246     223                  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
p <- qplot(x = Books, y = Grade, data = books.data, main = "Polynomial Fit of Grade ~ Books")
p + layer(data = books.data, mapping = aes(x = Books, y = Grade), stat = "smooth", 
    stat_params = list(method = "glm", formula = y ~ poly(x, 2)))
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

*Books* do not have a curvilinear relationship with Grades, $p = n.s.$

## 3
Download the excel file “crime.xls” from the course website and import it into your favorite statistics program. Test to see whether the set of predictors “Burglary” and “Auto”(Theft) add to the prediction of “Murder” once “Assault” and “Robbery” are taken into account (i.e., “over and above Assault and Robbery”).

Model C: $\hat{Murder} = b_0 + b_1 Assault + b_2 Robbery$

Model A: $\hat{Murder} = b_0 + b_1 Burglary + b_2 Auto + b_3 Assault + b_4 Robbery$


```r
murder.data <- read.csv(header = TRUE, file = "Crime.csv")
modelC <- lm(murder ~ assault + robbery, data = murder.data)
summary(modelC)
```

```
## 
## Call:
## lm(formula = murder ~ assault + robbery, data = murder.data)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -5.124 -1.890 -0.511  1.807  7.780 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  2.00024    0.98772    2.03  0.04856 *  
## assault      0.02120    0.00506    4.19  0.00012 ***
## robbery      0.00777    0.00574    1.35  0.18245    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
## 
## Residual standard error: 2.95 on 47 degrees of freedom
## Multiple R-squared: 0.442,	Adjusted R-squared: 0.419 
## F-statistic: 18.6 on 2 and 47 DF,  p-value: 1.1e-06
```

```r
modelA <- lm(murder ~ assault + robbery + auto + burglary, data = murder.data)
summary(modelA)
```

```
## 
## Call:
## lm(formula = murder ~ assault + robbery + auto + burglary, data = murder.data)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -4.893 -1.780 -0.561  1.636  6.853 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  3.493254   1.365820    2.56  0.01398 *  
## assault      0.020749   0.005558    3.73  0.00053 ***
## robbery      0.016065   0.006843    2.35  0.02334 *  
## auto        -0.005637   0.002816   -2.00  0.05133 .  
## burglary    -0.000231   0.001447   -0.16  0.87375    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1 
## 
## Residual standard error: 2.86 on 45 degrees of freedom
## Multiple R-squared: 0.497,	Adjusted R-squared: 0.452 
## F-statistic: 11.1 on 4 and 45 DF,  p-value: 2.34e-06
```

```r
anova(modelC, modelA)
```

```
## Analysis of Variance Table
## 
## Model 1: murder ~ assault + robbery
## Model 2: murder ~ assault + robbery + auto + burglary
##   Res.Df RSS Df Sum of Sq    F Pr(>F)  
## 1     47 409                           
## 2     45 368  2      40.1 2.45  0.098 .
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

SSC = 408.56 [error for Model C above]

SSA = 368.48 [error for Model A as above]

$PRE = (SSC – SSA)/SSC$

$PRE = (408.56 – 368.48)/(408.56) = 0.098$

Because the observed PRE is less than the critical PRE (of .125), we fail to reject the null, and conclude that burglary and auto as a set of predictors do not significantly add to the prediction of murder above and beyond assault and robbery. 

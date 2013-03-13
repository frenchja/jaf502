---
layout: page
title: "Multiple Regression in R"
date: 2013-03-11 18:50
comments: true
sharing: true
footer: true
---

This R Guide for multiple regression hypothesis testing was created by [Jason A. French](mailto:frenchja@u.northwestern.edu).  Email him with any comments and suggestions.

Before we begin, if you're using this R Guide I'm assuming some previous knowledge:

  1.  **You're familiar with how to enter basic commands.**  If not, there are plenty of tutorials regarding this, [many of which](http://personality-project.org/r/) are written by my adviser, [Bill Revelle](https://en.wikipedia.org/wiki/William_Revelle).  I don't want to redo what he has more successfully done.
  2.  **The dataset was successfully entered into R.**  If not, once again Bill comes to the rescue with [his guide](http://personality-project.org/r/#getdata).  I'm generally a fan of exporting my data as a .csv file and using the [`read.csv()`](http://stat.ethz.ch/R-manual/R-patched/library/utils/html/read.table.html) function (.e.g, `data <- read.csv(mydata.csv)`).  However, if you're a fan of Excel, you'll find Bill's [`read.clipboard()`](http://www.personality-project.org/r/html/read.clipboard.html) function in the [`psych`](http://cran.r-project.org/web/packages/psych/index.html) package helpful.
  3.  **You understand basic equation notation.**  Essentially, `Data = Model + Error`.  The parameters (or variables) in our model are often denoted by &beta;, and the error by &epsilon;.
  4.  **You understand why we test hypotheses using two models.**  If I want to look at the impact of Ability on Grades despite differences in family SES, I have to *control for* SES.  So our most basic model would be Grades ~ SES, and we test whether Grades ~ SES + Ability is a better model, 

Now that we're up to speed, I've organized this tutorial as a series of realistic situations that one might encounter.  Part 1 shows how to specify your models to do hypothesis testing and does not require R.  The second and third parts demonstrate how to use R to test for an interaction, curvilinear effect, and finally test whether a *set* of variables are significant.

Part 1:  Hypothesis Testing
-------

Using multiple regression in R involves testing specific models.  Unless your models are setup correctly, the significant difference between two models may not lead to the inference you think it does!  Unfortunately, the US learned this the hard way.  The fear of fat, which reached its height during the 1980s and 1990s, was due to a faulty regression submitted to the FDA.  The researcher tested whether levels of fat and sugar predicted rates of cardiovascular disease but never partialled out the effect of sugar (which was the real problem).  Thus, due to the witch hunt against fat, food manufactures began offered low-fat varieties of everything.  However, there are only so many things to get taste and calories from.  When you remove the fat from a food, you have to add a lot of sucrose/fructose to make up for the lack of flavor.  Thus, rates of heart disease continued to climb because of bad regression (see [Lustig's 2009 colloquium](http://www.uctv.tv/search-details.aspx?showID=16717)).

Consider the following situation involving judgements about political candidates.  Here, I specify what the various models would be.  You can plug in your own variables from your data to follow along.

### Political Candidates

A political scientist is interested in factors influencing judgments of political candidates. To explore these issues, he uses some national survey data collected from a randomly drawn sample of 989 voting age citizens. Data on each of the following five variables are available from each of these survey respondents:

`CAND`  : The respondent’s judgment of the liberalness/conservativeness of a particular candidate . Respondents make their judgments of the candidate on a seven point scale, with 1 defined as “very conservative” and 7 defined as “very liberal.”

`SELF` :  Respondent’s judgment of his or her own liberalness/conservativeness on the same seven point scale as for CAND.

`LIKE`  :  Respondent’s judgment of liking for the candidate. Judgments are on a 100 point scale with 0 indicating the respondent very much does not like the candidate and 100 indicating the respondent very much likes the candidate.

`TRUST`  :  Respondent’s judgment of the candidate’s trustworthiness on a seven-point scale where 1 indicates “very untrustworthy” and 7 indicates “very trustworthy.”

`Know`  :  A composite measure of the respondent’s political knowledge, ranging from 0 to 10, with a score of 10 indicating that the respondent is very knowledgeable about political issues and a score of 0 indicating that the respondent knows nothing about politics.

Based on these data, I conduct analyses to answer each of the following questions. For each question, I indicate the compact and augmented models that should be compared to answer the question and state the null hypothesis being tested. 

In addition, indicated the degrees of freedom associated with the F* that would be used to evaluate each hypothesis. Some of these questions may require that you construct new variables out of the five that were measured.

**Are judgments of liking for the candidate predictable from the respondent’s self judgment of liberalness/conservativeness?**

Model C: Like<sub>i</sub> = &beta;<sub>0</sub> + &epsilon;<sub>i</sub>

Model A:  Like<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &epsilon;<sub>i</sub>

F^*<sub>{1,987}</sub> = H<sub>0</sub>: &beta;<sub>1</sub> = 0

**Are judgments of the candidate’s trustworthiness related to candidate evaluation even when controlling for the respondent’s own political persuasion?**

Model C:  Like<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &epsilon;<sub>i</sub>

Model A:  Like<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &beta;<sub>2</sub> Trust<sub>i</sub> + &epsilon;<sub>i</sub>

F^*<sub>{1,986}</sub> = H<sub>0</sub>: &beta;<sub>2</sub> = 0

**Are judgments of how the candidate is evaluated predictable from trustworthiness judgments less strongly among politically knowledgeable respondents?**

Model C:  Like<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Trust<sub>i</sub> + &beta;<sub>2</sub> Know<sub>i</sub> + &epsilon;<sub>i</sub>

Model A:  Like<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Trust<sub>i</sub> + &beta;<sub>2</sub> Know<sub>i</sub> + &beta;<sub>3</sub> (Know<sub>i</sub>)(Trust<sub>i</sub>) + &epsilon;<sub>i</sub>

F^*<sub>{1,985}</sub> = H<sub>0</sub>: &beta;<sub>3</sub> = 0

**Do those respondents whose political views are relatively extreme in either a liberal or conservative direction, like the candidate less well than those who are more moderate, even controlling for the fact that liberals prefer the candidate to those who are more conservative?**

Model C:  Like<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &epsilon;<sub>i</sub>

Model A:  Like<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &beta;<sub>2</sub> Self^2<sub>i</sub> + &epsilon;<sub>i</sub>

F^*<sub>{1,986}</sub> = H<sub>0</sub>: &beta;<sub>2</sub> = 0

**On average, do respondents assimilate judgments of the candidate’s position to their own position? That is, the more liberal respondents are, the more they judge the candidate to be liberal. The more conservative respondents are, the more conservative they judge the candidate to be.**

Model C:  Cand<sub>i</sub></sub> = &beta;<sub>0</sub> + &epsilon;<sub>i</sub>

Model A:  Cand<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &epsilon;<sub>i</sub>

F^*<sub>{1,987}</sub> = H<sub>0</sub>: &beta;<sub>1</sub> = 0

**Does the extent to which respondents assimilate judgments of the candidate’s position to their own depend on the degree to which they like or dislike the candidate? Those who relatively like the candidate assimilate the candidate’s position to their own more strongly than those who like the candidate less well.**

Model C:  Cand<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &beta;<sub>2</sub> Like<sub>i</sub> + &epsilon;<sub>i</sub>

Model A:  Cand<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Self<sub>i</sub> + &beta;<sub>3</sub> Like<sub>i</sub> + &beta;<sub>3</sub> (Self<sub>i</sub>)(Like<sub>i</sub>) + &epsilon;<sub>i</sub>

F^*<sub>{1,985}</sub> = H<sub>0</sub>: &beta;<sub>3</sub> = 0

**Is liking for the candidate predictable from knowledge of the absolute difference between the respondent’s judgment of his or her own political position and his or her judgment of the candidate’s political position?**

First, create a new variable:  Diff<sub>i</sub> = |Self<sub>i</sub> - Cand<sub>i</sub>|

Model C:  Cand<sub>i</sub> = &beta;<sub>0</sub> + &epsilon;<sub>i</sub>

Model A:  Cand<sub>i</sub> = &beta;<sub>0</sub> + &beta;<sub>1</sub> Diff<sub>i</sub> + &epsilon;<sub>i</sub>

F^*<sub>{1,987}</sub> = H<sub>0</sub>: &beta;<sub>1</sub> = 0

Part 2:  Testing for Interactions
----------------------------

### The Effect of Books and Attendance on Grades

[Download]() this example dataset.  In this section, we'll look at the influence of Books and Attendance on one's Grades.  However, one might hypothesize that the number of books you read has a larger impact on your grade if you bother attending lecture more often.  If we're hypothesizing that people who attend class get more out of the books versus the students who don't attend and use them to cram at the end of the quarter or semester, we need to test for an *interaction* of Books x Classes, over and above the *main effects* of `Books` and `Classes` on our dependant variable, `Grades`.

Make sense?

```r Running the regression
# Load the psych library
library(psych)

# Since Excel files are garbage, convert it to a .csv file
books.data <- read.csv(file = "~/Desktop/Statslearninteractionexamp.csv", header = TRUE)

# Now center the Attend and Book variables
books.dataAttend <- scale(books.dataAttend, center = TRUE, scale = FALSE)
books.dataBooks <- scale(books.dataBooks, center = TRUE, scale = FALSE)

# Finally, use lm() to do the regression
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

```r Test the parameters using the anova() function 
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

In sum, we have signficant *main effects* of both Books (F(1,36) = 14.57, *p* < 0.001) and Attendance (F(1,36) = 5.22, *p* < .03).  However, this relation between Grades and Books depends on how many classes you attend (F(1,36) = 4.43, *p* < .05).

### Graphing the Interaction

Sometimes graphs can help us conceptualize what this interaction means.  I will graph the interaction as a series of simple relationships, which we'll derive using a bit of math.  As our main equation is \hat{Grade} = (61.60 + 1.333Attend<sub>0</sub>) + (4.155 x (0.735 x Attend<sub>0</sub>))Books<sub>0</sub>, we'll plug in values for Attend to change the simple slope.

I will graph `Grade` by the centered `Books` at 1) the centered `Attend`, 2) one standard deviation below from the average `Attend` (i.e., 4.278 days *missed*), and 3) one standard deviation above from the average `Attend` (4.278 days attended).  The three lines correspond to the three equations below:

If we plug in three representative values of Attend<sub>0</sub>...

Attend<sub>0</sub> = -4.278

Grade&#770; = (61.60 + 1.333 x -4.278) + (4.155 + (0.735 x -4.278))*Books<sub>0</sub>

Attend<sub>0</sub> =0

Grade&#770; = (61.60 + 1.333 x 0) +(4.155+(0.735 x 0)) x Books_0

Attend<sub>0</sub> = 4.278

Grade&#770; = (61.60 + 1.333 x 4.278) + (4.155 + (0.735 x 4.278)) x Books<sub>0</sub>

### Interpreting the interaction

For the people who attended fewer classes than average, the number of textbooks they read was rather weakly related with their grade.  On the other hand, for the people who attended more classes than average, the number of textbooks they read had a strong positive relationship with their grade. 

```r Plotting the interaction as 3 simple relationships
library(ggplot2)
p <- qplot(x = Books, y = Grade, data = books.data, main = "Grade ~ Books", 
    legend = TRUE)
p + geom_abline(intercept = (61.6 + 1.333 * -4.278), slope = (4.155 + (0.735 * 
    -4.278)), aes(colour = "1")) + geom_abline(intercept = (61.6 + 1.333 * 0), 
    slope = (4.155 + (0.735 * 0)), aes(colour = "2")) + geom_abline(intercept = (61.6 + 
    1.333 * 4.278), slope = (4.155 + (0.735 * 4.278)), aes(colour = "3"))
```

{% img http://gradstudents.wcas.northwestern.edu/~jaf502/images/unnamed-chunk-2.png %} 


Next we test if either `Books` or `Classes` has a curvilinear (quadratic) effect in the prediction of `Grade`.  To help the reader, I provide graphs of these results.

### Let's do Grades ~ Attend first
Grade&#770; = b<sub>0</sub> + b<sub>1</sub> Attend + b<sub>2</sub> Attend^2


```r Regression of Grade ~ Attend + Attend^2
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

```r Effect Tests for the Parameters
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

```r Plotting the Curvilinear Effect of Grade ~ Attend + Attend^2
p <- qplot(x = Attend, y = Grade, data = books.data, main = "Polynomial Fit of Grade ~ Attend")
p + layer(data = books.data, mapping = aes(x = Attend, y = Grade), stat = "smooth", 
    stat_params = list(method = "glm", formula = y ~ poly(x, 2)))
```

{% img http://gradstudents.wcas.northwestern.edu/~jaf502/images/unnamed-chunk-3.png %}

*Attendance* does not have a curvilinear relationship with Grades, *p* = n.s.

### Next, we'll test Grades ~ Books

Grade&#770; = b<sub>0</sub> + b<sub>1</sub> Books + b<sub>2</sub> Books^2

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

{% img http://gradstudents.wcas.northwestern.edu/~jaf502/images//unnamed-chunk-4.png %}

*Books* do not have a curvilinear relationship with Grades, *p* = n.s.

## The Final Countdown:  Testing sets of parameters

Download the file [here]().  I test to see whether the set of predictors "Burglary" and "Auto"(Theft) add to the prediction of "Murder"”" once "Assault" and "Robbery" are taken into account (i.e., "over and above Assault and Robbery").

Model C: Murder&#770; = b<sub>0</sub> + b<sub>1</sub> Assault + b<sub>2</sub> Robbery

Model A: Murder&#770; = b<sub>0</sub> + b<sub>1</sub> Burglary + b<sub>2</sub> Auto + b<sub>3</sub> Assault + b<sub>4</sub> Robbery

```r Regressing Murder onto Assault and Robbery
murder.data <- read.csv(header = TRUE, file = "Crime.csv")
modelC <- lm(murder ~ assault + robbery, data = murder.data)
summary(modelC)
```

```r
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

```r Regressing Murder onto Assualt, Robbery, Theft, and Burglary
modelA <- lm(murder ~ assault + robbery + auto + burglary, data = murder.data)
summary(modelA)
```

```r
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

```r Effects Tests for Parameters
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

The probability of the F-value corresponds to the probability that I would get had I calculated the proportion of reduced error (i.e., PRE) by hand:

SSC = 408.56 [error for Model C above]

SSA = 368.48 [error for Model A as above]

PRE = (SSC – SSA)/SSC

PRE = (408.56 – 368.48)/(408.56) = 0.098

Because the observed PRE is less than the critical PRE (of .125), we fail to reject the null, and conclude that burglary and auto as a set of predictors do not significantly add to the prediction of murder above and beyond assault and robbery. 
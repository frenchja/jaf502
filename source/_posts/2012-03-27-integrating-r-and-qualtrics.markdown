---
layout: post
title: "Analyzing Qualtrics Data in R using Github Packages"
date: 2012-03-27 14:23
comments: true
author: Jason A. French
categories: [R, Github, Development, Qualtrics]
---
[Qualtrics](http://www.qualtrics.com/) is an online survey platform similar to SurveyMonkey that is used by researchers to
collect data.  Until recently, one had to manually download the data in either SPSS or .csv format, making ongoing data
analysis difficult to check whether the trend of the incoming data supports the hypothesis.

[Jason Bryer](http://bryer.org/) has recently 
developed an R package published to Github for downloading data from Qualtrics within R using the Qualtrics API 
([see his Github repo](https://github.com/jbryer/qualtrics)).  Using this package, you can integrate your Qualtrics data 
with other experimental data collected in the lab and, by running an Rscript as a cronjob, get daily updates for your 
analyses in R.  I'll demonstrate the use of this package below.
<!-- more -->
## Installing the Qualtrics Package from Github
{% codeblock Installing Qualtrics lang:r %}
#!/usr/bin/env Rscript
# Authors:  Jason A. French
# Email:    frenchja@u.northwestern.edu
install.packages('devtools',dependencies=TRUE)
require(devtools)
install_github(repo='qualtrics',username='jbryer')
require(qualtrics)
{% endcodeblock %}

If you wanted to distribute the Rscript to colleagues with different setups, you would use something like this:
{% codeblock lang:r %}
#!/usr/bin/env Rscript
# Authors:  Jason A. French
# Email:    frenchja@u.northwestern.edu
# Check for devtools
if (!require(devtools)) {
    warning('devtools not found. Attempting to install!')
    install.packages('devtools',dependencies=TRUE)
    require(devtools,character.only=TRUE)        
}

# Check Qualtrics Package from Github
if (!require(qualtrics)) {
  warning('Qualtrics package not found')
  install_github(repo='qualtrics',username='jbryer')
  require(qualtrics)
}
{% endcodeblock %}

## Pulling Data from Qualtrics
{% img right /images/qualtricsid2.png %}
First, you'll need to find the Survey ID of the associated study on Qualtrics.  Access your `Account Settings` and click on
`Qualtrics IDs`.  From here, copy the entire ID into your R code (e.g., `SV_blahblah`).
{% img center /images/qualtricsid1.png %}

Next, load the `qualtrics` package in R and pull the data using `getSurveyResults()`. *If you receive an error in R 
regarding API permissions, you may need to email support@qualtrics.com and request API access.  The 
R function accesses your survey using XML.*
{% codeblock lang:r %}
# Get Qualtrics Survey Data
qualtrics.data <- getSurveyResults(username=qualtrics.user,
                      password=qualtrics.pass,
                      surveyid='SV_blahblah')
{% endcodeblock %}

Again, if you are distributing the analysis to collaborators, you may want to use a setup like below, which reads the user and 
password securely.  If you want to automate this, you'll obviously need hardcode your username and password.  At this time,
I don't believe R has a hashing mechanism for passwords like Python, meaning you'll need to have a plaintext password.
{% codeblock Password Protection for the Paranoid lang:r %}
# Get Qualtrics Survey Data
# Not sure this will work in R.app.  Only in Terminal.app
get_password <- function() {
  cat("Qualtrics Password: ")
  system("stty -echo")
  a <- readline()
  system("stty echo")
  cat("\n")
  return(a)
}

qualtrics.user <- readline(prompt='Enter your Qualtrics username')
qualtrics.pass <- get_password()
qualtrics.data <- getSurvey(username=qualtrics.user,
                      password=qualtrics.pass,
                      surveyid='SV_blahblah')
{% endcodeblock %}

## Outputting Data using xtable
Now that we have our data pulled from Qualtrics, we can analyze it as if it were a regular R data.frame. 
If you know your specific survey variables of interest, you'll want to modify the code using `qualtrics.data[,c('Var1','Var2)]`.

{% codeblock lang:r %}
require(psych)
require(xtable)
qualtrics.describe <- describe(qualtrics.data)
qual.table <- xtable(qualtrics.describe)
print.xtable(qual.table,type='html',file='~/Desktop/Qualtrics.html')

# Or...
qual.lm <- lm(DV ~ IV1*IV2, data=qualtrics.data)
lm.table <- xtable(qual.lm)
print.xtable(lm.table,type="html",file='~/Desktop/Qualtrics.html',append=TRUE)
{% endcodeblock %}
## Running an Rscript
In order for your script to be run daily, it needs to be a) marked as executable ([see here](https://en.wikipedia.org/wiki/Filesystem_permissions)) 
and b) added to [crontab](https://en.wikipedia.org/wiki/Cron).  First, mark it 
as executable:
{% codeblock lang:bash %}
chmod +x ~/Qualtrics.R
{% endcodeblock %}

*Make sure that `#!/usr/bin/env Rscript` is at the top of your script!*

Next, edit the system's [crontab](https://en.wikipedia.org/wiki/Cron) and add the script.
{% codeblock lang:bash %}
sudo nano /etc/crontab

0 1 * * * ~/Qualtrics.R
{% endcodeblock %}

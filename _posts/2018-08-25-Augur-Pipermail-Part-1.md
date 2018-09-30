---
layout: post
title: "CHAOSS - Augur: Pipermail Part 1"
date: 2018-08-25T10:20:00Z
categories: Augur
---
<br>
![augur](https://raw.githubusercontent.com/kmn5409/keanu-nichols/master/_includes/augur2.png)
This is my second blog entry for working at [CHAOSS](https://chaoss.community/) under [Augur](https://github.com/OSSHealth). This week was mostly spent on fixing some issues that were popping up when trying to run my pipermail code and fixing an error that occurred when doing sentiment analysis on pull requests. So this week's tasks with my mentor [Sean](http://www.seangoggins.net/) is as follows.


# Goals for the week:
1. Fix issues occurring when trying to implement Pipermail.
2. Finish uploading github pull requests for experimental github repositories.

## Goals

**Goal 1**:<br>
#### Pipermail
Now for this goal we seem to be encountering a problem whereby when you run [PiperMail](https://github.com/kmn5409/GSoC_CHAOSS/blob/master/Augur/Perceval/PiperMail%2013.ipynb) it seems that the mailing lists you want to download wasn't being output to the correct file "mailing_list.csv". So I added first line 8 this was so that the mailing lists I wanted to be downloaded would be outputted to the file. I would close the file and then reopen it (lines 8-9).

{% highlight python linenos %}
if (os.stat(path).st_size == 0):
   file.write("Link,mail_list\n")
   file.write("https://lists.opendaylight.org/pipermail/,\"aalldp-dev\"\n")
   file.write("https://lists.opendaylight.org/pipermail/,\"archetypes-dev\"\n")
   print("Please enter the mailing lists and the links for them please")
   print("Going to the default mailing lists")
   file.close()
   file = open(path,"r")
{% endhighlight %}

After I would iterate through the file to determine if anything was written to it (lines 2-6), after I would close the file (line 7).
{% highlight python linenos %}
count = 0
for line in file:
   print(line)
   count+=1
   if(count == 2):
       break
file.close()
{% endhighlight %}

**Goal 2**:<br>
#### Github Pull Requests Sentiment Analysis
The next problem that was occurring was then when I was performing sentiment analysis on the pull requests it seemed that some of them would be of type "None". The program would then crash when this happened because I would feed this into "[SentiCR](https://github.com/kmn5409/CHAOSS_Augur/blob/master/SentiCR.py)" (line 5). So I just set the message as " " (line 4) so that I  could continue iterating through the other pull requests.

{% highlight python linenos %}
for i in messages:
       #print(i)
       if(i == None):
           i = " "
       score=sentiment_analyzer.get_sentiment_polarity(i)
{% endhighlight %}


Resources:
My branch on [augur](https://github.com/OSSHealth/augur/tree/pipermail)

Files Used:

Python File -  [SentiCR](https://github.com/kmn5409/CHAOSS_Augur/blob/master/SentiCR.py)

Jupyter Notebook - [PiperMail 14](https://github.com/kmn5409/GSoC_CHAOSS/blob/master/Augur/Perceval/PiperMail%2014.ipynb)

Jupyter Notebook - [github_pull_requests_scores_senticr 2](https://github.com/kmn5409/CHAOSS_Augur/blob/master/github_pull_requests_scores_senticr%202.ipynb)





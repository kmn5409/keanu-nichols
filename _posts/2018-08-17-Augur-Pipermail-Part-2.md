---
layout: post
title: "CHAOSS - Augur: Pipermail Part 2"
date: 2018-09-29T10:20:00Z
categories: Augur
---
<br>
![augur](https://raw.githubusercontent.com/kmn5409/keanu-nichols/master/_includes/augur2.png)
This is my third blog entry for working at [CHAOSS](https://chaoss.community/) under [Augur](https://github.com/chaoss/augur). I haven't posted in awhile I know but well school has been hectic and I've been working on some other projects that I hope to speak about soon enough. But the past few weeks I have really been working on one main goal. So these past few week tasks with my mentor [Sean](http://www.seangoggins.net/) are as follows.


# Goals for the week:
1. Fix issue when downloading mailing lists from 2010 the program would go into a loop
2. Upload mailing lists for gluster

## Goals

**Goal 1**:<br>
#### Pipermail
For this goal what was happening was that I would try to download the mailing list ["gluter-users"](https://lists.gluster.org/pipermail/gluster-users/) and the program would seem to be going into a loop. So I would interrupt the program and then look at the SQL Database. What seemed to be happening was that the same messages were being uploaded repeatedly. After spending some time going through my code I discovered that I actually had a statement in my code. What was happening actually was that for example if I downloaded a large amount of messages say from the year 2010 to 2018 that's maybe around 1000's of emails. So my if statment (line 1) was supposed to upload the dataframe with the messages and then reset it. But what was actually happening was that I would just upload the dataframe (line 3) and then put more messages in the dataframe. I wasn't emptying the dataframe. So thankfully I was able to get this out, I didn't realize this before because I was only testing a small amount of messages so in the future I need to do some more rigurous testing.

{% highlight python linenos %}
if(row>=( (k*5000))):
    y+=1
    df5.to_sql(name=db_name, con=db,if_exists='append',index=False)
    val = True
    y = True
{% endhighlight %}

The updated code is as seen below and I really need to remove line 6 lol because that can be misleading, I used that fors when I was testing.
{% highlight python linenos %}
if(row>=( (k*5000))):
    y+=1
    df5.to_sql(name=db_name, con=db,if_exists='append',index=False)
    k+=1
    val = True
    print("Should not be here")
    y = True
    df5 = pd.DataFrame(columns=columns1)
{% endhighlight %}

**Goal 2**:<br>
#### Upload mailing lists
So all I need to do is upload a bunch of messages from different mailing lists, it's going to be a long process because it's from quite 2010 so it will take a few hours especially if I'm going to do sentiment analysis on it using SentiCR.


Resources:
My branch on [augur](https://github.com/chaoss/augur/tree/pipermail)

Files Used:

Python File -  [piper_reader 1](https://github.com/kmn5409/GSoC_CHAOSS/blob/master/Augur/Perceval/piper_reader%201.py)

Python File -  [piper_reader 3](https://github.com/kmn5409/GSoC_CHAOSS/blob/master/Augur/Perceval/piper_reader%203.py)






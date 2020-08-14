--
toc: true
layout: post
description: 
categories: 
comments: true
title: Titanic data cleaning and initial exploration
---

I should really be cracking on with my data science course but I’ve gone on a tangent to play with the Titanic data.

So far I have:

**Engineered out a Title column from the names.** \
This is actually quite easy with a regular expression.
```
import re
def ExtractTitle(name):
     ptn = "[^,]*,\s(the\s)?([^\.]*)"
     p = re.compile(ptn, re.IGNORECASE)
     m = p.match(name)
 
     if m is not None:
          return m.group(2)
     else:
          print("FAILED EXTRACTING TITLE FROM: " + name)
          return None
```

**Added a Noble flag.**\
This is for nobility titles like Sir, Countess, Lord, Lady etc. There actually aren’t as many of these in the train 
data as I expected, if there aren’t more in test then this flag may not be that helpful.

**Added SexPlus column.** \
This is to expand male/female to man/woman/boy/girl. I expect this to be a useful feature as children were obviously 
not treated in the same way as adults (but I expect that boys were not treated equally to girls).

I’ve chosen 16 as the cut-off point. Legally people were considered completely independent of their parents at 21 at this 
time, but there were a myriad of points at which children started progressing towards adulthood in different ways.

All I’m interested in is how they were treated in regards to places on lifeboats, and the information from historical 
accounts suggests that was somewhere around 14-16 but was not clearly defined.

**Dealt with missing ages.** \
20% of the data does not have an age set. So far I have used median ages based on Title. That’s good enough for men, 
boys, and married women, but for other females I should be able to look at Parch and SibSp and take a more educated 
guess at whether they are girls or single women.

**Plotting.** \
After that I did some basic plotting to look at survival rates.
[](../images/TitanicEDA1.jpg)

Unsurprisingly women and children did better than men, but within children, girls did better than boys.

There’s nothing especially stunning jumping out at this point.

Next I ran some regression plots to look at survival rates by age on a line.
[](../images/TitanicEDA2.jpg)

There is obviously at least one outlier in the male data causing an issue and I need to look at trimming here. Boys and girls needs further attention, possibly by Pclass.

The next things I need to investigate are:

- Cabin number. I can get the deck from cabin number but I’m not sure what else, and so few of these are present that 
I don’t expect it to be much use.
- Family groups. I need to look at Parch, SibSp, ticket numbers, cabin numbers etc and try to determine the groups 
travelling together, and add a column for GroupSize(?)
- Ticket numbers. This may be useful as part of family processing but I’m not sure what else I can gather from these.
- Fare. The size of the fare may be useful as fare can vary a lot even within Pclass (a 1st class berth was £30 but 
there were 2 suites going for £870 and presumably a fair bit in the middle). This needs to be in conjunction with trying 
to calculate a fare per person as some tickets prices appear to be a sum for everyone in that group.
- Look at which interactions I want to add. I expect most of the value I’m going to add from this point to come from here.

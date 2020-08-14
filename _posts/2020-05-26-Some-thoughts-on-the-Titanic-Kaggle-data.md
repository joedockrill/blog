---
toc: true
layout: post
description: 
categories: Kaggle, Titanic
comments: true
title: Some thoughts on the Titanic Kaggle data
---

So it seems the data science equivalent of “Hello World” is the Titanic survivor problem on Kaggle.

Some thoughts on the data so far before I download it and start playing:

| Variable | Definition |
|---|---|
| PassengerId	| ID | 
| Survived	| 0 = No, 1 = Yes | 
| Pclass	| Ticket class, 1=1st, 2=2nd, 3=3rd | 
| Name	| Name| 
| Sex	| Sex| 
| Age	| Age in years| 
| SibSp	| # of siblings / spouses aboard the Titanic| 
| Parch	| # of parents / children aboard the Titanic| 
| Ticket	| Ticket number| 
| Fare	| Passenger fare| 
| Cabin	| Cabin number| 
| Embarked | Port of Embarkation| 

**Name.** \
I see a lot of effort going standardising names and I don’t see the value. 
Is someone known as Colonel as likely to survive as someone known as Mr? 
Potentially it might be interesting to flag non-standard titles, eg: males NOT known as Mr or Master, 
females not known as Miss Mrs Mlle, Mme etc, but not to fudge them all to Mr & Mrs.

FWIW the full list of titles seems to be:
```
‘Mr’, ‘Master’, ‘Major’, ‘Rev’, ‘Dr’, ‘Col’, ‘Capt’, ‘Don’, ‘Jonkheer’
‘Mrs’, ‘Miss’, ‘Dr’, ‘Ms’, ‘Mlle’, ‘Mme’, ‘Countess’
```
Note that Dr appears in both male and female titles

The names in the data shown in brackets are maiden names as you would expect and this does not appear at first glance 
to be useful.

titanicfacts.net has a passenger list which indicates where certain passengers were employed by others 
(eg: nanny/maid/manservant/nurse to other_passenger). This is interesting but doesn’t appear to be in our data, 
at least from the sample rows I’m seeing so far. These account for 38 first class passengers and I assume that this 
would be a significant factor if I can engineer it back out of the kaggle data.

**SibSp/Parch.** \
Some people are using family size (SibSp + Parch) as an indicator. “Women and children first” surely took precedence 
over family sizes and the notion of giving up your place for a family member. If this is a factor then I think it 
needs treating carefully. One thing which is feasible is the idea that people with family members would have spent 
time and effort of looking after each other, or even just looking for each other (although the latter probably not 
a significant factor given the time of the accident; most families would probably have been in bed in their cabins). 
Also Parch as one variable is absurd and needs splitting back to Parents and Children. Similarly (if possible) SibSp 
to Siblings and Spouse although this is potentially more difficult for older passengers.

We know that some children traveled only with a nanny, therefore parch=0 for them. Can I spot those nannies? 
They must have been more likely to survive.

**Ticket.** \
Some ticket numbers are prefixed PC and W.E.P. – what are these?

There were 29 cross-channel passengers who disembarked in cherbourg (france) or queenstown (ireland). 
I wonder if I can spot these through ticket numbers or other means? I doubt it but worth a look.

**Age.** \
“If the age is estimated, is it in the form of xx.5”. Watch out for that and decide what to do with it. 
Is there a point at which older passengers may have been more likely to give up their place on a lifeboat for 
someone younger? How do survival rates by age look between sexes?

**Fare.** \
This varies massively even within passenger classes. Must be useful. Can probably see which passengers had suites. 
Some high value tickets also contain multiple cabin numbers which also suggests a certain level of affluence.

**Cabin.** \
Cabin numbers (mostly not present) tell you the deck the cabin was on and this is probably interesting.

**Embarked.**
Probably insignificant. If it’s a significant factor then I’d need to try and understand why.

**Other.** \
One article I read uses squares and products of base variables as indicators and I don’t understand enough about this generally so it’s a tangent I need to go on shortly.

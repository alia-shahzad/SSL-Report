SSl Final Report
================
Alia Shahzad
5/18/2020

    ## here() starts at /Users/ai-ming/iCloud Drive (Archive)/SSL Report Private

## Introduction

This report uses publically available data from the Strategic Subject
List (SSL), street name the “Heat List,” which was retired in 2019.
Over the decade of its use, the Chicago Police Department (CPD) used an algorithm
to assess the risk of a given individual becoming party to violence, and used the
data to identify and intervene with these high-risk individuals. 
But the SSL was subject to much public scrutiny, 
one reason being the lack of transparancy around the algorithm itself
and the data being fed into it. Its use was quietly retired in
2019.

Under pressure from the ACLU, the state of ILlinois’ Attorney General’s
Office, as well as a group of journalists who submitted a FOIA for SSL
data, in 2016, the CPD released a set of de-identified data of list
subjects that included, among other categories, scores, race, age
arrested, and current age; these are the variables I analyze in this
report. The CPD
[maintained](https://chicagounbound.uchicago.edu/cgi/viewcontent.cgi?article=1636&context=uclf)
that it did not use race as a factor when calculating risk scores. While
it’s impossible to assess the truth of this claim as they declined to
release the SSL algorithm itself, this analysis also aims to determine
to what extent risk scores were assosciated by race.

## Overview of Scores

![](images/ssl%20scores%20overview%20graph-1.png)

SSL Scores are placed on a scale ranging from 0 (extremely low risk of
violence) to 500 (extremely high risk violence). A score above 250
[earns](https://medium.com/equal-future/how-strategic-is-chicagos-strategic-subjects-list-upturn-investigates-9e5b4b235a7c)
“heightened police scrutiny,” and score above 450 is high-risk for
becoming party to violence. From this graph, we observe that the vast
majority of scores are between 0 - 450, and within this group, more
scores are high risk (250-350) than low risk (0-250). Very few scores
are at very high risk (above 450).

## Age

![](images/scores%20by%20age-1.png)
![](images/ssl%20scores%20by%20age%20group-1.png)
![](images/ssl%20scores%20by%20age%20group-2.png)

From the first graph, which counts SSL subjects by age first arrested,
we observe that most SSL subjects were under the age of 40 when they
were most recently arrested. The most represented age group of most
recent arrest is from 20-30, which is unsurprising considering that this
is also the age group most represented in committing crimes in the city.
For further analysis, it might be interesting to compare these figures
to age groups of SSL subjects when they were first arrested, but
unfortunately the CPD did not release this data. I suspect we’d see a
significantly larger bar for the under 20 age group in this case, since
many of the subjects are arrested repeatedly throughout their lives.

From the second and third graphs, which depict current age of Subjects,
we note that age group with the highest scores is under 20\! As subjects
get older, their risk for becoming party to violence decreases. These
observations lines up with age demographics of violent crime offenders;
past offenders are significantly less likely to commit crimes as they
age, particularly coming out of long periods of incarceration.

## Accuracy and Age

![](images/oldies%20visualization-1.png)

A common critique of the SSL was that its input data was not updated
frequently enough, leading to the assignment of high scores to elder
individuals, who are significantly less violence-prone that people under
50. In other words, keeping people on the list who had last been
arrested when they were in their 20s but are now 40 or 50 might lead to
inaccurate data. In this graph, I look at what percent of old people
(current aged 50+) constitute each SSL score bracket. The graph
demonstrates that people over the age of 50 are assesed at a fairly
low-risk score (below 250), which indicates that this worry is likely
not founded. We would be more worried if the elderly composed any
percentage of the scores above 250.

## Gang Affiliation

![](images/age%20&%20gang%20affiliation-2.png)
![](images/age%20&%20gang%20affiliation-1.png)

Here, we examine the relationship between SSL scores, age and gang
affiliation. In the faceted boxplot, we observe that the median SSL
score is a bit higher for people with gang affiliations, but maybe not
as much as we might anticipate. In the bar graph, we observe that
subjects currently aged 20-30 are most likely to be members of gangs.
This is unsurprising given the trend of younger criminals (20 and below)
to opt out of gangs and instead participate in smaller cliques or rogue
acts of violence.

## Race

![](images/by%20race-1.png) ![](images/by%20race-2.png)

These graphs map SSL scores by race. Looking to the boxplots, we observe
no significant differences in median SSL score by race. However, in the
bar charts, we do see that significantly more black men are subject on
the list than any other race, at a proportion signicantly higher than
their perctentage of the city’s population,
[30%](https://www.census.gov/quickfacts/chicagocityillinois).

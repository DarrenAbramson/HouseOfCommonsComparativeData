---
title: "Regan/Harper/Leslie Comparison"
author: "Darren Abramson"
date: "September 5, 2015"
output: html_document
---

csv voting records are available at <http://www.parl.gc.ca/Parliamentarians/en/members> by clicking on the member and then on "Work" and then "All votes by this Member." You can see the link I used for Megan Leslie at <http://www.parl.gc.ca/Parliamentarians/en/members/Megan-Anissa-Leslie(58550)/Votes>, in the Export: CSV link. That file was renamed ```Leslie41parl2session.csv```.

```{r}
regan41parl2session <- read.csv("Regan41parl2session.csv", header = TRUE)
leslie41parl2session <- read.csv("Leslie41parl2session.csv", header = TRUE)
harper41parl2session <- read.csv("Harper41parl2session.csv", header = TRUE)

leslieSmall <- data.frame(leslie41parl2session$Vote.Number, leslie41parl2session$Member.s.Vote)
reganSmall <- data.frame(regan41parl2session$Vote.Number, regan41parl2session$Member.s.Vote)
harperSmall <- data.frame(harper41parl2session$Vote.Number, harper41parl2session$Member.s.Vote)

names(leslieSmall)[1] <- "Vote.Number"
names(reganSmall)[1] <- "Vote.Number"
names(harperSmall)[1] <- "Vote.Number"

smallMerge <- merge(leslieSmall, reganSmall, by="Vote.Number")
smallMerge <- merge(smallMerge, harperSmall, by="Vote.Number")
```

```smallMerge``` now contains all of Leslie, Regan, and Harper's votes for the most recent Parliament -- that is, all the ones in which all three voted. Let's take a look at the ones in which Leslie and Regan voted differently, but Regan voted with Harper.

```{r}
L_Diff_R_H_Same <- smallMerge$Vote.Number[smallMerge$leslie41parl2session.Member.s.Vote !=     
                         smallMerge$regan41parl2session.Member.s.Vote
                       & smallMerge$harper41parl2session.Member.s.Vote == 
                         smallMerge$regan41parl2session.Member.s.Vote]

L_Diff_R_H_Same
```

It's a bit more readable to get the subject headings for those votes; note that the indices reported below are local to the vector. The subjects correspond, in order, to the vote numbers above.
```{r}
leslie41parl2session[leslie41parl2session$Vote.Number %in% L_Diff_R_H_Same, 
                     6]
```


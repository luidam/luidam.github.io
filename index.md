---
title       : 
subtitle    : 
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Read-And-Delete

1. Edit YAML front matter
2. Write using R Markdown
3. Use an empty line followed by three dashes to separate slides!

--- .class #id 

## Slide 2

rm(list=ls())

library(AppliedPredictiveModeling)

data(segmentationOriginal)
segdata<- subset(segmentationOriginal,Case=="Train")
cellid<-segdata$Cell
case <- segdata$Case 
class<- segdata$Class
segdata <- segdata[,-(1:3)]
statuscols<- grep("Status", names(segdata))
statuscols
segdata <- segdata[,-statuscols]

---
### ----- TRANSFORMATIONS --------------
# Skewness
library(e1071)
# for one predictor:
e1071::skewness(segdata$AngleCh1)
skewValues<-apply(segdata,2,skewness)

library(caret)
#Calculation of transformations parameters:
trAreaCh1<-BoxCoxTrans(segdata$AreaCh1)
#Original data:
head(segdata$AreaCh1)
#After transformation:
predict(trAreaCh1,head(segdata$AreaCh1))
((819^-0.9)-1)/(-0.9)
#the function preProcess can be used to apply the transformation for a set of predictors

---
## PCA - principal components analisys can be calculated by the function: prcomp
pcaObj <- prcomp(segdata,center=TRUE,scale=TRUE)
# calculation of the percent of variance each component accounts for:
percVar <- (pcaObj$sd)^2/(sum(pcaObj$sd^2))*100
round(percVar,2)



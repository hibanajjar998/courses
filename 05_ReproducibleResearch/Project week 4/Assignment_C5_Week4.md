---
title: "Severe weather events and their consequences in the United States"
author: "Hiba"
date: "14/03/2020"
output:
  html_document: 
    keep_md: yes
---


## Synopsis  
  
Storms and other severe weather events can cause both public health and economic problems for communities and municipalities.   
  
This project involves exploring the U.S. National Oceanic and Atmospheric Administration's (NOAA) storm database. This database tracks estimates of any fatalities, injuries, and property damage.    
  
The data analysis shows how **Tornado** is the type of event that is most threatening to the population health, and thet **Floods** cause the biggest damage on the economy.    



## Data Processing  
    
      
       
  
This project involves exploring the U.S. National Oceanic and Atmospheric Administration's (NOAA) storm database. This database tracks characteristics of major storms and weather events in the United States, including when and where they occur, as well as estimates of any fatalities, injuries, and property damage.  
  
Using the URL adress, we first dowload the raw data which comes in the form of a comma-separated-value file compressed via the bzip2 algorithm to reduce its size:  
  
  

```r
if (!file.exists("file.csv.bz2")){
  fileURL <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
  download.file(fileURL, "file.csv.bz2", method="curl")
  raw_data <- read.csv("file.csv.bz2")
}
```
## Data Analysing  

Let's first have a look at what our data contains:

```r
dim(raw_data)
```

```
## [1] 902297     37
```


```r
names(raw_data)
```

```
##  [1] "STATE__"    "BGN_DATE"   "BGN_TIME"   "TIME_ZONE"  "COUNTY"    
##  [6] "COUNTYNAME" "STATE"      "EVTYPE"     "BGN_RANGE"  "BGN_AZI"   
## [11] "BGN_LOCATI" "END_DATE"   "END_TIME"   "COUNTY_END" "COUNTYENDN"
## [16] "END_RANGE"  "END_AZI"    "END_LOCATI" "LENGTH"     "WIDTH"     
## [21] "F"          "MAG"        "FATALITIES" "INJURIES"   "PROPDMG"   
## [26] "PROPDMGEXP" "CROPDMG"    "CROPDMGEXP" "WFO"        "STATEOFFIC"
## [31] "ZONENAMES"  "LATITUDE"   "LONGITUDE"  "LATITUDE_E" "LONGITUDE_"
## [36] "REMARKS"    "REFNUM"
```

  
  

```r
str(raw_data)
```

```
## 'data.frame':	902297 obs. of  37 variables:
##  $ STATE__   : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_DATE  : Factor w/ 16335 levels "1/1/1966 0:00:00",..: 6523 6523 4242 11116 2224 2224 2260 383 3980 3980 ...
##  $ BGN_TIME  : Factor w/ 3608 levels "00:00:00 AM",..: 272 287 2705 1683 2584 3186 242 1683 3186 3186 ...
##  $ TIME_ZONE : Factor w/ 22 levels "ADT","AKS","AST",..: 7 7 7 7 7 7 7 7 7 7 ...
##  $ COUNTY    : num  97 3 57 89 43 77 9 123 125 57 ...
##  $ COUNTYNAME: Factor w/ 29601 levels "","5NM E OF MACKINAC BRIDGE TO PRESQUE ISLE LT MI",..: 13513 1873 4598 10592 4372 10094 1973 23873 24418 4598 ...
##  $ STATE     : Factor w/ 72 levels "AK","AL","AM",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 834 834 834 834 834 834 834 834 834 834 ...
##  $ BGN_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ BGN_AZI   : Factor w/ 35 levels "","  N"," NW",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_LOCATI: Factor w/ 54429 levels "","- 1 N Albion",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_DATE  : Factor w/ 6663 levels "","1/1/1993 0:00:00",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_TIME  : Factor w/ 3647 levels ""," 0900CST",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ COUNTY_END: num  0 0 0 0 0 0 0 0 0 0 ...
##  $ COUNTYENDN: logi  NA NA NA NA NA NA ...
##  $ END_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ END_AZI   : Factor w/ 24 levels "","E","ENE","ESE",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_LOCATI: Factor w/ 34506 levels "","- .5 NNW",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LENGTH    : num  14 2 0.1 0 0 1.5 1.5 0 3.3 2.3 ...
##  $ WIDTH     : num  100 150 123 100 150 177 33 33 100 100 ...
##  $ F         : int  3 2 2 2 2 2 2 1 3 3 ...
##  $ MAG       : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
##  $ PROPDMG   : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
##  $ PROPDMGEXP: Factor w/ 19 levels "","-","?","+",..: 17 17 17 17 17 17 17 17 17 17 ...
##  $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ CROPDMGEXP: Factor w/ 9 levels "","?","0","2",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ WFO       : Factor w/ 542 levels ""," CI","$AC",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ STATEOFFIC: Factor w/ 250 levels "","ALABAMA, Central",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ ZONENAMES : Factor w/ 25112 levels "","                                                                                                               "| __truncated__,..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LATITUDE  : num  3040 3042 3340 3458 3412 ...
##  $ LONGITUDE : num  8812 8755 8742 8626 8642 ...
##  $ LATITUDE_E: num  3051 0 0 0 0 ...
##  $ LONGITUDE_: num  8806 0 0 0 0 ...
##  $ REMARKS   : Factor w/ 436781 levels "","-2 at Deer Park\n",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ REFNUM    : num  1 2 3 4 5 6 7 8 9 10 ...
```

### Across the United States, which types of events (as indicated in the `EVTYPE` variable) are most harmful with respect to population health?
  
  To adress this question, I'll create a subset from the data, extracting:
  - `EVTYPE` column in order to get the different types of events;
  - `FATALITIES` column to get number of fatalities caused by the event;
  - `INJURIES` column to get number of injuries caused by the event;



```r
sub1 <- raw_data[c("EVTYPE", "FATALITIES", "INJURIES")]
str(sub1)
```

```
## 'data.frame':	902297 obs. of  3 variables:
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 834 834 834 834 834 834 834 834 834 834 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
```





```r
sub1_ag <- aggregate(.~EVTYPE, data=sub1, sum)
head(sub1_ag)
```

```
##                  EVTYPE FATALITIES INJURIES
## 1    HIGH SURF ADVISORY          0        0
## 2         COASTAL FLOOD          0        0
## 3           FLASH FLOOD          0        0
## 4             LIGHTNING          0        0
## 5             TSTM WIND          0        0
## 6       TSTM WIND (G45)          0        0
```
  
  We've got in each row in the `sub1_ag` dataframe a specific event with the corresponding total number of fatalities and injuries.
  
  Since the number of fatalities and injuries are correlated, I'll first drop the rows of events with small threat on the population health (less than **10** victims), and use information from summary in order to set a threshold on the number of injuries before I plot into a bar plot the data corresponding to the most harmful events: 



```r
sub1_ag1 <- subset(sub1_ag, INJURIES>10 & FATALITIES>10)
summary(sub1_ag1)
```

```
##              EVTYPE     FATALITIES        INJURIES      
##  AVALANCHE      : 1   Min.   :  11.0   Min.   :   12.0  
##  BLIZZARD       : 1   1st Qu.:  33.0   1st Qu.:   82.5  
##  COLD           : 1   Median :  89.0   Median :  309.0  
##  COLD/WIND CHILL: 1   Mean   : 312.6   Mean   : 2953.0  
##  DENSE FOG      : 1   3rd Qu.: 188.0   3rd Qu.: 1206.0  
##  DUST STORM     : 1   Max.   :5633.0   Max.   :91346.0  
##  (Other)        :41
```

I'll set the threshold on the `INJURIES` to **272**, the third quantile:


```r
sub1_ag2 <- subset(sub1_ag, INJURIES>1206)
summary(sub1_ag2)
```

```
##                EVTYPE    FATALITIES        INJURIES    
##  EXCESSIVE HEAT   :1   Min.   :  15.0   Min.   : 1275  
##  FLASH FLOOD      :1   1st Qu.: 122.0   1st Qu.: 1456  
##  FLOOD            :1   Median : 487.0   Median : 2038  
##  HAIL             :1   Mean   : 979.0   Mean   :10679  
##  HEAT             :1   3rd Qu.: 947.2   3rd Qu.: 6591  
##  HURRICANE/TYPHOON:1   Max.   :5633.0   Max.   :91346  
##  (Other)          :6
```

```r
dim(sub1_ag2)
```

```
## [1] 12  3
```
Now that I have only 12 types left, let's move to the bar plot:
  


```r
library(reshape2)
sub1_ag3 <- melt(sub1_ag2[,c('EVTYPE','FATALITIES','INJURIES')],id.vars = 1)
names(sub1_ag3) <- c("Event_type", "type_victims", "number_victims")
library(ggplot2)
ggplot(sub1_ag3,aes(x = Event_type,y = number_victims)) + 
    geom_bar(aes(fill = type_victims),stat = "identity",position = "dodge") + 
    theme(axis.text.x = element_text(angle = 90))+
    ggtitle("Plot 1: Total number of injuries and fatalities caused\n by the 12 most dangerous event types") +
      xlab("Most threatening event types") + ylab("Total number of injuries / fatalities")
```

![](Assignment_C5_Week4_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

I'll modify the scale to log10 in order to gain more visibility:

```r
ggplot(sub1_ag3,aes(x = Event_type,y = number_victims)) + 
    geom_bar(aes(fill = type_victims),stat = "identity",position = "dodge") + 
    scale_y_log10()+ theme(axis.text.x = element_text(angle = 90))+
    ggtitle("Plot 2: Total number of injuries and fatalities caused\n by the 12 most dangerous event types") +
      xlab("Most threatening event types") + ylab("Total number of injuries / fatalities")
```

![](Assignment_C5_Week4_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
### Across the United States, which types of events have the greatest economic consequences?  
    
To answer this question, I need to use the column `PROPDMG` to get the Property damage estimates rounded to
three significant digits, and the `PROPDMGEXP`to get an alphabetical character signifying the magnitude of the estimated damage


```r
table(raw_data$PROPDMGEXP)
```

```
## 
##             -      ?      +      0      1      2      3      4      5      6 
## 465934      1      8      5    216     25     13      4      4     28      4 
##      7      8      B      h      H      K      m      M 
##      5      1     40      1      6 424665      7  11330
```

Alphabetical characters used to signify magnitude
include “K” for thousands, “M” for millions, and “B” for billions. That's why I'll only keep rows containing one of these three characters, before I create a numerical column of the full estimate damage:
  

```r
sub2 <- subset(raw_data, PROPDMGEXP %in% c("K", "M", "B"))
sub2 <- sub2[c("EVTYPE", "PROPDMG", "PROPDMGEXP")]
sub2$EVTYPE <- droplevels(sub2$EVTYPE)
sub2$PROPDMGEXP <- droplevels(sub2$PROPDMGEXP)
sub2$estimatDMG <- paste(sub2$PROPDMGEXP)
sub2$estimatDMG[sub2$estimatDMG=="K"]  <- "1000"              #thousands 
sub2$estimatDMG[sub2$estimatDMG=="M"]  <- "1000000"           #millions 
sub2$estimatDMG[sub2$estimatDMG=="B"]  <- "1000000000"        #billions 
sub2$estimatDMG <- as.numeric(sub2$estimatDMG)
sub2$estimatDMG <- sub2$estimatDMG * sub2$PROPDMG
sub2 <- aggregate(estimatDMG~EVTYPE, data=sub2, sum)
head(sub2,10)
```

```
##                    EVTYPE estimatDMG
## 1      HIGH SURF ADVISORY     200000
## 2             FLASH FLOOD      50000
## 3               TSTM WIND    8100000
## 4         TSTM WIND (G45)       8000
## 5                       ?       5000
## 6           APACHE COUNTY       5000
## 7  ASTRONOMICAL HIGH TIDE    9425000
## 8   ASTRONOMICAL LOW TIDE     320000
## 9               AVALANCHE    3721800
## 10          Beach Erosion     100000
```
  
  Now that we got our column of full damage estimates, let's order the dataframe according to the `estimatDMG` variable:

```r
sub2 <- sub2[order(sub2$estimatDMG, decreasing = T),]
head(sub2,10)
```

```
##                EVTYPE   estimatDMG
## 62              FLOOD 144657709800
## 178 HURRICANE/TYPHOON  69305840000
## 331           TORNADO  56925660480
## 280       STORM SURGE  43323536000
## 50        FLASH FLOOD  16140811510
## 103              HAIL  15727366720
## 170         HURRICANE  11868319010
## 339    TROPICAL STORM   7703890550
## 397      WINTER STORM   6688497250
## 155         HIGH WIND   5270046260
```
  
And now let's plot the results:
  

```r
sub2_head <- head(sub2,10)
ggplot(sub2_head,aes(x = EVTYPE,y = estimatDMG)) + 
    geom_bar(stat = "identity",position = "dodge") + 
    theme(axis.text.x = element_text(angle = 90))+
    ggtitle("Plot 3: Total estimates of property damage\ncaused by the ten most harmful events:") +
      xlab("Most harmful event types") + ylab("Total estimates")
```

![](Assignment_C5_Week4_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


## Results  
### First Question:  
  
Here is the list of the **12 most threatening** type of event:  
  

```r
paste( unique( droplevels(sub1_ag3$Event_type) ) )
```

```
##  [1] "EXCESSIVE HEAT"    "FLASH FLOOD"       "FLOOD"            
##  [4] "HAIL"              "HEAT"              "HURRICANE/TYPHOON"
##  [7] "ICE STORM"         "LIGHTNING"         "THUNDERSTORM WIND"
## [10] "TORNADO"           "TSTM WIND"         "WINTER STORM"
```
And as we can see in **Plot 1**, `TORNADO`is the most harmful event since it causes the biggest number both within injuries and fatalities, as we can check with the following code: 

```r
paste("The worst event in number of fatalities is ", sub1_ag3[which.max(sub1_ag2$FATALITIES),1])
```

```
## [1] "The worst event in number of fatalities is  TORNADO"
```

```r
paste("The worst event in number of injuries is ", sub1_ag3[which.max(sub1_ag2$INJURIES),1])
```

```
## [1] "The worst event in number of injuries is  TORNADO"
```
  
    
  
### second question:  
By looking at the third plot, it's clear that **Floods have the greatest economic consequences**. And more generally, the 10 most damaging types of event are:  
  

```r
head(paste(sub2$EVTYPE), 10)
```

```
##  [1] "FLOOD"             "HURRICANE/TYPHOON" "TORNADO"          
##  [4] "STORM SURGE"       "FLASH FLOOD"       "HAIL"             
##  [7] "HURRICANE"         "TROPICAL STORM"    "WINTER STORM"     
## [10] "HIGH WIND"
```




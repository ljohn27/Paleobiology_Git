## Lab 7 

> Awesome! 20/20

## Problem Set 1
1) What do the max_ma and min_ma columns of DataPBDB represent? If you do not intuitively know, you can always check the Paleobiology Database API documentation.

min_ma returns only records whose temporal locality is at least this old, specified in Ma. Max_ma returns only records whose temporal locality is at most this old, specified in Ma.

2) What is oldest age of each genus? [Hint: Use the tapply( ) and max( ) functions we've used in previous labs]. Show the code you would use to find out.

`> tapply(DataPBDB[,"max_ma"],DataPBDB[,"genus"],max)`

3) What is the youngest age of each genus? [Hint: Use the tapply( ) and max( ) functions we've used in previous labs]. Show the code you would use to find out.

`> tapply(DataPBDB[,"min_ma"],DataPBDB[,"genus"],min)`

4) Find which genus has the most occurrences in the dataset [Hint: Use the table( ) function!]. What code did you use?

Anadara has the most occurences in the dataset. 

````R
> OrderedPBDB<-table(DataPBDB[,"genus"])
> sort(OrderedPBDB)
````

5) What is the stratigraphic range of this taxon (i.e., your answer to question 4). Show your code.

````R
> tapply(DataPBDB[,"min_ma"],DataPBDB[,"genus"],min)
From the alphabetical output, I found Anadara, which had the value of min: 0.0000

> tapply(DataPBDB[,"max_ma"],DataPBDB[,"genus"],max)
From the alphabetical output, I found Anadara, which had the value of max: 66.0000
````

The range for Anadara is 66 million years. 

## Problem Set 2
1) Qualitatively describe what is happening in the following line of code. A good answer should identify what the different arguments are for each function, and what they are used for.

`mean(sample(Lucina[,"paleolng"],length(Lucina[,"paleolng"]),replace=TRUE))`

The mean longitude for the genus Lucina is calculated through a random resample. 

The mean() function codes for calculating the mean from the indicated information within the parentheses. This information includes the sample() function with takes a sample of the specifies size for the longitudinal coordinates from genus Lucina. It also includes the length (indicated by the length() function) of the longitudinal coordinates from genus Lucina. 

The replace=TRUE portion means that any samples drawn from the data pool can be placed back into the pool for another random pick. 


2) Plot a kernel density graph of ResampledMeans. Show your code. Does the distribution look approximately Gaussian? Explain why you think it does or does not.

`> plot(density(ResampledMeans))`

Yes, the distribution looks approximately Gaussian, as it follows the general outline of the expected bell-shaped curve. However, the top of the curve is not smooth and has a slight bump/indent-- there is a short plateau that climbs and then drops down to form the right end of the curve. 

3) Find the mean of ResampledMeans, is it similar to the mean of the original data?

Yes, it is similar to the mean of the original data. The mean of Resampled means came out to be 24.16227 while the mean of the original data was 24.1997. 

4) Sort ResampledMeans from lowest to highest. [Hint: We learned how to sort a vector in Lab 6].

`> sort(ResampledMeans)`

5) Now that you have sorted ResampledMeans, what is the 2.5th percentile of ResampledMeans and what is the 97.5th percentile of Resampled means. If you do not know what a percentile is, or how to calculate it, you can use google. Show your code.

97.5th percentile is 26.2861.

````R
> sortRM<-sort(ResampledMeans)
> length(sortRM)
[1] 1000
> 1000*.975
[1] 975
> sortRM[975]
[1] 26.2861

2.5th percentile is 21.81826.
> 1000*.025
[1] 25
> sortRM[25]
[1] 21.81826
````

6) Incidentally, these numbers (your answer to question 5) are the lower and upper confidence interval of the mean! Qualitatively explain why this is the case.

When looking at a standard curve for a 95% confidence interval, the tail ends of the curve fall at 2.5% on each side (because the 100-95=5, which is divided by two to cover the tails centered around the mean). This causes the lower confidence interval of the mean to fall at 2.5%, and the upper confidence interval of the mean to fall at 97.5%. 

## Problem Set 3

1) Based on the confidence intervals given above, do you think it likely or unlikely that Lucina is still alive?

It is likely that Lucina is still alive. This is because the earliest value of 0 indicates that the earliest date of extinction is in the present, while the latest date of extinction is in 1.221208 years from now. 

2) Find the extinction confidence interval for the genus Dallarca. Show your code.

````R
> Dallarca<-subset(DataPBDB,DataPBDB[,"genus"]=="Dallarca")
> estimateExtinction(Dallarca[,"min_ma"],0.95)
Earliest   Latest 
 2.58800 -3.88749 
````

3) A pure reading of the fossil record says that Dallarca went extinct at the end of the Pliocene Epoch. Based on its confidence interval, do you think it is possible that Dallarca is still extant (alive)?

Yes, it is possible that Dallarca is still alive because the confidence interval states that the latest it can do extinct is 3.88749 million years from now. 

4) In this case, should we trust the confidence interval or a pure reading of the fossil record? Explain your reasoning.

I would trust a pure reading of the fossil record, because the confidence interval presents a large range that brings about a great deal of variability. 

## Problem Set 4
1) State one ecological reason why this assumption is unlikey to be true.

An ecological shift in the environment (such as a mass extinction event) is unpredictable and varied throughout time. This means that some periods in time will have higher fossil preservation, while others might have little to none. Thus, a assuming a random model throughout time is inconsistent with the actual events that take place. 

2) State one geological reason why this assumpiton is unlikely to be true.

The instance of laggerstaten is geological evidence that an assumption of random distribution is unlikely to be true. Laggerstaten represent sites of exceptional fossil preservation that often occur in less than 1 meter of shale. Such patterns of sedimentation shows that rock formations can have unequal distributions of fossil preservation, meaning that preservation must change up or down a stratigraphic section. 

## Problem Set 5

1) How many occurrences are in DataPBDB. How many are in ExtantData? How many occurrences were lost by limiting our anaysis to only extant bivalves?

````R
> nrow(DataPBDB)
[1] 67617

> nrow(ExtantData)
[1] 59096

> 67617-59096
[1] 8521
````

There are a total of 8521 occurences lost. 

2) How many unique( ) genera were in DataPBDB and ExtantData, respectively. Using this information, what percentage of Cenozoic bivalves in the PBDB are still extant today.

````R
> length(unique(DataPBDB[,"genus"]))
[1] 1018
> length(unique(ExtantData[,"genus"]))
[1] 532
````

About 52.25933% of Cenozoic bivalves in PBDB are still extant today. 

3) Find the stratigraphic range of fossil occurrences for each genus in the ExtantData dataset. If you do not remember how to do this, revisit Problem Set 1 of this lab.

````R
> tapply(ExtantData[,"min_ma"],ExtantData[,"genus"],min)
> tapply(ExtantData[,"max_ma"],ExtantData[,"genus"],max)
````

4) Using your answer to question 3, find which genera in ExtantData are not extant according to the PBDB - i.e., do not have a minimum min_age of zero. Show your code.

````R
> ED<-tapply(ExtantData[,"min_ma"],ExtantData[,"genus"],min)
> ED!=0.0000
````

5) Calculate the confidence interval for the extinction of the following genera (careful with your spelling!): Scrobicularia, Meiocardia, Dimya, Digitaria, Cuspidaria, Arctica, Aloides, Kurtiella, Gouldia, and Acrosterigma. Show your code. What percentage of these taxa have confidence intervals indicating that the taxon might still be extant?

````R
> Scrob<-subset(DataPBDB,DataPBDB[,"genus"]=="Scrobicularia")
> estimateExtinction(Scrob[,"min_ma"],0.95)
 Earliest    Latest 
  0.01170 -34.70966 

> Meio<-subset(DataPBDB,DataPBDB[,"genus"]=="Meiocardia")
> estimateExtinction(Meio[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -5.329574 

> Dimya<-subset(DataPBDB,DataPBDB[,"genus"]=="Dimya")
> estimateExtinction(Dimya[,"min_ma"],0.95)
 Earliest    Latest 
 0.781000 -2.054688 

> Digitaria<-subset(DataPBDB,DataPBDB[,"genus"]=="Digitaria")
> estimateExtinction(Digitaria[,"min_ma"],0.95)
 Earliest    Latest 
 0.781000 -3.761154 

> Cuspidaria<-subset(DataPBDB,DataPBDB[,"genus"]=="Cuspidaria")
> estimateExtinction(Cuspidaria[,"min_ma"],0.95)
 Earliest    Latest 
2.5880000 0.7771965

> Arctica<-subset(DataPBDB,DataPBDB[,"genus"]=="Arctica")
> estimateExtinction(Arctica[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -1.799104 

> Aloides<-subset(DataPBDB,DataPBDB[,"genus"]=="Aloides")
> estimateExtinction(Aloides[,"min_ma"],0.95)
Earliest   Latest 
   5.333     -Inf 

> Kurtiella<-subset(DataPBDB,DataPBDB[,"genus"]=="Kurtiella")
> estimateExtinction(Kurtiella[,"min_ma"],0.95)
 Earliest    Latest 
  0.01170 -34.70966

> Gouldia<-subset(DataPBDB,DataPBDB[,"genus"]=="Gouldia")
> estimateExtinction(Gouldia[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -2.047386

> Acrosterigma<-subset(DataPBDB,DataPBDB[,"genus"]=="Acrosterigma")
> estimateExtinction(Acrosterigma[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -3.481128 
````

90% of these taxa have confidence intervals indicating that the taxon might still be exant. 

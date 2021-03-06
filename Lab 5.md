## Lab 5

> 15/20

## Problem Set I
1) What is Bivalve generic richness in the Miocene? What code did you use to find out?
The bivalve generic richness is 634. 

````R
> sum(table(BivalveAbundance["Miocene",]))
[1] 2272
> 2272-1638
[1] 634
````

2) What is the Berger-Parker Index of Brachiopods genera in the Pliocene? What code did you use to find out? [Hint: the function max( ) may help you).
The Berger-Parker Index is 0.708%.

#find the abundance of the most common species  
> max(BrachiopodAbundance["Pliocene",])
[1] 22

#find the total abundance of all species
> sum(table(BrachiopodAbundance["Pliocene",]))
[1] 3107

#divide
> 22/3107
[1] 0.007080785


3) What is the Gini-Simpson Index of Brachiopods during the Late Ordovician? What code did you use to find out?

````R
The Gini-Simpson Index is 0.9784588. 
> Brachi<-BrachiopodAbundance["Late Ordovician",]
> sum(Brachi)
[1] 6154
> 1-sum((Brachi/6154)^2)
[1] 0.9784588
````

4) What is the Shannon's Entropy of Bivalves during the Late Cretaceous? What code did you use to find out?
Shannon's Entropy is 5.086654.

````R
> Biv<-BivalveAbundance["Late Cretaceous",]
> sum(Biv)
[1] 29684
> NotZero<-Biv[Biv>0]
>-sum((NotZero/29684)*(log(NotZero/29684)))
[1] 5.086654
`````

5) What is the Shannon's Entropy of Bivalves during the Paleocene? What code did you use to find out?
Shannon's Entropy is 4.511875.

````R
> Biv<-BivalveAbundance["Paleocene",]
> sum(Biv)
[1] 4238
> NotZero<-Biv[Biv>0]
> -sum((NotZero/4238)*(log(NotZero/4238)))
[1] 4.511875
````

6) What is the percent change in Shannon's Entropy between the Late Cretaceous and the Paleocene? Can you think of any major events that happened between the Late Cretaceous and Paleocene that might be relevant to biodiversity? [Hint: Use google if you don't know.] Is this reflected in this index?

````R
> 5.086654-4.511875
[1] 0.574779
`````

At the Cretaceous-Paleocene boundary, there was a mass extinction event. This was a time marked by the demise of non-avian dinosaurs, giant marine reptiles, alongside a great deal of fauna and flora. Due to the die-off of the dinosaurs, the Paleocene epoch had many unfilled ecological niches worldwide. Thus, biodiversity decreased significantly in comparison when looking at the percent change of Shannon's entropy between the two epochs. 

> A percentage is not a difference. You needed to divide, not subtract. -1 Points.

7) What if you use the exp( ) function to exponentiate the Shannon's Entropies you calculated in questions 4,5, and 6 (i.e., e^Shannon's Entropy)? What percent of diversity is gained/lost? Does this better reflect the change between the Late Cretaceous and Paleocene? Why or why not? 

````R
> exp(c(5.086654,4.511875,0.574779))
[1] 161.847414  91.092457   1.776738
`````

> You need 1-exp(4.511875)/Exp(5.086654)
> -1 Points


## Problem Set II

Install (if you have not already) and load the vegan package into R. Read the help file for the diversity( ) function - ?diversity or help(diversity). You must have already loaded the vegan package in order for it to run.

1) Use the specnumber( ) function (also from the vegan package) to find Bivalve richness in the Miocene. What code did you use to find out?

````R
> specnumber(BivalveAbundance["Miocene",])
[1] 634
````

2) Use the diversity( ) function to find the Gini-Simpson Index of Brachiopods during the Late Ordovician? What code did you use to find out?

````R
> diversity(BrachiopodAbundance["Late Ordovician",],"simpson")
[1] 0.9784588
````

3) Use the diversity( ) function to find the Shannon's Entropy of Bivalves during the Late Cretaceous? What code did you use to find out?

````R
> diversity(BivalveAbundance["Late Cretaceous",],"shannon",base = exp(1))
[1] 5.086654
````

4) Use the diversity( ) function to find the Shannon's Entropy of Bivalves during the Paleocene? What code did you use to find out?

````R
> diversity(BivalveAbundance["Paleocene",],"shannon",base = exp(1))
[1] 4.511875
````

## Problem Set III

1) Is brachiopod richness positively, negatively, or un-correlated with bivalve richness? Show your code?
It is weakly negatively correlated. 

````R
> BivalveEpoch
  1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26 
 40  48  42  56  66  81  51 111  88  70  99 104 160 191 156 101 151 242 217 202 270 393 573 304 512 355 
 27  28  29 
634 534 498 
> BrachiopodEpoch
  1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26 
 40 282 331 271 257 234 138 497 353 274 314 284 564 549 406  63 111 193 130 175 111 125  96  29  57  30 
 27  28  29 
 45  23  19 
> cor(BivalveEpoch,BrachiopodEpoch)
[1] -0.5373391
`````

2) Is brachiopod biodiversity positively, negatively, or un-correlated with bivalve biodiversity when using the Gini-Simpson index? Show your code?

`````R
> diversity((BivalveEpoch),"simpson")
[1] 0.9428139
> diversity((BrachiopodEpoch),"simpson")
[1] 0.945563
````

When using the Gini-Simpson index to look at biodiversity of Bivalves and Brachipods, they have positive correlation, as the calculated indices are very close in proximity (0.9428139 vs. 0.945563). 

> That is not what is meant by correlation. -1 Points Please re-read the lab, particularly the section on correlation.

3) Looking just at changes in brachiopod richness through time, when did the greatest drop in brachiopod richness occur (i.e., between what two consecutive epochs)? 

The greatest drop occurred between groups 15 and 16, aka Lopingian and Early Triassic. 

````R
> BrachiopodEpoch
  1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19  20 
 40 282 331 271 257 234 138 497 353 274 314 284 564 549 406  63 111 193 130 175 
 21  22  23  24  25  26  27  28  29 
111 125  96  29  57  30  45  23  19 
`````

> -1 Points, you don't explain what those numbers mean or state when the drop happened.

-------------
Problem Set IV
1) Repeat the above steps, but for the BrachiopodAbundance community matrix. What is the standardized richness you got for brachiopods. Show your code.

> SampleAbundances<-apply(BrachiopodAbundance,1,sum)
> SampleAbundances[which(SampleAbundances==min(SampleAbundances))]
Pleistocene 
         63 
> StandardizedRichness<-apply(BrachiopodAbundance,1,subsampleIndividuals,Quota=63)
StandardizedRichness
    Mississippian     Pennsylvanian  Early Ordovician Middle Ordovician 
            42.81             34.32             38.14             46.08 
  Late Ordovician        Llandovery           Wenlock            Ludlow 
            41.65             41.09             43.66             44.11 
          Pridoli    Early Devonian   Middle Devonian     Late Devonian 
            41.22             49.66             41.00             39.62 
       Cisuralian     Late Jurassic            Eocene   Late Cretaceous 
            50.29             33.95             27.02             28.19 
 Early Cretaceous   Middle Jurassic         Paleocene         Lopingian 
            36.67             37.24             17.08             43.19 
   Early Jurassic       Guadalupian   Middle Triassic     Late Triassic 
            31.10             50.76             33.37             36.13 
          Miocene    Early Triassic          Pliocene       Pleistocene 
            24.46             23.77             18.85             19.00 
        Oligocene 
            25.28 


2) How does the standardized brachiopod richness (previous question) compare to the unstandardized brachiopod richness from Problem Set 3? Show your code. Explain your reasoning. [Hint: Don't forget to put your biodiversities in temporal order]

0.4786513

````R
> StndArrayBrachi<-OutOrder<-array(c(StandardizedRichness),dimnames=list(c("1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29")))
> StndArrayBrachi
    1     2     3     4     5     6     7     8     9    10    11    12    13    14    15    16    17    18    19    20    21 
42.81 34.32 38.14 46.08 41.65 41.09 43.66 44.11 41.22 49.66 41.00 39.62 50.29 33.95 27.02 28.19 36.67 37.24 17.08 43.19 31.10 
   22    23    24    25    26    27    28    29 
50.76 33.37 36.13 24.46 23.77 18.85 19.00 25.28 
> cor(StndArrayBrachi,BrachiopodEpoch)
[1] 0.4786513
````

3) Make a scatter plot of standardized brachiopod richness versus standardized bivalve richness. Make a second scatter plot of unstandardized brachiopod richness versus unstandardized bivalve richness. Compare and contrast the two plots. What are the differences or similarities? Does standardizing or not standardizing matter? Show your code and explain your reasoning in detail. [Hint: If you forgot how to plot, revist the previous lab]

Due to difficulties with R, I was unable to complete this question. However, I would assume that standardization does matter because it allows the data to be comparative at equal measure, allowing for better analysis when  drawing conclusions. On the other hand, unstandardized data can be a source for an incomplete/irresolute depiction of the data due to its inequal grounds parameters between the sets of comparison. 

> I'm sorry you had difficulties with R, but you did show some good reasoning so I'll give you half credit! -1 Points.

4) Do you believe that there is any evidence in these analyses to support the idea that bivalves outcompeted brachiopods over time? Explain your reasoning.

Yes, my analysis of the datasets seem to support such a claim. The plot would ideally show a trend in higher values of richness for bivalves than brachipods in epochs occuring later in the timeline, with the most apparent most likely in the standard set. 

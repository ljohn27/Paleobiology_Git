> 20/20, with some EC

Lab 12 

Problem Set I

1) Download four data sets from the paleobiology database. First, a dataset of Anisian-Rhaetian Synapsids, name it TriassicSynapsids. Second, a dataset of Anisian-Rhaetian Diapsids, name it TriassicDiapsids. Third, a dataset of post-Triassic Diapsids, name it JurassicDiapsids. Fourth, a dataset of post-Triassic Synapsids, name it JurassicSynapsids. Show your code.

> TriassicSynapsids<-downloadPBDB(Taxa=c("Synapsida"),StartInterval="Anisian",StopInterval="Rhaetian")

> TriassicDiapsids<-downloadPBDB(Taxa=c("Diapsida"),StartInterval="Anisian",StopInterval="Rhaetian")

> JurassicDiapsids<-downloadPBDB(Taxa=c("Diapsida"),StartInterval="Jurassic",StopInterval="Neogene")

> JurassicSynapsids<-downloadPBDB(Taxa=c("Synapsida"),StartInterval="Jurassic",StopInterval="Neogene")


2) How many Diapsid genera were there in the Triassic dataset? How many Synapsid genera? Show your code.

> JurassicDiapsids<-cleanRank(JurassicDiapsids,"genus")
> JurassicSynapsids<-cleanRank(JurassicSynapsids,"genus")
> TriassicDiapsids<-cleanRank(TriassicDiapsids,"genus")
> TriassicSynapsids<-cleanRank(TriassicSynapsids,"genus")

> length(TriassicDiapsids[,28])
[1] 2081

> length(TriassicSynapsids[,28])
[1] 357

3) How many Triassic Diapsid genera survived the Triassic/Jurassic transition? How many were victiims? How many Triassic Synapsid genera surivived the Triassic/Jurassic Transition? How many were victims? Show your code.

> DiapsidGenera<-unique(TriassicDiapsids[,"genus"])
> SynapsidGenera<-unique(TriassicSynapsids[,"genus"])


> DiapsidSurvivors<-intersect(DiapsidGenera,unique(JurassicDiapsids[,"genus"]))
> DiapsidVictims<-setdiff(DiapsidGenera,unique(JurassicDiapsids[,"genus"]))
> SynapsidSurvivors<-intersect(SynapsidGenera,unique(JurassicSynapsids[,"genus"]))
> SynapsidVictims<-setdiff(SynapsidGenera,unique(JurassicSynapsids[,"genus"]))

> length(DiapsidSurvivors)
[1] 37

> length(DiapsidVictims)
[1] 352

> length(SynapsidSurvivors)
[1] 9

> length(SynapsidVictims)
[1] 107


4)Calculate the odds ratio and log-odds that Diapsid genera were more likely to survive the T/J transition than Synapsids.

> DiapsidOdds<-(length(DiapsidSurvivors)/length(DiapsidGenera)) / (length(DiapsidVictims)/length(DiapsidGenera))
> SynapsidOdds<-(length(SynapsidSurvivors)/length(SynapsidGenera)) / (length(SynapsidVictims)/length(SynapsidGenera))
> OddsRatio<-DiapsidOdds / SynapsidOdds
> OddsRatio
[1] 1.249684
> log(OddsRatio)
[1] 0.222891

5)Using a 95% confidence interval, can you say that this odds/ratio is "statistically significant"? Show your code.

> StandardError<-sqrt(1/length(DiapsidSurvivors) + 1/length(DiapsidVictims) + 1/length(SynapsidSurvivors) + 1/length(SynapsidVictims))
> StandardError
[1] 0.3877175
> UpperLimit<-log(OddsRatio) + (StandardError*1.96)
> UpperLimit
[1] 0.9828172
> LowerLimit<-log(OddsRatio) - (StandardError*1.96)
> LowerLimit
[1] -0.5370353

No, the result is not "statistically significant"

----------
Problem Set II 

1) Download a dataset of Anisian-Rhaetian Diapsids and Synapsids, and a dataset of post-Triassic Diapsids and Synapsids. Show your code.

> ARDiSyn<-downloadPBDB(Taxa=c("Diapsida","Synapsida"),StartInterval="Anisian",StopInterval="Rhaetian")

> PostTriDiSyn<-downloadPBDB(Taxa=c("Diapsida","Synapsida"),StartInterval="Jurassic",StopInterval="Neogene")

2)Find the mean latitude of each genus's occurrences in your Triassic dataset. Show your code.

> MeanLatitudesTri<-tapply(PostTriDiSyn[,"paleolat"],PostTriDiSyn[,"genus"],mean)
> head(MeanLatitudes)
Error in head(MeanLatitudes) : object 'MeanLatitudes' not found
> head(MeanLatitudesTri)
Aaptoryctes    Aardonyx   Abderites  Abdounodus  Abelichnus Abelisaurus 
   52.72333   -42.64000   -36.81500    34.32000   -38.33000   -39.85000 

3)Find which Triassic genera were survivors and which were victims of the Triassic/Jurassic event. Show your code.

> PostTriSurvivors<-subset(PostTriDiSyn,PostTriDiSyn[,"genus"]%in%unique(ARDiSyn[,"genus"])==TRUE)
> PostTriSurvivors<-unique(PostTriSurvivors[,"genus"])
> head(PostTriSurvivors)
[1] "Morganucodon"   "Massospondylus" "Coelophysis"    "Tritylodon"     "Moyenisauropus" "Megalosaurus"  

> PostTriVictims<-subset(PostTriDiSyn,PostTriDiSyn[,"genus"]%in%unique(ARDiSyn[,"genus"])!=TRUE)
> PostTriVictims<-unique(PostTriVictims[,"genus"])
> head(PostTriVictims)
[1] "Cheilophis"    "Palaeophis"    "Thoracosaurus" "Georgiacetus"  "Pezosiren"     "Prognathodon" 

4) Find which genera of your Triassic dataset were Diapsids and which were Synapsids. Show your code.

> PostTriSynapids<-subset(PostTriDiSyn,PostTriDiSyn[,"genus"]%in%JurassicSynapsids[,"genus"]==TRUE)

> PostTriDiapsids<-subset(PostTriDiSyn,PostTriDiSyn[,"genus"]%in%JurassicDiapsids[,"genus"]==TRUE)

5) Perform a logistic regression where the outcome variable is Survivor/Victim and the input variable is the mean latitude of each genus. Show your code. Was the mean latitude of a Triassic genus a good predictor of its survival across the T/J extinction?

#Creating a Matrix
> PTVictims<-array(0,dim=length(PostTriVictims),dimnames=list(PostTriVictims))
> head(PTVictims)
> FinalMatrix<-merge(MeanLatitudesTri,PTVictims,all=TRUE,by="row.names")
> head(FinalMatrix)
    Row.names         x y
1 Aaptoryctes  52.72333 0
2    Aardonyx -42.64000 0
3   Abderites -36.81500 0
4  Abdounodus  34.32000 0
5  Abelichnus -38.33000 0
6 Abelisaurus -39.85000 0

#Cleaning up the matrix 
> FinalMatrix<-transform(FinalMatrix,row.names=Row.names,Row.names=NULL)
> colnames(FinalMatrix)<-c("MeanLatitudes","Survivor/Victim")
> head(FinalMatrix)
            MeanLatitudes Survivor/Victim
Aaptoryctes      52.72333               0
Aardonyx        -42.64000               0
Abderites       -36.81500               0
Abdounodus       34.32000               0
Abelichnus      -38.33000               0
Abelisaurus     -39.85000               0

#Calculate logisitic regression
> Regression<-glm(FinalMatrix[,"Survivor/Victim"]~FinalMatrix[,"MeanLatitudes"],family="binomial")
> summary(Regression)

Call:
glm(formula = FinalMatrix[, "Survivor/Victim"] ~ FinalMatrix[, 
    "MeanLatitudes"], family = "binomial")

Deviance Residuals: 
     Min        1Q    Median        3Q       Max  
-0.24129  -0.11678  -0.09168  -0.08633   3.13451  

Coefficients:
                                Estimate Std. Error z value Pr(>|z|)    
(Intercept)                    -4.889136   0.148167 -32.997  < 2e-16 ***
FinalMatrix[, "MeanLatitudes"] -0.016047   0.003888  -4.127 3.68e-05 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 559.61  on 7438  degrees of freedom
Residual deviance: 543.68  on 7437  degrees of freedom
  (6 observations deleted due to missingness)
AIC: 547.68

Number of Fisher Scoring iterations: 8

The slope tells us that for every one degree increase in the lattude of the genus, its (log) odds of having survived the T/J event decreases by 0.016047. Yes, the mean latitudes of the Triassic genus was a good predictor of extinction across the T/J extinction.  

-----------------------------------------------
Extra Credit

Perform a multiple logistic regression where the outcome variable is Survivor/Victim status and the input variables are the mean latitude of each genus and whether the gneus is a Diapsid/Synapsid. Is status as a Synapsid/Diapsid more or less important average paleolatitude of occurrences for survival? Show your code.

#Matrix for Survivor/Victim
> PTVictims<-array(0,dim=length(PostTriVictims),dimnames=list(PostTriVictims))

#Matrix for Synapsid/Diapsid
> PTriSynapsids<-array(0,dim=length(PostTriSynapids), dimnames=list(PostTriSynapids))

#Creating and Cleaning the Final Matrix
> FinalMatrix2<-merge(MeanLatitudesTri,PTriSynapsids,PTVictims,all=TRUE,by="row.names")

> FinalMatrix2<-transform(FinalMatrix,row.names=Row.names,Row.names=NULL)
> colnames(FinalMatrix)<-c("MeanLatitudes","Synapsid/Diapsid","Survivor/Victim")

> FinalMatrix[is.na(FinalMatrix[,"Survivor/Victim"],[,"Synapsid/Diapsid"]),]<-1
> head(FinalMatrix2)

#Run logisitic regression
>Regression<-glm(FinalMatrix2[,"Survivor/Victim"]~FinalMatrix[,"MeanLatitudesTri"] + [,"Synapsid/Diapsid"],family="binomial")
>summary(Regression)

The status of synapsid/diapsid is more important than average paleolatitudes when considering occurences for survival because both synapsids and diapsids were present before and after the T/J extinction; thus it is not deemed as critical when attempting to speculate survival. 

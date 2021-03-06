Leema John
Geosci 541 
Lab 4
2/22/16

> 17.2/20

## Problem Set I

1) How many unique genera are in the Miocene,Early Jurasic, Cretaceous, and Pennsylvanian epochs (not total, each)? What code did you use to find out?

+ Miocene: 602
+ Early Jurassic: 169
+ Cretaceous: 276 + 375 = 651
+ Pennsylvanian: 133

I figured this out by defining the row and then looking at the number of genera that was defined by "1" which indicated that there was genera located within it. The following is an example that I used for all of the epochs:

````R
>Penn<-PresencePBDB["Pennsylvanian",]
>length(which(Penn==1))
````

2) How many geologic epochs in general are in this dataset? What code did you use to find out? 
There are 29 geological epochs because there are 29 rows. 

````R
>dim(PresencePBDB)
[1]  29 902
````

3) Which epochs contain specimens of the genus Mytilus? What code did you find out.

There are 17 epochs: Pridoli, Early Devonian, Cisuralian, Late Jurassic, Eocene, Late Cretaceous, Early Cretaceous, Middle Jurassic, Paleocene, Oligocene, Miocene, Early Jurassic, Pleistocene, Middle Triassic, Late Triassic, Pliocene and Early Triassic. 

````R
> Mytilus<-PresencePBDB[,"Mytilus"] #this listed all the epochs
> length(which(Mytilus==1)) #this counted the number of epochs
[1] 17
````

4) Look at the epochs in the geologic timescale. Using your answer to question 3, in which epochs can we infer that Mytilus was present, even though we have no record of them in the Paleobiology Database? How did you deduce this?

We can assume that if Mytilus existed between two epochs that have additional epochs between them, then Mytilus existed in the additional epochs as well. 

Lopingian, Guadalupian (They are between Early Triassic and Cisurlian)

Pennsylvanian, Mississippian, Late Devonian Middle Devonian (They are between Cisurlian and Early Devonian)

## Problem Set II

1) Using your own custom R code, find the Jaccard similarity of the Pleistocene and Miocene "samples" in your PresencePBDB matrix. It is possible to code this entirely using only functions discussed in the R Tutorial, but here are some additional functions that may be helpful.

````R
#calculate interlapping genera 
> Test<-PresencePBDB[c("Pleistocene","Miocene"),]
> Test[1,]+Test[2,]
> table(Test[1,]+Test[2,])

  0   1   2 
287 106 510 

#find percentage
510/(106+510)= 82.79%
````

2) How can you convert your similarity index into a distance?

1-Jaccard similarity. In this case, it would be (1-0.8279)= 0.1720779


3) Install and load the vegan package into R. Read the help file for the vegdist function - ?vegdist or help(vegdist). Again, calculate the jaccard distance of the "Miocene" and "Pleistocene" samples of PresencePBDB, but this time use the vegdist( ) function. This should be an identical answer to what you got in question 2.

````R
> Test2<-vegdist(PresencePBDB[c("Pleistocene","Miocene"),],"jaccard")
> Test2
        Pleistocene
Miocene   0.1720779
````

4) Using the vegdist( ) function. Calculate the Jaccard distances of all the following epochs in PresencePBDB - the "Pleistocene", "Pliocene", "Miocene", "Oligocene", "Eocene", "Paleocene". What code did you use? Which two epochs are the most dissimilar?

````R
> Test2<-vegdist(PresencePBDB[c("Pleistocene","Miocene","Pliocene","Oligocene","Eocene","Paleocene"),],"jaccard")
> Test2
          Pleistocene    Miocene   Pliocene  Oligocene     Eocene
Miocene    0.17207792                                            
Pliocene   0.12692967 0.08496732                                 
Oligocene  0.26910299 0.16065574 0.18968386                      
Eocene     0.21870048 0.08585056 0.13397129 0.19063005           
Paleocene  0.44444444 0.33333333 0.37914692 0.41042345 0.33385827
````

Miocene and Pliocene are the most dissimilar because its value in the Jaccard Similarty Index is closest to zero at 0.08496732.

> No, the Jaccard being calculated here is a distance. So it is higher the *less* similar they are. So the Paleocene and Pleistocene and the most dissimilar. -1 Points


## Problem Set III

1) Create a subset of the PresencePBDB matrix which contains just the following rows - "Pliocene", "Oligocene", "Paleocene", "Early Cretaceous", "Late Jurassic", and "Middle Jurassic". Name this subset RandomEpochs. Show your code.
> RandomEpochs<-data.matrix(PresencePBDB[c("Early Cretaceous","Late Jurassic","Pliocene","Oligocene","Middle Jurassic","Paleocene"),])

2) Using vegdist( ) find the dissimilarities of all the epochs in Random Epochs. Show your code.
> Test<-vegdist(PresencePBDB[c("Early Cretaceous","Late Jurassic","Pliocene","Oligocene","Middle Jurassic","Paleocene"),])

3) Find the two epochs that are the most dissimilar and make them the poles. Now, using the dissimilarities, order (ordinate) the remaining epochs based on their similarity to the poles. State the order of your inferred gradient.

````R
                Early Cretaceous Late Jurassic  Pliocene Oligocene Middle Jurassic
Late Jurassic          0.3075269                                                  
Pliocene               0.5952663     0.7625330                                    
Oligocene              0.5974843     0.7627119 0.1047794                          
Middle Jurassic        0.3230769     0.1739130 0.7941176 0.7879656                
Paleocene              0.4706685     0.6532508 0.2339181 0.2581967       0.6572327
````R

> No the most is between the Middle Jurassic and the Pliocene

The most dissimilarity is between Pliocene and Oligocene. 

Pliocene (pole)
Middle Jurassic
Late Jurassic
Early Cretaceous 
Paleocene
Oligocene (pole)

> No, I can see why this would be difficult to answer since you had similarity and dissimilarity backwards. It's Middle Jurassic, Late Jurassic, Early Cretaceous, Paleocene, Oligocene, Pliocene.

4) Can you deduce what "variable" is controlling this gradient (e.g., temperature, water depth, geographic distance)? [Hint: Check the geologic timescale]. State your reasoning.

My best guess would be that water depth is the variable that is controlling the gradient. These time periods featured a great amount of species transitioning to land from water, so one can assume that water depth was involved. 

> Nope, these time periods are much later than most of the transitions from water to land. I can see why this would have been hard to answer since you didn't get the previous answers. -1 Points

5). There is a relatively high dissimilarity between the Early Cretaceous and Paleocene epochs. Can you hypothesize why this is? Google these epochs if you need to.

My hypothesis as to why there is so much dissimilarity between the two epochs is because while the Early Cretaceous had new types of dinosaurs coming to prominence, the Cretaceous period concluded with a mass extinction event at the Cretaceous-Paleocene boundary. This was a time marked by the demise of non-avian dinosaurs, giant marine reptiles, alongside a great deal of fauna and flora. Due to the die-off of the dinosaurs, the Paleocene epoch had many unfilled ecological niches worldwide. 

## Problem Set IV

1) Download a dataset from the paleobioogy database of all Ordovician aged animals into R, and name the object Ordovician. This may take a few minutes. What R code did you use?
Ordovician<-read.csv("https://paleobiodb.org/data1.2/occs/list.csv?base_name=Animalia&interval=Ordovician")

2) Clean up the poorly resolved genus names. What function/code did you use?

````R
>Ordovician1<-cleanGenus(Ordovician)
````

3) Turn your object Ordovician into a community matrix of samples by genera, where the samples are different geoplate codes. Geoplate codes denote different ancient paleocontinents - i.e., your community matrix will list which genera were present in which ancient paleocontinent. Cull this matrix so that each sample has a minimum of 25 taxa and each taxon occurs in at least two samples. Show your code.

````R
>MatrixOrdovician<-abundanceMatrix(Ordovician1["genera","geoplate"],[[25]])
````

> You needed presenceMatrix, not abundance Matrix. -1 Points


4) Perform a DCA on your new community matrix. Analyze your new DCA with a plot. Do you think that the orientation of samples along either axis 1 or axis 2 is related to the average latitude or longitude of each plate in question? Explain how you figured this out. Show your code.

````R
>MatrixOrdovician[1:5,c("geoplate","paleotlat","paleolong")]
>tapply(MatrixOrdovician1[,"paleolat"],MatrixOrdovician[,"geoplate"],mean]
>tapply(MatrixOrdovician1[,"paleolong"],MatrixOrdovician[,"geoplate"],mean]
>Ordovician1[which(Ordovician[,"geoplate"]==101),]
````

I think that 

> You didn't answer the qustion? -1 Points

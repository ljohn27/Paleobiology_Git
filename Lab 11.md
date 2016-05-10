> great, really well formatted

> 20/20

## Problem Set I 

1) You first mission is to download a list of all Triassic units in the Macrostrat Database using the /units route into R. Name this object TriassicUnits. As always, show your code.

````R
> URL<-"https://macrostrat.org/api/units?interval_name=Triassic&format=csv"
> TriassicUnits<-read.csv(URL)
````

2) How many Triassic units did you download? What code did you use to find out?

838 Units. 

````R
> dim(TriassicUnits)
[1] 838  17
````

3) Look at the first 10 units that you downloaded. Are they mostly Igenous, Metamorphic, or Sedimentary rocks? Do you believe that these units are relevant for an investigation of fossil preservation? Show your code and explain your reasoning.

`> TriassicUnits[1:10,6]`

They are mostly igenous rock. Only the units that are sedimentary rocks are relevant for fossil preservation. This is because the sediment is able to cover and protect the actual fossil so that it can be preserved. 

4) Which columns of your TriassicUnits data.frame denote the starting age and ending age of each unit, respectively?

"b_age" is the starting age and "t_age" is the ending age 

5) Considering the bottom and top ages of the first 10 units, are these units constrained to only the Triassic or do some range through the Triassic?

`> TriassicUnits[1:10,12:13]`

Some range through the Triassic. 

## Part II 

1) Re-download TriassicUnits, however, this time restrict the data to only sedimentary rocks. Show your code.
````R
> URL<-"https://macrostrat.org/api/units?interval_name=Triassic&lith_class=sedimentary&format=csv"
> TriassicUnits<-read.csv(URL)
````

2) Restrict TriassicUnits to only units that with starting ages <= start of the Triassic and ending ages >= to the end of the Triassic. As always, show your code.
````R
> TriassicUnits[which(TriassicUnits[,"t_age"] > 201 & TriassicUnits[,"b_age"] < 254),]
> dim(TriassicUnits[which(TriassicUnits[,"t_age"] > 201 & TriassicUnits[,"b_age"] < 254),])
[1] 604  17
````

3) Repeat the above processes (download and subset) for the following geologic periods. The Cretaceous, Jurassic, Triassic (you don't have to download it twice), Permian, Carboniferous, Devonian, Silurian, and Ordovician. Show your code, but do not show me the downloaded data.

````R
#Cretaceous 
> URL<-"https://macrostrat.org/api/units?interval_name=Cretaceous&lith_class=sedimentary&format=csv"
> CretaceousUnits<-read.csv(URL)
> CretaceousUnits[which(CretaceousUnits[,"t_age"] > 66 & CretaceousUnits[,"b_age"] < 152),]
> dim(CretaceousUnits[which(CretaceousUnits[,"t_age"] > 66 & CretaceousUnits[,"b_age"] < 152),])
[1] 4448   17

#Jurassic
> URL<-"https://macrostrat.org/api/units?interval_name=Jurassic&lith_class=sedimentary&format=csv"
> JurassicUnits<-read.csv(URL)
> JurassicUnits[which(JurassicUnits[,"t_age"] > 145 & JurassicUnits[,"b_age"] < 208),]
> dim(JurassicUnits[which(JurassicUnits[,"t_age"] > 145 & JurassicUnits[,"b_age"] < 208),])
[1] 952  17

#Permian 
> URL<-"https://macrostrat.org/api/units?interval_name=Permian&lith_class=sedimentary&format=csv"
> PermianUnits<-read.csv(URL)
> PermianUnits[which(PermianUnits[,"t_age"] > 252 & PermianUnits[,"b_age"] < 303),]
> dim(PermianUnits[which(PermianUnits[,"t_age"] > 252 & PermianUnits[,"b_age"] < 303),])
[1] 932  17

#Carboniferous 
> URL<-"https://macrostrat.org/api/units?interval_name=Carboniferous&lith_class=sedimentary&format=csv"
> CarboniferousUnits<-read.csv(URL)
> CarboniferousUnits[which(CarboniferousUnits[,"t_age"] > 299 & CarboniferousUnits[,"b_age"] < 372),]
> dim(CarboniferousUnits[which(CarboniferousUnits[,"t_age"] > 299 & CarboniferousUnits[,"b_age"] < 372),])
[1] 3066   17

#Devonian 
> URL<-"https://macrostrat.org/api/units?interval_name=Devonian&lith_class=sedimentary&format=csv"
> DevonianUnits<-read.csv(URL)
> DevonianUnits[which(DevonianUnits[,"t_age"] > 359 & DevonianUnits[,"b_age"] < 423),]
> dim(DevonianUnits[which(DevonianUnits[,"t_age"] > 359 & DevonianUnits[,"b_age"] < 423),])
[1] 2016   17

#Silurian
> URL<-"https://macrostrat.org/api/units?interval_name=Silurian&lith_class=sedimentary&format=csv"
> SilurianUnits<-read.csv(URL)
> SilurianUnits[which(SilurianUnits[,"t_age"] 419 > SilurianUnits[,"b_age"] < 445)]
> dim(SilurianUnits[which(SilurianUnits[,"t_age"] > 419 & SilurianUnits[,"b_age"] < 445),])
[1] 1212   17

#Ordovician 
> URL<-"https://macrostrat.org/api/units?interval_name=Ordovician&lith_class=sedimentary&format=csv"
> OrdovicianUnits<-read.csv(URL)
> OrdovicianUnits[which(OrodovicianUnits[,"t_age"] > 443 & OrdovicianUnits[,"b_age"] < 489),]
> dim(OrdovicianUnits[which(OrdovicianUnits[,"t_age"] > 443 & OrdovicianUnits[,"b_age"] < 489),])
[1] 2191   17
````

4) Make a vector named UnitFreqs that records the number of units present in each epoch. Show your code.

````R
> UnitFreqs<-c(4448,952,604,932,3066,2016,1212,2191)
> UnitFreqs
[1] 4448  952  604  932 3066 2016 1212 2191
````

5) Find the mean and standard deviation of UnitFreqs not counting the Triassic. How many standard deviations above or below the mean is the number of Triassic Units? Show your code.

````R
> UnitFreqs2<-c(4448,952,932,3066,2016,1212,2191)

> mean(UnitFreqs2)
[1] 2116.714

> sd(UnitFreqs2)
[1] 1286.488
````

6) Given your answer to the above, do you believe that the Triassic has a statistically lower number of units than the other periods? Why?

Yes. It only has 604 units, which is between 1-2 standard deviations away from the mean units of all the periods. 

7) What if you consider both the Triassic and Jurassic as potential outliers? Explain (show your code) how you arrived at your answer.

Yes, they are potential outliers. 

## Problem Set III

1) Download and plot a map of all North American geologic columns (no color). Show your code.

````R
URL<-"https://macrostrat.org/api/columns?format=geojson_bare&intervals?all&project_id=1"
GotURL<-getURL(URL)
> AllMap<-readOGR(GotURL,"OGRGeoJSON",verbose=FALSE)
> plot(AllMap)
````

2) On top of your map from Question 1, plot a map of all North American columns with Induan-Anisian sedimentary units. Use the Olenekian hexcode color for this map. You can look up the Olenekian hexcode through the /defs/intervals route.

````R
> URL<-"https://macrostrat.org/api/units?age_top=242?age_bottom=253&lith_class=sedimentary&format=geojson_bare&project_id=1"
> GotURL<-getURL(URL)
> IndAniMap<-readOGR(GotURL,"OGRGeoJSON",verbose=FALSE)
> plot(AllMap)
> plot(IndAniMap,col="#B051A5")
````

3) Using the downloadPBDB( ) function from previous labs. Download all occurrences of animal fossils in the Paleobiology Database of Induan-Anisian age. Using the points( ) function, plot all of these occurrences on the map you made in question 2 as solid circles. If you do not remember how to use points, you can consult previous labs or use help(points). Show your code. Save this map for later questions.

````R
> DataPBDB<-downloadPBDB(Taxa=c("Animalia"),StartInterval="Induan",StopInterval="Anisian")
> points(DataPBDB,y=NULL,type="p",pch=19)
````

4) You can open a new plot window using quartz( ) if you are on a mac or `windows( ) if you are on a windows machine. Download and plot a map of all North American geologic columns (no color) - i.e., repeat Question 1 in this new plot window. On top of this map, plot a map of all North American columns with Lopingian aged sedimentary units. Use the appropriate Lopingian hexcode color for this new map. Show your code.

````R
> windows(plot(AllMap))
> URL<-"https://macrostrat.org/api/units?lith_class=sedimentary&interval_name=Lopingian&format=geojson_bare&project_id=1"
> GotURL<-getURL(URL)
> LopingianMap<-readOGR(GotURL,"OGRGeoJSON",verbose=FALSE)
> plot(LopingianMap,col="#FBA794")
````

5) Download all occurrences of animal fossils in the Paleobiology Database of Lopingian age. Plot all of these occurrences on the map you made in question 2 as solid circles. Show your code.

````R
> DataPBDB2<-downloadPBDB(Taxa=c("Animalia"),StartInterval="Lopingian",StopInterval="Lopingian")
> windows(points(DataPBDB2,y=NULL,type="p",pch=19))
````

6) Compare and contrast your Induan-Anisian map versus your Lopingian map. Does it seem based on these maps that...

I believe that there was a substantial drop in the percentage of sedimentary units with reported fossils in them across the P/T boundary, but not in the areal extent of North American sedimentary units across the P/T boundary. Overall, I think that there is sufficient evidence from these maps to accept the hypothesis that lower diversity in the Early Triassic is an artefact of poor fossil record sampling of the available sedimentary rock. 

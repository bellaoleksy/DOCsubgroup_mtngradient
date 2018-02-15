# DOCsubgroup_mtngradient
We want to construct a null model the describes DOC (input?) to lakes along an elevation gradient. Based on first principles we can look at the fundamental relationships between elevation and temperature/precipitation and how this controls vegetation, soil carbon turnover, and temperature controls on DOC leaching.

Primary Drivers 
Decrease in Temperature 
Increase in Precipitation

Decreasing temperatures will influence DOC concentrations to receiving lake water bodies through a number of factors. 

1) temperature will determine vegetation composition. We will create a model to describe a change in vegetation from forest, shrub, grass, lichen to barerock. Along with these vegetation types, there are well documented carbon input rates and densities to soils. This gradient will result in a decrease in carbon inputs with elevation

2) temperature will determine the residence time of carbon in soils. As temperature cool, the rate declines and more carbon accumulates. Trumbore 1996 et al (Science) have developed an equation to describe this relationship (Carbon inventory weighted turnover time = 138exp[0.11*T] ). This relationship creates an increase in soil carbon stocks with elevation until the reduced carbon inputs of lichen and bare rock result in a strong decline. 

3) Because temperature influences the turnover rate it also influences the amount of labile carbon available for transport to lake ecosystems. This relationship creates a decrease in the flux of DOC with decreasing temperature, and upslope. 

The landscape or lake mosaic factors to consider are
Catchment Area, Lake Area, and the CALA
Lake Depth
Evaporation
Snow and seasonality
..
..

#### Elevation null model of temperature, vegetation, soil carbon and DOC flux
#### Brahney, Baron, Olesky, Rosen, ... 
#### February 2018


#load hypothetical gradient in landcover with a treeline based on MAT 11C (Jobbagy et al. 2000)
Landcov<-read.csv('Landcover.csv', header=TRUE) #could we should we model this with temp?

#constants
elevation <-seq(200,3200,100)
Cf<- 0.2514 #kg/m2/yr, carbon input rate to soils from forestest Plant Soil (2010) 337:151–165
Cs<- 0.12  #kg/m2/yr, carbon input rate to soils form grasslands and shrub *citaions needed
Cl<- 0.009  #kg/m2/yr, carbon input rate to soils form lichen, Garnett and Bradwell 2010
Cr <- 0 #kg/m2/yr, carbon input
Cp <- 1 #mg/L, carbon input from precipitation. 
CA <- 100 #m2

#equations
Precip = 0.375*elevation + 787 #model derived from ClimateWNA elevation gradients
MAT = -0.0045*elevation +13.265 #model derived from ClimateWNA elevation gradients
CarbonIn = (Landcov$forest/100*Cf) + (Landcov$shrub/100*Cs) + (Landcov$lichen/100*Cl) + (Landcov$bare/100*Cr) + (Cp*Precip/1000000)
TurnT = 138*exp(-0.11*MAT) #Weighted carbon turnover time from Trumbore et al 1996 in yr
Runoff = (5.96*Landcov$forest)+(13.24*Landcov$shrub)+(Landcov$lichen*21.65)+(Landcov$bare*19.16) #runoff generated as a function of vegetation in L/m2
SoilC = CarbonIn*TurnT #Soil carbon stocks in kg/m2
DOCRunOff=SoilC/Runoff*1000 #run off in mg/L


plot(elevation, DOCRunOff)

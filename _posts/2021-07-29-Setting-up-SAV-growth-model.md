
Setting up a BGCM + SAV growth model involves many steps. So here are notes 
to setup the case and do read the summary of notes in the end. 
I will keep refining these notes. 



***Initial file*** 

SAV growth model, these are initialized using standard values
following empirical relations (no need for source data)
1. AGB
2. BGB 
3. EPB
4. Plant density, diameter, thickness, height
5. Dinsed


More inputs for Biogeochem model->
6.  LdetritusC   - Large fraction carbon detritus concentration
7.  LdetritusN
8.  NH4
9.  NO3
10. SdetritusC - Small fraction carbon detritus concentration 
11. SdetritusN 
12. TIC - Total inorganic Carbon 
13. Talk - Total alkanity 
14. Chlorophyll
15. Oxygen
16. Phytoplankton 
17. Zooplankton 
18. salt
19. temp 


 
***Boundary forcing*** 
It could be any direction. North, South, West, East
1. temperature 
2. salinity 
3. Oxygen 

***Climatological Boundary forcing*** 
1. Uwind
2. Vwind
3. Pair
4. Tair
5. Qair
6. rain 
7. Swrad
8. Lwrad
9. cloud 

***River forcing (if any)*** 
In West Falmouth Harbor Additional bc forcing came from river inputs
In West Falmouth setup, only NO3 was non-zero
1. temperature
2. salt
3. nutrient 
4. NO3
5. NH4
6. DON 
7. PON 
8. Detritus
9. SDeN
10. SDeC
11. TIC 
12. Alkanity
13. Oxygen 

***Analytical inputs***
Other inputs and notes (Not to add confusion)
ana_biology.h
ana_clouds.h 
ana_tobc.h -> Had oxygen temperture and salt coming from western bc.
dont know why it was not done in bc file. 
Taran notes: I think I am going to keep them same as WFH

ana_srflx- solar shortwave radiation- In idealized case, I have an input in ana file
but in WFH I don't. Because I think WFH already has a bulk forcing file that contains 
the inputs to get srflx within the model 
Taran notes: May be not do it for CH Bay 


****Taran summary notes****:
For initial conditions: I don't know how one can get all of the initial tracers.
Some inputs are almost constant in WFH so can assume no geospatial variation 
Below are inputs that had REAL geospatial variation in initial file for WFH 
On the other hand none of these inputs were specified in the idealized case. So
biology model works without all this too. 

6.   LdetritusC   - Large fraction carbon detritus concentration 
7.   LdetritusN
8.   NH4 
9.   NO3
10.  TIC
11.  Cholorphyll 
12.  Oxygen
13.  phytoplankton
14.  Zooplankton 

 For boundary condition:  only temp, salt, oxygen, and climatology is required
--> In WFH NO3 was input
 

1. We adapted the bgcm equations of fennel in this modified file 
https://github.com/DOI-USGS/COAWST/blob/main/ROMS/Nonlinear/Biology/estuarybgc.h

2. This is where we added the SAV biomass calculations 
I would refer you to study the two pieces of code that are responsible for this model 
https://github.com/DOI-USGS/COAWST/blob/main/ROMS/Nonlinear/Biology/sav_biomass.h

3. As always, start with the distributed test case in COAWST which will help in understanding the model.

1. Is it possible to set Epiphyte growth to zero?
Response: You can set scl(epb) in inputs to 0. I think that should lead to epiphyte growth to zero

-2 How was Dissolved Inorganic Nitrogen in the sediment (DINsed) calculated?

Response: Dinsed gets passed as Dinsed_loc in sav_biomass.h
Dinsed is based on below ground biomass respiration and mortality: 

See this: 
https://github.com/DOI-USGS/COAWST/blob/main/ROMS/Nonlinear/Biology/sav_biomass.h#L467

-3. Does plant mortality put nutrients back to water column and soil?

Response: When it comes to the water column, the equations alter the water column nutrient loading through SAV mortality. 
The soil in theory gets impacted but we don't work with sediment chemistry in this model. In other words, sediment chemistry is currently not modified. Sediment properties through
their physical characteristics only modify the SAV growth. 



Setting up a BGCM + SAV growth model involves many steps. So here are notes 
to setup the case and do read the summary of notes in the end. 
I will keep refining these notes. 


----------------
Initial file
----------------
SAV growth model, these are initialized using standard values
following empirical relations (no need for source data)
1. AGB
2. BGB 
3. EPB
4. Plant density, diameter, thickness, height
5. Dinsed
-------------------

More inputs for Biogeochem model 
6.LdetritusC   - Large fraction carbon detritus concentration 
7.LdetritusN
8. NH4
9. NO3
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


BOUNDARY Forcing 
-----------------------
 Boundary forcing 
-----------------------
It could be any direction. North, South, West, East
1. temperature 
2. salinity 
3. Oxygen 

--------------
Climatological boundary forcing
---------------
1. Uwind
2. Vwind
3. Pair
4. Tair
5. Qair
6. rain 
7. Swrad
8. Lwrad
9. cloud 

In West Falmouth Harbor Additional bc forcing came from river inputs
In West Falmouth setup, only NO3 was non-zero
------------------
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


------------------Other inputs and notes (Not to add confusion)
ana_biology.h
ana_clouds.h 
ana_tobc.h -> Had oxygen temperture and salt coming from western bc.
dont know why it was not done in bc file. 
Taran notes: I think I am going to keep them same as WFH
--------------------------------------------


-----------------
ana_srflx- solar shortwave radiation- In idealized case, I have an input in ana file
but in WFH I don't. Because I think WFH already has a bulk forcing file that contains 
the inputs to get srflx within the model 
Taran notes: May be not do it for CH Bay 
--------------------


-----------------------------------------------------------
Taran summary notes:
For initial conditions: I don't know how one can get Ldetritus..all that initialized
. some inputs are almost constnt in WFH so can assume no geospatial variation 
What inputs had REAL geospatial variation in initial file

6.LdetritusC   - Large fraction carbon detritus concentration 
7.LdetritusN
8. NH4 
9. NO3
10. TIC
11. Cholorphyll 
12. Oxygen
13. phytoplankton
14. Zooplankton 

----------------------------------------------------------------------------
For boundary condition:  only temp, salt, oxygen, and climatology is required
--> In WFH NO3 was input
----------------------------------------------------------------------------



 

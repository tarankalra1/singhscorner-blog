We implemented the vegetation model that can account for the effects of seagrass in 2016-2017. Since then it is one of the widely used modules in the COAWST framework.
It is great when users with different backgrounds use the model because each application is unique and each user looks at the model from a different set of eyes. I am compiling 
a list of commonly asked questions in this thread that come from various users. That way, it can help other users. 

List of questions ! .
1.	What is the difference between vegetation diameter and thickness?

Ans: The attached schematic shows the difference between diameter and thickness. Diameter is used in the frontal area calculation (drag) from SAV. Both thickness and diameter are used in the calculation of vegetation flexibility and turbulence changes due to SAV. The attached table from our paper summarizes the appearance of these inputs in the modeled equations within COAWST. The data on input is hard to find so I think in our paper we assumed based on another study (Larkum 2007) that thickness would be an order of magnitude smaller than diameter. In any case, this would depend on the species. 
 
2.	 Do you need root depth...or can I infer that from below-ground biomass? 

Ans: I don’t know if we can infer below ground biomass from root depth. May be Jeremy Testa or other biologists can answer that.  We currently don’t use root depth in the model inputs. We do have below ground biomass (BGB) as a separate variable in the model that can allow for SAV growth. BGB can change based on the water column biogeochemistry that provide nutrient loading to allow for SAV evolution. 

3.	Can you have more that one veg type per cell? 

Ans. Yes, we can have more than 1 veg species per cell within the ROMS component of COAWST. In the SWAN component, we don’t have that provision yet. 

4.	Have you experimented with maps of realistic veg. changes?

Ans. Yes

5. Are there any special cases of how we have parameterized turbulence in the model and how it impacts sediment transport ?
 
Ans. This an excerpt from Nepf's paper: "It is commonly expected that dense patches of vegetation, because they damp flow and
turbulence, are associated with muddification, an increase in fine particles and organic content of the
underlying sediment relative to adjacent bare bed conditions. Recently, van Katwijk et al (2010)
observed that sparse patches of vegetation are associated with sandification, a decrease in fine
particles and organic matter, and they attribute this to higher levels of turbulence within the sparse
patch, relative to adjacent bare regions. A transition from a tendency for sandification (elevated
turbulence) to a tendency for muddification (diminished turbulence intensity) with increasing canopy"

In simple words, bare veg increases near bed turbulence, dense veg decreases near bed turbulence. Currently, we assume dense veg in the model.

6. Can the flexible vegetation in the model cause monamis ? 

Ans. Monamis are eddies formed due to flexible vegetation. We don't model vegetation at that scale at the moment to account 
for such effects.

7. Can we use vegetation model for modeling dune grass ? 

Ans. Both ROMS and SWAN calculate the amount of drag force that is the first order effect of vegetation immersed in water column. 
For the submerged component of dune vegetation, the 3-D COAWST model provides an accurate representation of dune grass presence. However the present 
methods do not account for the effects of aelion flow on dune erosion that provide feedback from the emergent part of dune grass;
that could play a vital role during storms. In addition, for the submerged component, more research efforts can estimate the appropriate 
choices for representing dune vegetation properties such as stem density, height and drag coefficient etc.

8. Also, is the vegetation-induced streaming (M3bstm) added to the output of the bottom streaming term, or can it be output as its own variable?

Ans. Yes, it is added to the bottom streaming term. Currently the contribution of the veg term on streaming is not being output on its own. 
But I think the output of wave induced streaming is an output. We can run 2 simulations one with and without veg to see veg contribution.

9. I'm also interested in the TKE production within the veg canopy, as we suspect it is responsible for sediment suspension as opposed to bed 
shear stress. It seems like TKE can indirectly enhance transport in ROMS by increasing the eddy viscosity, but I was wondering your thoughts on this.

Ans. The effect of TKE is to increase eddy viscosity so you are right about that. Depending on the water column depth, TKE 
should vary in magnitude. For a dense veg, TKE production is reduced due to veg presence. Things completely change in the real world when
veg is sparse but our implementation assumes a dense canopy presence.

10. Regarding the flexible vegetation module, I noticed that SWAN assumes a static vegetation height. How difficult would it be to pass the deflected 
vegetation height from ROMS to SWAN?

Ans. Yes you are right about the static vegetation height. I recently added a new capability in COAWST that would allow us to have SWAN with 
a varying height, diameter and coefficient of drag. This capability has been developed for application on coral reefs where Cd in waves and currents is very important and it is not static. Anyways, so my short point is that this capability is there in the latest version here:
However, we need to test a simple case with flexible veg and see things work as expected. This is completely untested and we have not advertised this 
feature because it is super new. 

A schematic description of vegetation properties
<img  src="https://user-images.githubusercontent.com/10886837/154726424-198a401b-7eb9-4612-ab5f-fecd08527971.jpg" width="800" height="500" />


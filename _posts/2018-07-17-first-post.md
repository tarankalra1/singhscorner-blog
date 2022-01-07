---
layout: post
title: Basic modeling differences between aerodynamics and hydrodynamics
---
These notes are made from my experience of working in aerospace and oceanographic models.
The notes are highly generalized and present limited examples.

Brief description of the aerospace and ocean model:

#### *a) Aerospace*:
The code that I worked for aerospace application in my PhD was called OVERTURNS.
It is being developed and maintained at University of Maryland. It has been widely used in rotorcraft
applications. Another example of a popular choice for rotorcraft simulations is NASA's OVERFLOW.

#### *b) Ocean*:
The code that I currently develop and use is called COAWST. COAWST itself is a coupled model that consists of an ocean model
ROMS, wave model SWAN and atmosphere model WRF. It is being developed and maintained at US Geological Survey's Woods Hole Marine Center. Within the ocean model, it has a sediment transport model, vegetation model and biogeochemistry models.
The main purpose of COAWST development is to solve for coastal science problems. For instance, one application would be to study how a hurricane/storm causes coastal erosion. 

The similarities in both models (aerodynamics and ocean model) include that they solve for N-S equations using curvilinear structured grids. These notes explain how some terminology refers to the same things or sometimes to different things in the two different worlds of modeling.

Now we will proceed with the differences in the model equations and setup methods. 

#### 1. CFD Assumptions in the models

#### *a) Aerospace*:
- Usually the aerospace applications can have either a compressible or incompressible solver. Any application with Mach number > 0.3 employs a compressible solver (change in density is significant). You end up solving one more equation for density variation and additional terms in NS equations for momentum and energy.

- Boussinesq approximation generally refers to the eddy viscosity hypothesis in RANS models. It basically means that the Reynolds stress tensor can be directly related to the mean strain rate. For the models it is useful to then assume that all the turbulence is parameterized into an eddy viscosity coefficient. Eventually the goal of the turbulence models is to solve for this eddy viscosity coefficient. 

#### *b) Ocean*:

- The ocean model ROMS is a hydrostatic model. It cannot resolve the fine structures.  Hydrostatic models only consider pressure changes with depth and what it means from a practical standpoint is that if the horizontal scales in the model are much larger than the vertical scales, one does not have to resolve the non-hydrostatic flow. So a hydrostatic model would not resolve small scale features caused by variation of vertical velocity in the horizontal direction which is okay for coastal engineering applications. Take a look at the difference of a classical problem of lock exchange (Similar to the classical shock tube problem in aerospace). Two different density fluids are mixed. The hydrostatic model is showing a front but non-hydrostatic model also captures the vortex-type features (horizontal features). This image is from the volume of oceanography published by Fringer et al. 2006. It shows the comparison of hydrostatic vs non hydrostatic model results. Normally one would _not_ need to resolve for the *billow* type features for coastal science applications.  

![hydrostatic_vs_nonhydrostatic model](https://user-images.githubusercontent.com/10886837/44286198-38391980-a236-11e8-8248-0ff4d0875465.png)

- In addition to the NS-equations, one has to solve for tracer equations using the material transport equation. Tracers are quantities that advect and diffuse in the system. Because water density is dependent on salinity and temperature, the tracer equations for these two are solved separately. 

- Following the above, the equation of state is not the same as in aerodynamics because density is dependent on salinity and temperature. 

-Boussinesq approximation in ocean models generally refers to the fact that the perturbations in density are small compared to the mean density (*very different from aerospace models*). The implication to the N-S equations is the flow can be considered incompressible. In wave models, it refers to the approximation that the water waves are having a longer wavelength compared to the water depth.  

#### 2. Grids/Meshes

#### *a) Aerospace*:
- Typical grid types for blades are C-O type or C-H type grids

- Reynolds number (Re) determines the extent of mesh resolution in aerospace applications because it is desired that one resolves the boundary layer. To resolve the boundary layer, we can calculate the minimum grid size using NASA's viscous wall spacing calculator (y+=1)
(Link to viscous calculator - https://geolab.larc.nasa.gov/APPS/YPlus/). 
*Look at the C-O type blade mesh*. 

<img  src="https://user-images.githubusercontent.com/10886837/44048498-81ce502c-9eff-11e8-9084-bd358fb8d79b.png" width="500" height="500" />


#### *b) Ocean*:
- Typical grid types for coastal applications use Arakawa C-grids. Because the water level is varying, one has to resolve that and there is a method known as sigma coordinate system that allows for vertical layers to change dynamically. There are various flavors of sigma coordinate systems depending on what one is trying to resolve- free water surface, boundary layer at the bottom of the ocean etc.  

- The aspect ratio of grid cells is usually large. The horizontal cells are very coarse (of the order of 10's of meters or 1 to 2 km sometimes) compared to vertical grid cells (of the order of 1 m) for coastal applications. The area that we are trying to model horizontally is of the order of kilometers while the depths are ranging from 1m to 50m. Beyond 200m, deep ocean dynamics prevail and one cannot rely on the setups as described here. Take a look at the grid of Barnegat Bay in New Jersey with a horizontal resolution of 40m. The image is processed to show only every tenth point point in the domain. Thank you to Zafer Defne at USGS Woods Hole for providing this image. Also look at the snapshot of Barnegat Bay from Google Earth.   
![Barnegat Bay Grid](https://user-images.githubusercontent.com/10886837/45499866-3b4afb00-b74b-11e8-9852-8b9e1d916ac4.png)
![Barnegat Bay from Google Earth](https://user-images.githubusercontent.com/10886837/45499836-25d5d100-b74b-11e8-9d2a-0bfac7cc90fd.png)
 
#### 3. Advection schemes
#### *a) Aerospace*:
- Higher order schemes such as a fifth order WENO scheme is not uncommon. Resolving vortices with
higher resolution is one of the significant issues because they cause high amount of drag in both fixed wing and rotating flows.

#### *b) Ocean*:
- Higher order schemes are usually not used as the grids are coarser. MPDATA is a common scheme that is used in geosciences. It is from Lax-wendroff family of schemes. It ensures that the tracer quantities that are convected in ocean models do not go negative (to ensure physical solutions). The vertical dimension is treated differently than horizontal because it can be resolved to a finer degree. It can employ high order schemes and use implicit time marching. 

#### 4. Time Marching
#### *a) Aerospace*:
- Time marching is usually done implicitly. Loss of accuracy in implicit methods is compensated by higher grid resolution. The advantage of having implicit time marching that one can take bigger time steps (CFL condition is relaxed). For unsteady simulations Newton sub-iterations are used to march faster in time by using a dtpsuedo (psuedo time step)
to have steadiness within an unsteady step.

#### *b) Ocean*:
- Explicit time marching is used in horizontal directions. 
Mode splitting- Time steps are split up in 2 D and 3 D modes. The 2D resolves the depth averaged system (barotropic) i.e. density does not change with depth. 3D resolves the baroclinic system i.e. density is depth dependent on depth. Both the modes are coupled to each other to exchange velocity fields from the 2 D to 3 D modes. 


#### 5. Overset meshes/nesting
Nesting is done to resolve areas of importance in a flow field by having multiple meshes in the same region and telescoping from coarser to finer resolution. 

#### *a) Aerospace*:
- The curvilinear coordinates in aerospace applications utilize overset grids in such a way that the child grid (embedded grid) is stretched at the boundaries to match the cell size of the background mesh. This is to reduce the interpolation errors. 

#### *b) Ocean*:
- In ocean/atmosphere models, the child grid or embedded mesh does not stretch at the boundaries. The mesh is generally over a smaller region so it is much flattened out. That means the mesh is not very curvy and the interpolation errors would be minimal as long as it overlaps with the parent grid and one knows the ratio of child to parent grid. Because the vertical resolution is sufficiently resolved in both parent and child grid, the vertical spacing is normally not altered from child to parent in ocean/atmosphere models. That means only horizontal grid is nested. 


#### 6. Domain decomposition for parallel processing 
#### *a) Aerospace*:
- In overset grids/nested grids for aerospace the embedded grid and the background grid both do calculations at the same type. So, one can do the domain decomposition over all grids. so if you have 2 grids. You can divide both the grids into "N" number of processors. The exchange of information happens at each time step at the boundaries from child to parent.
So effectively you can use more computational power as you are domain decomposing all grids.

#### *b) Ocean*:
- In atmosphere/ocean models the domain decomposition happens only at a particular grid first so first the parent would run, then the child would run over the same processors. this means that when parent is running, child has to wait and vice versa. Then the information from child to parent is sent back. That completes one cycle of time stepping. 
The reason for this is that in these models, not only solve for velocity and pressure evolve (N-S equations) but also tracers evolve. For child grid usually a smaller time step is required for CFL criterion (Most models are explicit in time marching
except for vertical direction). So the child is slowly evolving in time. If one hypothetically ran all the grids in parallel that would mean that child and parent are both evolving differently.

#### 7. Turbulence Modeling
#### *a) Aerospace*:
- Usually RANS models utilize Spalart Allamaras model in its separate variant forms is very common for external flows. It is designed for thin shear layers and does not work for massively seperated airflow.

#### *b) Ocean*:
- Usual RANS models only solve for turbulence in vertical direction. COAWST model has several 2-equation turbulence models for the vertical including k-epsilon, k-omega model. Remember the horizontal dimensions are highly diffusive and can use simple algebraic model. The grids being highly coarse in horizontal direction in usual applications of ocean sciences.

#### 8. Boundary layer (BL) 
#### *a) Aerospace*:
- As I mentioned before, the resolution of boundary layer is required to resolve vortex structures. They can be a big source of drag. 
*Take a look at the tip vortices from a 1m blade with a chord length of 0.405 m operating at a Reynolds number of 250,000 at a Mach number of 0.24.* 
<img  src="https://user-images.githubusercontent.com/10886837/44286375-e47b0000-a236-11e8-8764-35a9be5a18b6.png" width="300" height="300" />

#### *b) Ocean*:
- Rarely in coastal engineering, the goal is to resolve the BL. So a logarithmic profile, linear or quadratic profile fit can be done to satisfy the no-slip condition. 

## Some final thoughts: 
The differences that I mentioned above are really summarized and each of the 8 points can be elaborated for more clarity. I may do that in the future posts.

In general, the concern in aerospace applications is to *design* rotors, wings etc. and the need of the hour is to resolve small scale physics. On the other hand, in coastal science applications; the need is to understand the physical phenomenon that causes storms, erosion patterns etc. at a larger scale. This is to study regional systems such as estuaries, bays and islands. In the latter, the local bathymetry, wind pattern, even local physical features such as seagrass can change the dynamics of a system. 

To put it simply, the challenges are entirely different. 


References for this post:
1. O. B. Fringer, J. C. McWilliams, and R. L. Street, 2006, "A new hybrid model for coastal simulations", Oceanography, 19 (1), 46-59.
2. T. S. Kalra, 2014, CFD modeling and analysis of rotor wake in hover interacting with a ground plane", PhD Dissertation, University of Maryland.
3. Z. Defne, N.K. Ganju, "Quantifying the residence time and flushing characteristics of a shallow, back-barrier estuary: application of hydrodynamic and particle tracking models," Estuaries and Coasts 38 (5), 1719-1734	44	2015

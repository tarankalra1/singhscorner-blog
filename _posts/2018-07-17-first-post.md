---
layout: post
title: Basic modeling differences between aerodynamics and hydrodynamics
---

### AEROSPACE vs OCEANOGRAPHY MODELING

These notes are made from my experience of working in aerospace and oceanographic models.
The notes are highly generalized and present limited examples.

Brief description of the aerospace and ocean model:

##### a) Aerospace model:
The code that I worked for aerospace application in my PhD was called OVERTURNS.
It is being developed and maintained at University of Maryland. It has been widely used in rotorcraft
applications. Another example of a popular choice for rotorcraft simulations is NASA's OVERFLOW.
##### b) Ocean model:
The code that I currently develop and use is called COAWST. COAWST itself is a coupled model that consists of an ocean model
ROMS, wave model SWAN and atmosphere model WRF. It is being developed and maintained at US Geological Survey's Woods Hole Marine Center. Within the ocean model, it has a sediment transport model, vegetation model and biogeochemistry models.


Now we will proceed with the differences in the model equations and setup methods. 

#### 1. CFD Assumptions in the models

##### a) Aerospace:

1.1) Usually the aerospace applications can have either a compressible or incompressible solver. Any application with Mach number > 0.3 employs a compressible solver (change in density is significant). You end up solving one more equation for density variation and additional terms in NS equations for momentum and energy.

1.2) Boussinesq approximation generally refers to the eddy viscosity hypothesis in RANS models. It basically means that the Reynolds stress tensor can be directly related to the mean strain rate. It allows 

##### b) Ocean:

1.1) The ocean model ROMS is a hydrostatic model. It cannot resolve the fine structures. Look at the difference between a hydrostatic and non-hydrostatic models. Hydrostatic models follow Boussinesq approximation that has a completely different meaning that the aerospace modeling one. It refers to the assumption that the density variation in vertical is negligible. It is true for coastal engineering applications. 

1.2) In addition to the NS-equations, one has to solve for tracer equations using the material transport equation. Tracers are quantities that advect and diffuse in the system. Because water density is dependent on salinity and temperature, the tracer equations for these two are solved separately. 

1.3) Following the above, the equation of state is not the same as in aerodynamics because density is dependent on salinity and temperature. 

#### 2. Grids/Meshes

##### a) Aerospace:
1.1) Typical grid types for blades are C-O type or C-H type grids

1.2) Reynolds number determines the extent of mesh resolution in aerospace applications because it is desired that one resolves the boundary layer. A blade operating at a subscale rotor of 1m radius operating at a Mach number of 0.24, i.e. velocity= would have a Reynolds number of . To resolve the boundary layer using NASA's viscous wall spacing calculator (y+=1)
gives a minimum grid size required on the blade to Note that this is the vertical dimension.

1.3) The aspect ratio of grid cells is usually 1:3 (Horizontal to vertical grid cells)

##### b) Ocean:
1.1) Typical grid types for coastal applications use Arakawa C-grids. Because the water level is varying, one has to resolve that and there is a method known as sigma coordinate system that allows for vertical layers to change dynamically. There are various flavors of sigma coordinate systems depending on what one is trying to resolve- free water surface, boundary layer at the bottom of the ocean etc.  

1.2) In ocean model for coastal engineering applications, grid resolution is decided by the amount of computational resources. It is of the order of 30-40 m. at the finest near to a coast in the horizontal (both x and y). Look at the grid of Barnegat Bay in New Jersey with a horizontal resolution of 40m. 

1.3) The aspect ratio of grid cells is usually 100:1 (Horizontal to vertical). The horizontal cells are very coarse compared to vertical grid cells for coastal applications. The area that we are trying to model horizontally is of the order of kilometers while the depths are ranging from 1m to 50m. Beyond 200m, deep ocean dynamics prevail and one cannot rely on the setups as described here. 



#### 3. Advection schemes (Space marching)
##### a) Aerospace:
- Higher order schemes such as a fifth order WENO scheme is not uncommon. Resolving vortices with
higher resolution is one of the significant issues because they cause high amount of drag in both fixed wing and rotating flows.

##### b) Ocean:
 Higher order schemes are usually not used as the grids are coarser. MPDATA is a common scheme that is used in geosciences. It is from Lax-wendroff family of schemes. It ensures that the tracer quantities that are convected in ocean models do not go negative (to ensure physical solutions).

#### 4. Time Marching
##### a) Aerospace:
Time marching is usually done implicitly. Loss of accuracy in implicit methods is compensated by higher grid resolution. The advantage of having implicit time marching that one can take bigger time steps (CFL condition is relaxed). For unsteady simulations Newton sub-iterations are used to march faster in time by using a dtpsuedo (psuedo time step)
to have steadiness within an unsteady step.

##### b) Ocean:
Explicit time marching is used in horizontal directions. 
Mode splitting- Time steps are split up in 2 D and 3 D modes
2D resolves the depth averaged system (barotropic) i.e. density does not change with depth
3D resolves the baroclinic system i.e. density is depth dependent
these 2 modes are coupled using a ....


In general the concern in aerospace applications is to design rotors, wings etc. and the need of the hour is to resolve small scale physics, however, in coastal science applications the need is to predict storms, erosion patterns etc. for regional systems such as bays, islands and the need of the hour is to account for different physical phenomenon. In the latter, the local bathymetry, wind pattern, even seagrass can change the dynamics of a system. 
To put it simply the challenges are entirely different. 
<img src="rect_tipvortex.png" width="200" height="200" />

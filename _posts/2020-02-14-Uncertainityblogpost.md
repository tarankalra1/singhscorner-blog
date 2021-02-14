I had contributed to the discourse on using Effective Quadratures as a tool for sensitivity analysis in coastal models in an original post here.

https://discourse.equadratures.org/t/eq-methodology-for-coastal-models/95/3

Here is a copy of my post made to the discourse forum. 

## Contrasting aerodynamics with coastal applications

CFD models in aerodynamic engineering applications involve solving the 3-D Navier Stokes equations. The turbulence in the simulations is the only unresolved or modeled component of CFD models that are “approximated” scales (use of Boussinesq approximation). This is because the density of air is depending on pressure and temperature. Most modeling applications are trying to resolve the effect of airflows around a surface in order to optimize vehicle design. While the world of aerodynamics is trying to engineer the flow around a surface, geophysical models are mostly attempting to model the processes. This discussion is focused only on coastal science applications within the larger world of geophysical flows. This is because a single simulation can attempt to model processes that can range from a scale ranging over 10’s of km to capture the eye of hurricane to a few cm close to the boundary layer. The density of water (the fluid in consideration) not only gets affected by pressure, temperature but also nutrient, sediment and salinity concentrations.

Significance of EQ type methods in coastal modeling
More specifically, there is a wide-ranging interest on the effects of tropical storms on coastal sediment budgets. The Hurricane damage due on the coasts of USA is estimated to be $136 billion for the years (2018-19) (Source: NOAA). With sea level rise, the effect of storm surge is bound to aggravate the problem. Due to these The damage to wetlands that provide several ecosystem. In the last 2 decades, the application of coastal models has focused on understanding on answering questions such as how much is the damage to ecosystems during storms ? How much can natural barriers such as seagrass or sand dunes alleviate the effects of storms ? How long can marshes keep up with sea level rise ? In answering those questions, coastal models have added on several sub-models that parameterize the physical effects of sub-systems within the framework of solving the hydrodynamics using the Navier Stokes equations. In contrast to aerodynamic models, coastal models not only model turbulence, but also model several processes that could encounter the dynamical processes involving sediment, atmosphere, wave, biogeochemical and geomorphological dynamics. In modeling a problem, a number of model input parameters can influence the output metrics of the model. Therefore, there is a case to be made of utilizing a tool that can effectively perform sensitivity and uncertainity analysis.

Working with several input parameters
Following this, a research effort in 2017 involved in using the sensitivity analysis package from the Effective Quadratures (EQ) methodolgy. The goal of the work was to quantify the sensitivity of four model input parameters that characterized seagrass (seagrass density, height, diameter and thickness) and knowing the sensitivity of these inputs to the output of water level, wave dissipation and kinetic energy change around the region of seagrass. In nature, these inputs depend on the species of seagrass, underlying weather conditions, thus leading to a large range of values. For example, a range for vegetation stem density is chosen as between 38 to 250 stems/m2.

The advantage of using the EQ methodology was that the user could provide a wide range of values of these inputs to the “solver”. It would then sample from the range and inform the user with a subset of simulations to be performed with various combinations of inputs. In this case study with four inputs, a total of 15 simulations were found to be required by the EQ software, corresponding to the number of coefﬁcients in a 4-D polynomial with a maximum order of 2. Sobol indices were found that quantified the relationship between the input and output parameters. Table 1 shows the results from the study. In simple words, results from table 1 imply that the wave dissipation depends more on stem density than any other input seagrass input. On the contrary, water level change depended both on seagrass height and stem density. The quantification of sensitivity of inputs to output metrics gives a detailed insight into the mechanics of the model.

Plant Stem Density	Plant Height	Plant Diameter	Plant Thickness
Wave dissipation	0.68	0.24	0.032	0.01
Kinetic energy	0.36	0.44	0.12	0.03
Maximum water level change	0.38	0.43	0.15	0.01
Turbulent kinetic energy	0.35	0.42	0.12	0.03
Table 1: Sobol Indices for output metrics (Source Kalra et al. 2017).

One can extrapolate to using a similar methodology to understand the variation of sediment inputs or biological inputs on the model input that in nature also exhibit a large variation.

Choice of sub-models

Other than the knowledge of varying input parameters to model output, a common practice in coastal models is to choose from a number of available sub models that parameterize the same physical phenomenon. For instance, the effects of surface wind stresses in driving the water column, the choice of various techniques quantifying the bottom boundary layer to name a few. Because the underlying variables in the choice of sub-models may or may not be common, a software package such as the EQ methodology can reveal their effect on underlying physics. It may reveal patterns that certain sub-models work better in a given regime of flow and vice-versa.

Uncertainty of cause and effect

Besides the input parameters, choice of sub-models, there is a need to understand the uncertainty of choosing a bathymetry/topography in the resulting simulation. Unlike the fixed surface geometry in an aerodynamic model, bathymetry or seafloor is both the cause and outcome of changing hydrodynamics. This is a problem that can affect the predictions of flooding from storm surge. The EQ methodology could be perhaps used to quantify the uncertainty in bathymetry and its effect on water level or flooding.

Future

In the end, the next few decades of coastal models would still work with similar methods that are discussed above. The confluence of sub-models from various sciences such as physics, biology, geology etc. is only going to increase as more processes are incorporated, thus adding to the complexity of models. This calls for a need of a toolkit that can help make sense of the results from the underlying models and the EQ-methodology could fit perfectly into that framework.

References:

Hurricane Costs 1
Sensitivity analysis of a coupled hydrodynamic-vegetation model using the effectively subsampled quadratures method (ESQM v5. 2) 1, TS Kalra, A Aretxabaleta, P Seshadri, NK Ganju, Journal of Geophysical Model Development 2017

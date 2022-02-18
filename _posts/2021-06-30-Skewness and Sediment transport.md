
I have been thinking about future model improvements for improving our calibration
of a bedload method in the COAWST model. The new bedload method in COAWST model allows for first estimating asymmetric wave shape and then using the characteristics of the wave shape as shown in fig.1 (borrowed from our annual review paper) to get the wave-driven bedload. There are good reasons for including wave driven bedload in the model and earlier bedload methods dealt with the shape of the waves in a rather crude manner. The goal of all parameterizations is to improve in time and provide higher fidelity solutions which resemble closer to "real" dynamics. The work of Vander et al., 2013 is implemented in COAWST now where we go from parameterization wave shape to estimating a wave-driven bedload.

![image](https://user-images.githubusercontent.com/10886837/124130202-83e31880-da4c-11eb-904d-a1af7aebc919.png)


One issue that the methodology of obtaining asymmetric wave shape is that the current methods only provide an asymmetry in such a way that crest cycle velocity and acceleration exceeds trough cycle. This is referred to as having positively skewed waves. However, when we look into near bed data of waves, the math of that analysis provides us with both positively and negatively skewed waves. So what does that mean ? Do negatively skewed waves exist in real life ? What are the repercussions of that to bedload transport ? There is one good study of Crawford and Hay on this topic. Because the methodology in the model never predicts negatively skewed waves, are we overestimating the bedload in one direction (onshore ) ? 

So in this post, I am thinking about the connection between skewness and sediment transport. One way that sediment transport manifests in real life is through ripple migration.  
The paper from Crawford and Hay (2001) is the first one that i found and the paper showed that  both negative and positive skewness at 3m depth during a storm event. Their work correlated positive skewness with onshore ripple migration and negative skewness with ripple offshore migration nonlinear irregular waves. From their paper: 

_Crawford and Hay: "The close association between skewness and cross-shore bed form migration (both onshore and offshore) is new. This
result provides compelling evidence for a direct connection between nonlinear wave forcing and the cross-shore direction
of sediment transport, particularly the bed load component. Thus simplified representations of the incident wave field, such 
as the commonly used significant wave height and peak period, would predict transport in a direction entirely opposite to that observed when the wave spectrum was bimodal. Therefore it would appear to be essential, when predicting cross-shore sediment transport in the field, that the full wave spectrum be considered for even weakly
nonlinear irregular waves."


Another paper from Houser et al. showed that the acceleration skewness became important during breaking waves caused offshore transport.  This makes sense because vandera's method does not get applied under breaking wave regime and the Houser paper mentions that under the effect of breaking waves, IG band and undertow led to sediment moving offshore. Houser et al. performed observations in a intertidal swash bar of transport mechanics. This is from the paper 
_As the average wave breaks close to, or landward of, the transport contribution
due to acceleration skewness under the surf bores and unbroken waves remains apparent, but sediment transport was
directed offshore in response to infragravity waves and undertow. While evidence is presented to suggest that the
infragravity transport was a response to infragravity wave modulation of the acceleration skewness. the
cross-shore mean flow alone was capable of suspending sediment. Using a conservative estimate of the threshold of
motion of the local sand the undertow was capable of entraining sediment in 97 per cent of the datasets
collected on the seaward slope.

The unpublished paper from Shawn Harrison found that negative skewness didn't result in any offshore
ripple migration (contrary to Crawford and Shay). He mentioned that even though the skewness was negative, the ripples in their observation at 12 m depth were always onshore and he attributed this to the fact that the ripple migration depended on the thin boundary layer of incident waves. This means negative skewness may not be critical for deeper sites and only gets important as we move to a shallower site (like Houser et al.) showed. 

In my upcoming paper, I also found that negative skewness existed in the dataset as I briefly shown in the poster but Ruessink's 
approach didn't care for including negative skewness. I also found similar to Crawford and Hay that a bimodal wave spectrum with a 
diffused wave spectrum corresponded with negative skewness while a peaky wave spectrum was correlated with positively skewed waves. 

For the morphodynamic model, my thoughts are that we need to improve our model calibration, we may first want to incorporate a
way of having full wave spectrum information and if that gives bimodal shape before waves break, somehow we estimate negative skewness. We may need more field data to get negative skewness from wave spectrum or even if we don't have that data, we can atleast reduce onshore sediment transport for waves corresponding with bimodal wave spectrum. 

Then once the waves break, use the information from  the Infragravity wave model + mean flow to do the sediment transport and 
lower the dependence on vandera's method because vandera's method does not apply for breaking wave regime.

This raises interesting research questions for implementations of parameterized methods to estimate asymmetric wave shapes and deriving bedload from them. May be in near future, we don't need to depend on parameterizations and can use phase resolved methods to get asymmetric wave shapes directly and then skewness would be a natural outcome from the model. We also need more field data to correlate skewness direction and ripple migration direction. Another challenge in this work as far as observations are concerned is that when waves get stronger to transport sediment in any direction, we may not even have ripple migration and then we need ways to quantify suspended sediment to a certain height above bottom. I guess like sheet flow. 

Some thoughts of this application are here: 
Shoaling Wave Shape Estimates from Field Observations and Derived Bedload Sediment Rates in the JMSE journal


References:
1. Crawford & Hay: Linear transition ripple migration and wave orbital velocity skewness: Observations (dal.ca)
2. Houser et al.: Divergent response of an intertidal swash bar - Houser - 2006 - Earth Surface Processes and Landforms - Wiley Online Library
3. van der A, D.A., Ribberink, J.S., van der Werf, J.J., O'Donoghue, T., Buijsrogge, R.H., Kranenburg, W.M., (2013). Practical sand transport formula for non-breaking waves and currents. Coastal Engineering, 76, pp.26-42

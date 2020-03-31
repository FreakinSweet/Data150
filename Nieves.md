# Nieves Response

​	Random forest was used to discover the most significant geospatial covariates in predicting population density across the globe. The RF algorithm randomly creates decision trees at first, and then trains based of of which random predictions were more accurate based on recorded census data. Following down a decision tree based on one gridcell's geospatial data will yield the prediction for the density - say, if the slope value is greater than 1000 but the night time light value is less than 400, the density might be, say, 100. Each learning cycle, certain decision trees are "pruned," temporarily removed so that the learning algorithm can assess their significance.

​	Dasymetric population allocation distributes known census population data among gridcells based on the decision trees formed from running the random forest algorithm.

​	Environmental covariates were shown to be the most significant in predicting population density. Vegetation and urbanization-related covariates were the most significant, and water
## Detecting and Predicting Deforestation in the Amazon -  Random Forests versus Convolutional Neural Networks

Shane Evanson

April 20, 2020



##### Introduction

​	In the long term, climate change is a cause for concern, as intensifying storms, droughts, fires, and flooding come as the result of carbon emissions.  Second only to burning fossil fuels is deforestation in producing these emissions [5], and it is therefore imperative that we can predict when, where, and why deforestation will occur. An international hotbed for forest loss is located in the Amazon rain forest, where the expansion of agriculture has been the primary driver as international demand for beef and soybean products increases. Although deforestation has slowed in recent years, most significantly by 80% in Brazil from 2004-2017 due to their Real Time System for Detection of Deforestation (DETER) and successful government enforcement, it persists. Forest loss continues at a rate of roughly 0.2% per year [1], with a net loss of nearly 180,000 square kilometers of forestland from 2001-2010 [2].

​	If the regional governments surrounding the Amazon have real-time data available to them which tracks deforestation, it can be stopped before great harm is done. Higher resolution geospatial data is necessary (both spatially and temporally), requiring both more accurate and more efficient methods to parse all the remotely sensed data. In recent years, Random Forest algorithms have been particularly popular in identifying land use/land cover (LULC). However, as the spatial resolution has increased from 250 meter MODIS imagery, to 15-30m Landsat imagery, to as high-resolution as 3-5m imagery coming from "Dove" satellites, finding a more efficient LULC classification algorithm has been an issue. At the same time though, newer methods of machine learning have been introduced, which have proven to be both more efficient and more accurate [1, 5]. For this reason, a comparison between the old (Random Forest) and the new (Convolutional Neural Networks) is merited.



##### Inquiry Type

​	The question being asked here is fundamentally of an exploratory nature: "Where is deforestation happening in the Amazon, and how can we predict it, present and future?". It requires not only a knowledge of current deforestation trends and locales, but its drivers. If the drivers behind deforestation are known to the governments over the forest, they can perhaps stop forest loss from its root cause as opposed to simply enforcing "band-aid"-like forest policies. For example, it was found by many [2, 6, 7, 8] that the expansion of cropland and pastures are leading causes of deforestation in Latin America. With that knowledge, restrictions specifically on soybean and cattle farming can be placed rather than having broader, easily abusable or avoidable laws. Or, better yet, alternatives could be sought out to disincentivize the production of soybeans/cattle in the first place, such as pea farming (more efficient than soybeans), or laboratory-grown meat (though at this point, it is very experimental, and costly). It narrows the broader question of "How can we stop deforestation?" down to some substantially more quantitative questions, putting questions of policy and government regulation (for the most part) aside.  How can we predict deforestation itself (where and when), what drives it (why and who), and what might happen should it continue (potential effects)? Knowing the potential effects gives reason to stop it, knowing the drivers gives means to stopping it, and knowing the extent and rates of deforestation give an idea of scale.



##### Methodology Comparison - Aide's RF versus Bem's CNN [2, 5]

​	The first source, Aide *et al*, used a Random Forest model to predict both deforestation and reforestation. Aide collected training data from 250m-pixel MODIS satellite data, observed from 2001 until 2010, using the Virtual Interpretation of Earth Web-Interface Tool (VIEW-IT). The VIEW-IT was their means of both training their RF algorithms, and assessing accuracy and performance. It also provided a means of processing the original MODIS data, and managing it for use with their algorithm. They then used the RandomForest package from R Studio to develop their model of deforestation, and to analyze possible drivers of deforestation. Their model was ultimately able to predict biome change with an accuracy of on average 84 percent, up to 93 percent in the major biome (moist tropical forest) of Latin America.



​	The second, Bem *et al*, used a few types of Convolutional Neural Networks, comparing them with each other, as well as an RF model, for accuracy and performance. Bem gathered their data from 15-30m Landsat 8 satellite data, observed from 2017 to 2019 in three relatively small locations within the Brazilian Amazon (small due to technical constraints). They then processed their data in Python, using the Keras python library, a wrapper for the Tensorflow library. Those libraries allowed Bem to convert a raster layer with multiple geospatial layers into a multi-dimensional matrix for simplified reading by the CNNs. They established the "ground truth" from data provided by the Brazilian Institute of Space Research (INPE) to assess their CNNs accuracy. Their best CNN model ultimately overestimated deforestation around 2-4%, as illustrated in Figure 1.

![Figure1](C:\Users\sevan\Desktop\Figure1.png)

​	

​	Aide used the random forest machine learning algorithm (represented simply in Figure 2) in order to generate land change rasters, which later were systematically analyzed and merged to produce a map of deforestation and reforestation. They began by taking 16 different spectral bands from the MODIS satellite, and calculated statistical values (mean, standard deviation, minimum, maximum, etc.) for each pixel throughout the 2001-2010 period. Latin America was separated into several zones, and those zones were divided into their major biomes. 26 separate RFs were created for each zone/biome, and each RF was trained with the spectral data and the statistics that they had created earlier. 

​	In reference to Figure 2 again, the RF process used in Aide's paper broadly works as follows: During training, the algorithm observes the different raster layers within the dataset, on a per-pixel basis. The RF algorithm does this for the entire dataset, generating decision trees which predict the likelihood of a certain biome being present there. Each iteration, the algorithm "votes" for the most likely biome from an ensemble of different decision trees, coming up with a more accurate prediction than those individual trees could produce.

​	Their RF algorithm ultimately was a tool for efficiently interpreting MODIS's data, converting the raster data into the desired deforestation/reforestation statistics. Municipalities with a statistically significant change in woody vegetation were considered, and mapped, to represent their prediction of deforestation and reforestation from 2001-2010 across all Latin America. 

​	

​	Bem used a few variations of convolutional neural network (represented simply in Figure 3) to predict LULC change within the Amazon, as well as an RF model and multilayer perceptron model. I will discuss their best-performing and most accurate model, the ResUNet model. The ResUNet model is a deep learning algorithm, which is different from the RF model in its capacity for pattern recognition spatially, spectrally, and temporally. Insofar as Bem's use of their CNNs, the process was similar to Aide's, though Bem did not calculate statistics for insertion into the CNN. It was not necessary due to the superior pattern-recognition of the CNNs, though whether or not that is the reason why Bem's RF model performed worse than Aide's (Figure 1, note 2017-2018 difference from ground truth of the RF model), that is confounding.

​	Referencing Figure 3, convolutional neural networks broadly work by downsampling the satellite imagery into more compact, meaningful information. For example, a 200x200 pixel raster layer may be shrunk to 100x100 pixels, and that again shrunk to 50x50, preserving significant information while effectively shrinking the dataset to be worked with down to a size which can be interpreted and predicted upon much more efficiently. In an oversimplified example, "significant information" might be pixels above a certain brightness. The simplified data is then upscaled back to the original size, and in the case of the ResUNet, layers further through the chain can "look back" upon previous layers, using skip connections to "remember" certain characteristics of the image that may have been lost in the downsampling process. At the end of the chain, a final prediction is made for deforestation across the raster.

![Figure2](C:\Users\sevan\Desktop\Figure2.jpg)

![Figure3](C:\Users\sevan\Desktop\Figure3.png)

​	Ultimately, both methods proved to be reliable in predicting deforestation. The CNNs proved to be the most accurate and most efficient methods, achieving much lower rates of error than the random forest method (ref. Figure 1, Figure 4). Aide *et al* did not directly compare their own RFs with any CNN in performance, however they provided accuracy information, 84% on average, suggesting that even outside of Bem *et al*, the Random Forest method is generally less accurate than Convolutional Neural Networks, especially residual ones. 



![Figure4](C:\Users\sevan\Desktop\Figure4.png)



##### The Gap

​	Unfortunately most papers like these miss the main point - they don't establish the significance of climate change. It's necessary to have an idea of what may come as a result of deforestation, because it is the negative potential that looms around climate change which gives deforestation its importance in the first place. If deforestation didn't release large amounts of carbon dioxide into the atmosphere, and if that doesn't cause climate change, and if that doesn't involve desertification and other forms of land destruction, and if that isn't harmful to our current way of life, why would it matter? This research is successful at predicting deforestation in the present and future, and is successful at predicting the drivers behind it, but there is lacking research here for the potential effects of deforestation. What might happen should the forests continue to be deforested?









**[1]**  Loh, A., & Soo, K., (2017). *Amazing Amazon: Detecting Deforestation in our Largest Rainforest*. Retrieved from http://cs231n.stanford.edu/reports/2017/pdfs/919.pdf

#### RF Papers (All use MODIS):

**[2]** Aide, T., et al., (2013). *Deforestation and Reforestation of Latin America and the Caribbean (2001–2010)* Retrieved from https://www.researchgate.net/publication/264731696_Deforestation_and_Reforestation_of_Latin_America_and_the_Caribbean_2001-2010

**[3]** Clark, M., L., et al., (2011). *An Analysis of Decadal Land Change in Latin America and the Caribbean Mapped from 250-m MODIS Data*. Retrieved from https://pdfs.semanticscholar.org/54c0/72caffb41f7b1250e61ef5ab2eccc606c965.pdf

**[4]** Fagua, J., & Ramsey, R., (2019). *Geospatial modeling of land cover change in the  Chocó-Darien global ecoregion of South America; One of most biodiverse  and rainy areas in the world*. Retrieved from https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0211324

#### CNN Papers:

**[5]** Bem, P., et al., (2020). *Change Detection of Deforestation in the Brazilian Amazon Using Landsat Data and Convolutional Neural Networks*. Retrieved from https://www.mdpi.com/2072-4292/12/6/901/htm

**[6]** De Sy, V., et al., (2015). *Land use patterns and related carbon losses following deforestation in South America.* Retrieved from https://iopscience.iop.org/article/10.1088/1748-9326/10/12/124004/pdf

**[7]** Müller, R., et al., (2011). *Proximate causes of deforestation in the Bolivian lowlands: An analysis of spatial dynamics.* Retrieved from https://www.researchgate.net/profile/Daniel_Mueller7/publication/228073049_Proximate_causes_of_deforestation_in_the_Bolivian_lowlands_An_analysis_of_spatial_dynamics/links/09e41501104821e4a0000000/Proximate-causes-of-deforestation-in-the-Bolivian-lowlands-An-analysis-of-spatial-dynamics.pdf

**[8]** Sosa, C., et al., (2018). *Identifying Drivers and Spatial Pattern of Deforestation in the Rio Grande Basin, Colombia*. Retrieved from https://digitalcommons.lsu.edu/cgi/viewcontent.cgi?article=1117&context=jlag
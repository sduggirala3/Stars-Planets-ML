### Souji Duggirala, Diego Granizo, Katie Wah, Karthik Yadavalli

# Introduction/Background:
There are many stars in the universe. Just as the earth orbits the sun, planet also orbit other stars. We have seen stars other than the sun from the beginning of time, but only recently have we started detected planets in orbit around other stars, known as exoplanets. One of the major forefronts in astrophysics today is the hunt for exoplanets. We have increased our detection capabilities immensely in the last decade, mainly through the Kepler space telescope and now TESS (Transiting Exoplanet Survey Satellite). As of today, scientists predict whether a star has an exoplanet through various methods including transit photometry (observing how the luminosity of the star decreases during the period when an orbiting planet goes across the planet) and radial velocity (observing the doppler shift of the star’s light as a planet causes the star to slightly orbit). The following picture shows what transit photometry is:

As the planet that orbits the star goes in between the star and earth, the overall luminosity decreases,which gives information about the size of the planet and the orbital period of the planet.
The following visualizes the radial velocity method:

As the planet orbits its host, the star also makes a small orbit around the common center of mass. During the star’s orbit, as it moves towards the earth,the frequency of the light changes due to doppler shifting By observing these variations over time, one can infer the existence of a planet in orbit around the star.


## Problem We’re Attempting to Solve
Despite the leaps and bounds that have allowed us to detect more than 3500 exoplanets there are limits to our current practice. One of these limits is noisy data. Noise is something that gets in the way of our perception of these star, such as dust clouds and asteroids that fall in the line of sight from our detection devices to the star. So far, much of the previous work has been in using machine learning models to reduce the noise in raw data and other such analysis on exoplanet transit data in order to increase the reliability of detection. A deeper question, however, is “how likely is a star to host a planet, given its spectral characteristics?” Not much work has been done in answering this question. Using a database that lists over 6000 stars’ properties, our project will attempt to find relationships between the composition of a star and how likely the star is to host a planet. In essence, this project will attempt to answer the question: “What types of stars are most likely to host a planet?” 

## Data We’re Using

The Hypatia Catalog is a publicly available dataset, which is a compilation of about 6000 known nearby stars (including both exo-hosts and non exo-hosts). It includes data about spectral abundance, temperature, luminosity, mass, disk thickness, and number of exoplanets. Using the Hypatia Catalog, we aim to cluster stars by all of the aforementioned features to find which regions of “spectral space” are most likely to host exoplanets.

## Procedure and Method
In this data set, there is information on the stars’ composition and other important characteristics. Stars are classified by spectral type based on their atmospheric temperature and luminosity and are notated with the letters O, B, K, F, G, K, M where O stars are the biggest and hottest and M stars are the coldest and smallest. There are 7 different spectral types for stars, but it is known that generally only F, G, and K stars are host planets. And for the data set from Hypatia, the included stars were of spectral types F, G, K, M.

After finding this data set, we looked through the data set and formatted it accordingly. For example, we decided that data about the position and the velocity of the star is not useful information. These data were removed from the dataset before generating the model. Also, throughout the data set, there were some values that were missing. In order to work with that data point, we filled in the missing entries with the existing mean of that specific feature for the entire data set.
Initially, we ran the GMM model with 4 clusters, as we had 4 spectral classes in the dataset that hosted planets. We extracted the cluster weights (or the probability of a star being in each cluster) and the cluster assignments (or what cluster each star was put into by the model). Using this output, we calculated host probabilities for each cluster and took the maximum. In essence, we looked at the probability that a star in each cluster will have a planet and took the maximum of all of the clusters. We wanted to maximize the maximum of the host probabilities for all the clusters, so this was the optimization objective. By looking at the objective value for the number of clusters from 1 to 10, we found that the objective was maximized when the number of clusters is 14. In essence, with 4 clusters, the maximum of all the host probability was about 7% but with 14 clusters, the maximum was 20%, so we decided to cluster the stars into 14 clusters. 

The dataset we have is 26 dimensional, which is impossible to visualize in its current state, so we had to perform a dimensionality reduction to reduce the data to 2 or 3 dimensions in order to visualize it. We used Principal Component Analysis for dimensionality reduction. After executing the PCA, we labeled the ground truth in 2 and 3 dimensions and then looked at the clustering visualization in 3 dimensions (basically changing the color of each point to match its cluster assignment). 

Then, we trained a Random Forest classifier on 70% of our data, where our response variable is whether a star hosts a planet.



## Visualizations

Dimensionality reduced data into 3 dimensions showing ground truth

This visualization shows the original spectral class assignments for the stars in the data set.

Dimensionality reduced into 3 dimensions with 4 clusters created by GMM

This visually shows the breakdown of the data into 4 clusters, assigned to each cluster using GMM. These cluster assignments differed from the the original spectral class assignments, and are more accurate in terms of helping us find stars that have a high host planet probability.

Dimensionality reduced data into 3 dimensions only showing the stars that have host planets

This visualization shows the same stars in the previous visualization but only highlights the stars that actually have host planets. 
Random Forest classifier on the star data to display the most important feature when determining if a star has a host planet
The accuracy was 94.6% and the most important feature was the carbon content of the star, followed closely by the surface gravity of the star.


## Conclusion
From the GMM algorithm, we were able to get the distribution of the characteristics for each cluster. For example, we know that for a certain cluster of stars, the elemental composition follows a certain distribution. In the future, when looking for exoplanets, astronomers can look for stars that have an elemental composition that follows the distribution of the clusters with the highest host probabilities.
Then, with the cluster with the highest host probability, we were able to extract the means for the different characteristics for the cluster most likely to host a planet. The value of these means makes the “spectral region”, in which a star is most likely to host a planet. The following shows this region: When we checked the means of the stellar elemental composition for the cluster most likely to host a planet, we saw that they closely matched that of the Sun, which was an interesting find. 
To find the most appropriate number of clusters when analyzing these stars, we looked analyzed the probability of a cluster containing a host planet in the data for 1-20 clusters. When comparing the data using different amounts of clusters, we compared the clusters with the highest probability of having a host planet and created the visualization below to show that separating the data into 14 clusters is the most effective for this analysis (as seen with the graph below).

## Future Work

In the future, any additional analysis to be done would require more star data. This way, we can fit our model to a bigger dataset to see if it holds for a larger sample.
The application of our model is for astronomers who want to look for a planet in the future. They can look for stars that have similar characteristics to stars in the cluster with the highest probability of hosting a planet.


## References

Fakhouri, O. (n.d.). Exoplanet Orbit Database | Exoplanet Data Explorer. Retrieved February 20, 2019, from http://exoplanets.org/

The Planetary Society. (2019). Transit Photometry. Retrieved February 20, 2019, from http://www.planetary.org/explore/space-topics/exoplanets/transit-photometry.html

Vanderbilt University. (2019). Hypatia Catalog Database. Retrieved February 20, 2019, from https://www.hypatiacatalog.com/hypatia

(2002, March 25). Retrieved April 16, 2019, from Deeg, Hans-Jorg. “The Transit Method.” The Transit Method, 2019, www.iac.es/proyecto/tep/transitmet.html.

(2005, May). Retrieved April 16, 2019, from https://en.wikipedia.org/wiki/Doppler_spectroscopy
# Stars-Planets-ML

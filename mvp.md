# Minimum Viable Product
## Interpreting the impact of lead-actor demographics on film revenue

The goal of this project is to determine the impact of lead-actor demographics (primarily, gender and age) on worldwide, lifetime movie gross of some of the most successful movies of all time.

To do this, I have scraped the following data for 6000 of the highest grossing movies of all time: title, worldwide lifetime movie gross (target), studio, domestic opening weekend gross, budget, MPAA rating, runtime, genre, lead actor name, height, gender, and age at the time of the movie opening, and the Rotten Tomatoes audience and Tomatometer scores. After removing movies for which sufficient information is not available (i.e., if any of the opening weekend gross, budget, Rotten Tomatoes scores, or lead actor age is not known), we are left with a sample of >2000 movies.

To begin building a regression model, I carry out a k-fold cross-validation (k=5) with linear regression, where the target is the  worldwide lifetime movie gross, and the included features are: domestic opening weekend gross (and its square), budget, the Rotten Tomatoes audience and Tomatometer scores, and the lead actor gender and age. The figure below (left) shows the lifetime movie grosses predicted for the training data (which contains 80% of the dataset) versus the actual values observed. For an R<sup>2</sup> = 1.0, the predictions should be exactly equal to the observed values, such that the blue data points all lie along the red, dashed line. For the model depicted here, the cross-validated R<sup>2</sup> = 0.732694.

The next figure (center) shows the residuals, and the last figure (right) shows the q-q plot. The q-q plot indicates that the predicted values are heavy-tailed, meaning that, in it's current form, the model cannot correctly summarize the underlying relationship. If this is true for your data, among other recommendations from Duke and the Additional Resources listed at bottom, you can transform your data to make it more linear using log-transformations or other means, or you may have to utilize a non-linear model on the dataset..

<p float="center">
  <img src="figures/lr_basic.png" width="300" />
  <img src="figures/lr_basic_resid.png" width="300" />
  <img src="figures/lr_basic_qq.png" width="300" />
</p>


These results show that there is a significant variation in *both* temperature and MTA ridership over the entire city. Understanding separately the impact of heat and crowds on each station is an important step in reaching our ultimate goal of creating a heat-illness risk index (which accounts for risk due to both heat and crowding) for each station.

To create a heat-illness risk index, the datasets containing the Census block heat indices and the station crowd indices need to be merged based on the Census block within which each station is located. Using the known geometry for each Census block, and the latitude and longitude coordinates for each station, we will assign each station a heat index from the heat index map.

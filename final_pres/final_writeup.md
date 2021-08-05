# Project Write-Up
## Interpreting the impact of lead-actor gender and age on film revenue


### Abstract

The goal of this project was to determine the impact of lead-actor demographics (specifically, gender and age) on worldwide, lifetime movie gross of some of the most successful movies of all time. [Historically](https://womenintvfilm.sdsu.edu/research/), female protagonists&mdash;particularly, female protagonists portrayed by actors over the age of ~35&mdash;are significantly underrepresented in movies; here, I aim to determine what (if any) impact these differences in actor demographics (i.e., gender and age) have on the lifetime gross of a movie. To do so, I build a ridge regression model that uses as features the domestic opening weekend gross (and its square), budget, the Rotten Tomatoes audience and Tomatometer scores, the lead actor gender and age, the studio that produced the movie, and the genre of the movie to predict the worldwide, lifetime gross of a movie. Scored on the test data, the resulting model has an R<sup>2</sup> = 0.795 and a mean absolute error of $63.4 million. The significant features in the model (i.e., those coefficients with _p_-values < 0.01) are the domestic opening weekend gross (and its square), budget, the Rotten Tomatoes audience and Tomatometer scores, and whether the movie falls into one of the following genre categories: adventure, animation, or sci-fi. Of note, the lead actor gender and age do not have a significant impact on predicting lifetime movie gross.


### Design

This project utilizes data scraped, using `requests` and `BeautifulSoup`, from Box Office Mojo, IMDb, and Rotten Tomatoes for movies with the highest worldwide, lifetime grosses. I aim to determine the impact of lead-actor gender and age on worldwide, lifetime movie gross of the top 6000 (by gross) movies of all time. Whether or not lead actor gender and age are significant factors in predicting movie gross, ultimately, I hope these results will in some way influence the number of movies released that are led by a more gender and age-diverse range of actors.


### Data
The target and features (detailed below) were scraped for 6000 movies, though some features were not available for all movies, so a significant number of these 6000 movies had to be removed from the dataset.

#### Box Office Mojo
From [Box Office Mojo](https://www.boxofficemojo.com/chart/ww_top_lifetime_gross/?offset=0), I scraped a list of the 6000 movies with the highest worldwide, lifetime grosses. This website provides the rank and title of each movie, as well as the worldwide, domestic, and foreign lifetime grosses. The ratio of the domestic to foreign gross is used to determine whether a movie was produced domestically; if international gross was greater than ~6&times; the domestic gross, the movie was assigned to the international film category. Box Office Mojo also provides the domestic opening gross, budget, release date, MPAA rating, run time, studio, and genre for each movie, as well as the name for the lead-actor (i.e., top billed actor).

#### IMDb
Given the name for the lead-actor of each movie, I also scrape from the actor's IMDb webpage a basic bio, including their birth date, height, and gender. Their birth dates will be used (along with the release date of each movie) to calculate their age at the time of release.

#### Rotten Tomatoes
Given the title of each movie, I also scrape the audience score and Tomatometer score from [Rotten Tomatoes](https://www.rottentomatoes.com)

### Data Summary
For each movie (i.e., an individual sample of analysis), our dataset will therefore include:

**Target**: worldwide lifetime gross <br>
**Features**: domestic lifetime gross, international lifetime gross, studio, domestic opening gross, budget, release date, MPAA rating, run time, genre, lead name, lead birth date, lead height, lead gender, Rotten Tomatoes audience and Tomatometer scores


### Algorithms

#### Cleaning & EDA
At this stage, I engineer a few features&mdash;including the age of the lead actor at the time of the movie release, and whether a movie can be classified as an international movie&mdash;and drop from the dataset any movies that have NaN values for features that are important for the regression model (e.g., domestic opening gross, budget, Rotten Tomatoes scores); this leaves ~2000 movies in the dataset.

#### Baselining
To determine a baseline, I fit simple linear, polynomial, ridge, and LASSO regression models utilizing the domestic opening gross (and its square), budget, and Rotten Tomatoes scores as features. The polynomial model performs poorly relative to the other models; the linear, ridge, and LASSO models all tend to overfit the training data, so it is important to select a model with regularization to minimize overfitting. Between ridge and LASSO, I select the ridge model because we do not want to zero the feature coefficients for any of the features of interest (i.e., lead actor gender and age), even if those features have low significance. The model will be trained and cross-validated on 80% of the data (60%&ndash;20% split between the training and validation sets), and tested on the remaining 20% of the data.

#### Expanded model
To expand the model, I add lead actor gender and age as features, as well as dummy variables for the studio that produced the movie and the genre of the movie. The final model (fit to the combined training and validation data) has a hyperparameter &alpha; = 14.5, and an R<sup>2</sup> = 0.795 and MAE = $63.4 million.


#### Interpretation
The significant coefficients in the final model (_p_ < 0.01) are the domestic opening weekend gross (and its square), budget, the Rotten Tomatoes audience and Tomatometer scores, and whether the movie falls into one of the following genre categories: adventure, animation, or sci-fi. Though the lead actor gender has a negative coefficient (i.e., the lifetime movie gross decreases for female leads) in the final model, the coefficients for both lead actor gender and age are not significantly different from a coefficient of 0. That is, the gender and age of the lead actor has no significant impact on predicting the worldwide, lifetime gross of a movie.

The figure below shows the relative feature (coefficient) importance of the significant features (_p_ < 0.01) in the final model, as well as the lead actor gender and age.

<p align="center">
<img src="https://github.com/hmlewis-astro/imdb_movie_gross/blob/main/figures/ridge_final_coef_studio_genre_trim_significant_include_demographics.png" width="600" />
</p>

### Tools
- Requests and BeautifulSoup for web scraping
- Pandas and Numpy for data analysis and exploration
- Scikit-learn (including functions from `preprocessing`, `model_selection`, `linear_model`, and `metrics`) for creating dummy variables, building, training, and testing the model
- Matplotlib and Seaborn for plotting and visualizations

### Communication

In addition to the slides and visuals presented here, an expanded version of this write-up will be included as a blog post on my GitHub Pages [website](https://hmlewis-astro.github.io/).

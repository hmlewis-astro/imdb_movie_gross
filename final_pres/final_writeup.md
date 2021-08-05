# Project Write-Up
## Interpreting the impact of lead-actor gender and age on film revenue


### Abstract

The goal of this project was to determine the impact of lead-actor demographics (specifically, gender and age) on worldwide, lifetime movie gross of some of the most successful movies of all time. [Historically](https://womenintvfilm.sdsu.edu/research/), female protagonists&mdash;particularly, female protagonists portrayed by actors over the age of ~35&mdash;are significantly underrepresented in movies; here, I aim to determine what (if any) impact these differences in actor demographics (i.e., gender and age) have on the lifetime gross of a movie. To do so, I build a ridge regression model that uses as features the domestic opening weekend gross (and its square), budget, the Rotten Tomatoes audience and Tomatometer scores, the lead actor gender and age, the studio that produced the movie, and the genre of the movie to predict the worldwide, lifetime gross of a movie. Scored on the test data, the resulting model has an R<sup>2</sup> = 0.795 and a mean absolute error of $63.4 million. The significant features in the model (i.e., those coefficients with _p_-values < 0.01) are the domestic opening weekend gross (and its square), budget, the Rotten Tomatoes audience and Tomatometer scores, and whether the movie falls into one of the following genre categories: adventure, animation, or sci-fi. Of note, the lead actor gender and age do not have a significant impact on predicting lifetime movie gross.


### Design

This project utilizes data scraped, using `requests` and `BeautifulSoup`, from Box Office Mojo, IMDb, and Rotten Tomatoes for movies with the highest worldwide, lifetime grosses. I aim to determine the impact of lead-actor gender and age on worldwide, lifetime movie gross of the top 6000 (by gross) movies of all time. Whether or not lead actor gender and age are significant factors in predicting movie gross, ultimately, I hope these results will in some way influence the number of movies released that are led by a more gender and age-diverse range of actors.


### Data
Again, the data required to address this question was scraped from Box Office Mojo, IMDb, and Rotten Tomatoes. The target and features, detailed below, were scraped for 6000 movies, though some features were not available for all movies, so a significant number of these 6000 movies had to be removed from the dataset.

#### Box Office Mojo
From [Box Office Mojo](https://www.boxofficemojo.com/chart/ww_top_lifetime_gross/?offset=0), I scraped a list of the 6000 movies with the highest worldwide, lifetime grosses. This website provides the rank and title of each movie, as well as the worldwide, domestic, and foreign lifetime grosses. The ratio of the domestic to foreign gross is used to determine whether a movie was produced domestically; if international gross was greater than ~6&times; the domestic gross, the movie was assigned to the international film category. Box Office Mojo also provides the domestic opening gross, budget, release date, MPAA rating, run time, and genre for each movie, as well as the name for the lead-actor (i.e., top billed actor).

#### IMDb
Given the name for the lead-actor of each movie, I also scrape from the actor's IMDb webpage a basic bio, including their birth date, height, and gender. Their birth dates will be used (along with the release date of each movie) to calculate their age at the time of release.

#### Rotten Tomatoes
Given the title of each movie, I also scrape the audience score and Tomatometer score from [Rotten Tomatoes](https://www.rottentomatoes.com)

#### Data Summary: Target and Features
For each movie (i.e., an individual sample of analysis), our dataset will therefore include: title, world and domestic/foreign gross, domestic opening gross, budget, MPAA rating, run time, genre, lead-actor age (at the time of release), height, and gender. Here, the total world gross will act as the target, and all other observations (excepting the movie title) will act as the features in the model.

| Target:      | worldwide lifetime gross |
| :---        |     ---: |
| Features:      | budget       |
|       | genre       |


### Algorithms

#### Cleaning & EDA
Given the NASA Landsat 8 image, the New York City Census block shapefile is used as reference to extract spectral radiances, and derive the median temperature within each Census block; outliers are replaced with the 1st and 99th percentile temperature values. From the observed temperature variations over the land area, a "heat index" in the range of 1 to 10 is calculated and assigned to each Census block, 10 being a land area with higher than median heat, 1 with lower than median heat.

Given the SQL database containing MTA turnstile data, I created a new table within that database with the available geolocation information. Using SQLAlchemy in Python, these tables are joined (on the `booth`/`C/A` and `unit`) so that each turnstile now also has an associated latitude and longitude. From the database containing all turnstile data for the year 2018, only data collected during summer months (i.e., between 06/01/2018 and 08/31/2018) were selected for this analysis.

I then calculated the time passed (in seconds) and the change in the turnstile `entries` counts between each reading; again, readings occur roughly every four hours. Here, there are two peculiarities in the data: (1) some turnstiles are counting backwards and (2) turnstiles appear to reset, leading to apparent increases in `entries` on the order of 10<sup>5</sup>-10<sup>7</sup> riders over just a few hours. To deal with these, I (1) always take the absolute value of the number of entries between measurements and (2) set an upper limit of 3 entries per turnstile per second. The later of these allows for a dynamic upper-limit to be set for each observation, depending on the time between measurements, rather than setting a single upper-limit.

#### Aggregation
The cleaned MTA data are then aggregated by station and linename, such that the net entries over the observed three month period can be derived. From the net entries, a "crowd index" in the range of 1 to 10 is calculated for each station, 10 being the most crowded, 1 being the least.

The MTA and heat data are then joined together based on the spatial location of each station.

By combining the derived "heat index" and "crowd index" for each station, I calculate a "risk index" (again, scaled from 1 to 10, with 10 being high risk) for heat-illness at each station.

#### Visualization
Maps of the stations colored by the various indices presented here are created.

Example: A map of New York City's subway stations, where each station location is colored by its "risk index", which considers heat-illness risk due to both high heat and large crowds. Redder colors indicate higher-risk stations.

<p align="center">
<img src="https://github.com/hmlewis-astro/mta_analysis/blob/main/heat_data/data/output/analysis_out/final/plots/new-york-station-risk-index.png" width="600" />
</p>


### Tools
- SQLAlchemy for querying SQL database in Python
- Mapshapper (employed by pre-packaged analysis algorithms by the USGS) for creating and altering geographic databases
- Pandas, GeoPandas, and Numpy for data analysis
- GeoPandas for handling and plotting geographic data
- Matplotlib and Tableau for plotting and interactive visualizations

### Communication

In addition to the slides and visuals presented here, the Tableau dashboard [NYC MTA Heat](https://public.tableau.com/views/NYCMTAHeatAnalysis/Dashboard1?:language=en-US&publish=yes&:display_count=n&:origin=viz_share_link) will be included in a forthcoming blog post to be shared on my (work-in-progress) GitHub Pages [website](https://hmlewis-astro.github.io/).

<p align="center">
<img src="https://github.com/hmlewis-astro/mta_analysis/blob/main/final_pres/NYC_MTA_heat_dashboard.png" width="512" />
</p>

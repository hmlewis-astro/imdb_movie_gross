# Project Proposal
## Interpreting the impact of lead-actor demographics on film revenue

Based on a [study of the top grossing US films of 2020](https://womenintvfilm.sdsu.edu/research/), The Center for the Study of Women in Television and Film found that there is a significant disparity in the _number of female versus male protagonists_ in movies released during 2020, and a significant difference in the _ages of those actors_. Here, we aim to determine what (if any) impact these differences in actor demographics (i.e., gender and age) have on the lifetime gross of a movie.


### Question:
In this project, we seek to **determine the impact of lead-actor demographics** (primarily, gender and age) **on worldwide, lifetime movie gross** of some of the most successful movies of all time. The impact of these specific features will be interpreted from a linear regression model built to predict the lifetime revenues (target) of the top 1000+ grossing movies of all time based on e.g., the budget, rating, run time, genre, and actor demographics (features) for each movie.


### Data description:
The data required to build this model will be scraped, using `requests` and `BeautifulSoup`, from Box Office Mojo and IMDb. We will first obtain the information detailed below for 1000 movies, which will be used to build the linear regression model. Given sufficient time, we will continue scraping and obtain this information for up to &sim;10000 movies.

#### Box Office Mojo
From [Box Office Mojo](https://www.boxofficemojo.com/chart/ww_top_lifetime_gross/?offset=0), we will scrape a list of the movies with the top worldwide, lifetime grosses; this website will provide the rank and title of each movie, as well as the worldwide, domestic, and foreign lifetime grosses, and the release year for each film.

By following the available link to the detailed summary of each ranked movies (e.g., [Avatar](https://www.boxofficemojo.com/title/tt0499549/?ref_=bo_cso_table_1)), we will also obtain the domestic opening gross, budget, release date, MPAA rating, run time, and genre. Further, by exploring the ["Cast and Crew" tab](https://www.boxofficemojo.com/title/tt0499549/credits/?ref_=bo_tt_tab#tabs) for each movie, we will scrape the name and an external link (to an IMDb webpage) for the lead-actor (i.e., top billed actor) for each film.

#### IMDb
Given the name and IMDb webpage link for the lead-actor of each film, we will scrape from the actor's IMDb webpage (e.g., for [Sam Worthington](https://www.imdb.com/name/nm0941777/), lead-actor in Avatar) a basic bio, including their birth date, height, and gender. Their birth dates will be used to calculate their age at the time of release of the film.

For each movie (i.e., an individual sample of analysis), our dataset will therefore include: title, world and domestic/foreign gross, domestic opening gross, budget, MPAA rating, run time, genre, lead-actor age (at the time of release), height, and gender. Here, the total world gross will act as the target, and all other observations (excepting the movie title) will act as the features in the model.

### Tools:
To scrape the data, the `requests` and `BeautifulSoup` packages in Python will be utilized. The scraped data will be stored in a comma-separated values file&mdash;this file will continue to be added to as more data is scraped&mdash;and will be read in to python and manipulated using the `pandas` package. The `pandas` package will also be used for initial exploratory data analysis.

The linear regression model in `scikit-learn` will then be used to interpret the importance of lead-actor gender and age in predicting worldwide, lifetime movie gross.

We will use the matplotlib package to create visualizations of the resulting model.

### MVP:

The minimal viable product (MVP) for this project will likely be a simple linear regression model including just a few features (e.g., budget and opening weekend gross), so that we can begin to understand what the most important features will be in predicting the lifetime gross of a movie. Given sufficient time, we may build separate models for male versus female lead-actors, to see how the model coefficients change between these groups.

# How To:
### Running the code in this repo

The code for this analysis is broken into multiple pieces: (1) scrape the data, (2) explore and clean the data, and (3) build the ridge regression model. To produce the results presented here, run the notebooks in the following order:

- `scrape_imdb.ipynb` &mdash; [this notebook](https://github.com/hmlewis-astro/imdb_movie_gross/blob/main/scrape_imdb.ipynb) scrapes information from Box Office Mojo, IMDb, and Rotten Tomatoes for movies with the highest worldwide, lifetime grosses, and writes the scraped data to a csv file (`top_gross_films.csv`); the parameter `max_movies` can be changed to adjust how many movies are scraped
- `eda_imdb.ipynb` &mdash; [this notebook](https://github.com/hmlewis-astro/imdb_movie_gross/blob/main/eda_imdb.ipynb) formats the data, derives some new features based on the scraped data, and drops observations (i.e., movies) without sufficient information for our analysis; the data are written to a new csv file (`top_gross_films_clean.csv`)
- `model_imdb.ipynb` &mdash; the final ridge regression model presented, as well as some other tested models, are built in [this notebook](https://github.com/hmlewis-astro/imdb_movie_gross/blob/main/model_imdb.ipynb) and [figures](https://github.com/hmlewis-astro/imdb_movie_gross/blob/main/figures) presenting the model and results are created

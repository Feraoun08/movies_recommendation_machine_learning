# Movie Recommendation System

A **content-based movie recommendation system** built with Python and
the TMDB 5000 Movies dataset. It recommends movies by comparing their
descriptions and metadata (genres, keywords, cast, and director).

## Dataset

The project uses two CSV files:

-   `tmdb_5000_movies.csv`
-   `tmdb_5000_credits.csv`

They are loaded with Pandas and merged using the `title` column.

## Steps Performed

1.  **Load and merge the data**\
    Read both CSV files and combine movie information with credits.

2.  **Select and clean columns**\
    Keep `movie_id`, `title`, `overview`, `genres`, `keywords`, `cast`,
    and `crew`. Remove rows with missing values and duplicates, then
    reset the index.

3.  **Extract useful information**\
    Use `ast.literal_eval()` to convert the metadata strings into Python
    lists. Extract names from genres and keywords, keep the first three
    cast members, and extract the director from the crew data.

4.  **Build movie tags**\
    Split each overview into words, remove spaces inside metadata names,
    and combine the overview, genres, keywords, cast, and director into
    one `tags` field for each movie.

5.  **Normalize the text**\
    Convert tags to lowercase and apply NLTK's `PorterStemmer` to reduce
    related word forms to a common stem.

6.  **Convert text into vectors**\
    Use Scikit-learn's `CountVectorizer` with a maximum of 5,000
    features and English stop-word removal. Each movie is represented as
    a numerical bag-of-words vector.

7.  **Calculate similarity**\
    Apply cosine similarity to compare every movie vector with every
    other movie vector. This produces a movie-to-movie similarity
    matrix.

8.  **Recommend movies**\
    Given a movie title, find its index, retrieve its similarity scores,
    sort movies from most to least similar, skip the selected movie, and
    display the next five results.

## Recommendation Architecture

``` text
TMDB Movies CSV + Credits CSV
              |
        Pandas: Merge
              |
      Select columns / Clean
              |
    Parse genres, keywords,
      cast, and director
              |
      Build combined tags
              |
     Lowercase + Stemming
              |
       CountVectorizer
       (movie vectors)
              |
      Cosine Similarity
              |
     Similarity Matrix
              |
       Input movie title
              |
   Find movie and its scores
              |
    Sort scores (descending)
              |
    Return the top 5 similar
             movies
```

## Technologies

-   **Python** --- main programming language
-   **Pandas** --- loading, merging, and cleaning data
-   **NLTK** --- text stemming with PorterStemmer
-   **Scikit-learn** --- CountVectorizer and cosine similarity

## Example

The notebook tests the recommendation function with:

``` python
recommend("The Dark Knight")
recommend("Inception")
recommend("Avatar")
```

## Recommendation Approach

This is a **content-based filtering** approach: recommendations are
based on similarities between movie content and metadata. It does not
use user ratings, viewing history, or collaborative filtering.

## Run the Project

1.  Place `movies_rec.ipynb`, `tmdb_5000_movies.csv`, and
    `tmdb_5000_credits.csv` in the same working directory.

2.  Install the required libraries:

    ``` bash
    pip install pandas nltk scikit-learn jupyter
    ```

3.  Open and run `movies_rec.ipynb` from top to bottom.

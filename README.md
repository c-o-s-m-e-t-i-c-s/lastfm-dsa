# **LastFM Data Science Project**

- Given a user's listening history, can we identify their music taste clusters and be able to recommend similar unheard artists based off of their genre clusters?
- This project will utilize the last.fm API as a data source in creating an artist recommendation system based off of scrobble count and user-based tags.
- In order to properly further streamline last.fm's user-based tagging system, I used MusicBrainz.org's list of valid genres and matched it with last.fm's user-based tagging system in order to classify an artist's genre.
  - There are a few limitations with this system, mainly that it is more dependent on users actually making use of tags to classify an artist's genre. An underground artist may have sparse tags making their data unusable.
  - Another limitation is that genre classification is inherently arbitrary, which can lead to inconsistent tagging of certain artists.


## **Data Pipeline:**
1. **Data Collection**
2. **Data Preparation**
3. **EDA**
    - Play Count Distribution
    - Tag / Genre Distribution
    - Tag Count Per Artist
    - Genre Correlation Heatmap
4. **Data Modeling**
    - Clustering Algorithms
    - K-Means 
    - DBScan
5. **Recommendation Algorithm**
    - Generate candidate pool
    - Fetch tags for candidates
    - Transform candidates using TF-IDF
    - Cosine Similarity
    - Recommend by Cluster / Artist
6. **Final Observations**

## **Final Observations:**
- The use of last.fm's API allows me to make personalized music recommendations based on artist's scrobble counts and overall genre clustering.
    - There was potential in using other open source API's such as Spotify or MusicBrainz, however I settled on using LastFM as it has a much more extensive source of listen counts and artist profiles over other API sources.
    - The main limitation of LastFM is that it requires you to make an account and a scrobbling source (i.e Spotify), and only then will LastFM track your listening data. Due to this, it'd require a fair bit amount of time to build up a large amount of listening data. 
- The sourced API data makes use of an optimal limit parameter in order to determine the 75th percentile threshold for actual meaningful listens, rather than ones who are accidental.
- The use of MusicBrainz.org's expansive genre list allowed me to properly define and streamline tags for all artists.
- EDA showed that my user's listening history would be heavily right skewed, showing that artists with a large number of listens are outliers and most artists play counts fall anywhere between $\small \leq$500 total scrobbles. Furthermore, most artists have around 4 tags, though there are still a few artists with 1 tag only. Lastly, genres you'd expect to be paired up with have high correlation scores between each other, while genres that are generally not paired with each other have low correlation scores.
- K-Means clustering on the TF-IDF feature matrix revealed genres being cleanly clustered with minimal overlap, showing how each artist and genre's similarities and differences being projected in a 2-dimensional plane.
- The recommendation algorithm makes use of two functions, ``recommend_new_artists`` (also named recommend by cluster) and ``recommend_by_artist``. It shows the recommended artist's name, its weighted score, and the cosine similarity score.
    - The recommendation algorithm scans the user's listening data, first scraping from the user's unique LastFM artists, and determining which artists have $\small \geq$5 total scrobbles, to which these artists would be excluded from being recommended. 
    - Afterwards, it generates a candidate pool using LastFM's ``artist.getSimilar`` from each artist in the original artists_df. 
    - It will then proceed to fetch tags for each candidate, and then prune candidates with null tags. 
    - Next, this candidate pool will be transformed using TF-IDF and then utilize cosine similarity to determine how similar a candidate is to an artist that it is most similar to, taking note of its closest cluster as well. 
    - It will then join ``play_counts`` to ``candidate_df``, and then scale ``max_similarity`` with ``play_counts`` to determine the total weighted score of an artist.

## **References:**
Anthropic. (2026). Claude. Claude.ai. https://claude.ai/new <br>
Last.fm. (2026). https://www.last.fm/ <br>
MusicBrainz. (2026). Genre list. MusicBrainz - the open music encyclopedia. https://musicbrainz.org/genres
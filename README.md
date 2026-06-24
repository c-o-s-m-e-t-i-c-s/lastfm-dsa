# <center>**LastFM Data Science Project**</center>

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
6. **Model Evaluation**

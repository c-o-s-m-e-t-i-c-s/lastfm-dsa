# **LastFM Music Recommendation Project**

- Last.fm is a widely used music tracking platform that aggregates listening data across players and devices into a persistent user profile. Each play event ("scrobble") is recorded with artist, track, and timestamp metadata, forming the dataset this project is built on.
- This project analyzes a given user's listening history and identifies their music taste clusters. It then recommends similar unheard artists based on their genre profile using the Last.fm API as its data source, with scrobble count and user-based tags as the primary signals.

## **Objectives**
1. **Determine Meaningful Listens vs. Accidental Listens.** Differentiate actual meaningful listens to one-time / accidental listens to only use useful data for the model.
2. **Establish Concrete Artist Genre Tags.** Validate Last.fm artist's user-based tagging system by pairing it with MusicBrainz.org's genre list.
3. **User Listening History Analysis.** Identify a user's music taste clusters based on their Last.fm scrobbles.
4. **Recommend Artists Using Genre Clusters.** Recommend music based on specific genres and artists with a user's listening data in mind.

## **Methodology & Data Pipeline**

### **1. Data Collection**
- The use of last.fm's API allows me to make personalized music recommendations based on artist's scrobble counts and overall genre clustering.
    - There was potential in using other open source API's such as Spotify or MusicBrainz, however I settled on using LastFM as it has a much more extensive source of listen counts and artist profiles over other API sources.
    - The main limitation of LastFM is that it requires you to make an account and a scrobbling source (i.e. Spotify or any other supported scrobbling source), and only then will LastFM track your listening data. Due to this, it'd require a fair amount of time to build up a large amount of listening data. 
- The sourced API data uses an optimal limit parameter that takes the 75th percentile of all listens in order to determine the threshold for actual meaningful listens, rather than accidental ones.

### **2. Data Preparation**
- In order to properly further streamline last.fm's user-based tagging system, I used MusicBrainz.org's list of valid genres and matched it with last.fm's user-based tagging system in order to classify an artist's genre.
  - There are a few limitations with this system, mainly that it is more dependent on users actually making use of tags to classify an artist's genre. An underground artist may have sparse tags making their data unusable.
  - Another limitation is that genre classification is inherently arbitrary, which can lead to inconsistent tagging of certain artists.

### **3. EDA**
- EDA showed that my user's listening history would be heavily right skewed, showing that artists with a large number of listens are outliers and most artists play counts fall anywhere between $\small \leq500$ total scrobbles. Furthermore, most artists have around 4 tags, though there are still a few artists with 1 tag only. Lastly, genres you'd expect to be paired up with have high correlation scores between each other, while genres that are generally not paired with each other have low correlation scores.

### **4. Data Modeling**
- K-Means clustering on the TF-IDF feature matrix revealed genre clusters having substantial overlap with weak boundaries (further proven with 0.134 silhouette score). This can be visualized by projecting the high-dimensional clustering results onto a 2-dimensional plane, showing each artist and genre's similarities and differences.
    - The clusters having substantial overlap as well as a low silhouette score is an expected byproduct of genre and user-based tags and boundaries being subjective rather than objective truths; one artist could be labeled indie rock by Last.fm while in fact it might be closer to emo in one's eyes. 
    - The K-Means model overall makes up for this, rather than being specific in categorizing artists into pigeonholed genres, it groups similar sounding artists into shared clusters of genres.  

### **5. Recommendation Algorithm**
- The recommendation algorithm makes use of two functions, ``recommend_new_artists`` (also named recommend by cluster) and ``recommend_by_artist``. It shows the recommended artist's name, its weighted score, and the cosine similarity score.
    - The recommendation algorithm scans the user's listening data, first scraping from the user's unique LastFM artists, and determining which artists have $\small \geq5$ total scrobbles, to which these artists would be excluded from being recommended. 
    - Afterwards, it generates a candidate pool using LastFM's ``artist.getSimilar`` from each artist in the original artists_df. 
    - It will then proceed to fetch tags for each candidate, and then prune candidates with null tags. 
    - Next, this candidate pool will be transformed using TF-IDF and then utilize cosine similarity to determine how similar a candidate is to an artist that it is most similar to, taking note of its closest cluster as well. 
    - It will then join ``play_counts`` to ``candidate_df``, and then scale ``max_similarity`` with ``play_counts`` to determine the total weighted score of an artist.

## **Setup & Usage**

### **Prerequisites**
- Python 3.12.0
- Last.fm account with listening history

### **Installation**

1. Clone the repository:
```bash
    git clone https://github.com/c-o-s-m-e-t-i-c-s/lastfm-dsa.git
    cd lastfm-dsa
```
2. Navigate to the project directory:
```bash
    cd lastfm-dsa
```
3. Download and install required dependencies from the repo:
```bash
    pip install -r requirements.txt
```
4. Create a `.env` file in the root directory and populate it with your Last.fm API credentials:
```bash
    LASTFM_API_KEY=your_api_key_here
    LASTFM_API_SECRET=your_api_secret_here
```

### **Running the notebook:**
```bash
    jupyter notebook "lastfm dsa.ipynb"
``` 
1. Run all cells top to bottom.
2. Optionally, set a different Last.fm username to run the pipeline on a different user's data:
```python
    USERNAME = "your_lastfm_username"
```


## **References:**
Anthropic. (2026). Claude. Claude.ai. https://claude.ai/new <br>
Last.fm. (2026). https://www.last.fm/ <br>
MusicBrainz. (2026). Genre list. MusicBrainz - the open music encyclopedia. https://musicbrainz.org/genres

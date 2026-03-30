Overview

I set out to investigate the emotions conveyed by songs and see if we could classify songs based on the moods they elicit using audio features pulled directly from Spotify. The idea behind this is simple - genre-based recommendations don't always capture how you're actually feeling at a given moment. A sad person doesn't want an upbeat pop song just because they like pop. Mood-based classification solves that.
A second goal was to investigate the moods of the Top 50 charting songs on Spotify in 2023, and find out why those songs topped the chart by identifying the prevalent moods at that point in time. For artists and producers, this is valuable intel: knowing the emotional fingerprint of chart-topping songs means you can deliberately engineer music that connects with where people are emotionally, regardless of genre.
_________________________________________
Data
The dataset was sourced using the Spotify API (Spotipy). Songs were pulled from random playlists as deliberately as possible to get a diverse and representative spread of moods across the training data. The Spotify Top 50 songs for 2023 were used as the test set for the final mood predictions.
Features used:
Feature          Description
danceability	   How danceable a song is
energy	         How energetic a song is
key	             The key of the song
loudness	       How loud the song is (dB)
mode	           Major or minor key
speechiness	     Presence of spoken words
acousticness	   Use of acoustic instruments
instrumentalness Presence (or absence) of vocals
liveness	       How live the song sounds
valence	         How positive the song sounds
tempo	           Tempo of the song
time_signature   Time signature
________________________________________
Methodology

PCA and Factor Analysis
I ran Factor Analysis to reduce the feature space and derive interpretable mood dimensions from the FA loadings before clustering. Four factors emerged:
. Factor 1 - Loud & Highly Energetic (somewhat positive)
. Factor 2 - Danceable & Positive/Motivational
. Factor 3 - Instrumental & Sad/Calm
.	Factor 4 - Live Performance in a Major Key / Happy Noise

Cluster Analysis
K-Means clustering was used to group songs into mood clusters. I cross-referenced the cluster means against the Factor Analysis loadings to validate the mood labels - essentially double-checking the clusters against what the FA already suggested. Four mood clusters emerged:
•	 Danceable / Happy / Motivational - High danceability, energy, loudness and valence
•	 Loud / Sing-Along / Danceable - High loudness and danceability, moderate energy and valence
•	 Calm / Chill - Highest acousticness, high mode, low energy and loudness
•	 Loud / Energetic / Noise - High loudness, energy and mode, low valence — think hard rock or sound-heavy tracks with little lyrical depth

Classification — KNN
After labeling the training data via clustering, I trained several classifiers to predict mood:
. K-Nearest Neighbors
. Support Vector Machine
. Random Forest
. Decision Tree
. AdaBoost
KNN (k=2, Euclidean distance) came out on top, achieving 91% accuracy with high precision, recall and F1 scores on the validation set. Grid search was used for hyperparameter tuning.
__________________________________________
**Results** - What Moods Topped the 2023 Spotify Chart?
Mood	# of Songs
Danceable / Happy / Motivational	26
Loud / Energetic / Noise	14
Loud / Sing-Along / Danceable	10
Calm / Chill	0
The dominant mood on the 2023 Spotify Top 50 was Danceable, Happy and Motivational - upbeat, high energy songs with positive vibes. Notably, zero chill/calm songs made the chart. Make of that what you will. 
_________________________________________
Tech Stack
. Python
. Spotipy (Spotify API)
. Scikit-learn (KNN, SVM, Random Forest, AdaBoost, Grid Search)
. Pandas, NumPy
.	Factor Analysis (via factor_analyzer)
. K-Means Clustering
. Jupyter Lab
________________________________________
Data Collection

The script used to pull the song data from the Spotify API is included in the repo (Spotify_API_calls(1).ipynb). You can use it to pull your own data by adding your own Spotify API credentials to a .env file in the following format:
SPOTIFY_CLIENT_ID=your_client_id
SPOTIFY_CLIENT_SECRET=your_client_secret

Future Directions
This project can serve as the foundation for a mood-based music recommender system - where a user inputs how they're feeling and gets song recommendations that actually match that mood. I'd also be curious to run this on data from different years (Top charts) to see how the emotional fingerprint of chart-topping music shifts over time.

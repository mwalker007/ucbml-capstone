### Can Spotify's artist and song metadata predict which songs will be hits?

**Author** Mike Walker

#### Executive summary
Given Spotify's collection of metadata about artists and their tracks, is it possible to predict which of the artists songs will be among their top 5 most popular?
The results were that a KNN (K-nearest neighbors) model could be helpful in predicting Top5 tracks since it did the best job overall of predicting the Top5 tracks while reducing False Positives and False Negatives (F1 score). Further experimentation with inclusion/exclusion of track metadata, constructing additional features like decade, type of music (rock,pop, rap, etc.) and additional class balancing could lead to improved model performance.

#### Non-technical summary
The goal of the project is to uses characteristics (metadata) of an artist's (bands, individual singers) songs to determine whether a song is likely to be among their Top 5 most popular. Spotify makes publicly available the metadata for each song and includes things like the energy, volume, tempo, and danceability of each track. Further description of these features (metadata) can be found here: https://developer.spotify.com/documentation/web-api/reference/#/operations/get-several-audio-features.

The project used the current members of the Rock and Roll Hall of fame as the list of artists to make comparisons on since they are well known and typically have a large number of songs. Initial analysis of the metadata identified trends such as more loudness and energy usually translating into more popularity. Also, aspects of the tracks like tempo and duration have a range in which most of the popular songs lie. There are other features of the songs like whether they are major or minor or in a certain key that do not seem to be big drivers of popularity.

With this collection of artist and track data, various binary (Top 5 or not Top 5) classification machine learning models were trained models with the objective of maximimizing the correct number of song that should be in the Top 5 while minimizing the incorrect number that are included in the Top 5. The predictions of these models would predict the songs with characterisics that make them most likely to appear in the Top 5 while accepting that under estimating Top 5 is better than over estimating. 

The model that performed best was the Nearest Neighbor model which groups songs with similar characterstics together so that the more similarity a track has to other Top 5 songs, there more likely it is to also be a Top 5 song. Based on the data observations energetic, loud, and danceable songs are more likely to be popular however the model helps determine how the other factors like length and tempo could otherwise make it more or less likely to be popular. Additional analysis on the impact of each characteristic would be useful in determing which were the key drivers in this prediction and which are essentially irrelevant.

Addition exploration for this project could include:
a) adding more data from a larger variety of artists beyond Rock Hall members
b) identifying addition song characteristics like when the song was originally created and examining the lyrics of the song ie. what is the sentiment or what words are most often in popular songs.
c) trying additional machine learning techniques like variations in the models types and length of time spent training 

#### Rationale
Bands have limited resources available to promote their new material so identifying which new songs are most likely to resonate with their fans allows them to focus
their resources in promoting those identified as the most likely to be popular. 

#### Research Question
Is the collection of metadata that Spotify gathers helpful in predicting which songs for an artist are going to be the most popular?

#### Data Sources
Spotify provides an API with metadata about artists and tracks including most popular tracks by artist, along with track details like energy, danceability, key, etc. 
The data is available here via public API: https://developer.spotify.com/documentation/web-api/reference/#/operations/get-several-audio-features and would require some scripting to collect sufficient samples of data. Which seems possible based on other Kaggle datasets that people have created i.e. https://www.kaggle.com/code/aeryan/spotify-music-analysis/data

#### Methodology
I expect to try various classification algorithms such as KNN, DecisionTrees,SVM, Logistic Regression, etc. to determine which class a track is likely to fall under: A) Top 5 or B) Not Top 5

### The expected results
While I do expect some metrics for the songs to be more indicative of others in predicting which songs are most popular for an artist, I'm not confident that the metadata Spotify identifies will be enough for a very high degree of accuracy. These resulting model by itself may not be an absolute predictor, yet still could be a useful signal for artists to consider. 
 
### Why this question is important
Bands have limited resources available to promote their new material so identifying which new songs are most likely to resonate with their fans allows them to focus their resources in promoting those with the best chance of success such as more plays and gaining new fans.

#### Results Summary
The initial results were heavily skewed towards predicting none or only a few of the tracks would be among the top 5 regardless of artist.  After balancing the classes by removing 80% of the non-Top5 tracks, the Precision went from ~0.10 to ~0.40 for most models. The KNN model had the best Recall & F1 score along with relatively high precision. The LogisticRegression model had the best Precision at 0.45. Based on the results so far, I would use this model to answer the question of which tracks are most likely to be in the Top5 since it's highest F1 score balances Precision and Recall best. 

#### Potential next steps:
1) The model metrics are based on the artists overall, while the goal is predict track popularity per artist so it would be helpful to compare metrics on an artist by artist basis
- This could also include retraining the models with the artist names onehot encoded vs. ordinal encoded to provide a stronger separation of the data
2) Obtain more data
- this could include more artists along with capturing more features like when (era,decade) the artist was popular, type of music (rock,pop,rap), and natural lanaguage features of the tracks lyrics like common words, sentiment, etc.
3) Try additional classes balancing
- the above results are based on removed 80% of the non-Top5 scores
- prior to the balancing classes, the Precision scores were around 0.10 so balancing greatly improved the results across all model types
4) Test eliminating highly correlated columns like energy and loudness
5) The HP tuned SVM model training ran very long which prevented me from trying additional hyperparameter options, so explore running in an environment with GPU available  

#### Exploratory Data Analysis Summary

##### Overall Feature Correlation:
After collecting the top 50 tracks for each of the Rock and Roll Hall of Fame members and comparing track metadata:
* Loudness and Energy are the most highly correlated: 0.74
* Energy and Acousticneess are highly correlated: -0.63
* Danceability and Valence are moderately correlated:0.49
* Acousticness and Loudness are moderately correlated: -0.42
* Energy and Valence are moderately correlated:0.38

##### Comparing Track Features to Popularity
* danceability > 0.2 popularity varies across the range of possibilities
* popularity varies across the range of enegry values however the most popular songs tend to have energy ratings > 0.1
* the range of popularity occurs across across all musical keys
* the range of popularity occurs across across all loudness values > -25. For Values < -25 tend to be in the middle popularity scores and not top values
* the range of popularity occurs across across both major and minor key songs
* the range of popularity occurs across across all speechiness values < 0.4. From dataset description: Values below 0.33 most likely represent music and other non-speech-like tracks. 
* the range of popularity occurs across almost the full range however the higly acoustic rated songs are not among the most popular for these artists
* the range of popularity occurs across the range of instrumentalness. The Dataset description says that songs > 0.5 are instrumental however I found exceptions in the data such as the Track "Eddie Cochran  Have I Told You Lately That I Love You" which was full of lyrics...
* the full range of popularity occurs for liveness values <= 0.8. Liveness values > 0.8 tend to not be among the most popular
* the range of popularity occurs across the range of valence values except for the extreme values <0.0.5 and > .99
* most popular songs have tempos between 60 and 205 BPM 
* Analysis: most popular songs have duration between 80 and 480 secs (80k and 480K ms). Tracks < 80 seconds are typically not full songs.
* the most popular songs are in 4/4 time.According to the dataset description, the values are supposed to range from 3-7 so 0 and 1 have unclear meaning. 

#### Summary of Basic (default settings) Model traing
|               Model 	| Train Time (secs) 	| Train Accuracy 	| Test Accuracy 	| Precision 	|   Recall 	| F1 Score 	|
|--------------------:	|------------------:	|---------------:	|--------------:	|----------:	|---------:	|---------:	|
|      Baseline Dummy 	|          0.003166 	|       0.582317 	|      0.581754 	|         0 	|        0 	|        0 	|
| Logistic Regression 	|          0.153274 	|       0.602134 	|      0.597156 	|  0.546763 	| 0.215297 	| 0.308943 	|
|                 KNN 	|          0.005496 	|       0.694614 	|      0.492891 	|   0.37785 	| 0.328612 	| 0.351515 	|
|       Decision Tree 	|          0.036863 	|              1 	|      0.537915 	|  0.447592 	| 0.447592 	| 0.447592 	|
|                 SVM 	|          0.170101 	|       0.582317 	|      0.581754 	|         0 	|        0 	|        0 	|

* Results: The best performing basic model for this use case was the Decision Tree which had the which had relatively high Precision, Recall, and F1 score.

#### Summary of Advanced (Hypertuned) Model traing
|               Model 	| Mean Fit time (secs) 	| Accuracy 	| Precision 	|   Recall 	| F1 Score 	|                                    Best Model Parameters 	|
|--------------------:	|---------------------:	|---------:	|----------:	|---------:	|---------:	|---------------------------------------------------------:	|
| Logistic Regression 	| 0.0518               	| 0.577014 	| 0.46875   	| 0.084986 	| 0.143885 	| LogisticRegression(C=0.1, max_iter=10000)                	|
| KNN                 	| 0.002053             	| 0.489336 	| 0.385294  	| 0.371105 	| 0.378066 	| KNeighborsClassifier(metric='manhattan', n_neighbors=3)  	|
| Decision Tree       	| 0.018739             	| 0.578199 	| 0.363636  	| 0.011331 	| 0.021978 	| DecisionTreeClassifier(criterion='entropy', max_depth=3) 	|
| SVM                 	| 18.532704            	| 0.581754 	| 0         	| 0        	| 0        	| SVC(gamma=0.1, kernel='linear')                          	|

* Results:The KNN model had the best Recall and F1 score - Based on the results so far, I would use this model to answer the question of which tracks are most likely to be in the Top5 since it's highest F1 score balances Precision and Recall best. 


#### Outline of project

- [Main Jupyter Notebook for project](module_20.cap_project.ipynb)
- [Rock Hall of Fame Members](rockhall.members.txt)
- [Generated Dataset for Rock Hall of Fame Members Top 50 Tracks](out.all.top50.csv)




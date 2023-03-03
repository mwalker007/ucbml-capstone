### Can Spotify's metadata Predict the hit songs for a band?

**Author** Mike Walker

#### Executive summary
Given Spotify's collection of metadata about artists and their tracks, is it possible to predict which of the artists songs will be among their top 5 most popular?
A Decision Tree model could be helpful in predicting Top5 tracks since it did the best job overall of predicting the Top5 tracks while reducing False Positives and False Negatives. Further experimentation with inclusion/exclusion of track meta data and class balancing could lead to improved model performance.

#### Rationale
Bands have limited resources available to promote their new material so identifying which new songs are most likely to resonate with their fans allows them to focus
their resources in promoting those identified. 

#### Research Question
Is the collection of metadata that Spotify gathers helpful in predicting which songs for an artist are going to be the most popular.

#### Data Sources
Spotify provides an API with metadata about artists and tracks including most popular tracks by artist, and track details like energy, danceability, key, etc. 
The data is available here via public API: https://developer.spotify.com/documentation/web-api/reference/#/operations/get-several-audio-features and would require some scripting to collect sufficient samples of data. Which seems possible based on other Kaggle datasets that people have created i.e. https://www.kaggle.com/code/aeryan/spotify-music-analysis/data

#### Methodology
I expect to try various classification algorithms such as KNN, DecisionTrees,SVM, Logistic Regression, etc. to determine which class a track is likely to fall under: A) Top 5 or B) Not Top 5

### The expected results
While I do expect some metrics for the songs to be more indicative of others in predicting which songs are most popular for an artist, I'm not confident that the metadata Spotify identifies will be enough for a very high degree of accuracy. These resulting model by itself may not be an absolute predictor, yet still could be a useful signal for artists to consider. 
 
### Why this question is important
Bands have limited resources available to promote their new material so identifying which new songs are most likely to resonate with their fans allows them to focus their resources in promoting those with the best chance of success such as more plays and gaining new fans.

#### Results
The initial results were heavily skewed towards predicting none or only a few of the tracks would be among the top 5 regardless of artist.  After balancing the classes by removing 80% of the non-Top5 tracks, the Precision went from ~0.10 to ~0.40 for most models. The Decision Tree model had the best F1 score along with relatively high precision. The LogisticRegression model had the best Precision at 0.45 scores. Based on my analysis so far, I would use the Decision Tree model since it predicted the most correct Top5 in the Test dataset with a lessor number of False Positives as compared to the LogisticRegression model. 


#### Outline of project

- [Main Jupyter Notebook for project](module_20.cap_project.ipynb)
- [Rock Hall of Fame Members](rockhall.members.txt)
- [Generated Dataset for Rock Hall of Fame Members Top 50 Tracks](out.all.top50.csv)

##### Contact and Further Information







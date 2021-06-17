# Milestone-2-Spotify-Music-Clustering-with-Unsupervised-Learning

**3D Plots of clusters viewable here:** https://nbviewer.jupyter.org/github/Cody-Lange/Milestone-2-Spotify-Music-Clustering-with-Unsupervised-Learning/blob/main/Music_Clustering.ipynb

**Motivation**

Music is always evolving, and at the core of its evolution is the formation of new genres and the reimagination of existing genres. Genres are created and reformed due to the fluidity of their inherent characteristics. Qualities such as form, style, and subject matter are in flux and can overlap between genres and subgenres. The main motivation for our music genre classification project was to explore the differences between genres: how well-defined genre boundaries are according to the sonic characteristics of songs as well as whether an unsupervised learning model would generate classifications similar to an average music listener. We theorize that the borders between genres are becoming blurred and that the broad classifications are similar.

**Data Source**

We used Spotipy, a Python library designed to integrate with the Spotify Web API. We focused on Spotify-created genre specific playlists spanning across a number of genres including rock, metal, indie, EDM, hip hop, pop, rap, country, classical, Latin, and K-pop. We collected 6 playlists for each genre and used Spotipy to collect details and characteristics about each song including acousticness, danceability, energy, key, loudness, mode, speechiness, instrumentalness, liveness, valence, tempo, duration, and time signature. We also collected mp3s from each song’s 30 second preview. 
In addition to Spotify’s song characteristics, we leveraged the Librosa package which is used for audio and music analysis. Librosa offered us the ability to analyze waveforms of audio for each song and compute several characteristics of said audio. The main characteristics we chose to incorporate are Mel-frequency cepstral coefficients (MFCCs), commonly used in speech recognition, and spectral contrast which measures the dynamic range between peaks and valleys of the audio’s spectrograph. After removing records for which there were no audio previews available, we ended up with 3089 records to analyze, about 280 records per genre. 

**Unsupervised Learning Methods**

After the data was mined, standard scaling was applied to make the data more suitable for clustering. Afterwards, either principal component analysis or t-sne was applied to find three features that best capture the variance of the data and can generate 3d plots for visualization. PCA was chosen because it works well with larger datasets and preserves global distances between clusters.T-Sne was also chosen because it preserves local distances as opposed to global distances, which was initially thought to possibly make the clusters more distinguishable, as it is a very different approach to dimensional reduction. Models were then clustered via either K-Means clustering or agglomerative clustering, with the first pass attempting to gage whether or not unsupervised models could reconstruct existing genre clusters. K-means was chosen because we already know 11 clusters exist naturally, it is simple, and  is guaranteed to converge. Agglomerative clustering was chosen because it was unknown whether or not music data inherently follows a “treelike structure'', it still clusters a predetermined amount of clusters, and captures groups of varying shapes and sizes better.
After initial visualizations for clusters of 11 were made, the elbow plot in figure 2 was used to determine a mathematically more “appropriate” cluster size. Elbow plots iterate through K-Means models of different cluster sizes and sums up the squared distances from each point to the centroid of its cluster. (“inertia) It is commonly observed that lower inertia values give clusters that are more distinguishable, and the cluster sizes with acceptable inertia values fall within the range of 3 to 6 clusters. The next set of 3D clustering visualizations used a cluster size of 6 in order to be closer to the ground truth of 11 genres.

![image](https://user-images.githubusercontent.com/50972659/122320655-914abf80-cef0-11eb-96bd-e4619cc952a6.png)

Figure 1.  An elbow plot used assist in determining “appropriate” cluster size
 
![image](https://user-images.githubusercontent.com/50972659/122320901-00c0af00-cef1-11eb-8039-d46ef7685393.png)

Figure 2. K-means Clustering w/ t-SNE - 6 clusters^^

![image](https://user-images.githubusercontent.com/50972659/122320927-0b7b4400-cef1-11eb-9427-0854b98ed880.png)

Figure 3. K-means Clustering w/ t-SNE - 11 clusters^^

![image](https://user-images.githubusercontent.com/50972659/122320955-1930c980-cef1-11eb-852f-74cdc66e2ae6.png)

Figure 4. K-means Clustering w/ PCA - 6 clusters^^

![image](https://user-images.githubusercontent.com/50972659/122320994-25b52200-cef1-11eb-9036-09005f075176.png)

Figure 5. K-Means Clustering w/ PCA - 11 Clusters^^

![image](https://user-images.githubusercontent.com/50972659/122320832-e25ab380-cef0-11eb-8939-bf7cc505bbfe.png)

Figure 6. Agglomerative Clustering w/ t-SNE - 11 Clusters^^

![image](https://user-images.githubusercontent.com/50972659/122320871-f43c5680-cef0-11eb-8522-ed919d2c67ad.png)

Figure 7. Agglomerative Clustering w/ PCA - 11 Clusters^^


**Evaluation**

The 3D clustering visualizations are all listed in an appendix at the end for space reasons.

![image](https://user-images.githubusercontent.com/50972659/122321363-bf7ccf00-cef1-11eb-9932-782636359106.png)

Table 1. Davies Bouldin and Calinski-Harabasz scores for all clusters

We were interested to see what number of clusters would produce the most distinguishable results and how this would compare to the 11 genres we began with. We found that the lines between genres are heavily blurred, especially when talking about ‘modern’ music - pop, rap, and EDM for example. We found that 6 clusters produced the most distinguishable results when differentiating between songs according to the feature representations we chose to use. Built-in audio characteristics such as key, loudness, and energy for instance may well be similar across different genres, especially considering that popular modern music across the top genres carry very similar qualities according to what the public responds positively to. High tempos, danceability, and valence (emotional positivity) are rife among popular music, and these songs are usually the ones inputted into spotify playlists. The PCA biplot in Figure 3. Shows that the first principal components mostly depended on acousticness, loudness, energy, and danceability, although acousticness was inversely related with loudness and energy. This can be attributed to the classical class having large acoustic values. The internal computation of these metrics were loosely defined on their development site, but according to their metrics, genres blur to a very high degree according to these characteristics. 

![image](https://user-images.githubusercontent.com/50972659/122319125-2dbf9280-ceee-11eb-8b6d-0190ba4d56f7.png)

Figure 8. PCA biplot of the Spotify data

It is interesting, however, to observe the differences in scores across different clustering methods as applied to our dataset of 3098 songs. We used the Davies Bouldin and Calinski Harabasz scores as our main evaluation metrics. The Davies-Bouldin score dictates the average similarity between clusters, essentially measuring how much overlap exists between separate clusters. Clusters which are more 
distinct and farther apart from each other will receive a better score, with lower scores indicating better clusters. Overall there was not a lot of variance in the Davies-Bouldin scores, with the K-Means-PCA 11 cluster group having the lowest score, and therefore the most distinguishability among clusters. This came out to be similar to the t-SNE K-means 11 cluster group. The worst score and therefore the most indistinguishable clusters by similarity and distance were in the agglomerative-PCA 11 cluster group and the K-means-PCA 6 cluster group. Interestingly, the K-means 6 cluster group prepared with t-SNE scored much better on the Davies-Bouldin index. These different groups can be referenced visually with the 3-D plots in the appendix section.

The second metric we used to evaluate our clusters was the Calinski-Harabasz index. The Calinski-Harabasz index, also known as Variance Ratio Criterion, is defined as the ratio between within-cluster dispersion and between-cluster dispersion. In this case, a higher score indicates better performance. The Calinski-Harabasz score rewards clusters that are both dense and well-separated, which is intuitive for clustering. In this case, we saw a very clear relationship between the dimensionality reduction technique and the Calinski-Harabasz score. When PCA was used, we saw scores ranging from 111-332, with the K-means-PCA 6 cluster group scoring the best. However, when t-SNE was used we observed a significant increase in scores as compared to the PCA groups. In this case, the K-mean-PCA 6 cluster group was still the best performer at 1252, but the other two t-SNE groups also scored above 1000 on the Calinski-Harabasz score. This proved to us that t-SNE resulted in much denser clusters that were also more distinct. Once again, these different groups can be referenced visually with the 3-D plots in the appendix section.

**Discussion**
It was learned that there is a lot of crossover between the main genres of music, but the K-means t-SNE 6 cluster model best represents the differences between the genres. Although 11 official genres were included in the dataset, it was surprising to find that the songs included were best represented by six different clusters. If we had more time and resources, we could compile more songs into our dataset as well as incorporate more subgenres which would yield interesting results. It would also be interesting to incorporate more characteristics of the spectral waveform for each audio sample to see if those features play a larger role in the classification of songs. 

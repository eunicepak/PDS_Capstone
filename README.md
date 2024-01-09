# 2022 DS Capstone Project  
**Course**: Principles of Data Science (DS-UA 112)\
**Instructor**: Pascal Wallish, PhD 

### Dataset description:
This dataset features ratings data of 400 movies from 1097 research participants. It is organized as follows:
* Row 1: Headers (Movie titles/questions)
* Row 2-1098: Responses from individual participants
* Columns 1-400: Ratings for the 400 movies (0 to 4, and missing)
* Columns 401-420: Self-assessments on sensation seeking behaviors (1-5)
* Columns 421-464: Responses to personality questions (1-5)
* Columns 465-474: Self-reported movie experience ratings (1-5)
* Column 475: Gender identity (1 = female, 2 = male, 3 = self-described)
* Column 476: Only child (1 = yes, 0 = no, -1 = no response)
* Column 477: Social viewing preference – “movies are best enjoyed alone” (1 = y, 0 = n, -1 = nr)

### Questions:
1. What is the relationship between sensation seeking and movie experience?\
My result shows that there is **no relationship between sensation seeking behaviors and movie experience** such that neither of the two affects the other. First, I used PCA to reduce the dimensions of both sets of data. \
![image](https://github.com/eunicepak/PDS_Capstone/assets/97565903/ad55b863-0e9b-4c21-b964-e0719dc3c16a)
![image](https://github.com/eunicepak/PDS_Capstone/assets/97565903/2977886c-e565-4ef7-bf54-35fd56715d4c) \
I used the Kaiser criterion to determine the number of factors: there are six factors in sensation seeking data and two factors in movie experience.  Using the transformed data in terms of their factor values, I correlated the two data, respectively and found that **none of them are correlated to each other**.

2. Is there evidence of personality types based on the data of these research participants?\
My result shows that there are **roughly two personality types based on the data of these research participants**. First, I used PCA to reduce the dimension of personality data. Accordingly, I used the Elbow criterion and selected 6 factors that accounted for approximately 50% of the variance. Then, using the transformed data as the predictors, I used the silhouette method to find the optimal value of k. As shown in the graph on the left, the peak of the silhouette score is at 2. \
![image](https://github.com/eunicepak/PDS_Capstone/assets/97565903/f1832ecd-a13d-4e25-bbac-c87de580a9e7)
![image](https://github.com/eunicepak/PDS_Capstone/assets/97565903/4cf2d5af-f679-43cb-a978-1c369656c359) \
The graph on the right shows the 2D visualization of the plotted data. In overview, the two types can generally be seen as the extroverts and the introverts, the rather apathetic personality vs.aesthetically sensitive personality. Yet, there are many overlaps and outliers as seen in the graph, and thus a more complex clustering model can be used to more accurately delineate the locations of the cluster centers.

3. Are movies that are more popular rated higher than movies that are less popular? \
H0: There is no difference between the ratings of more popular movies and less popular movies as they came from the same population/distribution. \
H1: Movies that are more popular are rated higher than movies that are less popular as the two groups came from a different population/distribution. \
Using Mann-Whitney U test between less popular movies and more popular movies, I **reject the null hypothesis; the observed result is unlikely to be due to chance alone, with p < 0.5**.
In order to split the data into two groups— ‘less popular’ and ‘more popular’— count all NaN values for each movie and get the total number of ratings for each movie by subtracting NaN
counts from the length of the original data. Then, store this in a DataFrame alongside movie titles. Split this DataFrame:‘less popular’ for all movies with its total number of ratings below the median and ‘more popular’ otherwise. Extract and store index values for each group. Using these indices, get the movie ratings for the two groups, then extract NaN values, separately for each group, row-wise. The reason I did this separately is to keep the different counts of ratings for the two groups (Mann-Whitney U test works with different sample sizes).

4. Do people who are only children enjoy ‘The Lion King (1994)’ more than people with siblings? \
H0: There is no difference in the ratings of the movie ‘The Lion King (1994)’ between people with siblings and without siblings as the two groups came from the same underlying distribution. \
H1: People who are only children enjoy ‘The Lion King (1994)’ more than people with siblings as the two groups came from a different distribution. \
Using Mann-Whitney U test between the no-sibling ratings and with-sibling ratings for ‘The Lion King (1994)’, I **reject the null hypothesis; the observed result is unlikely to be due to chance alone, with p < 0.05**.  I first made a ‘combinedData’ with ratings of ‘The Lion King (1994)’ column and the ‘Are you an only child?’ column. Then, I extracted NaN values of this ‘combinedData’ row-wise. This is done to get rid of all the NaN ratings of ‘The Lion King (1994)’ regardless of the value for ‘only-child’ column. Finally, I splitted this ‘combinedData’ into only-child and with-sibling groups to do the Mann-Whitney U test.

5. Do people who like to watch movies socially enjoy ‘The Wolf of Wall Street (2013)’ more than those who prefer to watch them alone?
H0: There is no difference in the ratings of the movie ‘The Wolf of Wall Street (2013)’ between people who like to watch movies socially and people who prefer to watch movies alone, as the two groups came from the same underlying distribution. \
H1: People who like to watch movies socially enjoy ‘The Wolf of Wall Street (2013)’ more than those who prefer to watch them alone, as the two groups came from a different distribution.\
Using Mann-Whitney U test between the social-people ratings and alone-people ratings for ‘The Wolf of Wall Street (2013)’, I failed reject the null hypothesis, with
p > 0.05, such that the two groups share the same underlying distribution; **there is no difference in the ratings of the movie between social-people and alone-people**.

6. Build a prediction model to predict movie ratings (for all 400 movies) from personality factors only.
Using a support vector machine, the average accuracy score for all 400 movie ratings was approximately 45%. I first created a crossValidation function that splits the data into training and test sets using train_test_split in sklearn library. Using the dimension-reduced personality data, I implemented the model by a for-loop that loops through all 400 movies (columns), creates test and training sets for each movie (i th column), and uses preprocessing package in sklearn library to encode the label for y_test and y_training. This for loop also updates the counter variable, ‘accuracyScore_sum,’ by adding each accuracy score. After running the model, I calculated the average score around all 400 movie ratings by summing all the scores up and dividing it by the number of movies.

7. Build a prediction model of your choice to predict movie ratings (for all 400 movies) from gender identity, sibship status and social viewing preferences (columns 475-477) only.
Using a random forest model, the average accuracy score for all 400 movie ratings was approximately 57%. I first changed 'no response' values in sibship and social_view into NaN values, then combined all the predictors (gender_id, sibship, and social_view) to remove the NaN values, row-wise, in order to successfully get rid of all ratings that are incomplete. \
![image](https://github.com/eunicepak/PDS_Capstone/assets/97565903/b4b79140-c141-470f-8a9d-bfe5c30395a8) \
This histogram shows the counts for each category in all three predictors. Before I implemented the model, I first imported the crossValidation function I used in question 6. The
actual model was implemented by a for-loop, looping for 400 times (numMovies), creating test & training sets for each movie (i th column), and using preprocessing package in sklearn library to encode the label for y_test and y_training sets. Then I calculated the average accuracy score around all 400 movie ratings by summing all the scores up and dividing it by the number of movies.

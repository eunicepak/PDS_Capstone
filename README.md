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
My result shows that there is no relationship between sensation seeking behaviors and
movie experience such that neither of the two affects the other. First, I used PCA to
reduce the dimensions of both sets of data. \
![image](https://github.com/eunicepak/PDS_Capstone/assets/97565903/ad55b863-0e9b-4c21-b964-e0719dc3c16a)
![image](https://github.com/eunicepak/PDS_Capstone/assets/97565903/2977886c-e565-4ef7-bf54-35fd56715d4c)



3. df
**Question**: What is the relationship between sensation seeking and movie experience?\
My result shows that there is no relationship between sensation seeking behaviors and movie experience such that neither of the two affects the other. 
I used PCA to reduce the dimensions of both sets of data.
I used the Kaiser criterion to determine the number of factors: there are six factors in
sensation seeking data and two factors in movie experience.




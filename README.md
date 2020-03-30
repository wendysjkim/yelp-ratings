# Yelp-Help:
## Insights into the Manhattan Restaurant Landscape

### Introduction
According to [NYC Health](https://a816-health.nyc.gov/ABCEatsRestaurants/#/Search), there are approximately 27k restaurants in NYC, of which about 11k (40%) are located in Manhattan.
In other words, it would take a New Yorker almost 10 years to try every single restaurant in Manhattan, assuming he or she eats out 3 meals everyday and the list of restaurants stay the same. If we consider more realistic scenario where a myriad of restauarnts newly open or close every year, it would be virtually impossible to try out every single restaurants in Manhattan.

![](https://github.com/wendysjkim/yelp-ratings/blob/master/Images/NYRestaurants.png)

Therefore, restaurant review or reservation platforms like yelp, opentable and resy are great resources for both consumers and businesses. The ratings driven by user reviews could serve as not only a helpful guide to both locals and tourists, but also an effective marketing tool for the restaurants.

In this project, I compiled restaurant data in Manhattan using [Yelp Fusion API](https://www.yelp.com/developers/documentation/v3/get_started), and analyzed potential characteristics that contribute to higher ratings.

### Yelp API
The data was compiled using two types of business endpoints -- business search and business details. The available features include, but not limited to:

      - Ratings
      - Price level
      - Review count
      - Claimed
      - Categories (Italian, Coffee, Thai, etc.)
      - Transactions (delivery, pickup, reservation)

I additionally created some features based on the available information, such as:

      - Number of average hours open on a weekday (or a weekend)
      - Neighborhood (based on distance from the 'center' of a neighbrhood searched)
      - Offers messaging feature
      - Restaurant name length (in words and characters)
      - Mainstream vs. rare category (based on top 20 most common categories)
      - Number of breaks in between operating hours
 
### Insights from EDA

The majority of the ratings ranged between 3.5 and 4.5, which is consistent with the [entire rating distribution as of December 31, 2019](https://www.yelp-press.com/company/fast-facts/default.aspx).
![](https://github.com/wendysjkim/yelp-ratings/blob/master/Images/RatingsDistribution.png)

In terms of price level, the cheapest or the most expensive restaurants had higher proportion of 4.5 ratings, suggesting people consider value per dollar into their reviews. 
![](https://github.com/wendysjkim/yelp-ratings/blob/master/Images/RatingByPrice.png)

The rating distribution by neighborhood indicates restaurants in East Village would be facing more competition than other neighborhoods. Although both East Village and Stuyvesant Town have similar rating distribution, Stuyvesant Town has far less restaruants than East Village. This suggests that Stuyvesant Town is potentially an underserved neighborhood with more business opportunities. 
![](https://github.com/wendysjkim/yelp-ratings/blob/master/Images/Neighborhood.png)


### Modeling
The final model focuses on predicting ratings between 3.5 and 4.5 -- with more emphasis on correctly predicting 4.5 ratings. 
I first ran multiple vanilla models (logistic, decision tree, random forest, adaboost, gradient boosting, and XGBoost) to determine models to refine. 
I attempted to account for the class imbalance problem by adjusting class weights, oversampling and undersampling. My modeling process is summarized as follows.   

1. Low-performers vs. average-or-above-perfomers
  
    A. Using balanced class weight
  
    B. Using balanced class weight & less features selected from method (A) based on feature importances
  
    C. Method (B) but excluding 5-star ratings



2. Average-or-above performers (Excluding 5-star ratings)
  
    A. Using balanced class weight
  
    B. Using *balanced* class weight & less features selected from method (A) based on feature importances
  
    C. Using *different* class weights & less features selected from method (A) based on feature importances
    
    D. Using *SMOTE oversampling* & less features selected from method (A) based on feature importances
    
    E. Using *random undersampling* & less features selected from method (A) based on feature importances
    
    F. Refning models selected from method (D) using GridSearchCV
    
 
### Results
Vanilla gradient boosting model using SMOTE oversampled data had highest weighted F1 score and accuracy of almost 50% for both.
Interestingly, restaurants with long names tend to have higher ratings. 
![](https://github.com/wendysjkim/yelp-ratings/blob/master/Images/results.png)

As shown below, I used LIME package to further understand the results and how each feature affects the prediction.

#### Example 1.

In this example, we see the model expects the rating of a restaurant, which had 4.5 rating at the time of data compilation, to be 4.0 with fairly high probability.
However, this restaurant currently has a 4.0 rating.
![](https://github.com/wendysjkim/yelp-ratings/blob/master/Images/LimeEx1.png)

#### Example 2.

In this example, the model correctly predicted the restaurant's 4-star rating. However, the prediction probability of 3.5 and 4.0 are very close.
![](https://github.com/wendysjkim/yelp-ratings/blob/master/Images/LimeEx2.png)

### Conclusion & Next Steps

- Restaurants should target ratings of 4 or above.
- Ratings change over time, especially when there are very few reviews.
- More reviews are helpful -- restaurants could consider free offers or promotions to encourage users to post reviews.
- Restaurant names don't necessarily have to be short or easy to remember.
- List two categories on yelp (for exmaple, "Brunch, American").
- Consumers care about value per dollar.
- The neighborhood variable could be improved to be based on exact coordinates, rather than distance from the center of searched neighborhood. 
- More feature engineering that captures meaningful information from the entire sample (instead of specific categories that only applies to the minority)
could improve model performance.
- Consider setting minimum review count threshold.




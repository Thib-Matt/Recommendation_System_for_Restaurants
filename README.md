# Recommendation System for Restaurants in Zurich

## Project overview

In a world where the large commercial offer is overwhelming us, online reviews have a significant impact on customers' decision.

This project consisted in creating a tool that proposes a personalized list of restaurants based on users profiles.
The different steps:
- Scraped around 1800 restaurants and 1900 reviewers' profiles from TripAdvisor using Python and Selenium
- Engineered features from restaurants' description to extract usable information (type of cuisine, price range and popularity)
- Built profiles' clusters based on an unspervised learning algorithm (K-means)
- Productionized the solution: created a Python Flask web-app: [Click here to try it!](https://tmattera.pythonanywhere.com/)

## Project content

Content of the repositories:  
- [**ETL**](https://github.com/Thibault-Mattera/Recommendation_System_Restaurants/tree/main/ETL): Extract, Transform & Load pipeline - from the raw scraped data to a Restaurants SQL database.
- [**app**](https://github.com/Thibault-Mattera/Recommendation_System_Restaurants/tree/main/app): Flask web-app (recommendation system).  
- [**scripts**](https://github.com/Thibault-Mattera/Recommendation_System_Restaurants/tree/main/scripts): Python files to build the clustering model: exploratory data analysis, data preparation, data modelling.  

## Extracting, Transforming and Loading

I built a pipeline that automatically:
- extracts the data from TripAdvisor as a raw csv file
- transforms the data (clean the csv file and build reviewers profiles)
- load the data in a SQL database

### Extracting (Web-scraping)

As, the first step of the project, I scraped around 1800 restaurants from TripAdvisor.
For each restaurant, we extract the following information:
- URL
- Title
- Price range & category ($ symbol)
- Cuisines
- Special diets
- Location
- Number of reviews
- Reviews (reviewer name, quote, body, date, helpful vote)

Additionally, I scraped reviewers' profiles:
- reviewer's name
- reviewer's contribution (number of posted reviews)

### Transforming

This step consist in transforming the raw csv file from scraping into a clean and usable dataframe.    

### Loading

The transformed data is loaded in a SQL database containing restaurants details.  

<img src="/images/restaurants_database.png" width="90%">

## Exploratory Data Analysis

With a high number of categorical variables (over 150 types of cuisine and special diet), it was important to reduce them by studying their correlations.

Correlations between European cuisines            |  Correlations between Asian cuisines   
:-------------------------:|:-------------------------:
![](/images/correlation1_resized.PNG)  |  ![](/images/correlation2.PNG)

Also, I found interested to analyze the correlations between prices and cuisines.

![](/images/price_cuisine_corr_updated.png)

## Feature engineering

First, when analyzing the categorical variables (, I created new columns:
- cuisines regions (refering to a region)
- cuisine style (refering to a type of served food: 'Pizza', 'Street Food',...)
- specific criteria ('Accept credit cards', 'Outdoor Seating', 'Vegatarian Friendly'...)

The approach to reduce the number of categorical among the cuisine countries was to:
- remove the too "general" categories that hide the relevant information  (example: "European", "International")
- assign the categories containing very low number of restaurant to a global one (example: "Peruvian", "Chilean" to a global region "South American")


## Modeling: Unsupervised learning

After trying different clustering methods, I concluded that K-Means provided satisfying results.

We can see below 4 main profiles of restaurants' customers:
- Cluster A: avoiding Swiss local cuisine, prefering exotic food
- Cluster B: fans of Italian cuisine
- Cluster C: going everywhere
- Cluster D: loyal to Swiss restaurants

<img src="/images/clusters.png" width="90%">

## Productionization

This final step consisted in building a recommendation engine.
I created a flask API connected to an HTML web-interface. 
The API takes the user profile to propose him a personalized selection of restaurant based on his preferences.

The concept is simple and intuitive.
First, you define your "restaurant customer profile" (cuisine, restaurants' price range and popularity).
Your profile is compared to the database containing the Trip Advisor reviewers' profiles.
The system finds the profiles that are closest to yours and extract the corresponding restaurants.

<img src="/images/demo-recommendation-system.gif" width=90%>

### Try it!

Access the web-app directly [here](https://tmattera.pythonanywhere.com/)

**1. Clone repo locally**    
*git clone https://github.com/Thibault-Mattera/Recommendation_System_Restaurants*  
**2. Create virtual environment**  
Check if you have virtualenv:  
*virtualenv --version*  
If you don't, install it with:  
*pip install virtualenv*  
Create a virtual environment "my_env":  
*python -m venv my_env*  
On mac:   
*virtualenv my_venv*  
**3. Activate virtual environment (Python 3.8):**  
*.\my_env\Scripts\activate.ps1*   
On mac:   
*source my_venv/bin/activate*   
**4. Make sure you are using the upgrated pip version and install librairies**     
*python -m pip install --upgrade pip*  
*pip install -r requirements.txt*  
**5. Run flask webapp:**     
*cd app*  
*python app.py*  
**6. Go to your server localhost to access the web-app:**    
*http://127.0.0.1:5000/*



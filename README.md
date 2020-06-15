#### Contents:
- [Problem Statement](#Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [Data Dictionary](#Data-Dictionary)
- [Conclusions and Recommendations](#Conclusions-and-Recommendations)
- [Sources](#Sources)

Authored by:
- Molly Baird
- Patrick Kajibale
- Allan Merrick

#### Problem Statement
This tool will rely on websites that provide employment information and sector-specific wage estimations (such as Glassdoor and Indeed) to project the economic loss (wage loss) due to a disaster. Based on the type of businesses and services in a given affected area and/or using supplementary demographic data (for example, from the Census Bureau of Statistics), the tool will provide an estimation about the projected economic loss in a given locality based on the reported or estimated wage loss in the locality.

#### Executive Summary
**DUE TO THE SIZE OF OUR DATASET, IT IS HOSTED FOR PUBLIC ACCESS:**
- [DSI11 Disaster Dataset SEA](https://drive.google.com/file/d/1x_OnbMtpY_N8TFhXFK-TMyL8HfDOO2HS/view?usp=sharing)

**Modeling and EDA**
- [Discretized Model](./code/Discretized-Modeling/discretized-modeling.ipynb)
  - Categorized wage loss into ranges based on percentile
  - Fit decision tree to predict which wage loss tier a given occupation would be in given the state, disaster type, and time of year
  - Mainly modeled the food service industry
- [Principal Component Analysis Regressor and Random Forest Pt.1](./code/PCA-and-RF-Modeling/final1.ipynb)
  - 25 th  percentile of wage is expected to gain roughly by 42852$  after a disaster
  - 50 th percentile of  wage is expected to gain  roughly  by  99243$ after a disaster
  - 75 th percentile of wage  is expected to increase by 140 521$
- [Principal Component Analysis Regressor and Random Forest Pt.2](./code/PCA-and-RF-Modeling/final2.ipynb)
  - 25th  percentile of wage is expected loss of  of  9565$  after a disaster
  - 50th percentile of  wage is expected to gain  roughly  by  0$ after a disaster
  - 75th percentile of wage  is expected to increase by 52582$
- [Webscraper EDA](./code/Webscraper/Glassdoor-Scrape-EDA-FINAL.ipynb)
  - Anti-scraping methods identified and outlined
  - Final function annotated for adaptability and multi-use




#### Data Dictionary
|Feature|Type|Dataset|Description|
|---|---|---|---|
|**disasterNumber**|*integer*|main_df|ID number associated to each disaster event.|
|**state**|*object*|main_df|State where disaster occurred.|
|**incidentType**|*object*|main_df|Type of disaster (tornado, fire, hurricane, etc.).|
|**year**|*float*|main_df|Year disaster declared.|
|**month**|*float*|main_df|Month dsaster declared.|
|**occ_code**|*object*|main_df|Occupation code (first two digits indicate industry type).|
|**occ_title**|*object*|main_df|Occupation title.|
|**tot_emp**|*integer*|main_df|Total number of people employed in occupation in state at time of disaster.|
|**h_mean**|*integer*|main_df|Hourly average wage for occupation in state.|
|**a_mean**|*integer*|main_df|Annual average wage for occupation in state.|
|**employment_rate_during**|*integer*|main_df|State employment rate at time of disaster.|
|**employment_rate_before**|*integer*|main_df|State employment rate at one month before time of disaster.|
|**employment_rate_after**|*integer*|main_df|State employment rate two months after time of disaster.|
|**wage_change**|*integer*|main_df|Wage change across time one month before disaster and two months after. Total wages gained or lost from all people working in a particular occupation in each state at time of disaster.|
|**job_status()**|*function*|scraper| basic scraper function for checking current job postings, average salary, and location currently on GlassDoor.com.
|**City**|*object*| scraper output | Result from `jobs_status()` function.
|**State**|*object*| scraper output | Result from `jobs_status()` function.
|**Total Listings**|*object*| scraper output | Result from `jobs_status()` function.
|**Average Salary(usd)**|*object* | scraper output | Returns GlassDoor's average salary calculation for the selected area.
|**Posts (3, 7, 30 days)**|*integer*| scraper output | quantity of posts in the selected job title and area within each bin of days.

#### Conclusions and Recommendations
Decision trees provide a fast, efficient, and versatile model for predicting wage loss tiers of a given job for a particular state and disaster type. They are straightforward to interpret, and make few assumptions on the distribution of the data, making them good models for any industry type. However, they are very prone to overfitting as was found here. Random forests found improvements in variance, but little improvement in overall predictive ability. Our decision tree performed with 70% accuracy, 2.4 times better than the baseline model which assigns every data point belongs to the largest wage tier.

The principal component analysis regression has not been an ideal choice to predict the an economic loss disaster in the industry of construction and reparation. Random forest regressor performed quite well with 97%.

We can see huge marge error or  difference in loss prediction between the 2 models on the 50th percentile of income which  is expected to see a significant loss of 99243$  using component analyze regression while the random forest regression predicts 0$ loss on the 50 percentile of income in the construction industry.

Scraper functionally manages to access Glassdoor website, use search bar, return information typically nested in anti-scraping HTML (shadow agents). The function is annotated heavily for repurposing or iteration (depending on the information needed.) Currently the function is configured for current status job summaries based on the inputs of job name transformed location (location in put is optional.)


#### Sources
- [Glass Door Developer Page](https://www.glassdoor.com/developer/index.htm) : Dev page for API scrape
- [Glass Door first URL](https://www.glassdoor.com/Job/index.htm) : Initial URL
- [Glass Door final URL](https://www.glassdoor.com/Job/jobs.htm?suggestCount=0&suggestChosen=false&clickSource=searchBtn&typedKeyword=data+scientes&sc.keyword=data+scientest&locT=&locId=&jobType=) : Final URL that worked with our scraper
- [Glassdoor scrape example - Selenium (git)](https://github.com/arapfaik/scraping-glassdoor-selenium) : github link to author of Medium article
- [Glassdoor scrape example - Selenium (Medium)](https://medium.com/@jamievaron/to-anyone-who-has-lost-themselves-9c5e3049cb13) : Link to Medium article covering Glassdoor and Selenium
- [Selenium Review SEA-FLEX-11](https://git.generalassemb.ly/charles-rice/SEA-Flex-11/tree/master/08_week/selenium-webscraping) : Selenium flex review lab completed with our instructor.
- [Xpath guide](https://devhints.io/xpath) : reference guide for Xpath commands
- [Selenium Keys_Input Documentation](https://selenium-python.readthedocs.io/api.html) : reference guide for special keys
- [FEMA Disasters](https://www.fema.gov/openfema-dataset-disaster-declarations-summaries-v1) : Data about every official disaster in the US since 1953
- [Occupational Statistics from the Bureau of Labor Statistics ](https://www.bls.gov/oes/tables.htm) : Data regarding employment and wages by job type and state
- [Local Area Employment Statistics from the Bureau of Labor Statistics ](https://www.bls.gov/lau/rdscnp16.htm) : Data regarding employment rates for each state

# <p align="center">Capstone Project</p>
### <p align="center">The battle of neighborhoods</p>

## 1. Introduction
This document represents the final assignment for the final course of the [IBM Data Science Professional
Certificate](https://www.coursera.org/professional-certificates/ibm-data-science"). In this series of courses we learned to understand and use various tools and techniques to find, process and learn from structured data. This final assignment is meant to test if we are now able to use these abilities to tackle real-world data science problems.

The task to solve for the final assignment was stated as follows: 
>"Come up with an idea to leverage the Foursquare location data to explore or compare neighborhoods or cities of your choice or come up with a problem that you can use the the Foursquare location data to solve."

The sections of this document, with which I want to solve that task, are structured as follows:
1. This introduction.
2. Description of the business problem I will tackle and a discussion of the background.
3. A list of all the data sources I want to use and how it will enable me to solve the problem.
5. Methodology
6. Results.

**Note:**
If you are a peer reviewer and read this to review the assignment for week 4, please start from [here](#jump_point_week_5). If you want to review the assignment for week 5, please start from [here](#jump_point_week_6). If you are not a peer reviewer, simply go on.

<a name="jump_point_week_5"></a>
## 3. Business Problem
When regularly taking the train, one often has the problem of having some hours of free time at train stations with nothing to do. For these people it may be very interesting to find out what they can do instead of just waiting mindlessly for the train.  
In my suggested business problem I will try to leverage data to make suggestions on what one can do in the surrounding area, while still minding the limited time.

For this business problem I will make some assumptions: First, I am gonna assume that the stakeholder is in a foreign city without any knowledge on local venues. Secondly, I will assume that they got there by train, so they do not have a car or other type of private transport at their disposal and have to rely on local train and bus connections. Resulting from this is the need for relative closeness of avenues to train and bus stations, as to be easily reachable.

This business model can be summarized as following: I will try to build clusters of venues centered on reachable train stations, find similar clusters and return for each type of cluster the most promising candidates.

## 4. Data Sources
This is a list of all the data and how I want to use it to solve my problem. For the example in this document I have chosen to use data for the capital city of the German state of *Baden-Württemberg*, *Stuttgart*. Note that, because of this, some of the data sources may be written in German. I will add short bits of information where necessary for your convenience.

##### Foursquare Places API:
[Foursquare API](https://foursquare.com/) - This has been discussed previously in the course, therefore I will not go further into details here.

Important from this dataset will be data on **Geo location**, **venue names**, **venue types**, **popularity** and **addresses**.


##### VVS list of stops:
 [VVS list of stops](https://www.openvvs.de/dataset/haltestellen/resource/d87d1f01-5c14-4d08-8452-e405a6472ab4) - A list of all stops in the VVS (Verkehrsverbund Stuttgart; responsible for public transport in the region of Stuttgart) with additional data such as geographic position.  
Relevant data from this dataset are **Geo location data**, **transport type** and other data relevant to specifics of public transport.  
**License:** This dataset is licensed under the **Creative Commons Attribution License** ( [see here](http://opendefinition.org/licenses/cc-by/) ) and may be used under the condition that the creator is appropriately credited, which is hereby done.

<a name="jump_point_week_6"></a>
## 5. Methodology
> You can find the notebooks, relating to this project in this GitHub account. There are two notebooks, one [containing the preprocessing of the data](https://github.com/meisto/Coursera_Capstone/blob/main/IBM_Data_Science_Capstone_Project_Preprocessing.ipynb) and the other [containing the main components](https://github.com/meisto/Coursera_Capstone/blob/main/IBM_Data_Science_Capstone_Project.ipynb).
### 5.1. Preprocessing
To start of the preprocessing, I loaded the *VVS* dataset from the internet using pandas. Problem occurred for this as the data was not encoded in the standard UTF-8 encoding but instead in a encoding called iso-8859-1 which led to problems and weird strings when working with the dataframe. 
After rectifying this problem I also had to filter the field "Verkehrsmittel" (mode of transport). The original dataset contained not only the data for the "S-Bahn" (regional train) but also data for other modes of transport such as bus or taxi. These were encapsulated in a single field as values separated by ";". To remove irrelevant data I used regular expressions.  
After dropping irrelevant columns, mostly different identifiers and other data used for administration, I only had to translate the column names in English and I was done with the train station data and could write it to a CSV file (in UTF-8 encoding).  

After this I used the now cleaned name, latitude and longitude fields in the train station dataset to query foursquare for locations using the search API. I decided to use the search API instead of the explore API because I wanted to not only include exciting venues (such as bars, restaurants, etc.) but also more mundane venues (such as gas stations, supermarkets, etc.) which, in tests, where not often included in responses contained using the explore API. It should however
be easily possible to change this behaviour by altering the URL in the preprocessing notebook. I restricted the amount of venues to 10 per request and the maximum radius around the location to 500 meters, mostly due to resource constraints.  
After querying the API for answers I collected the data in a dataframe containing the venue name, address, latitude, longitude, categories as well as the name of the train station nearest to it. This was then written to a CSV file.  

### 5.2. Exploratory Data Analysis
#### Physical Spread
After loading the data from the files generated in the preprocessing step and rectifying wrongly detected data types I started by visualizing the data in a map.  

To get an interactive map you can pull this repository and execute it on your own machine.
![You should see a picture here](https://github.com/meisto/Coursera_Capstone/blob/main/images/spread_base_far.png)
Close up of the center:
![You should see a picture here](https://github.com/meisto/Coursera_Capstone/blob/main/images/spread_base_close.png)
This shows that most venues lie somewhat close to their train station, some being within 100 meters, some even further. There are however some outliers, especially in north-western and south-eastern directions. This could be because of data sparsity (no closer candidates where found) or simply a quirk of foursquare. I can not really explain this, because all the outliers lie in pretty big cities so there should better candidates available. In all likelihood this is either a fault in
the data or a quirk of the API.  

#### Categories
After analyzing the physical spread, I will now come to the categories. For this I aggregated all venues in groups according to their associated train station and converted this in categorical values which I used to train a clustering algorithm. Testing revealed that the number three is well suited as the number of clusters for KMeans-clustering. We will come back to this later.  
First I determined the overall distribution of categories. The results will be discussed later.
After calculating the distribution over the complete datasets, I calculated the same distribution over the different clusters. This produced some interesting characteristics.  
This concludes the section about exploratory data analysis.
### 5.3. Machine Learning Models
As previously mentioned the K-Means algorithm was used for clustering and the *elbow*-method to determine the number of clusters.  
 K-Means has several advantages such as simplicity, ease of use and speed while still being quite powerful. As any hierarchies between clusters were not relevant for this project (there are too few clusters for it to be useful) using hierarchical clustering was not necessary and K-Means is sufficient.  
That said, in a more complex dataset with more diverse venues and clusters it might be very interesting to observe the relationships and groupings of clusters and using hierarchical clustering might prove insightful.  

In an early approach, using linear regression with the clusters as target to determine the relative importance of different components was contemplated, but the relative sparsity of the training data and the existence of other methods, such as the ones that were actually used, made this unnecessary.
### 5.4. Using this data
The data and insights that were produced in the previous sections can be used to recommend stations to a user, both if he has specific wishes and if he just wants to browse. This can be done by using a list of user interests to match the most fitting cluster type and then returning a train station returning to that cluster or even a specific venue. Please see my notebook for an example on how one could go about doing this.

## 6. Results 
> Note: The results presented here are the same that can be seen in the notebooks as they stem from a previous run. That said the data should be similar, but variables that depend on stochstic processes, such as the naming of the clusters, might differ.
### Distribution of Categories
At first let's look at the distribution of the 25 most often seen categories in the overall dataset.
![You should see a picture here](https://github.com/meisto/Coursera_Capstone/blob/main/images/dist_overall.png)
We can see that the top venue category "Bakery" occurs more often than all other categories. The three following categories ("Hotel", "Gas Station" and "Office") occur in similar rates. All other categories after this occur in frequencies descrbed by a flat downwards slope.  
The group of categories most often seen in the top 25 could be described as providers of necessary services (such as bakeries and doctors offices). This group consists of at least six elements (Bakery, Gas Station, Supermarket, Doctor's Office and Banks). Among them being three of the top six categories.  
Another group of often seen categories could be named Work-related venues such as hotels, offices and factories.  
The third major group contains free time activities such as various restaurants, cafés, nightclubs and Athletics & Sports shops. The individual categories in this group rank typically lower than categories in other groups.  

Overall we can see that the venue categories most often seen near train stations are mostly consisting of necessary and work related categories while spare time categories are further away.
### Clusters
The data analysis process yielded three clusters for this data. Most of these clusters are caracterized by one major category, which occurs very often, while the remaining categories occur less frequently. The distribution is typically best described by a exponential distribution.  

**First cluster:**  
![You should see a picture here](https://github.com/meisto/Coursera_Capstone/blob/main/images/dist_0.png)  
The first cluster is characterized by the occurence of many bakeries. The difference between the top and the second categorie is significant as the the top categorie occures almost thrice as much as the second.  
We could call this the baker-cluster.  

**Second cluster:**  
![You should see a picture here](https://github.com/meisto/Coursera_Capstone/blob/main/images/dist_1.png)  
The second cluster is the most balanced one of the three. It is dominated by the office category and contains several other work-related categories such as factory and maybe parking.  
We could call this the work-cluster.  

**Third cluster:**  
![You should see a picture here](https://github.com/meisto/Coursera_Capstone/blob/main/images/dist_2.png)  
We could call this the travel-cluster.


## 7. Discussion
TODO: Bias because of nearness to train stations

While the results presented in the previous 
## 8. Conclusion
This concludes my report. Thank you for reading until here. I spent much time and thought in this project and hope you enjoyed reading it. 
I think this is one of the very many interesting use cases for data science and its methodology.  
Have a nice day!
<!---
As given in the review criteria, this report will consist of the following components:
1. An introduction to the problem and who could be interested in such a project.
2. Discussion of the data used for solving the problem.
3. A discussion of the methodology that was used to solve the problem.
4. Presentation and discussion of the results.
5. Discussion of observations and recommendation based on the results.
6. Conclusion.
-->

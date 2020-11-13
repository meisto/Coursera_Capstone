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

This business model can be summarized as following: I will try to build clusters of venues centered on reachable train stations, find similar clusters and return for each type of cluster the most prominsing candidates.

## 4. Data Sources
This is a list of all the data and how I want to use it to solve my problem. For the example in this document I have choosen to use data for the capital city of the German state of *Baden-Württemberg*, *Stuttgart*. Note that, because of this, some of the data sources may be written in German. I will add short bits of informations where necessary for your convenience.

##### Foursquare Places API:
[Foursquare API](https://foursquare.com/) - This has been discussed previously in the course, therefore I will not go further into details here.

Important from this dataset will be data on **geo location**, **venue names**, **venue types**, **popularity** and **adresses**.


##### VVS list of stops:
 [VVS list of stops](https://www.openvvs.de/dataset/haltestellen/resource/d87d1f01-5c14-4d08-8452-e405a6472ab4) - A list of all stops in the VVS (Verkehrsverbund Stuttgart; responsible for public transport in the region of Stuttgart) with additional data such as geographic position.  

Relevant data from this dataset are **geo location data**, **transport type** and other data relevant to specifics of public transport.

**License:**  
This dataset is licensed under the **Creative Commons Attribution License** ( [see here](http://opendefinition.org/licenses/cc-by/) ) and may be used under the condition that the creator is appropriately credited, which is hereby done.




## Note: The following text is part of next weeks assignment, so please do not pay it any mind.

<a name="jump_point_week_6"></a>
## Data Refinement and Analysis: 
-
## Report:
As given in the review criteria, this report will consist of the following components:
1. An introduction to the problem and who could be interested in such a project.
2. Discussion of the data used for solving the problem.
3. A discussion of the methodology that was used to solve the problem.
4. Presentation and discussion of the results.
5. Discussion of observations and recommendation based on the results.
6. Conclusion.

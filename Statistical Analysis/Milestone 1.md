---
title: "Analysis of Anime Popularity"
subtitle: "By MyAnimeList Fan-Service"
author: "Brown, Aaron"
date: "Spring 2023"
output:
  html_document: default
  pdf_document: default
---

### *Analyzing the relationship between an anime's popularity and its fan-service (favorites, scores, subscribers).* 

#### Using data from MyAnimeList, an anime's popularity can be measured several different ways, thus "the best" anime can vary based on variables used to make the assessment. Some of these variables can include:

#### 1) Most Viewed Anime
#### 2) Most Members and Scorers (basically subscribers)
#### 3) Most Favorited Anime
#### 4) Best Rated/Scored Anime

### About the Data.
#### The data used in this analysis was scrapped from an anime rating and listing site (MyAnimeList) that provides details of almost all available anime. Though the site does not allow users to actually watch any of the programs directly from the site, it provides a space for anime fans to compile all of their favorite anime into a list, as well as ranking, sharing, commenting, and a host of other features.
#### The datasets used provide details of over 18,000 separate anime productions, including series, movies, OVAs, ONAs, music, and others. Information such as number of episodes, duration, score, popularity, members, etc. can be extracted from the data. The three datasets contain data from three years: 2023, 2022, and 2021. 

### Relevance.
#### The debate of "The Best Anime" is one of endless squabble; this analysis will help to provide insight to the determination of The Best Anime by analyzing fan-driven data. Recommendations provided by websites and articles can be biased or sponsor influenced, analyses such as this help fans to determine what to watch next, or provide gateway anime to new or prospect fans. This analysis is also one of personal interest! 

### Key Points.
#### This analysis intends to answer key questions such as:
#### 1) What is the "Best" anime based on fan-service?
#### 2) What variables best estimate popularity?
#### 3) Is there a relationship between an anime's score and its popularity? 
#### 4) Is there a relationship between the number of scores and its popularity? 
#### 5) Do anime with the most members get the most scores?
#### 6) Do anime with the most members get the better scores?

### Analysis.
#### The approach is to utilize, primarily: scores, popularity, members, and favorites to determine the most popular anime and determine if there are any relationships between an anime's popularity and the amount of fans, as well as the different means to which fans interact with anime on MyAnimeList. As MyAnimeList allows only registered users to "score", and un-registered users can "favorite" there is expected to be a difference with the relationship between scores and favorites as they relate to popularity, possibly due to a more dedicated fan-service willing to go through the motions of creating an account.
#### GGPlot will and Patchwork will be used to produce visualizations of the various relationships mentioned before.
#### This analysis does not seek to determine a true "Best Anime"; this verbiage is used to refer to the most popular anime based on data and certain accompanying variables. The intention of this study is to outline any relationships between our variables of interest, and the popularity of anime.

### Data.
#### This dataset contains some of the information publicly displayed at https://myanimelist.net. Contains information about Anime itself as well as user ratings and user data. Dataset contains the list and metadata of all published anime (released/unreleased) currently about 18,000 on the MyAnimeList site.
#### MyAnimeList 2023:
##### https://www.kaggle.com/datasets/mangarahutagalung/anime-and-manga-data-from-myanimelist-march-2023
#### MyAnimeList 2022: 
##### https://www.kaggle.com/datasets/vishalmane10/anime-dataset-2022
#### MyAnimeList 2021:
##### https://www.kaggle.com/datasets/snehaanbhawal/anime-list-for-recommendation-system-june-2021

### Future Steps. 
#### Possibly controlling for potential confounding varibales like:
##### - The popularity of the MyAnimeList site during anime release dates affecting scores and ratings and how many individuals submitted scores and ratings.
##### - The incentive to favorite an anime for future consumption rather than as a method for rating it.
##### - Shows with more loyal fanbases are more willing to score and rate.
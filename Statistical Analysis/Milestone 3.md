---
title: "Analysis of Anime Popularity:"
subtitle: "by MyAnimeList Fan-Service - Milestone 3"
author: "Aaron J. Brown"
date: "Spring 2023"
output:
  html_document: default
  pdf_document: default
---

### Introduction.
#### With the increasing popularity of anime, we are seeing the emergence of more anime titles in more recent years. In contribution to anime's constant uprising, we see more platforms in which to consume our animated favorites; some of these platforms also allow users to subscribe, watch, favorite, and rate new and old titles to their liking. The avaliabilty of data like this has allowed programmers to determine the most popular, most viewed, or most favorited anime titles, and serve them right back to their audience as recommendations! 

### About the Data.
#### The data used in this analysis was scrapped from an anime rating and listing site (MyAnimeList) that provides details of almost all available anime. Though the site does not allow users to actually watch any of the programs directly from the site, it provides a space for anime fans to compile all of their favorite anime into a list, as well as ranking, sharing, commenting, and a host of other features.
The datasets used provide details of over 18,000 separate anime productions, including series, movies, OVAs, ONAs, music, and others. Information such as number of episodes, duration, score, popularity, members, etc. can be extracted from the data. The three datasets contain data from three years: 2023, 2022, and 2021. 

### The problem statement you addressed. 
#### This analysis will help shed light on what titles and genres are most liked and consumed, and what contributes to the popularity of those titles. Visualizations will be used to display the relationship between popularity and related variables.

### How you addressed this problem statement.
#### The approach is to utilize, primarily: scores, popularity, members, and favorites to determine the relationship between these variables and the most popular anime, and will determine if there are any relationships between an anime's popularity and the amount of members, as well as the different means to which fans interact with anime on MyAnimeList. As MyAnimeList allows only registered users to "score", and un-registered users can "favorite" there is expected to be a difference with the relationship between scores and favorites as they relate to popularity, possibly due to a more dedicated fan-service willing to go through the motions of creating an account.
GGPlot and Patchwork are used to produce visualizations of the various relationships mentioned above.

### Analysis.
```{r setup, include=FALSE}
setwd("/Users/aaronbrown/Documents/ClassWork/DSC 520 - Statistics for Data Science/Project")
knitr::opts_chunk$set(echo = TRUE)
```

```{r include = FALSE} 
library(tidyverse)
library(scales)
library(patchwork)
```

```{r include = TRUE}

anime_data23 <- read.csv("/Users/aaronbrown/Documents/ClassWork/DSC 520 - Statistics for Data Science/Project/anime_data_2023.csv")
View(anime_data23)

anime_tidy23 <- anime_data23%>%
  select(-c(url, images, trailer, approved, titles, title_japanese,  producers, studios, genres, themes, demographics, title_synonyms, aired, synopsis, background, broadcast, licensors, explicit_genres))
head(anime_tidy23)

View(anime_tidy23)
```

```{r include = TRUE}
unique_anime <- anime_tidy23%>%
distinct()%>%
filter(!is.na(title_english))
View(unique_anime)
```

```{r include = TRUE}
tv_anime <- unique_anime%>%filter(type=="TV")
tv_plot <-tv_anime%>%
  mutate(graph_name=paste0(title_english," (",year,")"))%>%
  top_n(-20,wt=popularity) %>% 
  
    ggplot(aes(reorder(graph_name,desc(popularity)),popularity,colour=title_english))+
      geom_point(show.legend = FALSE)+ eom_segment(aes(x=graph_name,xend=graph_name,y=0,yend=popularity),show.legend = FALSE)+
      coord_flip()+theme_classic()+labs(x="",y="Popularity",title = "Top 20 Most Popular Anime TV Shows")
  

tv_plot
```

```{r include = TRUE}
tv_anime_airing <- anime_tidy23%>%filter(type=="TV",airing=="TRUE")
View(tv_anime_airing)
tv_plot_airing <-tv_anime_airing%>%
  mutate(graph_name=paste0(title," (",year,")"))%>%
  ggplot(aes(reorder(graph_name,desc(popularity)),popularity,colour=title))+
  geom_point(show.legend = FALSE)+
  geom_segment(aes(x=graph_name,xend=graph_name,y=0,yend=popularity),show.legend = FALSE)+
  coord_flip()+
  theme_classic()+
  labs(x="",y="Popularity",title = "Top 20 Most Popular Anime TV Shows")+
  theme(axis.text.y.left = element_text(size = 12))
tv_plot_airing
```

```{r include = TRUE}
tv_anime %>% 
  filter(popularity <= 50) %>%
  mutate(title_english = str_replace(title_english, "Code Geass: Lelouch of the Rebellion", "Code Geass")) %>%
  ggplot(aes(score, scored_by)) + 
  geom_point(shape=21,aes(fill=title_english,size=members)) + 
  geom_text(aes(label = title_english ), check_overlap = T, show.legend = F, size = 3, hjust = 1) + 
  xlim(c(6, 10)) +
  scale_y_log10()+
  labs(title = "Popularity vs High Score", 
       subtitle = "Top 50 Most Popular Anime Titles",
       y = "Number of Scoring Users",
       x = "Score (1-10)") +
  theme_classic()+
  theme(legend.position = 'none',aspect.ratio = 0.5)
```

```{r include = TRUE}
tv_anime%>%
  filter(popularity<=100) %>% 
  ggplot(aes(popularity,score,fill=rating))+
  geom_point(shape=21,size=3,alpha=0.8)+
  geom_text(aes(label=title_english),size=3,check_overlap = TRUE)+
  scale_x_log10()+
  theme_classic()+
  scale_color_brewer(palette = 'Set1')+
  theme(legend.position = "bottom")+
  labs(title = "Popularity vs Score",subtitle = "Do all Popular Anime score high?")
```
  
```{r include = TRUE}
most_members <- tv_anime%>%
  top_n(20,wt=members) %>%
  mutate(graph_name=paste0(title_english," (",year,")"),graph_name=fct_reorder(graph_name,members))%>%
  ggplot(aes(graph_name,members,fill=graph_name))+
  geom_bar(stat = 'identity',width=0.5,show.legend = FALSE,color='black')+
  coord_flip()+
  theme_classic()+
  scale_y_continuous(limits = c(0,1700000),labels = comma)+
  geom_text(aes(label=comma(members)),size=3)+
  labs(x="",y="Rank",title = "Top 20 Most Members")
most_members
```

```{r include = TRUE}
most_favorite<-  tv_anime%>%top_n(20,wt=favorites) %>%  mutate(title_english=fct_reorder(title_english,favorites))%>%
  ggplot(aes(title_english,favorites,fill=title_english))+geom_bar(stat = 'identity',width=0.5,show.legend = FALSE,color='black')+
  coord_flip()+ theme_classic()+ scale_y_continuous(limits = c(0,150000),labels = comma)+ geom_text(aes(label=comma(favorites)),size=3,hjust=1,fontface='bold')+
  labs(x="",y="Number of Favorites",title = "Top 20 Most Favorites")
most_favorite
```

```{r include = TRUE}
most_scored<- tv_anime%>% top_n(20,wt=scored_by) %>%  mutate(title_english=fct_reorder(title_english,scored_by))%>%
  ggplot(aes(title_english,scored_by,fill= title_english))+ geom_bar(stat = 'identity',width=0.5,color='black',show.legend = FALSE)+
  coord_flip()+ theme_classic()+scale_y_continuous(limits = c(0,1500000),labels = comma)+ geom_text(aes(label=comma(scored_by)),size=3,hjust=1,fontface='bold')+
  labs(x="",y="Number of Favorites",title = "Top 20 Most Scored")
most_favorite + most_members+ most_scored + plot_layout(widths = 2)
most_scored
```

```{r include = TRUE}
# POPULARITY x SCORE
model1 = lm(popularity ~ score, data = tv_anime)
anova_table1 = anova(model1)
anova_table1

# POPULARITY x FAVORITES
model2 = lm(popularity ~ favorites, data = tv_anime)
anova_table2 = anova(model2)
anova_table2

# POPULARITY x MEMBERS
model3 = lm(popularity ~ members, data = tv_anime)
anova_table3 = anova(model3)
anova_table3

# POPULARITY x SCORED_BY
model4 = lm(popularity ~ scored_by, data = tv_anime)
anova_table4 = anova(model4)
anova_table4

```

### Implications. 
#### From the analysis, we can see that the most "popular" titles are not necessarily the most-scored titles, or even the highest-scored titles. For the most part though, many of the top 20 most popular titles appear in both the "top members", "top scored", and "top favorite" graphs, but are not generally seen in the same order, as expected. We can see clustering to the upper right on the "Popularity vs Score" graph, suggesting that popularity and score have a positive relationship. An obvious relationship can be observed between the "members" varible and "scores", as titles with more members would have more scorers; the ranking by these variables produce a more similar graph than other variables included in this study.

### Limitations.
#### A large portion of this data remained unusable due to excessive missing values. Acquiring a revised dataset with less missing values, or a method for filling in these missing values with predicted values could really help improve the statistical backbone of this analysis. Combining this data with data from other websites similar to MyAnimeList could also assist with the large quantity of missing values. The formatting for several variables like genre, producer, and studio caused errors when attempting to apply them to the analysis, separating the entries, making genre, producer, and studio more versatile would allow for a more detailed and explanatory analysis.

### Concluding Remarks.
#### Through this analysis, we have observed how popularity coincides with score, favorites, members, and scored_by. The "popularity" of an anime title does not seem to result from either one of our dependent variables, directly, though we did see that there are similarities between the most members and the most scored, which makes sense. Though the analysis is unable to outline a relationship between popularity and our independent variables, we still ended up with several recommendations for anime to watch!

### Resources
#### MyAnimeList

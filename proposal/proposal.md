Project proposal
================
Team 404

``` r
library(tidyverse)
library(broom)
library(friends)
```

## 1\. Introduction

We are going to work with the “Friends” data package from TidyTuesday.
It was aggregated, packaged, and shared by Emil Hvitfeldt.

We will try to look behind the levels of popularity throughout all 10
seasons, and ask the ultimate question:

What makes the perfect “Friends” episode?

The package consists of 4 data sets: friends : transcript of all
dialogues containing information on the speaker, episode, season, scene,
utterance friends\_emotions : information on the emotions of each
utterance with specified scene, episode, and season number
friends\_entities : character entities with info on utterance, scene,
episode, season friends\_info : here we have information on each
episode: title, episode number, season number, name of writer and
director, airing date, number of views and IMDB ratings.

## 2\. Data

``` r
glimpse(friends)
```

    ## Rows: 67,373
    ## Columns: 6
    ## $ text      <chr> "There's nothing to tell! He's just some guy I work with!",…
    ## $ speaker   <chr> "Monica Geller", "Joey Tribbiani", "Chandler Bing", "Phoebe…
    ## $ season    <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ episode   <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ scene     <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ utterance <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, …

``` r
glimpse(friends_emotions)
```

    ## Rows: 12,606
    ## Columns: 5
    ## $ season    <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ episode   <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    ## $ scene     <int> 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 5, 5, 5,…
    ## $ utterance <int> 1, 3, 4, 5, 6, 7, 8, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19…
    ## $ emotion   <chr> "Mad", "Neutral", "Joyful", "Neutral", "Neutral", "Neutral"…

``` r
glimpse(friends_info)
```

    ## Rows: 236
    ## Columns: 8
    ## $ season            <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    ## $ episode           <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, …
    ## $ title             <chr> "The Pilot", "The One with the Sonogram at the End"…
    ## $ directed_by       <chr> "James Burrows", "James Burrows", "James Burrows", …
    ## $ written_by        <chr> "David Crane & Marta Kauffman", "David Crane & Mart…
    ## $ air_date          <date> 1994-09-22, 1994-09-29, 1994-10-06, 1994-10-13, 19…
    ## $ us_views_millions <dbl> 21.5, 20.2, 19.5, 19.7, 18.6, 18.2, 23.5, 21.1, 23.…
    ## $ imdb_rating       <dbl> 8.3, 8.1, 8.2, 8.1, 8.5, 8.1, 9.0, 8.1, 8.2, 8.1, 8…

## 3\. Data analysis plan

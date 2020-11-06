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
It was aggregated, packaged, and shared by Emil Hvitfeldt. We load it
from the `friends` R package.

We will try to look behind the levels of popularity throughout all 10
seasons, and ask the ultimate question:

What makes the perfect “Friends” episode?

The package consists of 4 data sets:

friends : consists of 67,373 rows and 6 columns. It is a transcript of
all dialogues where each row represents an utterance. The variables
contain information on `text`, the `speaker`, `episode`, `season`,
`scene`, and the number of utterance - `utterance`;

friends\_emotions : consists of 12,606 rows and 5 columns. Each row
representing an utterance, variables give information on the number of
each `utterance` and its `emotion`, with specified `scene`, `episode`,
and `season` number;

friends\_entities : consists of 10.557 rows and 5 columns. Each row
being one utterance, variables show detail on utterance number -
`utterance`, `entity`, `scene`, `episode`, `season`;

friends\_info : consists of 236 rows and 8 columns. Each row represents
an episode. Variables are: title, episode number, season number, name of
writer and director, airing date, number of views and IMDB ratings.

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

We will explore this question through a number of visualizations. We are
using the word ‘Perfect’ to mean the episode which generates the highest
audience satisfaction and so we will use the IMDB rating (imdb\_rating)
of each episode as the response variable to evaluate the impact of each
factor on the quality of the episode. We decided against using the view
count, as the number of views of an episode does not necessarilly
reflect how it was recieved.

The factors which we will explore include: the characters (speaker)
which appear with the most prominence, the tone (emotion), the
writer(written\_by), and the director (directed\_by) of each episode. We
can also see which variable has the largest impact on viewer
satisfaction in this way.

``` r
friendsWanted <- friends %>%
  filter( 
    speaker == "Joey Tribbiani"|
    speaker == "Chandler Bing"|
    speaker == "Monica Geller"|
    speaker == "Phoebe Buffay"|
    speaker == "Rachel Green"|
    speaker == "Ross Geller"
  ) %>% 
  group_by(season, episode) %>% 
  count(speaker) %>%
  merge(friends_info, by=c("episode","season"))
```

``` r
friendsWanted %>% 
  ggplot(aes( x = n, y = imdb_rating, color = speaker))+
  geom_point()+
  geom_smooth(color = "black")+
  facet_wrap('.~speaker')+
  labs( title = "Episode Ratings vs How much each character spoke",
        x = "Number of Lines",
        y = "IMBD Rating",
        color = "Character"
      )+
  scale_color_viridis_d()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](proposal_files/figure-gfm/character-vs-rating-1.png)<!-- -->

This visualization enables us to see how the Number of lines spoken by
each character influences the IMDB rating for that episode. Using just a
scatter plot it is difficult to see a connection between the variables,
so a trend line was used to reveal a connection. However, the trend line
seems to be largely influenced by extreme values, so we will need to do
more analysis to see if these values are outliers which need to be
removed.

``` r
friendsWanted %>% 
  group_by(written_by) %>% 
  summarise(mean_rating = mean(imdb_rating), med_rating = median(imdb_rating)) %>% 
  arrange(desc(mean_rating))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 102 x 3
    ##    written_by                                             mean_rating med_rating
    ##    <chr>                                                        <dbl>      <dbl>
    ##  1 Jill Condon & Amy Toomin                                      9.5        9.5 
    ##  2 Story by : Brian BoyleTeleplay by : Suzie Villandry           9.1        9.1 
    ##  3 Story by : Zachary RosenblattTeleplay by : Adam Chase         9.1        9.1 
    ##  4 Gregory S. MalinsMarta Kauffman & David Crane                 9.05       9.05
    ##  5 Shana Goldberg-Meehan & Scott SilveriAndrew Reich & T…        9.05       9.05
    ##  6 Story by : Brian CaldirolaTeleplay by : Sherry Bilsin…        9          9   
    ##  7 Gregory S. Malins                                             8.97       9.2 
    ##  8 Michael BorkowStory by : Jill Condon & Amy ToominTele…        8.95       8.95
    ##  9 Ted Cohen & Andrew ReichGregory S. Malins & Scott Sil…        8.95       8.95
    ## 10 Robert Carlock & Dana Klein Borkow                            8.9        8.9 
    ## # … with 92 more rows

In this summary statistic you can see which writers achieved the highest
mean IMDB rating. It also becomes clear that the ‘written\_by’ variable
does not only contain singular names. We many need to alter this data
again to provide only singular names in the ‘written\_by’ variable in
order to see more clearly which writers are preferred.

Furthermore, some of the observations show that some writers have the
same median and mean rating, leading us to the think that there may only
be one episode represented. The sample size of these writers’ episodes
may thus be too small to draw any meaning full conclusions

``` r
friendsWanted %>% 
  count(written_by) %>%
  arrange(n)
```

    ##                                                                                               written_by
    ## 1                                                                                  Alicia Sky Varinaitis
    ## 2                                                                                          Bill Lawrence
    ## 3                                                                                          Brian Buckner
    ## 4                                                                                          Brown Mandell
    ## 5                                                                             Gigi McCreery & Perry Rein
    ## 6                                          Jeffrey Astrof & Mike Sikowitz & Adam Chase & Ira Ungerleider
    ## 7                                                                              Jill Condon\n& Amy Toomin
    ## 8                                                                               Jill Condon & Amy Toomin
    ## 9                                                                                              Patty Lin
    ## 10                                                                            Perry Rein & Gigi McCreery
    ## 11                                                                                         Peter Tibbals
    ## 12                                                                                    R. Lee Fleming Jr.
    ## 13                                                                    Robert Carlock & Dana Klein Borkow
    ## 14                                                                 Scott Silveri & Shana Goldberg-Meehan
    ## 15                                                                           Scott Silveri & Wil Calhoun
    ## 16                                                                                       Sebastian Jones
    ## 17                                                                  Shana Goldberg-Meehan & Seth Kurland
    ## 18                                 Story by : Adam ChaseTeleplay by : Gregory S. Malins & Michael Curtis
    ## 19                                                    Story by : Alexa JungeTeleplay by : Michael Borkow
    ## 20                              Story by : Alicia Sky VarinaitisTeleplay by : Gigi McCreery & Perry Rein
    ## 21                   Story by : Alicia Sky VarinaitisTeleplay by : Scott Silveri & Shana Goldberg-Meehan
    ## 22                             Story by : Andrew Reich & Ted CohenTeleplay by : Jill Condon & Amy Toomin
    ## 23                           Story by : Andrew Reich & Ted CohenTeleplay by : Perry Rein & Gigi McCreery
    ## 24                                                   Story by : Brian BoyleTeleplay by : Suzie Villandry
    ## 25                      Story by : Brian Buckner & Sebastian JonesTeleplay by : Andrew Reich & Ted Cohen
    ## 26         Story by : Brian Buckner & Sebastian JonesTeleplay by : Sherry Bilsing-Graham & Ellen Plummer
    ## 27                            Story by : Brian Buckner & Sebastian JonesTeleplay by : Zachary Rosenblatt
    ## 28                         Story by : Brian CaldirolaTeleplay by : Sherry Bilsing-Graham & Ellen Plummer
    ## 29                                                Story by : Dana Klein BorkowTeleplay by : Mark Kunerth
    ## 30                   Story by : David Crane & Marta KauffmanTeleplay by : Jeff Greenstein & Jeff Strauss
    ## 31                                                 Story by : David J. LaganaTeleplay by : Scott Silveri
    ## 32                                                          Story by : Earl DavisTeleplay by : Patty Lin
    ## 33                Story by : Ellen Plummer & Sherry BilsingTeleplay by : Brian Buckner & Sebastian Jones
    ## 34                             Story by : Gregory S. MalinsTeleplay by : Brian Buckner & Sebastian Jones
    ## 35                                                 Story by : Ira UngerleiderTeleplay by : Brown Mandell
    ## 36                                                      Story by : Judd RubinTeleplay by : Peter Tibbals
    ## 37                                     Story by : Mark KunerthTeleplay by : David Crane & Marta Kauffman
    ## 38                                                  Story by : Mark KunerthTeleplay by : Richard Goodman
    ## 39                                 Story by : Michael Curtis & Gregory S. MalinsTeleplay by : Adam Chase
    ## 40                                                   Story by : Michael CurtisTeleplay by : Seth Kurland
    ## 41                                     Story by : Pang-Ni Landrum & Mark KunerthTeleplay by : Adam Chase
    ## 42          Story by : Pang-Ni Landrum & Mark KunerthTeleplay by : Shana Goldberg-Meehan & Scott Silveri
    ## 43                                                    Story by : Peter TibbalsTeleplay by : Mark Kunerth
    ## 44                                           Story by : R. Lee Fleming Jr.Teleplay by : Steven Rosenhaus
    ## 45                                                   Story by : Robert CarlockTeleplay by : Tracy Reilly
    ## 46                                               Story by : Scott SilveriTeleplay by : Gregory S. Malins
    ## 47                                                  Story by : Scott SilveriTeleplay by : Robert Carlock
    ## 48                                       Story by : Seth KurlandTeleplay by : Gigi McCreery & Perry Rein
    ## 49                               Story by : Seth KurlandTeleplay by : Gregory S. Malins & Michael Curtis
    ## 50                            Story by : Seth KurlandTeleplay by : Sherry Bilsing-Graham & Ellen Plummer
    ## 51                                             Story by : Shana Goldberg-MeehanTeleplay by : Wil Calhoun
    ## 52                       Story by : Sherry Bilsing & Ellen PlummerTeleplay by : Andrew Reich & Ted Cohen
    ## 53                                    Story by : Sherry Bilsing & Ellen PlummerTeleplay by : Brian Boyle
    ## 54                        Story by : Sherry Bilsing-Graham & Ellen PlummerTeleplay by : Steven Rosenhaus
    ## 55                                          Story by : Ted Cohen & Andrew ReichTeleplay by : Wil Calhoun
    ## 56                               Story by : Vanessa McCarthyTeleplay by : Ellen Plummer & Sherry Bilsing
    ## 57                                      Story by : Wil CalhounTeleplay by : David Crane & Marta Kauffman
    ## 58                                                 Story by : Zachary RosenblattTeleplay by : Adam Chase
    ## 59                                                Story by : Zachary RosenblattTeleplay by : Brian Boyle
    ## 60                                                                                      Vanessa McCarthy
    ## 61                                                                                    Zachary Rosenblatt
    ## 62                                                                                           Betsy Borns
    ## 63                                                                                           Brian Boyle
    ## 64                                            Gregory S. Malins & Adam ChaseDavid Crane & Marta Kauffman
    ## 65                                                                    Gregory S. Malins & Michael Curtis
    ## 66                                                         Gregory S. MalinsMarta Kauffman & David Crane
    ## 67                                                          Jeffrey Astrof & Mike SikowitzMichael Borkow
    ## 68  Michael BorkowStory by : Jill Condon & Amy ToominTeleplay by : Shana Goldberg-Meehan & Scott Silveri
    ## 69                                                                                        Michael Curtis
    ## 70                                                             Scott SilveriMarta Kauffman & David Crane
    ## 71                                         Shana Goldberg-Meehan & Scott SilveriAndrew Reich & Ted Cohen
    ## 72                                     Shana Goldberg-Meehan & Scott SilveriMarta Kauffman & David Crane
    ## 73                                                                        Sherry Bilsing & Ellen Plummer
    ## 74                             Story by : Dana Klein BorkowTeleplay by : Brian Buckner & Sebastian Jones
    ## 75                                Story by : Robert CarlockTeleplay by : Brian Buckner & Sebastian Jones
    ## 76                                Story by : Shana Goldberg-MeehanTeleplay by : Andrew Reich & Ted Cohen
    ## 77                                             Ted Cohen & Andrew ReichGregory S. Malins & Scott Silveri
    ## 78                                                                                           Chris Brown
    ## 79                                                                                     Gregory S. Malins
    ## 80                                                                                       Ira Ungerleider
    ## 81                                                                        Jeff Greenstein & Jeff Strauss
    ## 82                                                                                        Robert Carlock
    ## 83                                                                          Adam Chase & Ira Ungerleider
    ## 84                                                                                     Dana Klein Borkow
    ## 85                                                                                          Mark Kunerth
    ## 86                                                                                        Michael Borkow
    ## 87                                                                    Michael Curtis & Gregory S. Malins
    ## 88                                                                 Shana Goldberg-Meehan & Scott Silveri
    ## 89                                                                       Brian Buckner & Sebastian Jones
    ## 90                                                                          Marta Kauffman & David Crane
    ## 91                                                                              Ted Cohen & Andrew Reich
    ## 92                                                                                            Adam Chase
    ## 93                                                                        Jeffrey Astrof & Mike Sikowitz
    ## 94                                                                                         Scott Silveri
    ## 95                                                                 Sherry Bilsing-Graham & Ellen Plummer
    ## 96                                                                                           Wil Calhoun
    ## 97                                                                                          Seth Kurland
    ## 98                                                                                 Shana Goldberg-Meehan
    ## 99                                                                          David Crane & Marta Kauffman
    ## 100                                                                                          Doty Abrams
    ## 101                                                                                          Alexa Junge
    ## 102                                                                             Andrew Reich & Ted Cohen
    ##      n
    ## 1    6
    ## 2    6
    ## 3    6
    ## 4    6
    ## 5    6
    ## 6    6
    ## 7    6
    ## 8    6
    ## 9    6
    ## 10   6
    ## 11   6
    ## 12   6
    ## 13   6
    ## 14   6
    ## 15   6
    ## 16   6
    ## 17   6
    ## 18   6
    ## 19   6
    ## 20   6
    ## 21   6
    ## 22   6
    ## 23   6
    ## 24   6
    ## 25   6
    ## 26   6
    ## 27   6
    ## 28   6
    ## 29   6
    ## 30   6
    ## 31   6
    ## 32   6
    ## 33   6
    ## 34   6
    ## 35   6
    ## 36   6
    ## 37   6
    ## 38   6
    ## 39   6
    ## 40   6
    ## 41   6
    ## 42   6
    ## 43   6
    ## 44   6
    ## 45   6
    ## 46   6
    ## 47   6
    ## 48   6
    ## 49   6
    ## 50   6
    ## 51   6
    ## 52   6
    ## 53   6
    ## 54   6
    ## 55   6
    ## 56   6
    ## 57   6
    ## 58   6
    ## 59   6
    ## 60   6
    ## 61   6
    ## 62  12
    ## 63  12
    ## 64  12
    ## 65  12
    ## 66  12
    ## 67  12
    ## 68  12
    ## 69  12
    ## 70  12
    ## 71  12
    ## 72  12
    ## 73  12
    ## 74  12
    ## 75  12
    ## 76  12
    ## 77  12
    ## 78  18
    ## 79  18
    ## 80  18
    ## 81  18
    ## 82  18
    ## 83  24
    ## 84  24
    ## 85  24
    ## 86  24
    ## 87  24
    ## 88  24
    ## 89  30
    ## 90  30
    ## 91  30
    ## 92  36
    ## 93  36
    ## 94  42
    ## 95  42
    ## 96  42
    ## 97  48
    ## 98  48
    ## 99  54
    ## 100 54
    ## 101 66
    ## 102 66

In this table we can see the how many episodes each ‘writer’ has
written, and by arranging them in ascending order we can see that the
minimum number of episodes is 6. We must explore as to whether this is
an adequate sample size

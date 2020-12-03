The Perfect FRIENDS Episode
================
by Team 404

## Summary

For our project, we decided to look into one of the most well-known TV shows, Friends, and see what influences the popularity of each individual episode.
We worked with the `friends` package from tidyverse. It contains a transcript of all the dialogue from each episode, along with the speaker and emotion in it, and also information on the writer, director, IMDB rating etc.
So, the question is: what influences the IMDB rating?
 
Firstly, the 6 friends (Monica, Rachel, Joey, Chandler, Phoebe and Ross) might not be equally popular with the audience, and people may like to see (or hear in this case) some of them, more than the others.
We looked into how the number of utterances from each character influences the rating, using a model and a plot.
An interesting surprise was the estimate of the slope from the model did not match the slope on the plot. That's because the estimate shows the relationship between additional lines of each character, whereas the plot shows the number of utterances as if other characters did not exist.
Here we might note that it would be more precise to study the interactions between particular characters, but that would require a more detailed data set that what we are working with.
However, from the sources that we have now, it seems that audience loves to see the Geller siblings the most.
 
Next factor that we examined is the tone, or the emotion, that is present in each episode.
All lines are sorted into 7 tone categories:  Joyful, Mad, Peaceful, Neutral, Powerful, Sad or Scared. We looked at how the level of each emotion affected the episode ratings with the same tools as we did in previous slides.
The results show that:
·         Audiences do not rate episodes high in negative emotions like anger and sadness highly
·         Neutral and joyful episodes are not too appealing either, maybe it's boring?
·         The best episodes are where the most prominant tones are are peaceful, powerful and, surprisingly, scared
Here we have to note that the data set only provides information on the first 4 seasons.
 
Another potential difference in popularity may be caused by who  the writer and the director were of the episode. There are multiple writers and directors that contributed throughout the 10 seasons, often working together too.
 
We first looked at the top average ratingsfor the writers, however, when also looking at the number of episodes each have written, most of the ones shown have only written one, or a few episodes. It is not very reliable to compare them with the writers of multiple episodes, so we decided to focus on the ones that have written 10 or more episodes, and the most successful writer seems to be David Crane followed by Marta Kauffman.
It's worth noting that these two co-wrote a lot of episodes.
An interesting thing to point out is also that there is a great distribution of episodes between the writers, unlike some other TV shows that are mostly written by the same person/team.
 
Looking at the directors, the most highly rated ones are Kevin S. Bright and David Schwimmer, who also played Ross.
 
We thought we would also look at how the rating developed throughout the seasons, after all, the show ran for a decade, and a lot can change in that time.
Our plot doesn't show any significant change in popularity throughout seasons, but we can see there is a certain level of ratings for each season that most episodes tend to reach, for example 8.5 in the 4th season.
 
We've reviewed some of the factors affecting the imdb rating, and for each of them, we found what should *theoretically* add up to the highest-rated episode.
That episode should be:
·         Written by David Crane (or Suzie Villandry)
·         Directed by Kevin S. Bright (or David Schwimmer)
·         Mostly Monica and Ross talking
·         In a peaceful and/or scared tone.
 
Now let's look at the actual best-rated episodes. There are 2 episodes that reached 9.7 rating, one of them being the final, which is a double episode.
 
Season 5, ep 14:
·         Written by Alexa Junge
·         Directed bu Michael Lembeck
·         Pheobe, Chandler and Rachel spoke the most
·         (data not available)
What happened?
The gang found out about Monica and Chandler being in a relationship.
 
Season 10, ep 17&18:
·         Written by Marta Kauffman & David Crane
·         Directed by Kevin S. Bright
·         Ross and Joey spoke the most, Monica the least
·         (data not available)
What happened?
The final episode, story comes to a happy end.

Seems our predictions need improvement, as we only got the writer and director of one of the episodes right.
 
In conclusion, we have been able to identify several factors which certainly contribute to a higher IMDB rating. It is clear that when Kevin S. Bright is directing and one or both of Marta Kauffman and David Crane is writing it leads to a more highly rated episode, as well as when the Geller siblings are more involved with a mix of peaceful and scared tone to the episode. However, there are variables which were not included in the dataset, for example whether or not an episode contained a scene which has particular importance to the story-line, or factors which are tough to quantify, for example how funny the jokes in that episode are. These are both things which would influence a viewers enjoyment of an episode and hence increase the IMDB score. So whilst it is near impossible to detail a perfect episode we have certainly outlined criteria that make an episode better. 

## Presentation

Our presentation can be found [here](presentation/presentation.html).

## Data

Include a citation for your data here. See
<http://libraryguides.vu.edu.au/c.php?g=386501&p=4347840> for guidance
on proper citation for datasets. If you got your data off the web, make
sure to note the retrieval date.

## References

Emil Hvitfeldt 2020, *Friends*, electronic dataset, `friends` R package,
<https://github.com/emorynlp/character-mining>

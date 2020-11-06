# data

Place data file(s) in this folder.

Then, include codebooks (variables, and their descriptions) for your data file(s)
using the following format.

## name of data file

Friends.csv
-`text`: This variable shows every line of dialogue as text, from when a character starts speaking until they stop. It's class is 'character'
-`speaker`: This variable tells us which character said the dialogue. It's class is 'character'
-`season`: This variable tells us the season number that the dialogue featured in. It's class is 'double'
-`episode`: This variable tells us which episode the dialogue featured in. It's class is 'double'.
-`scene`: This variable tells us the scene number which the dialogue featured in. It's class is 'double'.
-`utterance`: This variable tells us the utterance number of the dialogue. It's class is 'double'

Friends_emotions.csv
-`season`: Same as in previous data set
-`episode`: Same as in previous data set
-`scene`: Same as in previous data set
-`utterance`: Same as in previous data set
-`emotion`: The emotion that a certain line of dialogue is delivered with, there are 7 possible emotions. It's class is 'character'.

Friends_entities.csv (within above package)
-`entities`: The entities of each character. It's class is 'list'.

Friends_info.csv
-`season`: Same as in previous data set
-`episode`: Same as in previous data set
-`title`: The title of each episode. It's class is 'character'.
-`directed_by`: Who directed any given episode, may be more than one person. It's class is 'character'.
-`written_by`: Who wrote each episode, may be more than one person. It's class is 'character'.
-`air_date`: The date that the episode first aired. It's class is 'date'.
-`us_views_millions`: The number of viewers in the USA of that episode in millions. It's class is 'double'.
-`imdb_rating`: The IMDB rating of that episode. It's class is 'double'.
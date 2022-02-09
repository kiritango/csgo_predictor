# csgo_predictor
A data science project on predicting professional CS:GO matches.
# Intro
Since I'm a big fan of professional CS:GO and recently started to learn data science, I've decided to combine these 2 interests into a project. The result of the project supposed to be a model based on data about teams, maps and tournaments. Doing this project, I don't expect great accuracy of my model because there are much more different factors reflecting on teams' performance than just statistics. The main goal of the project is to experience all stages of work with raw data.  
# Data collection
All data in this project has been extracted from an Internet resource HLTV.org, specializing in collecting various statistics of professional esports matches. For scraping the data I've used my module [hltv_scraper](https://github.com/kiritango/hltv_scraper). Since alignment of forces in CS:GO tend to change quite frequiently, I've decided to use data only for the past year. Here some information that I got from HLTV.org: 
- Matches
- Tournaments
- Stats of teams based on previous 30 days of performance

All csv files are available [here](https://github.com/kiritango/csgo_predictor/tree/main/data).

# Data processing

For a more detailed picture of working with data you can go [here](https://github.com/kiritango/csgo_predictor/blob/main/Data_preparation.ipynb).

## Teams' regions

First thing I've done after collecting data is comparison of performance of teams from different regions. My theory is that teams from regions with strong competitors (CIS, Europe, NA) are much more professional than teams from other parts of the world (Asia, Oceania) where CS:GO is less popular. To prove this, I picked the best team from Asian region (Tyloo) and mediocre team from Europe (Mouz). Next, I just compared their performance on all tournaments and on major ones. Here what I've got:

![image](https://user-images.githubusercontent.com/97898643/153187012-692ae5f1-a8ab-4c13-85b5-0a036107b29e.png) ![image](https://user-images.githubusercontent.com/97898643/153187121-956d165d-e51e-485d-bb87-206bc8b48497.png)

Overall, looks like both teams have similar results in genereal(~65% win/ ~35% loss).

However, when we look into their performance only during big tournaments the great differnces can be seen.

![image](https://user-images.githubusercontent.com/97898643/153189184-24083c83-3ccd-4064-8436-38f32922c99f.png) ![image](https://user-images.githubusercontent.com/97898643/153189386-e3117376-06a3-4c32-aa72-f94dbc30f519.png)

So, as we can see, team from Asia have absolutely opposite results on big events (~30% win/ 70% loss), while team from Europe still shows quite strong performance (60% win/ 40% loss). According to this, my theory is confirmed and addition of new feature "region" seems necessary. 

## Classification of events

Another thing that potentially plays big role in teams performance is an event. So, I've added new feature "importance" describing how big and important tournaments are. The importance takes following values:
- 1 : Tournaments with prize less than 50k$;
- 2 : Tournaments with prize more than 50k$ or qualifications on bigger events;
- 3 : Tournaments with prize more than 100k$;
- 4 : Tournaments with prize more than 250k$.

## Updating teams' names

For the past year some teams (Gambit Youngsters - Gambit; Winstrike - Entropiq) changed their names without changing roster so I had to update their names.

## Adding stats

The next important step is to add team statistics for the previous month relative to the match date. Unfortunately, the information on the website hltv.org it is presented in a rather strange way, so there was a situation when there were no statistics on the date of the match. In such cases, statistics were taken for the nearest date preceding the match.

## Teams' ratings

In conclusion, the rating of teams was added to the dataset, which was calculated based on the achievements of teams at tournaments over the past year. The data was taken from the website of one of the tournament organizers. The rating is an integer from 0 to 2696 (the highest rating at the time of the work).

## Miscellaneous

Some addition actions with the data:
- Excluding show matches;
- Sorting and dividing dataset in chronological order;
- One-hot encoding of categorical data (team regions, map).

# Modelling
# Conclusions

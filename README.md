# Data Serialization Formats - Cumulative Lab

## Introduction

Now that you have learned about CSV and JSON file formats individually, it's time to bring them together with a cumulative lab! Even as a junior data scientist, you can often produce novel, interesting analyses by combining multiple datasets that haven't been combined before.

## Objectives

You will be able to:

* Practice reading serialized data from files into Python objects
* Practice extracting information from a nested data structure
* Practice filtering records
* Practice cleaning data (normalizing locations)
* Combine data from multiple sources into a single data structure
* Use descriptive statistics and data visualizations to present your findings

## Your Task: Analyze the Relationship between Population and World Cup Performance

![Russia 2018 branded soccer ball and trophy](images/world_cup.jpg)

<span>Photo by <a href="https://unsplash.com/@fznsr_?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Fauzan Saari</a> on <a href="https://unsplash.com/s/photos/soccer-world-cup?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>

### Business Understanding

Intuitively, we might assume that countries with larger populations would have better performance in international sports competitions. While this has been demonstrated to be [true for the Olympics](https://www.researchgate.net/publication/308513557_Medals_at_the_Olympic_Games_The_Relationship_Between_Won_Medals_Gross_Domestic_Product_Population_Size_and_the_Weight_of_Sportive_Practice), the results for the FIFA World Cup are more mixed:

<p><a href="https://commons.wikimedia.org/wiki/File:World_cup_countries_best_results_and_hosts.PNG#/media/File:World_cup_countries_best_results_and_hosts.PNG"><img src="https://upload.wikimedia.org/wikipedia/commons/b/b7/World_cup_countries_best_results_and_hosts.PNG" alt="World cup countries best results and hosts.PNG" height="563" width="1280"></a><br><a href="http://creativecommons.org/licenses/by-sa/3.0/" title="Creative Commons Attribution-Share Alike 3.0">CC BY-SA 3.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=578740">Link</a></p>

In this analysis, we are going to look specifically at the sample of World Cup games in 2018 and the corresponding 2018 populations of the participating nations, to determine the relationship between population and World Cup performance for this year.

### Data Understanding

The data sources for this analysis will be pulled from two separate files.

#### `world_cup_2018.json`

* **Source**: This dataset comes from [`football.db`](http://openfootball.github.io/), a "free and open public domain football database & schema for use in any (programming) language"
* **Contents**: Data about all games in the 2018 World Cup, including date, location (city and stadium), teams, goals scored (and by whom), and tournament group
* **Format**: Nested JSON data (dictionary containing a list of rounds, each of which contains a list of matches, each of which contains information about the teams involved and the points scored)

#### `country_populations.csv`

* **Source**: This dataset comes from a curated collection by [DataHub.io](https://datahub.io/core/population), originally sourced from the World Bank
* **Contents**: Data about populations by country for all available years from 1960 to 2018
* **Format**: CSV data, where each row contains a country name, a year, and a population

### Requirements

#### 1. List of Teams in 2018 World Cup

Create an alphabetically-sorted list of teams who competed in the 2018 FIFA World Cup.

#### 2. Associating Countries with 2018 World Cup Performance

Create a data structure that connects a team name (country name) to its performance in the 2018 FIFA World Cup. We'll use the count of games won in the entire tournament (group stage as well as knockout stage) to represent the performance.

This will help create visualizations to help the reader understand the distribution of games won and the performance of each team.

#### 3. Associating Countries with 2018 Population

Add to the existing data structure so that it also connects each country name to its 2018 population, and create visualizations comparable to those from step 2.

#### 4. Analysis of Population vs. Performance

Choose an appropriate statistical measure to analyze the relationship between population and performance, and create a visualization representing this relationship.

### Checking for Understanding

Before moving on to the next step, pause and think about the strategy for this analysis.

First, what is our **unit of analysis**, and what is the **unique identifier**? In other words, what will one record in our final data structure represent, and what attribute uniquely describes it?

.

.

.

*Answer: Our unit of analysis is a* ***country*** *and the unique identifier we'll use is the* ***country name***

Next, what **features** are we analyzing? In other words, what attributes of each country are we interested in?

.

.

.

*Answer: Our features are* ***2018 population*** *and* ***count of wins in the 2018 World Cup***

Finally, which dataset should we **start** with? In this case, any record with missing data is not useful to us, so we want to start with the smaller dataset.

.

.

.

*Answer: There are only 32 countries that compete in the World Cup each year, compared to hundreds of countries in the world, so we should start with the* ***2018 World Cup*** *dataset. Then we can join it with the relevant records from the country population dataset.*

## Getting the Data

Below we import the `json` and `csv` modules, which will be used for reading from `world_cup_2018.json` and `country_populations.csv`, respectively.


```python
import json
import csv
```

Next, we open the relevant files.


```python
world_cup_file = open("data/world_cup_2018.json")
population_file = open("data/country_populations.csv")
```

**Hint:** if your code below is not working, (e.g. `ValueError: I/O operation on closed file.`, or you get an empty list or dictionary) try re-running the cell above to reopen the files, then re-run your code.

### 2018 World Cup Data

In the cell below, use the `json` module to load the data from `world_cup_file` into a dictionary called `world_cup_data`


```python
world_cup_data = json.load(world_cup_file)

# Close the file now that we're done reading from it
world_cup_file.close()
```

Make sure the `assert` passes, ensuring that `world_cup_data` has the correct type.


```python

# Check that the overall data structure is a dictionary
assert type(world_cup_data) == dict

# Check that the dictionary has 2 keys, 'name' and 'rounds'
assert list(world_cup_data.keys()) == ['name', 'rounds']
```

### Population Data

Now use the `csv` module to load the data from `population_file` into a list of dictionaries called `population_data`

(Recall that you can convert a `csv.DictReader` object into a list of dictionaries using the built-in `list()` function.)


```python
population_data = list(csv.DictReader(population_file))

# Close the file now that we're done reading from it
population_file.close()
```

Make sure the `assert`s pass, ensuring that `population_data` has the correct type.


```python

# Check that the overall data structure is a list
assert type(population_data) == list

# Check that the 0th element is a dictionary
# (csv.DictReader interface differs slightly by Python version;
# either a dict or an OrderedDict is fine here)
from collections import OrderedDict
assert type(population_data[0]) == dict or type(population_data[0]) == OrderedDict
```

## 1. List of Teams in 2018 World Cup

> Create an alphabetically-sorted list of teams who competed in the 2018 FIFA World Cup.

This will take several steps, some of which have been completed for you.

Let's start by exploring the structure of `world_cup_data`. Here is a pretty-printed preview of its contents:

```
{
  "name": "World Cup 2018",
  "rounds": [
    {
      "name": "Matchday 1",
      "matches": [
        {
          "num": 1,
          "date": "2018-06-14",
          "time": "18:00",
          "team1": { "name": "Russia",       "code": "RUS" },
          "team2": { "name": "Saudi Arabia", "code": "KSA" },
          "score1":  5,
          "score2":  0,
          "score1i": 2,
          "score2i": 0,
          "goals1": [
            { "name": "Gazinsky",   "minute": 12,              "score1": 1, "score2": 0 },
            { "name": "Cheryshev",  "minute": 43,              "score1": 2, "score2": 0 },
            { "name": "Dzyuba",     "minute": 71,              "score1": 3, "score2": 0 },
            { "name": "Cheryshev",  "minute": 90, "offset": 1, "score1": 4, "score2": 0 },
            { "name": "Golovin",    "minute": 90, "offset": 4, "score1": 5, "score2": 0 }
          ],
          "goals2": [],
          "group": "Group A",
          "stadium": { "key": "luzhniki", "name": "Luzhniki Stadium" },
          "city": "Moscow",
          "timezone": "UTC+3"
        }
      ]
    },
    {
      "name": "Matchday 2",
      "matches": [
        {
          "num": 2,
          "date": "2018-06-15",
          "time": "17:00",
          "team1": { "name": "Egypt",   "code": "EGY" },
          "team2": { "name": "Uruguay", "code": "URU" },
          "score1":  0,
          "score2":  1,
          "score1i": 0,
          "score2i": 0,
          "goals1": [],
          "goals2": [
            { "name": "Giménez",  "minute": 89,  "score1": 0, "score2": 1 }
          ],
          "group": "Group A",
          "stadium": { "key": "ekaterinburg", "name": "Ekaterinburg Arena" },          
          "city": "Ekaterinburg",
          "timezone": "UTC+5"
        },
        ...
      ],
    },
  ],  
}
```

As noted previously, `world_cup_data` is a dictionary with two keys, 'name' and 'rounds'.


```python
world_cup_data.keys()
```




    dict_keys(['name', 'rounds'])



The value associated with the 'name' key is simply identifying the dataset.


```python
world_cup_data["name"]
```




    'World Cup 2018'



### Extracting Rounds

The value associated with the 'rounds' key is a list containing all of the actual information about the rounds and the matches within those rounds.


```python

rounds = world_cup_data["rounds"]

print("type(rounds):", type(rounds))
print("len(rounds):", len(rounds))
print("rounds[3]:")
rounds[3]
```

    type(rounds): <class 'list'>
    len(rounds): 20
    rounds[3]:





    {'name': 'Matchday 4',
     'matches': [{'num': 9,
       'date': '2018-06-17',
       'time': '21:00',
       'team1': {'name': 'Brazil', 'code': 'BRA'},
       'team2': {'name': 'Switzerland', 'code': 'SUI'},
       'score1': 1,
       'score2': 1,
       'score1i': 1,
       'score2i': 0,
       'goals1': [{'name': 'Coutinho', 'minute': 20, 'score1': 1, 'score2': 0}],
       'goals2': [{'name': 'Zuber', 'minute': 50, 'score1': 1, 'score2': 1}],
       'group': 'Group E',
       'stadium': {'key': 'rostov', 'name': 'Rostov Arena'},
       'city': 'Rostov-on-Don',
       'timezone': 'UTC+3'},
      {'num': 10,
       'date': '2018-06-17',
       'time': '16:00',
       'team1': {'name': 'Costa Rica', 'code': 'CRC'},
       'team2': {'name': 'Serbia', 'code': 'SRB'},
       'score1': 0,
       'score2': 1,
       'score1i': 0,
       'score2i': 0,
       'goals1': [],
       'goals2': [{'name': 'Kolarov', 'minute': 56, 'score1': 0, 'score2': 1}],
       'group': 'Group E',
       'stadium': {'key': 'samara', 'name': 'Samara Arena'},
       'city': 'Samara',
       'timezone': 'UTC+4'},
      {'num': 11,
       'date': '2018-06-17',
       'time': '18:00',
       'team1': {'name': 'Germany', 'code': 'GER'},
       'team2': {'name': 'Mexico', 'code': 'MEX'},
       'score1': 0,
       'score2': 1,
       'score1i': 0,
       'score2i': 1,
       'goals1': [],
       'goals2': [{'name': 'Lozano', 'minute': 35, 'score1': 0, 'score2': 1}],
       'group': 'Group F',
       'stadium': {'key': 'luzhniki', 'name': 'Luzhniki Stadium'},
       'city': 'Moscow',
       'timezone': 'UTC+3'}]}



Translating this output into English:

Starting with the original `world_cup_data` dictionary, we used the key `"rounds"` to extract a list of rounds, which we assigned to the variable `rounds`.

`rounds` is a list of dictionaries. Each dictionary inside of `rounds` contains a name (e.g. `"Matchday 4"`) as well as a list of matches.

### Extracting Matches

Now we can go one level deeper and extract all of the matches in the tournament. Because the round is irrelevant for this analysis, we can loop over all rounds and combine all of their matches into a single list.

**Hint:** This is a good use case for using the `.extend` list method rather than `.append`, since we want to combine several lists of dictionaries into a single list of dictionaries, not a list of lists of dictionaries. [Documentation here.](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)


```python
matches = []

# "round" is a built-in function in Python so we use "round_" instead
for round_ in rounds:
    # Extract the list of matches for this round
    round_matches = round_["matches"]
    # Add them to the overall list of matches
    matches.extend(round_matches)
    
matches[0]
```




    {'num': 1,
     'date': '2018-06-14',
     'time': '18:00',
     'team1': {'name': 'Russia', 'code': 'RUS'},
     'team2': {'name': 'Saudi Arabia', 'code': 'KSA'},
     'score1': 5,
     'score2': 0,
     'score1i': 2,
     'score2i': 0,
     'goals1': [{'name': 'Gazinsky', 'minute': 12, 'score1': 1, 'score2': 0},
      {'name': 'Cheryshev', 'minute': 43, 'score1': 2, 'score2': 0},
      {'name': 'Dzyuba', 'minute': 71, 'score1': 3, 'score2': 0},
      {'name': 'Cheryshev', 'minute': 90, 'offset': 1, 'score1': 4, 'score2': 0},
      {'name': 'Golovin', 'minute': 90, 'offset': 4, 'score1': 5, 'score2': 0}],
     'goals2': [],
     'group': 'Group A',
     'stadium': {'key': 'luzhniki', 'name': 'Luzhniki Stadium'},
     'city': 'Moscow',
     'timezone': 'UTC+3'}



Make sure the `assert`s pass before moving on to the next step.


```python

# There should be 64 matches. If the length is 20, that means
# you have a list of lists instead of a list of dictionaries
assert len(matches) == 64

# Each match in the list should be a dictionary
assert type(matches[0]) == dict
```

### Extracting Teams

Each match has a `team1` and a `team2`. 


```python
print(matches[0]["team1"])
print(matches[0]["team2"])
```

    {'name': 'Russia', 'code': 'RUS'}
    {'name': 'Saudi Arabia', 'code': 'KSA'}


Create a list of all unique team names by looping over every match in `matches` and adding the `"name"` values associated with both `team1` and `team2`. (Same as before when creating a list of matches, it doesn't matter right now whether a given team was "team1" or "team2", we just add everything to `teams`.)

We'll use a `set` data type ([documentation here](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)) to ensure unique teams, then convert it to a sorted list at the end.


```python
teams_set = set()

for match in matches:
    # Add team1 value to teams_set
    teams_set.add(match["team1"]["name"])
    # Add team2 value to teams_set
    teams_set.add(match["team2"]["name"])
    
teams = sorted(list(teams_set))
print(teams)
```

    ['Argentina', 'Australia', 'Belgium', 'Brazil', 'Colombia', 'Costa Rica', 'Croatia', 'Denmark', 'Egypt', 'England', 'France', 'Germany', 'Iceland', 'Iran', 'Japan', 'Mexico', 'Morocco', 'Nigeria', 'Panama', 'Peru', 'Poland', 'Portugal', 'Russia', 'Saudi Arabia', 'Senegal', 'Serbia', 'South Korea', 'Spain', 'Sweden', 'Switzerland', 'Tunisia', 'Uruguay']


Make sure the `assert`s pass before moving on to the next step.


```python

# teams should be a list, not a set
assert type(teams) == list

# 32 teams competed in the 2018 World Cup
assert len(teams) == 32

# Each element of teams should be a string
# (the name), not a dictionary
assert type(teams[0]) == str
```

Great, step 1 complete! We have unique identifiers (names) for each of our records (countries) that we will be able to use to connect 2018 World Cup performance to 2018 population.

## 2. Associating Countries with 2018 World Cup Performance

> Create a data structure that connects a team name (country name) to its performance in the 2018 FIFA World Cup. We'll use the count of games won in the entire tournament (group stage as well as knockout stage) to represent the performance.

> Also, create visualizations to help the reader understand the distribution of games won and the performance of each team.

So, we are building a **data structure** that connects a country name to the number of wins. There is no universal correct format for a data structure with this purpose, but we are going to use a format that resembles the "dataframe" format that will be introduced later in the course.

Specifically, we'll build a **dictionary** where each key is the name of a country, and each value is a nested dictionary containing information about the number of wins and the 2018 population.

The final result will look something like this:
```
{
  'Argentina': { 'wins': 1, 'population': 44494502 },
  ...
  'Uruguay':   { 'wins': 4, 'population': 3449299  }
}
```

For the current step (step 2), we'll build a data structure that looks something like this:
```
{
  'Argentina': { 'wins': 1 },
  ...
  'Uruguay':   { 'wins': 4 }
}
```

### Initializing with Wins Set to Zero

Start by initializing a dictionary called `combined_data` containing:

* Keys: the strings from `teams`
* Values: each value the same, a dictionary containing the key `'wins'` with the associated value `0`. However, note that each value should be a distinct dictionary object in memory, not the same dictionary linked as a value in multiple places.

Initially `combined_data` will look something like this:
```
{
  'Argentina': { 'wins': 0 },
  ...
  'Uruguay':   { 'wins': 0 }
}
```


```python
# There are a couple different ways you could do this

# Basic:
# (Make an empty dictionary, loop over all of the
# teams, adding the team as the key and the appropriate
# dictionary as the value)
combined_data = {}
for team in teams:
    combined_data[team] = {"wins": 0}
    
# Dictionary comprehension:
# (This is preferable once you're familiar with the
# syntax, but don't worry too much about using this
# vs. the previous answer)
combined_data = {team: {"wins": 0} for team in teams}

# Note that this from_keys technique is incorrect, since it
# will have the same dict as the value for every country,
# i.e. if you add 1 to Argentina's wins, it also adds 1
# to every other country's wins
# combined_data = dict.fromkeys(teams, dict([("wins", 0)]))
```

Check that the `assert`s pass.


```python

# combined_data should be a dictionary
assert type(combined_data) == dict

# the keys should be strings
assert type(list(combined_data.keys())[0]) == str

# the values should be dictionaries
assert combined_data["Japan"] == {"wins": 0}
```

### Adding Wins from Matches

Now it's time to revisit the `matches` list from earlier, in order to associate a team with the number of times it has won a match.

This time, let's write some functions to help organize our logic.

Write a function `find_winner` that takes in a `match` dictionary, and returns the name of the team that won the match.  Recall that a match is structured like this:

```
{
  'num': 1,
  'date': '2018-06-14',
  'time': '18:00',
  'team1': { 'name': 'Russia',       'code': 'RUS' },
  'team2': { 'name': 'Saudi Arabia', 'code': 'KSA' },
  'score1': 5,
  'score2': 0,
  'score1i': 2,
  'score2i': 0,
  'goals1': [
    { 'name': 'Gazinsky',  'minute': 12, 'score1': 1, 'score2': 0 },
    { 'name': 'Cheryshev', 'minute': 43, 'score1': 2, 'score2': 0 },
    { 'name': 'Dzyuba',    'minute': 71, 'score1': 3, 'score2': 0 },
    { 'name': 'Cheryshev', 'minute': 90, 'offset': 1, 'score1': 4, 'score2': 0 },
    { 'name': 'Golovin',   'minute': 90, 'offset': 4, 'score1': 5, 'score2': 0 }
  ],
  'goals2': [],
  'group': 'Group A',
  'stadium': { 'key': 'luzhniki', 'name': 'Luzhniki Stadium' },
  'city': 'Moscow',
  'timezone': 'UTC+3'
}
```

The winner is determined by comparing the values associated with the `'score1'` and `'score2'` keys. If score 1 is larger, then the name associated with the `'team1'` key is the winner. If score 2 is larger, then the name associated with the `'team2'` key is the winner. If the values are the same, there is no winner, so return `None`. (Unlike the group round of the World Cup, we are only counting *wins* as our "performance" construct, not 3 points for a win and 1 point for a tie.)


```python

def find_winner(match):
    """
    Given a dictionary containing information about a match,
    return the name of the winner (or None in the case of a tie)
    """
    score_1 = match["score1"]
    score_2 = match["score2"]
    
    if score_1 > score_2:
        return match["team1"]["name"]
    elif score_2 > score_1:
        return match["team2"]["name"]
    else:
        return None
    # ^ this else: return None is not actually necessary since
    # Python returns None if nothing is returned
```


```python
assert find_winner(matches[0]) == "Russia"
assert find_winner(matches[1]) == "Uruguay"
assert find_winner(matches[2]) == None
```

Now that we have this helper function, loop over every match in `matches`, find the winner, and add 1 to the associated count of wins in `combined_data`. If the winner is `None`, skip adding it to the dictionary.


```python

for match in matches:
    # Get the name of the winner
    winner = find_winner(match)
    # Only proceed to the next step if there was
    # a winner
    if winner:
        # Add 1 to the associated count of wins
        combined_data[winner]["wins"] += 1
        
# Visually inspect the output to ensure the wins are
# different for different countries
combined_data
```




    {'Argentina': {'wins': 1},
     'Australia': {'wins': 0},
     'Belgium': {'wins': 6},
     'Brazil': {'wins': 3},
     'Colombia': {'wins': 2},
     'Costa Rica': {'wins': 0},
     'Croatia': {'wins': 3},
     'Denmark': {'wins': 1},
     'Egypt': {'wins': 0},
     'England': {'wins': 3},
     'France': {'wins': 6},
     'Germany': {'wins': 1},
     'Iceland': {'wins': 0},
     'Iran': {'wins': 1},
     'Japan': {'wins': 1},
     'Mexico': {'wins': 2},
     'Morocco': {'wins': 0},
     'Nigeria': {'wins': 1},
     'Panama': {'wins': 0},
     'Peru': {'wins': 1},
     'Poland': {'wins': 1},
     'Portugal': {'wins': 1},
     'Russia': {'wins': 2},
     'Saudi Arabia': {'wins': 1},
     'Senegal': {'wins': 1},
     'Serbia': {'wins': 1},
     'South Korea': {'wins': 1},
     'Spain': {'wins': 1},
     'Sweden': {'wins': 3},
     'Switzerland': {'wins': 1},
     'Tunisia': {'wins': 1},
     'Uruguay': {'wins': 4}}



### Analysis of Wins

While we could try to understand all 32 of those numbers just by scanning through them, let's use some descriptive statistics and data visualizations instead!

#### Statistical Summary of Wins

The code below calculates the mean, median, and standard deviation of the number of wins. If it doesn't work, that is an indication that something went wrong with the creation of the `combined_data` variable, and you might want to look at the solution branch and fix your code before proceeding.


```python
import numpy as np

wins = [val["wins"] for val in combined_data.values()]

print("Mean number of wins:", np.mean(wins))
print("Median number of wins:", np.median(wins))
print("Standard deviation of number of wins:", np.std(wins))
```

    Mean number of wins: 1.5625
    Median number of wins: 1.0
    Standard deviation of number of wins: 1.5194057226429023


#### Visualizations of Wins

In addition to those numbers, let's make a histogram (showing the distributions of the number of wins) and a bar graph (showing the number of wins by country).


```python
import matplotlib.pyplot as plt

# Set up figure and axes
fig, (ax1, ax2) = plt.subplots(ncols=2, figsize=(12, 7))
fig.set_tight_layout(True)

# Histogram of Wins and Frequencies
ax1.hist(x=wins, bins=range(8), align="left", color="green")
ax1.set_xticks(range(7))
ax1.set_xlabel("Wins in 2018 World Cup")
ax1.set_ylabel("Frequency")
ax1.set_title("Distribution of Wins")

# Horizontal Bar Graph of Wins by Country
ax2.barh(teams[::-1], wins[::-1], color="green")
ax2.set_xlabel("Wins in 2018 World Cup")
ax2.set_title("Wins by Country");
```


![png](index_files/index_47_0.png)


#### Interpretation of Win Analysis

Before we move to looking at the relationship between wins and population, it's useful to understand the distribution of wins alone. A few notes of interpretation:

* The number of wins is skewed and looks like a [negative binomial distribution](https://en.wikipedia.org/wiki/Negative_binomial_distribution), which makes sense conceptually
* The "typical" value here is 1 (both the median and the highest point of the histogram), meaning a typical team that qualifies for the World Cup wins once
* There are a few teams we might consider outliers: Belgium and France, with 6x the wins of the "typical" team and 1.5x the wins of the next "runner-up" (Uruguay, with 4 wins)
* This is a fairly small dataset, something that becomes more noticeable with such a "spiky" (not smooth) histogram


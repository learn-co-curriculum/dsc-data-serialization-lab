# Data Serialization Formats - Cumulative Lab

## Introduction

Now that you have learned about CSV and JSON file formats individually, it's time to bring them together with a cumulative lab! Even as a junior data scientist, you can often produce novel, interesting analyses by combining multiple datasets that haven't been combined before.

## Objectives

You will be able to:

* Practice reading serialized JSON and CSV data from files into Python objects
* Practice extracting information from nested data structures
* Practice cleaning data (filtering, normalizing locations, converting types)
* Combine data from multiple sources into a single data structure
* Interpret descriptive statistics and data visualizations to present your findings

## Your Task: Analyze the Relationship between Population and World Cup Performance

![Russia 2018 branded soccer ball and trophy](images/world_cup.jpg)

<span>Photo by <a href="https://unsplash.com/@fznsr_?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Fauzan Saari</a> on <a href="https://unsplash.com/s/photos/soccer-world-cup?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>

### Business Understanding

#### What is the relationship between the population of a country and their performance in the 2018 FIFA World Cup?

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

Remember, our business question is:

> What is the relationship between the population of a country and their performance in the 2018 FIFA World Cup?

#### Unit of Analysis

First, what is our **unit of analysis**, and what is the **unique identifier**? In other words, what will one record in our final data structure represent, and what attribute uniquely describes it?

.

.

.

*Answer:* 

> What is the relationship between the population of a **country** and their performance in the 2018 FIFA World Cup?

*Our unit of analysis is a* ***country*** *and the unique identifier we'll use is the* ***country name***

#### Features

Next, what **features** are we analyzing? In other words, what attributes of each country are we interested in?

.

.

.

*Answer:* 

> What is the relationship between the **population** of a country and their **performance in the 2018 FIFA World Cup**?

*Our features are* ***2018 population*** *and* ***count of wins in the 2018 World Cup***

#### Dataset to Start With

Finally, which dataset should we **start** with? In this case, any record with missing data is not useful to us, so we want to start with the smaller dataset.

.

.

.

*Answer: There are only 32 countries that compete in the World Cup each year, compared to hundreds of countries in the world, so we should start with the* ***2018 World Cup*** *dataset. Then we can join it with the relevant records from the country population dataset.*

## Getting the Data

Below we import the `json` and `csv` modules, which will be used for reading from `world_cup_2018.json` and `country_populations.csv`, respectively.


```python
# Run this cell without changes
import json
import csv
```

Next, we open the relevant files.


```python
# Run this cell without changes
world_cup_file = open("data/world_cup_2018.json", encoding='utf8')
population_file = open("data/country_populations.csv")
```

**Hint:** if your code below is not working, (e.g. `ValueError: I/O operation on closed file.`, or you get an empty list or dictionary) try re-running the cell above to reopen the files, then re-run your code.

### 2018 World Cup Data

In the cell below, use the `json` module to load the data from `world_cup_file` into a dictionary called `world_cup_data`


```python
# Replace None with appropriate code
world_cup_data = None

# Close the file now that we're done reading from it
world_cup_file.close()
```

Make sure the `assert` passes, ensuring that `world_cup_data` has the correct type.


```python
# Run this cell without changes

# Check that the overall data structure is a dictionary
assert type(world_cup_data) == dict

# Check that the dictionary has 2 keys, 'name' and 'rounds'
assert list(world_cup_data.keys()) == ['name', 'rounds']
```

### Population Data

Now use the `csv` module to load the data from `population_file` into a list of dictionaries called `population_data`

(Recall that you can convert a `csv.DictReader` object into a list of dictionaries using the built-in `list()` function.)


```python
# Replace None with appropriate code
population_data = None

# Close the file now that we're done reading from it
population_file.close()
```

Make sure the `assert`s pass, ensuring that `population_data` has the correct type.


```python
# Run this cell without changes

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

### Exploring the Structure of the World Cup Data JSON

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
# Run this cell without changes
world_cup_data.keys()
```

The value associated with the 'name' key is simply identifying the dataset.


```python
# Run this cell without changes
world_cup_data["name"]
```

### Extracting Rounds

The value associated with the 'rounds' key is a list containing all of the actual information about the rounds and the matches within those rounds.


```python
# Run this cell without changes
rounds = world_cup_data["rounds"]

print("type(rounds):", type(rounds))
print("len(rounds):", len(rounds))
print("type(rounds[3])", type(rounds[3]))
print("rounds[3]:")
rounds[3]
```

Translating this output into English:

Starting with the original `world_cup_data` dictionary, we used the key `"rounds"` to extract a list of rounds, which we assigned to the variable `rounds`.

`rounds` is a list of dictionaries. Each dictionary inside of `rounds` contains a name (e.g. `"Matchday 4"`) as well as a list of matches.

### Extracting Matches

Now we can go one level deeper and extract all of the matches in the tournament. Because the round is irrelevant for this analysis, we can loop over all rounds and combine all of their matches into a single list.

**Hint:** This is a good use case for using the `.extend` list method rather than `.append`, since we want to combine several lists of dictionaries into a single list of dictionaries, not a list of lists of dictionaries. [Documentation here.](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)


```python
# Replace None with appropriate code
matches = []

# "round" is a built-in function in Python so we use "round_" instead
for round_ in rounds:
    # Extract the list of matches for this round
    round_matches = None
    # Add them to the overall list of matches
    None
    
matches[0]
```

Make sure the `assert`s pass before moving on to the next step.


```python
# Run this cell without changes

# There should be 64 matches. If the length is 20, that means
# you have a list of lists instead of a list of dictionaries
assert len(matches) == 64

# Each match in the list should be a dictionary
assert type(matches[0]) == dict
```

### Extracting Teams

Each match has a `team1` and a `team2`. 


```python
# Run this cell without changes
print(matches[0]["team1"])
print(matches[0]["team2"])
```

Create a list of all unique team names by looping over every match in `matches` and adding the `"name"` values associated with both `team1` and `team2`. (Same as before when creating a list of matches, it doesn't matter right now whether a given team was "team1" or "team2", we just add everything to `teams`.)

We'll use a `set` data type ([documentation here](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)) to ensure unique teams, then convert it to a sorted list at the end.


```python
# Replace None with appropriate code
teams_set = set()

for match in matches:
    # Add team1 name value to teams_set
    None
    # Add team2 name value to teams_set
    None
    
teams = sorted(list(teams_set))
print(teams)
```

Make sure the `assert`s pass before moving on to the next step.


```python
# Run this cell without changes

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
# Replace None with appropriate code

# Create the variable combined_data as described above
None
```

Check that the `assert`s pass.


```python
# Run this cell without changes

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
# Replace None with appropriate code

def find_winner(match):
    """
    Given a dictionary containing information about a match,
    return the name of the winner (or None in the case of a tie)
    """
    None
```


```python
# Run this cell without changes
assert find_winner(matches[0]) == "Russia"
assert find_winner(matches[1]) == "Uruguay"
assert find_winner(matches[2]) == None
```

Now that we have this helper function, loop over every match in `matches`, find the winner, and add 1 to the associated count of wins in `combined_data`. If the winner is `None`, skip adding it to the dictionary.


```python
# Replace None with appropriate code

for match in matches:
    # Get the name of the winner
    winner = None
    # Only proceed to the next step if there was
    # a winner
    if winner:
        # Add 1 to the associated count of wins
        None
        
# Visually inspect the output to ensure the wins are
# different for different countries
combined_data
```

### Analysis of Wins

While we could try to understand all 32 of those numbers just by scanning through them, let's use some descriptive statistics and data visualizations instead!

#### Statistical Summary of Wins

The code below calculates the mean, median, and standard deviation of the number of wins. If it doesn't work, that is an indication that something went wrong with the creation of the `combined_data` variable, and you might want to look at the solution branch and fix your code before proceeding.


```python
# Run this cell without changes
import numpy as np

wins = [val["wins"] for val in combined_data.values()]

print("Mean number of wins:", np.mean(wins))
print("Median number of wins:", np.median(wins))
print("Standard deviation of number of wins:", np.std(wins))
```

#### Visualizations of Wins

In addition to those numbers, let's make a histogram (showing the distributions of the number of wins) and a bar graph (showing the number of wins by country).


```python
# Run this cell without changes
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

#### Interpretation of Win Analysis

Before we move to looking at the relationship between wins and population, it's useful to understand the distribution of wins alone. A few notes of interpretation:

* The number of wins is skewed and looks like a [negative binomial distribution](https://en.wikipedia.org/wiki/Negative_binomial_distribution), which makes sense conceptually
* The "typical" value here is 1 (both the median and the highest point of the histogram), meaning a typical team that qualifies for the World Cup wins once
* There are a few teams we might consider outliers: Belgium and France, with 6x the wins of the "typical" team and 1.5x the wins of the next "runner-up" (Uruguay, with 4 wins)
* This is a fairly small dataset, something that becomes more noticeable with such a "spiky" (not smooth) histogram


## 3. Associating Countries with 2018 Population

> Add to the existing data structure so that it also connects each country name to its 2018 population, and create visualizations comparable to those from step 2.

Now we're ready to add the 2018 population to `combined_data`, finally using the CSV file!

Recall that `combined_data` currently looks something like this:
```
{
  'Argentina': { 'wins': 1 },
  ...
  'Uruguay':   { 'wins': 4 }
}
```

And the goal is for it to look something like this:
```
{
  'Argentina': { 'wins': 1, 'population': 44494502 },
  ...
  'Uruguay':   { 'wins': 4, 'population': 3449299  }
}
```

To do that, we need to extract the 2018 population information from the CSV data.

### Exploring the Structure of the Population Data CSV

Recall that previously we loaded information from a CSV containing population data into a list of dictionaries called `population_data`.


```python
# Run this cell without changes
len(population_data)
```

12,695 is a very large number of rows to print out, so let's look at some samples instead.


```python
# Run this cell without changes
np.random.seed(42)
population_record_samples = np.random.choice(population_data, size=10)
population_record_samples
```

There are **2 filtering tasks**, **1 data normalization task**, and **1 type conversion task** to be completed, based on what we can see in this sample. We'll walk through each of them below.

(In a more realistic data cleaning environment, you most likely won't happen to get a sample that demonstrates all of the data cleaning steps needed, but this sample was chosen carefully for example purposes.)

### Filtering Population Data

We already should have suspected that this dataset would require some filtering, since there are 32 records in our current `combined_data` dataset and 12,695 records in `population_data`. Now that we have looked at this sample, we can identify 2 features we'll want to use in order to filter down the `population_data` records to just 32. Try to identify them before looking at the answer below.

.

.

.

*Answer: the two features to filter on are* ***`'Country Name'`*** *and* ***`'Year'`***. *We can see from the sample above that there are countries in `population_data` that are not present in `combined_data` (e.g. Malta) and there are years present that are not 2018.*

In the cell below, create a new variable `population_data_filtered` that only includes relevant records from `population_data`. Relevant records are records where the country name is one of the countries in the `teams` list, and the year is "2018".

(It's okay to leave 2018 as a string since we are not performing any math operations on it, just make sure you check for `"2018"` and not `2018`.)


```python
# Replace None with appropriate code

population_data_filtered = []

for record in population_data:
    # Add record to population_data_filtered if relevant
    None
    
len(population_data_filtered) # 27
```

Hmm...what went wrong? Why do we only have 27 records, and not 32?

Did we really get a dataset with 12k records that's missing 5 of the data points we need?

Let's take a closer look at the population data samples again, specifically the third one:


```python
# Run this cell without changes
population_record_samples[2]
```

And compare that with the value for Iran in `teams`:


```python
# Run this cell without changes
teams[13]
```

Ohhhh...we have a data normalization issue! One dataset refers to this country as `'Iran, Islamic Rep.'`, while the other refers to it as `'Iran'`. This is a common issue we face when using data about countries and regions, where there is no universally-accepted naming convention.

### Normalizing Locations in Population Data

Sometimes data normalization can be a very, very time-consuming task where you need to find "crosswalk" data that can link the two formats together, or you need to write advanced regex formulas to line everything up.

For this task, there are only 5 missing, so we'll just go ahead and give you a function that makes the appropriate substitutions.


```python
# Run this cell without changes
def normalize_location(country_name):
    """
    Given a country name, return the name that the
    country uses when playing in the FIFA World Cup
    """
    name_sub_dict = {
        "Russian Federation": "Russia",
        "Egypt, Arab Rep.": "Egypt",
        "Iran, Islamic Rep.": "Iran",
        "Korea, Rep.": "South Korea",
        "United Kingdom": "England"
    }
    # The .get method returns the corresponding value from
    # the dict if present, otherwise returns country_name
    return name_sub_dict.get(country_name, country_name)

# Example where normalized location is different
print(normalize_location("Russian Federation"))
# Example where normalized location is the same
print(normalize_location("Argentina"))
```

Now, write new code to create `population_data_filtered` with normalized country names.


```python
# Replace None with appropriate code

population_data_filtered = []

for record in population_data:
    # Get normalized country name
    None
    # Add record to population_data_filtered if relevant
    if None:
        # Replace the country name in the record
        None
        # Append to list
        None
        
len(population_data_filtered) # 32
```

Great, now we should have 32 records instead of 27!

### Type Conversion of Population Data

We need to do one more thing before we'll have population data that is usable for analysis. Take a look at this record from `population_data_filtered` to see if you can spot it:


```python
# Run this cell without changes
population_data_filtered[0]
```

Every key has the same data type (`str`), including the population value. In this example, it's `'44494502'`, when it needs to be `44494502` if we want to be able to compute statistics with it.

In the cell below, loop over `population_data_filtered` and convert the data type of the value associated with the `"Value"` key from a string to an integer, using the built-in `int()` function.


```python
# Replace None with appropriate code
for record in population_data_filtered:
    # Convert the population value from str to int
    None
    
# Look at the last record to make sure the population
# value is an int
population_data_filtered[-1]
```

Check that it worked with the assert statement below:


```python
# Run this cell without changes
assert type(population_data_filtered[-1]["Value"]) == int
```

### Adding Population Data

Now it's time to add the population data to `combined_data`! Recall that the data structure currently looks like this:


```python
# Run this cell without changes
combined_data
```

The goal is for it to be structured like this:
```
{
  'Argentina': { 'wins': 1, 'population': 44494502 },
  ...
  'Uruguay':   { 'wins': 4, 'population': 3449299  }
}
```

In the cell below, loop over `population_data_filtered` and add information about population to each country in `combined_data`:


```python
# Replace None with appropriate code
for record in population_data_filtered:
    # Extract the country name from the record
    country = None
    # Extract the population value from the record
    population = None
    # Add this information to combined_data
    None
    
# Look combined_data
combined_data
```

Check that the types are correct with these assert statements:


```python
# Run this cell without changes
assert type(combined_data["Uruguay"]) == dict
assert type(combined_data["Uruguay"]["population"]) == int
```

### Analysis of Population

Let's perform the same analysis for population that we performed for count of wins.

#### Statistical Analysis of Population


```python
# Run this cell without changes
populations = [val["population"] for val in combined_data.values()]

print("Mean population:", np.mean(populations))
print("Median population:", np.median(populations))
print("Standard deviation of population:", np.std(populations))
```

#### Visualizations of Population


```python
# Run this cell without changes

# Set up figure and axes
fig, (ax1, ax2) = plt.subplots(ncols=2, figsize=(12, 7))
fig.set_tight_layout(True)

# Histogram of Populations and Frequencies
ax1.hist(x=populations, color="blue")
ax1.set_xlabel("2018 Population")
ax1.set_ylabel("Frequency")
ax1.set_title("Distribution of Population")

# Horizontal Bar Graph of Population by Country
ax2.barh(teams[::-1], populations[::-1], color="blue")
ax2.set_xlabel("2018 Population")
ax2.set_title("Population by Country");
```

#### Interpretation of Population Analysis

* Similar to the distribution of the number of wins, the distribution of population is skewed.
* It's hard to choose a single "typical" value here because there is so much variation.
* The countries with the largest populations (Brazil, Nigeria, and Russia) do not overlap with the countries with the most wins (Belgium, France, and Uruguay)

## 4. Analysis of Population vs. Performance

> Choose an appropriate statistical measure to analyze the relationship between population and performance, and create a visualization representing this relationship.

### Statistical Measure
So far we have learned about only two statistics for understanding the *relationship* between variables: **covariance** and **correlation**. We will use correlation here, because that provides a more standardized, interpretable metric.


```python
# Run this cell without changes
np.corrcoef(wins, populations)[0][1]
```

In the cell below, interpret this number. What direction is this correlation? Is it strong or weak?


```python
# Replace None with appropriate code
"""
None
"""
```

### Data Visualization

A **scatter plot** is he most sensible form of data visualization for showing this relationship, because we have two dimensions of data, but there is no "increasing" variable (e.g. time) that would indicate we should use a line graph.


```python
# Run this cell without changes

# Set up figure
fig, ax = plt.subplots(figsize=(8, 5))

# Basic scatter plot
ax.scatter(
    x=populations,
    y=wins,
    color="gray", alpha=0.5, s=100
)
ax.set_xlabel("2018 Population")
ax.set_ylabel("2018 World Cup Wins")
ax.set_title("Population vs. World Cup Wins")

# Add annotations for specific points of interest
highlighted_points = {
    "Belgium": 2, # Numbers are the index of that
    "Brazil": 3,  # country in populations & wins
    "France": 10,
    "Nigeria": 17
}
for country, index in highlighted_points.items():
    # Get x and y position of data point
    x = populations[index]
    y = wins[index]
    # Move each point slightly down and to the left
    # (numbers were chosen by manually tweaking)
    xtext = x - (1.25e6 * len(country))
    ytext = y - 0.5
    # Annotate with relevant arguments
    ax.annotate(
        text=country,
        xy=(x, y),
        xytext=(xtext, ytext)
    )
```

### Data Visualization Interpretation

Interpret this plot in the cell below. Does this align with the findings from the statistical measure (correlation), as well as the map shown at the beginning of this lab (showing the best results by country)?


```python
# Replace None with appropriate text
"""
None
"""
```

### Final Analysis

> What is the relationship between the population of a country and their performance in the 2018 FIFA World Cup?

Overall, we found a very weakly positive relationship between the population of a country and their performance in the 2018 FIFA World Cup, as demonstrated by both the correlation between populations and wins, and the scatter plot.

In the cell below, write down your thoughts on these questions:

 - What are your thoughts on why you may see this result?
 - What would you research next?


```python
# Replace None with appropriate text
"""
None
"""
```

## Summary

Congratulations! That was a long lab, pulling together a lot of material. You read data into Python, extracted the relevant information, cleaned the data, and combined the data into a new format to be used in analysis. While we will continue to introduce new tools and techniques, these essential steps will be present for the rest of your data science projects from here on out!

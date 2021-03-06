---
layout: post
title: My First Post
subtitle: Can we determine the strongest player out of 20 in 10 randomized tug of war matches?
tags: [test]
---

## One of my favorite math youtubers, 3Blue1Brown, posted a challenge problem from "The Art of Problem Solving" at the end of one of his videos:

**"AoPS must choose one of 20 people to send to a tug-of-war tournament. We don't care who we send, as long as we don't send our weakest person.**

**Each Person has a different strength, but we don't know those strengths. We get 10 intramural 10-on-10 matches to determine who we send.
Can we make sure we don't send our weakest person?"**

The solution to this problem can be found [here](https://artofproblemsolving.com/3b1b).

I misunderstood the problem, and instead tried to solve it assuming the games were made at random. After making an effort to solve a much more difficult version of the problem, I realized the mistake but decided to keep trying to solve my version of the problem for fun.









# Can we determine the strongest player out of 20 in 10 randomized tug of war matches?
**I shall attempt to solve this by creating a simulation and analyzing the results:**

### I will be using Python 3 and the following libraries:
```python
import random
from random import shuffle
import pandas as pd
```

### This function creates a Roster of 20 Players, all with a random strength value between 0 and 1:

```python
def create_roster():
  players = {}
  keys = range(20)
  values = []
  for i in range(20):
    values.append(random.uniform(0,1))

  for i in keys:
    players[i] = values[i]

  return(players)
```

### This function breaks the roster into two random teams and simulates one tug of war game between them. It returns a table showing the players on each team and the winner of the match:

**It accepts a roster from the above function as an argument**
```python
def sim_game(roster):
  tuple_roster = list(roster.items())
  random.shuffle(tuple_roster)
  shuffled_roster = tuple_roster


  team1 = []
  team2 = []


  for i in range(10):
    team1.append(shuffled_roster[i])
  for i in range(10,20):
    team2.append(shuffled_roster[i])
  
  
  team1_score = sum(n for _, n in team1)
  team2_score = sum(n for _, n in team2)

  if team1_score > team2_score:
    winner = "Team 1"
  else:
    winner = "Team 2"
  
  team1_members = [i[0] for i in team1]
  team2_members = [i[0] for i in team2]
  
  d = {'Team': [], 'Member 1': [],'Member 2': [],'Member 3': [],
      'Member 4': [],'Member 5': [],'Member 6': [],'Member 7': [],
      'Member 8': [],'Member 9': [],'Member 0': [],'Won':[]}
  df = pd.DataFrame(data=d)
  df = df.append(pd.DataFrame([team1_members,team2_members],
       columns=['Member 1','Member 2','Member 3','Member 4',
       'Member 5','Member 6','Member 7','Member 8','Member 9','Member 0',]))
  df.Team[0] = 1
  df.Team[1] = 2
  if team1_score > team2_score:
    df.Won[0] = 1
    df.Won[1] = 0
  else:
    df.Won[0] = 0
    df.Won[1] = 1
  return df

```

### This function creates a roster, simulates 10 games and outputs the result as a dataframe

```python
def sim_ten_games():
  roster = create_roster()
  game1 = sim_game(roster)
  for i in range(9):
    game2 = sim_game(roster)
    frames = [game1,game2]
    game1 = pd.concat(frames)
    
  return game1
```


### An Example DataFrame showing 20 games between the players arranged into a random team for each game
***A single game is described by two seperate rows, the rows of two starting from the top, each row shows the numbers of the players on that team. 0 means a team lost that game, 1 means a team won that game.***

Member 0|Member 1|Member 2|Member 3|Member 4|Member 5|Member 6|Member 7|Member 8|Member 9|Team|Won
---|---|---|---|---|---|---|---|---|---|---|---
4.0|19.0|16.0|2.0|10.0|14.0|8.0|7.0|11.0|1.0|1.0|1.0
5.0|17.0|3.0|6.0|12.0|15.0|9.0|13.0|0.0|18.0|2.0|0.0
0.0|10.0|4.0|16.0|6.0|8.0|9.0|18.0|14.0|17.0|1.0|1.0
2.0|19.0|3.0|13.0|12.0|1.0|7.0|11.0|15.0|5.0|2.0|0.0
15.0|3.0|8.0|12.0|13.0|4.0|7.0|6.0|5.0|19.0|1.0|0.0
0.0|2.0|1.0|11.0|16.0|17.0|18.0|14.0|10.0|9.0|2.0|1.0
15.0|0.0|4.0|19.0|2.0|9.0|6.0|3.0|12.0|11.0|1.0|0.0
18.0|7.0|17.0|10.0|16.0|5.0|8.0|14.0|1.0|13.0|2.0|1.0
9.0|17.0|12.0|2.0|19.0|1.0|4.0|3.0|16.0|0.0|1.0|0.0
8.0|7.0|15.0|11.0|5.0|13.0|18.0|6.0|14.0|10.0|2.0|1.0
14.0|4.0|2.0|11.0|0.0|7.0|10.0|1.0|12.0|3.0|1.0|1.0
17.0|9.0|13.0|18.0|8.0|5.0|16.0|19.0|15.0|6.0|2.0|0.0
7.0|17.0|10.0|11.0|19.0|12.0|4.0|3.0|18.0|15.0|1.0|0.0
0.0|5.0|9.0|6.0|13.0|2.0|1.0|16.0|8.0|14.0|2.0|1.0
8.0|5.0|17.0|19.0|16.0|13.0|7.0|1.0|18.0|12.0|1.0|0.0
10.0|11.0|9.0|0.0|14.0|3.0|15.0|6.0|2.0|4.0|2.0|1.0
10.0|2.0|8.0|15.0|6.0|9.0|19.0|7.0|17.0|11.0|1.0|0.0
13.0|16.0|12.0|4.0|0.0|14.0|5.0|1.0|3.0|18.0|2.0|1.0
16.0|3.0|17.0|14.0|19.0|6.0|11.0|5.0|1.0|10.0|1.0|1.0
4.0|12.0|9.0|8.0|2.0|15.0|7.0|13.0|0.0|18.0|2.0|0.0



~~~

~~~

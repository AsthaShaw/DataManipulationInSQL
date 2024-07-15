# DataManipulationInSQL
Datacamp Tutorial

- CASE statements

```jsx
CASE WHEN x=1 THEN 'a'
CASE WHEN x=2 THEN 'b'
ELSE 'c' END AS new_column
```

Example

```jsx
SELECT
id, home_goal, away_goal,
CASE WHEN home_goal>away_goal THEN 'Home Team Win'
     WHEN home_goal<away_goal THEN 'Away Team Win'
     ELSE 'Tie' END AS outcome
 FROM match
 WHERE season='2013/2014'
```

Exercise 1

# **Basic CASE statements**

What is your favorite team?

The *European Soccer Database* contains data about 12,800 matches from 11 countries played between 2011-2015! Throughout this course, you will be shown filtered versions of the tables in this database in order to better explore their contents.

In this exercise, you will identify matches played between *FC Schalke 04* and *FC Bayern Munich*. There are 2 teams identified in each match in the `hometeam_id` and `awayteam_id` columns, available to you in the filtered `matches_germany` table. ID can join to the `team_api_id` column in the `teams_germany` table, but you cannot perform a join on both at the same time.

However, you can perform this operation using a `CASE` statement once you've identified the `team_api_id` associated with each team!

- Create a `CASE` statement that identifies whether a match in Germany included `FC Bayern Munich`, `FC Schalke 04`, or neither as the home team.
- Group the query by the `CASE` statement alias, `home_team`.

```jsx
SELECT 
	CASE WHEN hometeam_id = 10189 THEN 'FC Schalke 04'
             WHEN hometeam_id = 9823 THEN 'FC Bayern Munich'
             ELSE 'Other' END AS home_team,
	COUNT(id) AS total_matches
FROM matches_germany
-- Group by the CASE statement alias
GROUP BY home_team;
```

Exercise 2

# **CASE statements comparing column values**

Barcelona is considered one of the strongest teams in Spain's soccer league.

In this exercise, you will be creating a list of matches in the 2011/2012 season where Barcelona was the *home team*. You will do this using a `CASE` statement that compares the values of two columns to create a new group -- wins, losses, and ties.

In 3 steps, you will build a query that identifies a match's winner, identifies the identity of the opponent, and finally filters for Barcelona as the home team. Completing a query in this order will allow you to watch your results take shape with each new piece of information.

The `matches_spain` table currently contains Barcelona's matches from the 2011/2012 season, and has two key columns, `hometeam_id` and `awayteam_id`, that can be joined with the `teams_spain` table. However, you can only join `teams_spain` to one column at a time.

1. 
- Create a `CASE` statement that identifies whether a match in Germany included `FC Bayern Munich`, `FC Schalke 04`, or neither as the home team.
- Group the query by the `CASE` statement alias, `home_team`.

- Left join the `teams_spain` table `team_api_id` column to the `matches_spain` table `awayteam_id`. This allows us to retrieve the *away* team's identity.
- Select `team_long_name` from `teams_spain` as `opponent` and complete the `CASE` statement from Step 1.

```jsx
SELECT 
	m.date,
	--Select the team long name column and call it 'opponent'
	t.team_long_name AS opponent, 
	-- Complete the CASE statement with an alias
	CASE WHEN m.home_goal > m.away_goal THEN 'Home win!'
         WHEN m.home_goal < m.away_goal THEN 'Home loss :('
         ELSE 'Tie' END AS outcome
FROM matches_spain AS m
-- Left join teams_spain onto matches_spain
LEFT JOIN teams_spain AS t
ON m.awayteam_id = t.team_api_id;
```

- Complete the same `CASE` statement as the previous steps.
- Filter for matches where the home team is *FC Barcelona* (id = `8634`).

```jsx
SELECT 
	m.date,
	t.team_long_name AS opponent,
    -- Complete the CASE statement with an alias
	CASE WHEN m.home_goal > m.away_goal THEN 'Barcelona win!'
        WHEN m.home_goal < m.away_goal THEN  'Barcelona loss :(' 
        ELSE 'Tie' END AS outcome 
FROM matches_spain AS m
LEFT JOIN teams_spain AS t 
ON m.awayteam_id = t.team_api_id
-- Filter for Barcelona as the home team
WHERE m.hometeam_id = 8634; 

```

Exercise 3

# **CASE statements comparing two column values part 2**

Similar to the previous exercise, you will construct a query to determine the outcome of Barcelona's matches where they played as the ***away team***. You will learn how to combine these two queries in chapters 2 and 3.

Did their performance differ from the matches where they were the home team?

- Complete the `CASE` statement to identify Barcelona's away team games (`id = 8634`) as wins, losses, or ties.
- Left join the `teams_spain` table `team_api_id` column on the `matches_spain` table `hometeam_id` column. This retrieves the identity of the *home team* opponent.
- Filter the query to only include matches where Barcelona was the *away* team.

```jsx

SELECT  
	m.date,
	t.team_long_name AS opponent,
	CASE WHEN home_goal < away_goal THEN 'Barcelona win!'
        WHEN home_goal > away_goal THEN'Barcelona loss :(' 
        ELSE 'Tie' END AS outcome
FROM matches_spain AS m
-- Join teams_spain to matches_spain
LEFT JOIN teams_spain AS t 
ON m.hometeam_id = t.team_api_id
WHERE m.awayteam_id  = 8634;
```

### CASE statements with AND and END IS NOT NULL

```jsx
SELECT date, season,
CASE WHEN hometeam_id=8455 AND home_goal>away_goal THEN 'Chelsea home win!'
     WHEN awayteam_id=8455 AND home_goal<away_goal THEN 'Chelsea away win!'
     END AS outcome
FROM match
WHERE CASE WHEN hometeam_id=8455 AND home_goal>away_goal THEN 'Chelsea home win!'
           WHEN awayteam_id=8455 AND home_goal<away_goal THEN 'Chelsea away win!'
           END IS NOT NULL;
```
### Day 2

- Complete the first `CASE` statement, identifying Barcelona or Real Madrid as the home team using the `hometeam_id` column.
- Complete the second `CASE` statement in the same way, using `awayteam_id`.

```sql
SELECT 
	date,
	-- Identify the home team as Barcelona or Real Madrid
	CASE WHEN hometeam_id = 8634 THEN 'FC Barcelona' 
        ELSE 'Real Madrid CF' END AS home,
    -- Identify the away team as Barcelona or Real Madrid
	CASE WHEN awayteam_id = 8634 THEN 'FC Barcelona' 
        ELSE 'Real Madrid CF' END AS away
FROM matches_spain
WHERE (awayteam_id = 8634 OR hometeam_id = 8634)
      AND (awayteam_id = 8633 OR hometeam_id = 8633);
```

- Construct the final `CASE` statement identifying who won each match. Note there are 3 possible outcomes, but 5 conditions that you need to identify.
- Fill in the logical operators to identify Barcelona or Real Madrid as the winner.

```sql
SELECT 
	date,
	CASE WHEN hometeam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END as home,
	CASE WHEN awayteam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END as away,
	-- Identify all possible match outcomes
	CASE WHEN home_goal > away_goal AND hometeam_id = 8634 THEN 'Barcelona win!'
        WHEN home_goal > away_goal AND hometeam_id = 8633 THEN 'Real Madrid win!'
        WHEN home_goal < away_goal AND awayteam_id = 8634 THEN 'Barcelona win!'
        WHEN home_goal < away_goal AND awayteam_id = 8633 THEN 'Real Madrid win!'
        ELSE 'Tie!' END AS outcome
FROM matches_spain
WHERE (awayteam_id = 8634 OR hometeam_id = 8634)
      AND (awayteam_id = 8633 OR hometeam_id = 8633);
```

- Select the `home_goal` and `away_goal` for each match.
- Use the `CASE` statement in the `WHERE` clause to filter all `NULL` values generated by the statement in the previous step.

```sql
SELECT 
	season,
    date,
	home_goal,
	away_goal
FROM matches_italy
WHERE 
-- Exclude games not won by Bologna
	CASE WHEN hometeam_id = 9857 AND home_goal > away_goal THEN 'Bologna Win'
		WHEN awayteam_id = 9857 AND away_goal > home_goal THEN 'Bologna Win' 
		END IS NOT NULL;
```

# **COUNT using CASE WHEN**

Do the number of soccer matches played in a given European country differ across seasons? We will use the European Soccer Database to answer this question.

You will examine the number of matches played in 3 seasons within each country listed in the database. This is much easier to explore with each season's matches in separate columns. Using the `country` and unfiltered `match` table, you will count the number of matches played in each country during the 2012/2013, 2013/2014, and 2014/2015 match seasons.

- Create a `CASE` statement that identifies the `id` of matches played in the 2012/2013 season. Specify that you want `ELSE` values to be `NULL`.
- Wrap the `CASE` statement in a `COUNT` function and group the query by the `country` alias.

```sql
SELECT 
	c.name AS country,
    -- Count games from the 2012/2013 season
	COUNT(CASE WHEN m.season = '2012/2013' 
        	   THEN m.id
			   ELSE NULL END) AS matches_2012_2013
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY country;
```

- Create 3 `CASE WHEN` statements counting the matches played in each country across the 3 seasons.
- `END` your `CASE` statement without an `ELSE` clause.

  ```sql
SELECT 
	c.name AS country,
    -- Count matches in each of the 3 seasons
	COUNT(CASE WHEN m.season = '2012/2013' THEN m.id END) AS matches_2012_2013,
	COUNT(CASE WHEN m.season = '2013/2014' THEN m.id END) AS matches_2013_2014,
	COUNT(CASE WHEN m.season = '2014/2015' THEN m.id END) AS matches_2014_2015
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY country;
```

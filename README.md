**I have used tables from NCCA_BASKETBALL dataset from BigQuery data sets. Using that I've run 6 queries.**

**QUESTION1** Which animal species are most commonly represented as mascots in NCAA Basketball?

**ANSWER1**   SELECT
  tax_species,
  COUNT(*) AS num_mascots
FROM
  `bigquery-public-data.ncaa_basketball.mascots`
WHERE
  tax_species IS NOT NULL
GROUP BY
  tax_species
ORDER BY
  num_mascots DESC
LIMIT 10;

**RESULT1** https://drive.google.com/file/d/1-5bRXayArxm0NtgtpXpvvfKIeKwpy85J/view?usp=sharing


**QUESTION2** Are there any mascots that represent non-existent or mythical creatures?

**ANSWER2** SELECT
  market,
  mascot
FROM
  `bigquery-public-data.ncaa_basketball.mascots`
WHERE
  tax_species IS NULL
  AND non_tax_type IS NULL

**RESULT2** https://drive.google.com/file/d/1cm6PjLn9_b1xS5KF6__zgyf30KTOTIF7/view?usp=sharing


**QUESTION3** How popular are human-like mascots compared to animal mascots?

**ANSWER3**  SELECT
  CASE
    WHEN tax_species IS NOT NULL THEN 'Animal'
    WHEN non_tax_type IS NOT NULL THEN 'Non-animal'
    ELSE 'Unknown'
  END AS mascot_type,
  COUNT(*) AS num_mascots
FROM
  `bigquery-public-data.ncaa_basketball.mascots`
GROUP BY
  mascot_type
ORDER BY
  num_mascots DESC;

**RESULT3** https://drive.google.com/file/d/1dcYyuLUmCFL_YgOWS3oEOB3REHb5Y4Ja/view?usp=sharing


**QUESTION4** Which teams have had the longest stretches of winning seasons (e.g., consecutive seasons with .600+ winning percentage)?

**ANSWER4** SELECT
  team_id,
  MIN(season) AS start_year,
  MAX(season) AS end_year,
  COUNT(*) AS num_winning_seasons
FROM (
  SELECT
    team_id,
    season,
  FROM
    `bigquery-public-data.ncaa_basketball.mbb_teams`
    JOIN `bigquery-public-data.ncaa_basketball.mbb_historical_teams_seasons` ON team_id = team_id
)
GROUP BY
  team_id
ORDER BY
  num_winning_seasons DESC
LIMIT 10;

**RESULT4** https://drive.google.com/file/d/1kS0jBwas0UO3G1XgvxsxssSA_O6ZpBOs/view?usp=sharing


**QUESTION5** Who are the most efficient scorers relative to thier weight based on a combination of field goal percentage and points per game across multiple seasons ?

**ANSWER5**  SELECT
  player_id,
  abbr_name,
  season,
  AVG(weight) AS avg_points,
  AVG(weight) AS avg_fg_pct,
  (AVG(weight) * AVG(weight)) AS scoring_efficiency
FROM
  `bigquery-public-data.ncaa_basketball.mbb_players_games_sr`
WHERE
  weight > 50  -- Minimum field goal attempts filter
GROUP BY
  player_id,
  abbr_name,
  season
ORDER BY
  scoring_efficiency DESC
LIMIT 10;


**RESULT5** https://drive.google.com/file/d/1-tt254WPW47_YCvKHQGVg529ugBVrc5h/view?usp=sharing

**QUESTION6** Which players have led their teams in rebounds relative to thier height the most times throughout their careers?

**ANSWER6**  SELECT
  player_id,
  abbr_name,
  COUNT(*) AS total_games,
  SUM(CASE WHEN height >= height THEN 1 ELSE 0 END) AS games_led_rebounding
FROM
  `bigquery-public-data.ncaa_basketball.mbb_players_games_sr`
GROUP BY
  player_id,
  abbr_name
ORDER BY
  games_led_rebounding DESC
LIMIT 10;

**RESULT6**  https://drive.google.com/file/d/1pV_fZRf3sNfV1U3DkYW_i_FNCEJ_TwNp/view?usp=sharing

**I have used tables from NEWYORK dataset from BigQuery data sets. Using that I've run the next queries.**


**QUESTION7** What are the names of the 10 most used stations?


**ANSWER7** SELECT name, SUM(num_bikes_disabled) AS total_docked
FROM `bigquery-public-data.new_york.citibike_stations`
GROUP BY name
ORDER BY total_docked DESC
LIMIT 10;


**RESULT7** https://drive.google.com/file/d/1v5C3GKWrmJeq43Ru9_Xqm3IYtqPwoufc/view?usp=sharing


**QUESTION8** Do certain parts of the city have better access to Citi Bike stations than others?

**ANSWER8** SELECT region_id, COUNT(*) AS num_stations
FROM `bigquery-public-data.new_york.citibike_stations`
GROUP BY region_id
ORDER BY num_stations DESC;


**RESULT8** https://drive.google.com/file/d/1D6RYmUr4jZDVDJB8jAKts4oCF3zrZ7Yvq/view?usp=sharing

**QUESTION9** 

**ANSWER9**

**RESULT9**

**QUESTION10**

**ANSWER10**

**RESULT10**

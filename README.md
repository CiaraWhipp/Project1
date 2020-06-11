Project 1
================
Ciara Whipp
6/10/2020

## Introduction to JSON Data

### What is JSON and what is its use?

JavaScript Object Notation or
[JSON](https://www.tutorialspoint.com/json/json_overview.htm#:~:text=JSON%20or%20JavaScript%20Object%20Notation,stands%20for%20JavaScript%20Object%20Notation.)
is a text-based, data-interchangable format used to store and transport
data. It is often used to transmit data between a server and web
applications, and web services and APIs use JSON to provide data. We
will explore JSON provided by an API in this document.

### Why use JSON?

JSON is language independent, so major programming languages are cable
of handling JSON data - they can get data out of JSON and turn data into
JSON. The intuitive nature of the syntax is easy to read and write, and
since the syntax is derived from and similar to existing programming
languages, it is easy for machines to generate and parse JSON data. This
[video](https://www.youtube.com/watch?v=0IoG-mSvWSo) provides a brief
overview of JSON data: what it is, advantages to JSON, and an
introduction to JSON syntax.

### R Packages to Accomodate JSON Data

There are three major R packages used to deal with JSON data:
`jsonlite`, `rjson`, and `RJSONIO`.  
Some helpful functions included in all three packages are `fromJSON()`
to convert JSON data into an R object, `toJSON()` to convert an R object
into JSON. Addtionally, `flatten` is a useful argument that creates a
single non-nested data frame from a nested data frame (common in JSON
data retrieved from the web). I will choose to use `jsonlite` since the
R documentation for this package is more robust than the doucmentation
for the other two packages.

## Retrieving Data from an API

The following code creates the base url that can be used to read in data
from the National Hockey League (NHL) API.

``` r
baseURL <- "https://records.nhl.com/site/api/"
```

The following function can be used to access the **franchise** data and
**franchise-team-totals** data. Note the `jsonlite` and `httr` packages
are required to utilize this function.

``` r
library(jsonlite)
library(httr)
overallData <- function(x){
  fullURL <- paste0(baseURL,x)
  GET(fullURL) %>% content("text") %>% fromJSON(flatten=TRUE) %>% as_tibble()
}
```

Use the function above to return tibbles for the **franchise** data and
**franchise-team-totals** data.

``` r
overallData(x="franchise")
```

    ## # A tibble: 38 x 2
    ##    data$id $firstSeasonId $lastSeasonId $mostRecentTeam… $teamCommonName
    ##      <int>          <int>         <int>            <int> <chr>          
    ##  1       1       19171918            NA                8 Canadiens      
    ##  2       2       19171918      19171918               41 Wanderers      
    ##  3       3       19171918      19341935               45 Eagles         
    ##  4       4       19191920      19241925               37 Tigers         
    ##  5       5       19171918            NA               10 Maple Leafs    
    ##  6       6       19241925            NA                6 Bruins         
    ##  7       7       19241925      19371938               43 Maroons        
    ##  8       8       19251926      19411942               51 Americans      
    ##  9       9       19251926      19301931               39 Quakers        
    ## 10      10       19261927            NA                3 Rangers        
    ## # … with 28 more rows, and 2 more variables: $teamPlaceName <chr>, total <int>

``` r
overallData(x="franchise-team-totals")
```

    ## # A tibble: 104 x 2
    ##    data$id $activeFranchise $firstSeasonId $franchiseId $gameTypeId $gamesPlayed
    ##      <int>            <int>          <int>        <int>       <int>        <int>
    ##  1       1                1       19821983           23           2         2937
    ##  2       2                1       19821983           23           3          257
    ##  3       3                1       19721973           22           2         3732
    ##  4       4                1       19721973           22           3          272
    ##  5       5                1       19261927           10           2         6504
    ##  6       6                1       19261927           10           3          515
    ##  7       7                1       19671968           16           3          433
    ##  8       8                1       19671968           16           2         4115
    ##  9       9                1       19671968           17           2         4115
    ## 10      10                1       19671968           17           3          381
    ## # … with 94 more rows, and 25 more variables: $goalsAgainst <int>,
    ## #   $goalsFor <int>, $homeLosses <int>, $homeOvertimeLosses <int>,
    ## #   $homeTies <int>, $homeWins <int>, $lastSeasonId <int>, $losses <int>,
    ## #   $overtimeLosses <int>, $penaltyMinutes <int>, $pointPctg <dbl>,
    ## #   $points <int>, $roadLosses <int>, $roadOvertimeLosses <int>,
    ## #   $roadTies <int>, $roadWins <int>, $shootoutLosses <int>,
    ## #   $shootoutWins <int>, $shutouts <int>, $teamId <int>, $teamName <chr>,
    ## #   $ties <int>, $triCode <chr>, $wins <int>, total <int>

The following function can be used to access
**franchise-season-records**, **franchise-goalie-records**, and
**franchise-skater-records** for a specified franchise ID.

``` r
franchiseData <- function(x,ID){
  fullURL <- paste0(baseURL,x,"?cayenneExp=franchiseId=",ID)
  GET(fullURL) %>% content("text") %>% fromJSON(flatten=TRUE) %>% as_tibble()
}
```

Use the function above to return tibbles for the Capitals franchise
(ID=24) for **franchise-season-records**, **franchise-goalie-records**,
and **franchise-skater-records**.

``` r
franchiseData(x="franchise-season-records", ID="24")
```

    ## # A tibble: 1 x 2
    ##   data$id $fewestGoals $fewestGoalsAga… $fewestGoalsAga… $fewestGoalsSea…
    ##     <int>        <int>            <int> <chr>            <chr>           
    ## 1      15          181              182 2016-17 (82)     1974-75 (80)    
    ## # … with 53 more variables: $fewestLosses <int>, $fewestLossesSeasons <chr>,
    ## #   $fewestPoints <int>, $fewestPointsSeasons <chr>, $fewestTies <int>,
    ## #   $fewestTiesSeasons <chr>, $fewestWins <int>, $fewestWinsSeasons <chr>,
    ## #   $franchiseId <int>, $franchiseName <chr>, $homeLossStreak <int>,
    ## #   $homeLossStreakDates <chr>, $homePointStreak <int>,
    ## #   $homePointStreakDates <chr>, $homeWinStreak <int>,
    ## #   $homeWinStreakDates <chr>, $homeWinlessStreak <int>,
    ## #   $homeWinlessStreakDates <chr>, $lossStreak <int>, $lossStreakDates <chr>,
    ## #   $mostGameGoals <int>, $mostGameGoalsDates <chr>, $mostGoals <int>,
    ## #   $mostGoalsAgainst <int>, $mostGoalsAgainstSeasons <chr>,
    ## #   $mostGoalsSeasons <chr>, $mostLosses <int>, $mostLossesSeasons <chr>,
    ## #   $mostPenaltyMinutes <int>, $mostPenaltyMinutesSeasons <chr>,
    ## #   $mostPoints <int>, $mostPointsSeasons <chr>, $mostShutouts <int>,
    ## #   $mostShutoutsSeasons <chr>, $mostTies <int>, $mostTiesSeasons <chr>,
    ## #   $mostWins <int>, $mostWinsSeasons <chr>, $pointStreak <int>,
    ## #   $pointStreakDates <chr>, $roadLossStreak <int>, $roadLossStreakDates <chr>,
    ## #   $roadPointStreak <int>, $roadPointStreakDates <chr>, $roadWinStreak <int>,
    ## #   $roadWinStreakDates <chr>, $roadWinlessStreak <int>,
    ## #   $roadWinlessStreakDates <chr>, $winStreak <int>, $winStreakDates <chr>,
    ## #   $winlessStreak <lgl>, $winlessStreakDates <lgl>, total <int>

``` r
franchiseData(x="franchise-goalie-records", ID="24")
```

    ## # A tibble: 30 x 2
    ##    data$id $activePlayer $firstName $franchiseId $franchiseName $gameTypeId
    ##      <int> <lgl>         <chr>             <int> <chr>                <int>
    ##  1     241 FALSE         Olie                 24 Washington Ca…           2
    ##  2     326 TRUE          Braden               24 Washington Ca…           2
    ##  3     339 FALSE         Don                  24 Washington Ca…           2
    ##  4     357 FALSE         Craig                24 Washington Ca…           2
    ##  5     468 FALSE         Gary                 24 Washington Ca…           2
    ##  6     489 FALSE         Mike                 24 Washington Ca…           2
    ##  7     494 FALSE         Ron                  24 Washington Ca…           2
    ##  8     498 FALSE         Clint                24 Washington Ca…           2
    ##  9     564 FALSE         Roger                24 Washington Ca…           2
    ## 10     648 FALSE         Mike                 24 Washington Ca…           2
    ## # … with 20 more rows, and 24 more variables: $gamesPlayed <int>,
    ## #   $lastName <chr>, $losses <int>, $mostGoalsAgainstDates <chr>,
    ## #   $mostGoalsAgainstOneGame <int>, $mostSavesDates <chr>,
    ## #   $mostSavesOneGame <int>, $mostShotsAgainstDates <chr>,
    ## #   $mostShotsAgainstOneGame <int>, $mostShutoutsOneSeason <int>,
    ## #   $mostShutoutsSeasonIds <chr>, $mostWinsOneSeason <int>,
    ## #   $mostWinsSeasonIds <chr>, $overtimeLosses <int>, $playerId <int>,
    ## #   $positionCode <chr>, $rookieGamesPlayed <int>, $rookieShutouts <int>,
    ## #   $rookieWins <int>, $seasons <int>, $shutouts <int>, $ties <int>,
    ## #   $wins <int>, total <int>

``` r
franchiseData(x="franchise-skater-records", ID="24")
```

    ## # A tibble: 509 x 2
    ##    data$id $activePlayer $assists $firstName $franchiseId $franchiseName
    ##      <int> <lgl>            <int> <chr>             <int> <chr>         
    ##  1   16910 FALSE              361 Calle                24 Washington Ca…
    ##  2   16982 TRUE               572 Alex                 24 Washington Ca…
    ##  3   17011 TRUE               684 Nicklas              24 Washington Ca…
    ##  4   17021 FALSE              375 Dale                 24 Washington Ca…
    ##  5   17075 FALSE              249 Dennis               24 Washington Ca…
    ##  6   17106 FALSE               42 Alan                 24 Washington Ca…
    ##  7   17158 FALSE              392 Mike                 24 Washington Ca…
    ##  8   17179 FALSE              259 Larry                24 Washington Ca…
    ##  9   17250 FALSE                0 Keith                24 Washington Ca…
    ## 10   17255 FALSE               98 Greg                 24 Washington Ca…
    ## # … with 499 more rows, and 25 more variables: $gameTypeId <int>,
    ## #   $gamesPlayed <int>, $goals <int>, $lastName <chr>,
    ## #   $mostAssistsGameDates <chr>, $mostAssistsOneGame <int>,
    ## #   $mostAssistsOneSeason <int>, $mostAssistsSeasonIds <chr>,
    ## #   $mostGoalsGameDates <chr>, $mostGoalsOneGame <int>,
    ## #   $mostGoalsOneSeason <int>, $mostGoalsSeasonIds <chr>,
    ## #   $mostPenaltyMinutesOneSeason <int>, $mostPenaltyMinutesSeasonIds <chr>,
    ## #   $mostPointsGameDates <chr>, $mostPointsOneGame <int>,
    ## #   $mostPointsOneSeason <int>, $mostPointsSeasonIds <chr>,
    ## #   $penaltyMinutes <int>, $playerId <int>, $points <int>, $positionCode <chr>,
    ## #   $rookiePoints <int>, $seasons <int>, total <int>

Create a new variable for the **franchise-skater-records** data for the
Washington Capitals called goalsPerGame that gives the average number of
goals per game by dividing `goals` by `gamesPlayed`. Return a table for
the top 6 goals per game
statistic.

``` r
skaterCaps <- franchiseData(x="franchise-skater-records", ID="24")$data %>% rename("Last Name"=lastName, "Position Code"=positionCode, "Number of Seasons Played"=seasons) %>%mutate("Goals per Game"=goals/gamesPlayed) %>% select(`Last Name`, `Position Code`, `Goals per Game`, `Number of Seasons Played`) %>% arrange(desc(`Goals per Game`))
knitr::kable(head(skaterCaps))
```

| Last Name  | Position Code | Goals per Game | Number of Seasons Played |
| :--------- | :------------ | -------------: | -----------------------: |
| Ovechkin   | L             |      0.6128472 |                       15 |
| Berezin    | L             |      0.5555556 |                        1 |
| Maruk      | C             |      0.5306122 |                        5 |
| Gartner    | R             |      0.5237467 |                       10 |
| Ciccarelli | R             |      0.5022422 |                        4 |
| Carlson    | D             |      0.5000000 |                        1 |

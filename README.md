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

The following function can be used to access **franchise** data and
**franchise-team-totals** data.

``` r
franchiseData <- function(x){
  fullURL <- paste0(baseURL,x)
  GET(fullURL) %>% content("text") %>% fromJSON() %>% as_tibble() %>% print(n=104)
}
franchiseData(x="franchise")
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
    ## 11      11       19261927            NA               16 Blackhawks     
    ## 12      12       19261927            NA               17 Red Wings      
    ## 13      13       19671968      19771978               49 Barons         
    ## 14      14       19671968            NA               26 Kings          
    ## 15      15       19671968            NA               25 Stars          
    ## 16      16       19671968            NA                4 Flyers         
    ## 17      17       19671968            NA                5 Penguins       
    ## 18      18       19671968            NA               19 Blues          
    ## 19      19       19701971            NA                7 Sabres         
    ## 20      20       19701971            NA               23 Canucks        
    ## 21      21       19721973            NA               20 Flames         
    ## 22      22       19721973            NA                2 Islanders      
    ## 23      23       19741975            NA                1 Devils         
    ## 24      24       19741975            NA               15 Capitals       
    ## 25      25       19791980            NA               22 Oilers         
    ## 26      26       19791980            NA               12 Hurricanes     
    ## 27      27       19791980            NA               21 Avalanche      
    ## 28      28       19791980            NA               53 Coyotes        
    ## 29      29       19911992            NA               28 Sharks         
    ## 30      30       19921993            NA                9 Senators       
    ## 31      31       19921993            NA               14 Lightning      
    ## 32      32       19931994            NA               24 Ducks          
    ## 33      33       19931994            NA               13 Panthers       
    ## 34      34       19981999            NA               18 Predators      
    ## 35      35       19992000            NA               52 Jets           
    ## 36      36       20002001            NA               29 Blue Jackets   
    ## 37      37       20002001            NA               30 Wild           
    ## 38      38       20172018            NA               54 Golden Knights 
    ## # … with 2 more variables: $teamPlaceName <chr>, total <int>

``` r
franchiseData(x="franchise-team-totals")
```

    ## # A tibble: 104 x 2
    ##     data$id $activeFranchise $firstSeasonId $franchiseId $gameTypeId
    ##       <int>            <int>          <int>        <int>       <int>
    ##   1       1                1       19821983           23           2
    ##   2       2                1       19821983           23           3
    ##   3       3                1       19721973           22           2
    ##   4       4                1       19721973           22           3
    ##   5       5                1       19261927           10           2
    ##   6       6                1       19261927           10           3
    ##   7       7                1       19671968           16           3
    ##   8       8                1       19671968           16           2
    ##   9       9                1       19671968           17           2
    ##  10      10                1       19671968           17           3
    ##  11      11                1       19241925            6           2
    ##  12      12                1       19241925            6           3
    ##  13      13                1       19701971           19           2
    ##  14      14                1       19701971           19           3
    ##  15      15                1       19171918            1           3
    ##  16      16                1       19171918            1           2
    ##  17      17                1       19921993           30           2
    ##  18      18                1       19921993           30           3
    ##  19      19                1       19271928            5           2
    ##  20      20                1       19271928            5           3
    ##  21      21                1       19992000           35           2
    ##  22      22                1       19992000           35           3
    ##  23      23                1       19971998           26           3
    ##  24      24                1       19971998           26           2
    ##  25      25                1       19931994           33           2
    ##  26      26                1       19931994           33           3
    ##  27      27                1       19921993           31           2
    ##  28      28                1       19921993           31           3
    ##  29      29                1       19741975           24           2
    ##  30      30                1       19741975           24           3
    ##  31      31                1       19261927           11           3
    ##  32      32                1       19261927           11           2
    ##  33      33                1       19321933           12           2
    ##  34      34                1       19321933           12           3
    ##  35      35                1       19981999           34           2
    ##  36      36                1       19981999           34           3
    ##  37      37                1       19671968           18           2
    ##  38      38                1       19671968           18           3
    ##  39      39                1       19801981           21           3
    ##  40      40                1       19801981           21           2
    ##  41      41                1       19951996           27           2
    ##  42      42                1       19951996           27           3
    ##  43      43                1       19791980           25           2
    ##  44      44                1       19791980           25           3
    ##  45      45                1       19701971           20           2
    ##  46      46                1       19701971           20           3
    ##  47      47                1       19931994           32           3
    ##  48      48                1       19931994           32           2
    ##  49      49                1       19931994           15           2
    ##  50      50                1       19931994           15           3
    ##  51      51                1       19671968           14           2
    ##  52      52                1       19671968           14           3
    ##  53      53                1       19961997           28           2
    ##  54      54                1       19961997           28           3
    ##  55      55                1       19911992           29           3
    ##  56      56                1       19911992           29           2
    ##  57      57                1       20002001           36           2
    ##  58      58                1       20002001           36           3
    ##  59      59                1       20002001           37           2
    ##  60      60                1       20002001           37           3
    ##  61      61                1       19671968           15           2
    ##  62      62                1       19671968           15           3
    ##  63      63                1       19791980           27           3
    ##  64      64                1       19791980           27           2
    ##  65      65                1       19791980           28           2
    ##  66      66                1       19791980           28           3
    ##  67      67                1       19791980           26           2
    ##  68      68                1       19791980           26           3
    ##  69      69                1       19761977           23           2
    ##  70      70                1       19761977           23           3
    ##  71      71                0       19171918            3           3
    ##  72      72                0       19171918            3           2
    ##  73      73                0       19201921            4           2
    ##  74      74                0       19251926            9           2
    ##  75      75                0       19251926            9           3
    ##  76      76                0       19301931            9           2
    ##  77      77                1       19261927           12           2
    ##  78      78                1       19261927           12           3
    ##  79      79                0       19171918            2           2
    ##  80      80                0       19191920            4           2
    ##  81      81                0       19241925            7           2
    ##  82      82                0       19241925            7           3
    ##  83      83                0       19251926            8           2
    ##  84      84                0       19251926            8           3
    ##  85      85                0       19341935            3           2
    ##  86      86                0       19671968           13           2
    ##  87      87                0       19671968           13           3
    ##  88      88                1       19721973           21           2
    ##  89      89                1       19721973           21           3
    ##  90      90                1       19741975           23           2
    ##  91      91                0       19761977           13           2
    ##  92      92                1       19301931           12           2
    ##  93      93                1       19301931           12           3
    ##  94      94                0       19411942            8           2
    ##  95      95                1       20112012           35           3
    ##  96      96                1       20112012           35           2
    ##  97      97                1       20142015           28           2
    ##  98      98                1       20172018           38           2
    ##  99      99                1       20172018           38           3
    ## 100     100                0       19701971           13           2
    ## 101     101                1       19171918            5           2
    ## 102     102                1       19171918            5           3
    ## 103     103                1       19191920            5           3
    ## 104     104                1       19191920            5           2
    ## # … with 26 more variables: $gamesPlayed <int>, $goalsAgainst <int>,
    ## #   $goalsFor <int>, $homeLosses <int>, $homeOvertimeLosses <int>,
    ## #   $homeTies <int>, $homeWins <int>, $lastSeasonId <int>, $losses <int>,
    ## #   $overtimeLosses <int>, $penaltyMinutes <int>, $pointPctg <dbl>,
    ## #   $points <int>, $roadLosses <int>, $roadOvertimeLosses <int>,
    ## #   $roadTies <int>, $roadWins <int>, $shootoutLosses <int>,
    ## #   $shootoutWins <int>, $shutouts <int>, $teamId <int>, $teamName <chr>,
    ## #   $ties <int>, $triCode <chr>, $wins <int>, total <int>

Quantium Virtual Internship - Retail Strategy and Analytics - Task 1
================
Abu RIjal
2022-11-28

## Install and load necessary packages:

-   `tidyverse`
-   `readxl`

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(readxl) 
```

## Import data

``` r
transactionData = read_excel("QVI_transaction_data.xlsx")
customerData = read_csv("QVI_purchase_behaviour.csv")
```

    ## Rows: 72637 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): LIFESTAGE, PREMIUM_CUSTOMER
    ## dbl (1): LYLTY_CARD_NBR
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## Exploratory data analysis

### Examining transaction data

``` r
str(transactionData)
```

    ## tibble [264,836 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ DATE          : num [1:264836] 43390 43599 43605 43329 43330 ...
    ##  $ STORE_NBR     : num [1:264836] 1 1 1 2 2 4 4 4 5 7 ...
    ##  $ LYLTY_CARD_NBR: num [1:264836] 1000 1307 1343 2373 2426 ...
    ##  $ TXN_ID        : num [1:264836] 1 348 383 974 1038 ...
    ##  $ PROD_NBR      : num [1:264836] 5 66 61 69 108 57 16 24 42 52 ...
    ##  $ PROD_NAME     : chr [1:264836] "Natural Chip        Compny SeaSalt175g" "CCs Nacho Cheese    175g" "Smiths Crinkle Cut  Chips Chicken 170g" "Smiths Chip Thinly  S/Cream&Onion 175g" ...
    ##  $ PROD_QTY      : num [1:264836] 2 3 2 5 3 1 1 1 1 2 ...
    ##  $ TOT_SALES     : num [1:264836] 6 6.3 2.9 15 13.8 5.1 5.7 3.6 3.9 7.2 ...

``` r
transactionData
```

    ## # A tibble: 264,836 × 8
    ##     DATE STORE_NBR LYLTY_CARD_NBR TXN_ID PROD_NBR PROD_NAME      PROD_…¹ TOT_S…²
    ##    <dbl>     <dbl>          <dbl>  <dbl>    <dbl> <chr>            <dbl>   <dbl>
    ##  1 43390         1           1000      1        5 Natural Chip …       2     6  
    ##  2 43599         1           1307    348       66 CCs Nacho Che…       3     6.3
    ##  3 43605         1           1343    383       61 Smiths Crinkl…       2     2.9
    ##  4 43329         2           2373    974       69 Smiths Chip T…       5    15  
    ##  5 43330         2           2426   1038      108 Kettle Tortil…       3    13.8
    ##  6 43604         4           4074   2982       57 Old El Paso S…       1     5.1
    ##  7 43601         4           4149   3333       16 Smiths Crinkl…       1     5.7
    ##  8 43601         4           4196   3539       24 Grain Waves  …       1     3.6
    ##  9 43332         5           5026   4525       42 Doritos Corn …       1     3.9
    ## 10 43330         7           7150   6900       52 Grain Waves S…       2     7.2
    ## # … with 264,826 more rows, and abbreviated variable names ¹​PROD_QTY,
    ## #   ²​TOT_SALES

#### Convert DATE column to a date format

``` r
transactionData$DATE = as.Date(transactionData$DATE, origin = "1899-12-30")
```

#### Check

``` r
transactionData
```

    ## # A tibble: 264,836 × 8
    ##    DATE       STORE_NBR LYLTY_CARD_NBR TXN_ID PROD_NBR PROD_NAME PROD_…¹ TOT_S…²
    ##    <date>         <dbl>          <dbl>  <dbl>    <dbl> <chr>       <dbl>   <dbl>
    ##  1 2018-10-17         1           1000      1        5 Natural …       2     6  
    ##  2 2019-05-14         1           1307    348       66 CCs Nach…       3     6.3
    ##  3 2019-05-20         1           1343    383       61 Smiths C…       2     2.9
    ##  4 2018-08-17         2           2373    974       69 Smiths C…       5    15  
    ##  5 2018-08-18         2           2426   1038      108 Kettle T…       3    13.8
    ##  6 2019-05-19         4           4074   2982       57 Old El P…       1     5.1
    ##  7 2019-05-16         4           4149   3333       16 Smiths C…       1     5.7
    ##  8 2019-05-16         4           4196   3539       24 Grain Wa…       1     3.6
    ##  9 2018-08-20         5           5026   4525       42 Doritos …       1     3.9
    ## 10 2018-08-18         7           7150   6900       52 Grain Wa…       2     7.2
    ## # … with 264,826 more rows, and abbreviated variable names ¹​PROD_QTY,
    ## #   ²​TOT_SALES

We should check that we are looking at the right products by examining
PROD_NAME.

``` r
transactionData %>% 
  count(PROD_NAME)
```

    ## # A tibble: 114 × 2
    ##    PROD_NAME                                  n
    ##    <chr>                                  <int>
    ##  1 Burger Rings 220g                       1564
    ##  2 CCs Nacho Cheese    175g                1498
    ##  3 CCs Original 175g                       1514
    ##  4 CCs Tasty Cheese    175g                1539
    ##  5 Cheetos Chs & Bacon Balls 190g          1479
    ##  6 Cheetos Puffs 165g                      1448
    ##  7 Cheezels Cheese 330g                    3149
    ##  8 Cheezels Cheese Box 125g                1454
    ##  9 Cobs Popd Sea Salt  Chips 110g          3265
    ## 10 Cobs Popd Sour Crm  &Chives Chips 110g  3159
    ## # … with 104 more rows

Looks like we are definitely looking at potato chips but how can we
check that these are all chips? We can do some basic text analysis by
summarising the individual words in the product name.

``` r
trans1 = transactionData %>% 
  separate(PROD_NAME,into=c("Brand name","type"),sep=" ")
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 261666 rows [1, 2, 3,
    ## 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].

``` r
head(trans1)
```

    ## # A tibble: 6 × 9
    ##   DATE       STORE_NBR LYLTY_CARD…¹ TXN_ID PROD_…² Brand…³ type  PROD_…⁴ TOT_S…⁵
    ##   <date>         <dbl>        <dbl>  <dbl>   <dbl> <chr>   <chr>   <dbl>   <dbl>
    ## 1 2018-10-17         1         1000      1       5 Natural Chip        2     6  
    ## 2 2019-05-14         1         1307    348      66 CCs     Nacho       3     6.3
    ## 3 2019-05-20         1         1343    383      61 Smiths  Crin…       2     2.9
    ## 4 2018-08-17         2         2373    974      69 Smiths  Chip        5    15  
    ## 5 2018-08-18         2         2426   1038     108 Kettle  Tort…       3    13.8
    ## 6 2019-05-19         4         4074   2982      57 Old     El          1     5.1
    ## # … with abbreviated variable names ¹​LYLTY_CARD_NBR, ²​PROD_NBR, ³​`Brand name`,
    ## #   ⁴​PROD_QTY, ⁵​TOT_SALES

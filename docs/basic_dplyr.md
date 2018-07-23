
---
output: 
  html_document: 
    df_print: paged
---
# Basic dplyr

## Introduction

This chapter will mimic the **Basic SQL** lesson by using function from the dplyr package. As specified in the [introduction](#sql-intro), the dplyr verbs are best when used with the pipe-operator `%>%`. 

We will using Parch and Posey data in this and all other SQL lessons, so make sure that the `parchposey` package is installed and loaded. The same applies to the `dplyr` package.


```r
# Load the parchposey and dplyr packages
library(parchposey)
library(dplyr)
```


## Importing Parch and Posey data 

Now that you've loaded `parchposey`, you're able to load the necessary tables into R using the `data(table_name)` function. For example, `data(orders)` will load the __*orders*__ table into your environment. Below you can see that the table has been loaded and that it contains the same variable as the tables in the SQL editor. 


```r
data("orders") # Add the orders table to you environment
glimpse(orders) # Get a glimpse of the loaded table
```

```
#> Observations: 6,912
#> Variables: 11
#> $ id               <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14...
#> $ account_id       <int> 1001, 1001, 1001, 1001, 1001, 1001, 1001, 100...
#> $ occurred_at      <dttm> 2015-10-06 17:31:14, 2015-11-05 03:34:33, 20...
#> $ standard_qty     <int> 123, 190, 85, 144, 108, 103, 101, 95, 91, 94,...
#> $ gloss_qty        <int> 22, 41, 47, 32, 29, 24, 33, 47, 16, 46, 36, 3...
#> $ poster_qty       <int> 24, 57, NA, NA, 28, 46, 92, 151, 22, 8, NA, 3...
#> $ total            <int> 169, 288, 132, 176, 165, 173, 226, 293, 129, ...
#> $ standard_amt_usd <dbl> 613.77, 948.10, 424.15, 718.56, 538.92, 513.9...
#> $ gloss_amt_usd    <dbl> 164.78, 307.09, 352.03, 239.68, 217.21, 179.7...
#> $ poster_amt_usd   <dbl> 194.88, 462.84, 0.00, 0.00, 227.36, 373.52, 7...
#> $ total_amt_usd    <dbl> 973.43, 1718.03, 776.18, 958.24, 983.49, 1067...
```

## Select columns from tables with `select()`

From the **Basic SQL** lesson, we know that every SQL query will at least have a `SELECT` and `FROM` statement. We will follow the same convention for now while introducing our first dplyr verb function: `select`.

> `select()` is able to take 2 arguments:  
1. `.data` which points to a table  
2. the columns to select. 

To see how this works, let's use `select` with the **orders** table as the value for `.data` but no other values.


```r
select(.data = orders)
```

```
#> # A tibble: 6,912 x 0
```

```r
# Translates to:
## SELECT FROM orders: 
```

As you can see, the function returned the full table with 6,912 rows but no columns. This is because `select` is usually used to keep only the columns specified within the function. Therefore, since no columns were specified then no columns were returned. 

However, there are a number of special "helper" functions that work inside the function to select columns based on different criteria. One such helper function, `everything()`, is the equivalent to the asterisk (*) used in a query to select all columns within a table.

Therefore, the following SQL query

```sql
SELECT * 
FROM orders
```
is equivalent to 

```r
select(.data = orders, everything())
# SELECT FROM orders: everything!
```
or using the pipe

```r
orders %>% 
  select(everything())
# FROM orders, SELECT everything!
```

```
#> # A tibble: 6,912 x 11
#>       id account_id occurred_at         standard_qty gloss_qty poster_qty
#>    <int>      <int> <dttm>                     <int>     <int>      <int>
#>  1     1       1001 2015-10-06 17:31:14          123        22         24
#>  2     2       1001 2015-11-05 03:34:33          190        41         57
#>  3     3       1001 2015-12-04 04:21:55           85        47         NA
#>  4     4       1001 2016-01-02 01:18:24          144        32         NA
#>  5     5       1001 2016-02-01 19:27:27          108        29         28
#>  6     6       1001 2016-03-02 15:29:32          103        24         46
#>  7     7       1001 2016-04-01 11:20:18          101        33         92
#>  8     8       1001 2016-05-01 15:55:51           95        47        151
#>  9     9       1001 2016-05-31 21:22:48           91        16         22
#> 10    10       1001 2016-06-30 12:32:05           94        46          8
#> # ... with 6,902 more rows, and 5 more variables: total <int>,
#> #   standard_amt_usd <dbl>, gloss_amt_usd <dbl>, poster_amt_usd <dbl>,
#> #   total_amt_usd <dbl>
```

You may have noticed the change in order in the last chunk of code, where the name of the table comes before the verb. 

The reason for doing this is to facilitate the reading of the code for someone who may not have a background in programming. We know that **orders** is a table within our environment and that the `%>%` operator can be read as *THEN*. Putting these together we can read the above as:

> Take the *orders* table and **THEN**  
> &nbsp;&nbsp;**select** everything within the table

We don't need to limit ourselves to only one pipe within a call. We can keep adding functions as actions to be taken on the **orders** table. 

Remember that for both SQL and R, the `SELECT/select()` statement/function is where you put the **columns** for which you would like to show the data. 

**Your Turn**



Convert the following query to R code. Make sure the R output matches the SQL output and try to use the `%>%` operator. Protip: In RStudio, use *CTRL+SHIFT+M* to add a pipe. 


```sql
SELECT id, account_id, occurred_at
FROM orders
```


<div class="knitsql-table">


Table: (\#tab:unnamed-chunk-10)Displaying records 1 - 10

id    account_id  occurred_at         
---  -----------  --------------------
1           1001  2015-10-06 17:31:14 
2           1001  2015-11-05 03:34:33 
3           1001  2015-12-04 04:21:55 
4           1001  2016-01-02 01:18:24 
5           1001  2016-02-01 19:27:27 
6           1001  2016-03-02 15:29:32 
7           1001  2016-04-01 11:20:18 
8           1001  2016-05-01 15:55:51 
9           1001  2016-05-31 21:22:48 
10          1001  2016-06-30 12:32:05 

</div>


```r
orders %>% 
  select(id, account_id, occurred_at)
```

```
#> # A tibble: 6,912 x 3
#>       id account_id occurred_at        
#>    <int>      <int> <dttm>             
#>  1     1       1001 2015-10-06 17:31:14
#>  2     2       1001 2015-11-05 03:34:33
#>  3     3       1001 2015-12-04 04:21:55
#>  4     4       1001 2016-01-02 01:18:24
#>  5     5       1001 2016-02-01 19:27:27
#>  6     6       1001 2016-03-02 15:29:32
#>  7     7       1001 2016-04-01 11:20:18
#>  8     8       1001 2016-05-01 15:55:51
#>  9     9       1001 2016-05-31 21:22:48
#> 10    10       1001 2016-06-30 12:32:05
#> # ... with 6,902 more rows
```

## Limit the number of rows returned with `head()`

As you were taught in the course, the **LIMIT** statement is useful when you want to see just the first few rows of a table. In SQL, this can be much faster for loading than if we load the entire dataset. This statement also holds true within R.

For example, the following query will return all columns for the first 3 rows from the orders table.


```sql
SELECT *
FROM orders
LIMIT 3;
```


<div class="knitsql-table">


Table: (\#tab:unnamed-chunk-12)3 records

id    account_id  occurred_at            standard_qty   gloss_qty   poster_qty   total   standard_amt_usd   gloss_amt_usd   poster_amt_usd   total_amt_usd
---  -----------  --------------------  -------------  ----------  -----------  ------  -----------------  --------------  ---------------  --------------
1           1001  2015-10-06 17:31:14             123          22           24     169             613.77          164.78           194.88          973.43
2           1001  2015-11-05 03:34:33             190          41           57     288             948.10          307.09           462.84         1718.03
3           1001  2015-12-04 04:21:55              85          47           NA     132             424.15          352.03             0.00          776.18

</div>

The equivalent function in R is `head()`, where the argument needed is the number of rows to return. To show just the first 10 rows of the orders table with all columns, you could write the following code: 


```r
orders %>% 
  select(everything()) %>% 
  head(3)
```

```
#> # A tibble: 3 x 11
#>      id account_id occurred_at         standard_qty gloss_qty poster_qty
#>   <int>      <int> <dttm>                     <int>     <int>      <int>
#> 1     1       1001 2015-10-06 17:31:14          123        22         24
#> 2     2       1001 2015-11-05 03:34:33          190        41         57
#> 3     3       1001 2015-12-04 04:21:55           85        47         NA
#> # ... with 5 more variables: total <int>, standard_amt_usd <dbl>,
#> #   gloss_amt_usd <dbl>, poster_amt_usd <dbl>, total_amt_usd <dbl>
```

**Your Turn**

Convert the following query to R code. Make sure the R output matches the SQL output.


```sql
SELECT *
FROM web_events
LIMIT 15
```


<div class="knitsql-table">


Table: (\#tab:unnamed-chunk-14)Displaying records 1 - 10

id    account_id  occurred_at           channel 
---  -----------  --------------------  --------
1           1001  2015-10-06 17:13:58   direct  
2           1001  2015-11-05 03:08:26   direct  
3           1001  2015-12-04 03:57:24   direct  
4           1001  2016-01-02 00:55:03   direct  
5           1001  2016-02-01 19:02:33   direct  
6           1001  2016-03-02 15:15:22   direct  
7           1001  2016-04-01 10:58:55   direct  
8           1001  2016-05-01 15:26:44   direct  
9           1001  2016-05-31 20:53:47   direct  
10          1001  2016-06-30 12:09:45   direct  

</div>


```r
web_events %>% 
  select(everything()) %>% 
head(15)
```

```
#> # A tibble: 15 x 4
#>       id account_id occurred_at         channel
#>    <int>      <int> <dttm>              <chr>  
#>  1     1       1001 2015-10-06 17:13:58 direct 
#>  2     2       1001 2015-11-05 03:08:26 direct 
#>  3     3       1001 2015-12-04 03:57:24 direct 
#>  4     4       1001 2016-01-02 00:55:03 direct 
#>  5     5       1001 2016-02-01 19:02:33 direct 
#>  6     6       1001 2016-03-02 15:15:22 direct 
#>  7     7       1001 2016-04-01 10:58:55 direct 
#>  8     8       1001 2016-05-01 15:26:44 direct 
#>  9     9       1001 2016-05-31 20:53:47 direct 
#> 10    10       1001 2016-06-30 12:09:45 direct 
#> 11    11       1001 2016-07-30 03:06:26 direct 
#> 12    12       1001 2016-08-28 06:42:42 direct 
#> 13    13       1001 2016-09-26 23:14:59 direct 
#> 14    14       1001 2016-10-26 20:21:09 direct 
#> 15    15       1001 2016-11-25 22:52:29 direct
```

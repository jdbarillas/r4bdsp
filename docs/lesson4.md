

## Lesson 4 Variability

In this exercise we will start by looking at the variability of our dataset. All definitions and data come from the original Udacity lesson and can be found [here.](https://storage.googleapis.com/supplemental_media/udacityu/1471748603/Lesson4.pdf "Lesson4.pdf")

### Data
We start by assigning data to income and looking at the summary statistics. I am also going to grab all of the stats to use for labels.

```r
income <- c(2500, 3000, 2900
            ,2650, 3225, 2700
            ,2740, 3000, 3400
            ,2500, 3100, 2700)

summary(income)
```

```
#>    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#>    2500    2688    2820    2868    3025    3400
```

```r
#names(income_stats)
Income_min <- min(income)
Income_max <- max(income)
Income_med <- median(income)
Income_lowQ <- unname(quantile(income, c(0.25)))
Income_highQ <- unname(quantile(income, c(0.75)))
```

### Interquartile range
The Interquartile range (IQR) is the distance between the 1st quartile and 3rd quartile and gives us the range of the middle 50% of our data.
The IQR is easily found by computing: Q3 - Q1

### Box Plot
A good way to view this is using a box plot. The IQR will be displayed within the box between Q1 and Q3.


```r
boxplot(income, col = "lightblue")
text(x = .75,y = Income_min, labels = "min")
text(x = .75,y = Income_max, labels = "max")
text(x = .75,y = Income_med, labels = "median")
text(x = .75,y = Income_lowQ, labels = "Q1")
text(x = .75,y = Income_highQ, labels = "Q3")
```

<img src="lesson4_files/figure-html/unnamed-chunk-3-1.png" width="672" />

### Outliers
You can use the IQR to identify outliers:

1. Upper outliers: Q3 + (1.5 * IQR)

2. Lower outliers: Q1 - (1.5 * IQR)

```r
IQR <- Income_highQ - Income_lowQ
upper_outlier <- Income_highQ + (1.5 * IQR)
lower_outlier <- Income_lowQ - (1.5 * IQR)
cat(paste0("The Income IQR = $",IQR,"\n",
              "Upper Outliers are above: $", upper_outlier,"\n",
              "Lower Outliers are below: $",lower_outlier))
```

```
#> The Income IQR = $337.5
#> Upper Outliers are above: $3531.25
#> Lower Outliers are below: $2181.25
```

### Variance
The variance is the average of the squared differences from the mean. The formula for computing variance is:
$$\sigma^{2} = \frac{\sum_{i=1}^{n} 
  \left(x_{i} - \bar{x}\right)^{2}}
  {n-1}$$


```r
# first let's calculate it from scratch
# get the mean of income
s_mean <- mean(income)
# get the difference between each income and the mean
r_diff <- function(x){x - s_mean}
diff_of_mean <- r_diff(income)
# get the sqr_root of each data point
sqr_of_diff <- diff_of_mean^2
#sum the squares
sum_sqrs <- sum(sqr_of_diff)
# Divide by the number of samples - 1
Var_income <- sum_sqrs / (length(income) - 1)
print(paste0("The variance caluclated by hand: ", Var_income))
```

```
#> [1] "The variance caluclated by hand: 81033.9015151515"
```

```r
# we can also use the built in function in R
print(paste0("The variance using the R built in function: ",var(income)))
```

```
#> [1] "The variance using the R built in function: 81033.9015151515"
```

### Standard Deviation
The standard deviation is the square root of the variance and is used to measure distance from the mean. In a normal distribution 65% of the data lies within 1 standard deviation from the mean,95% within 2 standard deviations, and 99.7% within 3 standard deviations.


```r
std_income <- round(sqrt(Var_income),2)

print(paste0("The standard deviation caluclated by hand: ", std_income))
```

```
#> [1] "The standard deviation caluclated by hand: 284.66"
```

```r
# we can also use the built in function in R
print(paste0("The standard deviation using the R built in function: ",round(sd(income),2)))
```

```
#> [1] "The standard deviation using the R built in function: 284.66"
```

```r
pos_1_std <- s_mean + std_income
pos_2_std <- s_mean + std_income * 2
neg_1_std <- s_mean - std_income
neg_2_std <- s_mean - std_income * 2
```

Let's visualize this. I add in lines for one and two standard deviations but, because the data are not normal the standard deviations fall outside of the ranges defined by the three sigma rule of thumb. 


```r
hist(income, probability = TRUE, col = "orange")
lines(density(income))
abline(v = s_mean, col = "blue", )
abline(v = pos_1_std, col="blue", lty = "dashed")
abline(v = neg_1_std, col="blue", lty = "dashed")
abline(v = pos_2_std, col="red", lty = "dashed")
abline(v = neg_2_std, col="red", lty = "dashed")
text(s_mean - 20,.0015, expression(mu))
text(pos_1_std - 40,.0015, expression(mu + sigma))
text(neg_1_std - 40,.0015, expression(mu - sigma))
text(pos_2_std - 40,.0015, expression(mu + 2*sigma))
```

<img src="lesson4_files/figure-html/unnamed-chunk-7-1.png" width="672" />

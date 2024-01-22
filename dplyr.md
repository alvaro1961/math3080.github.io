# DPLYR and Conditional Probability

dplyr is a popular R library used to manipulate data consistently and efficiently. It uses a set of functions, that they call "verbs". These functions are used to solve the most common data manipulation problems. The main verbs are:

* *mutate* to create new columns. The syntax is 
```
mutate(data_frame, new_column_name = expression)
```
* *filter* to select rows. The syntax is 
```
filter(data_frame,condition)
```
* *select* to select columns. The syntax is 
```
select(data_frame, column1,column2,...,columnk)
```
* *arrange* reorders the rows. The syntax is
```
arange(data_frame,column1,columns2,...)
```

These functions are combined with group_by() and summarise() to easily perform group-wise operations and compute summary statistics within those groups.

To use a library we type library(name_of_library). If the library is not intalled, we install it through Packages in RStudio.


## Conditional Probability - dice {#conditional-probability-dice}

:::{.example}
You roll two dice. Estimate the conditional probability that the first die is 6 if the sum is 10
:::

Step 1: Select a large number of simulations and generate the dice data.

```r
n <- 10000
die1 <- sample(1:6,n,replace = T)
die2 <- sample(1:6,n,replace = T)
```

Step 2: Make the data into a dataframe.

```r
df <- data.frame(die1,die2)
head(df)
#>   die1 die2
#> 1    5    2
#> 2    1    3
#> 3    5    6
#> 4    5    4
#> 5    5    6
#> 6    4    5
```
Step 3: Use mutate to create a new column that includes the sum.

```r
dfm <- mutate(df,sum = die1+die2)
head(dfm)
#>   die1 die2 sum
#> 1    5    2   7
#> 2    1    3   4
#> 3    5    6  11
#> 4    5    4   9
#> 5    5    6  11
#> 6    4    5   9
```

Step 4: Select the rows that add up to ten. This is the key point of conditional probability.

```r
dff <- filter(dfm,sum == 10)
head(dff)
#>   die1 die2 sum
#> 1    6    4  10
#> 2    6    4  10
#> 3    4    6  10
#> 4    6    4  10
#> 5    4    6  10
#> 6    4    6  10
```

Step 5: Extract the column that has the first die, and find the proportion of sixes.

```r
mean(dff$die1 == 6)
#> [1] 0.3591636
```


## The Pipe Operator (%>%) in DPLYR

Saving new dataframes every time we perform new operations is tedious and it can be confusing. In dplyr, the pipe operator (%>%) passes the result of one operation as the first argument to another operation. This improves code readability by creating a concise syntax that chains multiple operations together. 

For example, these four lines reproduce the previous operations and it adds a new column named ***experiment*** that checks if die1 is equal to 6. 



```r
df %>% 
  mutate(sum = die1+die2) %>%
  filter(sum == 10) %>%
  mutate(experiment = die1 == 6) %>%
  head()
#>   die1 die2 sum experiment
#> 1    6    4  10       TRUE
#> 2    6    4  10       TRUE
#> 3    4    6  10      FALSE
#> 4    6    4  10       TRUE
#> 5    4    6  10      FALSE
#> 6    4    6  10      FALSE
```



## Conditional Probability - test for disease  {#conditional-probability-test} 

:::{.example}
A new test was developed for a disease that only 1% of the population has. The test identifies individuals with the disease 90% of the time. However, it also wrongly identifies individuals without the disease as positive 5% of the time. Estimate the probability that a person who tests positive for the disease actually has the disease.
:::
Notice that this test has a 10% false negative rate and a 5% false positive rate.

We first choose a large number of simulations and then proceed to randomly assign individuals. Each person is assigned a value of 1 with a probability of 1%, indicating the presence of the disease, or 0 with a probability of 99%, signifying the absence of the disease.



```r
n <- 100000
people <- sample(c(0,1),n,replace = T,prob = c(0.99,0.01)) 
```

Then we write the function that simulates the test for the disease.

```r
test <- function(x){
  if (x==1) {
    value = sample(c(0,1),1,prob = c(0.1,0.9))
  } else {
    value = sample(c(0,1),1,prob = c(0.95,0.05))
  }
  return(value)
}
```

Now we apply the test to each person. Since the function has conditionals, we use the sapply() function.

```r
results <- sapply(people, test)
```


```r
df <- data.frame(people,results)
head(df)
#>   people results
#> 1      0       0
#> 2      0       1
#> 3      0       0
#> 4      0       1
#> 5      0       0
#> 6      0       0
```

Now we select the people who tested positive, and we use the base R function ***table*** to find out how many of them have the disease and how many don't.

The function ***table*** counts the occurrences of each unique value in a given vector.


```r
positive <- filter(df,results==1)
head(positive)
#>   people results
#> 1      0       1
#> 2      0       1
#> 3      0       1
#> 4      0       1
#> 5      0       1
#> 6      1       1
```


```r
table(positive$people)
#> 
#>    0    1 
#> 5006  957
```
The output of the table function can be plot directly as a pie plot or as a bar plot.


```r
pie(table(positive$people))
```

![](dplyr_files/figure-epub3/unnamed-chunk-14-1.png)<!-- -->

```r
barplot(table(positive$people))
```

![](dplyr_files/figure-epub3/unnamed-chunk-14-2.png)<!-- -->

We can easily add titles and make the graphs more informative


```r
pie(table(positive$people),main="People who tested positive", labels=c("do not have the disease","have the disease"), col=c("#4CAF50","#FF5252"))
```

![](dplyr_files/figure-epub3/unnamed-chunk-15-1.png)<!-- -->



```r
barplot(table(positive$people),main="People who tested positive", names.arg = c("do not have the disease","have the disease"), col=c("#4CAF50","#FF5252"))
```

![](dplyr_files/figure-epub3/unnamed-chunk-16-1.png)<!-- -->




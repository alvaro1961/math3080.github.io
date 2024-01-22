
# Simulations

R is widely used for simulations due to its extensive set of statistical and mathematical functions, ease of coding, and strong support for random number generation. R's flexibility allows researchers and analysts to implement various simulation models.

In this class we will use the **sample** function and the **replicate** function extensively.

## The sample function

The ```sample()``` function in R is used to randomly sample elements of a vector. 
The basic syntax is: for a vector ```x```, ```sample(x,size,replace=FALSE,prob=NULL)```

The arguments: *size*, *replace* and *prob* are optional. If they are not included, size is the length of the vector, replace is ```FALSE``` and prob is ```NULL```. In this case, we simply get a random permutation of the vector. For example


```r
sample(1:6)
#> [1] 3 4 6 1 2 5
```
The sample function can be used to simulate several experiments

### Getting objects out of a Box {-}

Suppose that we have a box with 3 red balls, 2 yellow ones and 1 green. We represent the box by the vector 
```
box <- c("red","red","red","yellow","yellow","green")
```

1. To randomly select one ball from the box we write ```sample(box,1)```
2. To randomly select two balls from the box we write ```sample(box,2)```. Notice that this doesn't include replacement. We either take two balls, or we take one ball and then another one.
3. To select two balls with replacement means that we select one ball. We look at it. Put it back in the box and then select a second one at random. To do this we write ```sample(box,2,replace=T)```. 


### Rolling dice {-}

1. To roll a die we write ```sample(1:6,1)```
2. To roll two dice we write ```sample(1:6,2,replace=TRUE)```.  Notice that we need to add ```replace=TRUE``` because we can get the same number.

### Selecting a birthday at random {-}

1. To select a birthday at random we write ```sample(1:365,1)```. We ignore leap years and assume that birthdays are described by numbers from 1 to 365. January 1st is 1. January 2nd is 2, etc.
2. To select 10 birthdays at random, we write ```sample(1:365,10,replace=T)```. Notice that we need to add the replace parameter since birthdays can be the same.
3. If we want to select 10 **different** birthdays at random, we write ```sample(1:365,10)```. 

### Getting objects out of a Box using the prob Argument {-}

The following is an alternative way to select one ball at random from a box that contains 3 red balls, 2 yellow ones and 1 green. 

```r
sample(c("red","yellow","green"),1,prob = c(3,2,1))
#> [1] "green"
```
The optional *prob* argument is assigned a vector of weights for obtaining the elements of the vector being sampled. They do not need to sum to one. 

### Simulating a Test with False Positives and False Negatives {-}

Consider a diagnostic test for a specific medical condition. The test identifies individuals with the disease 90% of the time. However, it also wrongly identifies individuals without the disease as positive 8% of the time. This test has a 10% false negative rate and a 8% false positive rate.

Let's simulate this test in R. We start by agreeing that the input is 0 or 1 (0 means that the person doesn't have the disease and 1 means that the person has the disease) and that the output is also 0 and 1 (0 means the test is negative and 1 means the test is positive).


```r
test <- function(x){
  if (x==1) {
    value = sample(c(0,1),1,prob = c(0.1,0.9))
  } else {
    value = sample(c(0,1),1,prob = c(0.92,0.08))
  }
  return(value)
}
```


Let's see the results of the test on 40 **healthy** people. We use the ```rep( )``` function.  Notice that the function ```test``` has conditionals. To [vectorize it](#vectorize), we use the ```sapply``` base R function as was [described before](#vectorize)

```r
healthy <- rep(0,40)
healthy
#>  [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
#> [29] 0 0 0 0 0 0 0 0 0 0 0 0
resultsH <- sapply(healthy, test)
resultsH
#>  [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0
#> [29] 0 0 0 0 0 0 0 0 0 0 1 0
```

And now let's see the results on 40 **sick** people

```r
sick <- rep(1,30)
sick
#>  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
#> [29] 1 1
resultsS <- sapply(sick, test)
resultsS
#>  [1] 1 1 0 1 0 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1
#> [29] 1 1
```

## The Replicate Function

The ```replicate( )``` function in R is used to replicate the execution of an expression or a function multiple times. The basic syntax is ```replicate(n,expression)```. For example

1. To roll a die 10 times we write ```replicate(10,sample(1:6,1))```. We can also do this using the sample function.
2. To select 1 ball from the box 10 times, we write ```replicate(10,sample(box,1))```. 

## Simulating Experiments

We combine the sample function, the replicate function and vector functions to simulate more complicated experiments. We illustrate this with these examples:

:::{.example}
Roll a die 20 times and count the number of 6's.
:::

First we roll a die 20 times


```r
values <- sample(1:6,20,replace = T)
values
#>  [1] 5 5 5 6 5 6 5 1 4 1 2 1 3 1 6 2 2 3 4 2
```

Then we check the values that are equal to 6


```r
values == 6
#>  [1] FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE
#> [10] FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE
#> [19] FALSE FALSE
```

R treats ```FALSE``` values as 0's and ```TRUE``` values as 1's. If we sum these values we find out the number of 6's


```r
sum(values == 6)
#> [1] 3
```

:::{.example}
Run the previous experiment 100 times and find the average.
:::

We use the replicate function to run the experiment 100 times and then we use the ```mean()``` function to find the average


```r
exper <-replicate(100,sum(sample(1:6,20,replace=T) == 6))
exper 
#>   [1]  3  2  2  2  1  2  7  3  2  3  3  5  1  2  7  1  3  4
#>  [19]  6  4  1  7  3  2  4  8  4  2  3  4  4  0  1  4  3  4
#>  [37]  2  4  4  5  3  5  2  2  3  1  6  3  3  2  0  5  5  4
#>  [55]  1  3  4 11  2  6  2  4  5  4  4  4  3  1  8  2  1  4
#>  [73]  5  0  2  3  1  3  4  3  2  4  4  3  5  3  8  4  3  5
#>  [91]  6  4  2  3  4  3  2  1  4  4
mean(exper)
#> [1] 3.4
```

To simplifying reading the code or to handle more complex situations, we can wrap the expression in curly braces to compute it.


```r
replicate(100,{
  values <- sample(1:6,20,replace=T)
  sum(values == 6)
})
#>   [1] 6 5 2 3 5 3 2 4 5 2 3 5 5 3 2 4 2 3 2 1 3 4 7 5 2 1 2
#>  [28] 4 6 1 3 3 5 6 3 7 3 5 7 2 1 4 2 5 2 3 4 5 3 3 3 2 4 5
#>  [55] 4 5 2 5 4 3 2 7 5 4 2 5 5 3 5 3 2 8 3 5 1 3 2 1 5 5 2
#>  [82] 2 4 3 2 6 1 3 2 4 3 3 3 6 8 3 4 4 5 3
```
By default, the function replicates the value of the last line


```r
replicate(100,{
  values <- sample(1:6,20,replace=T)
  sum(values == 6)
  8
})
#>   [1] 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
#>  [28] 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
#>  [55] 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
#>  [82] 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
```




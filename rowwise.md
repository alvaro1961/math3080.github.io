# Row Wise Operations

In R and in dplyr it is easier to perform column operations than row operations. We will illustrate this with the following data frame:




```r
df
#>   x y  z
#> 1 5 1  9
#> 2 6 2 10
#> 3 7 3 11
#> 4 8 4 12
```


If we add the columns using the variables we get the right result.


```r
mutate(df,sum = x+y+z)
#>   x y  z sum
#> 1 5 1  9  15
#> 2 6 2 10  18
#> 3 7 3 11  21
#> 4 8 4 12  24
```

On the other hand, if we use vectorized operators (i.e., we convert ```x,y,z`` into a vector and add it), we get the wrong answer. R (or dplyr) computes the sum accross all the rows:


```r
mutate(df, sum = sum(c(x,y,z)))
#>   x y  z sum
#> 1 5 1  9  78
#> 2 6 2 10  78
#> 3 7 3 11  78
#> 4 8 4 12  78
```

Something similar happens if we use functions. 

The following block has four functions. We use the first two to compute the sum of the columns and the last two to select an element from each row. The first function computes the sum adding the variables. The second one vectorizes the variables and adds them. The third function vectorizes the variables and selects one at random, and the last one avoids vectorizing the variables with if then statements.


```r
suma1 <- function(a,b,c){
  return(a+b+c)
}

suma2 <- function(vec){
  return(sum(vec))
}

choose_one1 <- function(a,b,c){
  return(sample(c(a,b,c),1))
}

choose_one2 <- function(a,b,c){
  n <- sample(1:3,1)
  if (n==1){
    return(a)
  } else if (n==2){
    return(b)
  } else {
    return(c)
  }
}
```

Then we add a column for each function


```r
mutate(df,sum1 = suma1(x,y,z), sum2 = suma2(c(x,y,z)), choose_one1 = choose_one1(x,y,z), choose_one2 = choose_one2(x,y,z))
#>   x y  z sum1 sum2 choose_one1 choose_one2
#> 1 5 1  9   15   78          11           5
#> 2 6 2 10   18   78          11           6
#> 3 7 3 11   21   78          11           7
#> 4 8 4 12   24   78          11           8
```

As you see, the first sum works well but he second one doesn't. The other functions don't work well either. ***choose_one1*** selected one element from all the rows. ***choose_one2*** selected different elements, but all were chosen from the same column. 

## rowwise()

To perform row operations properly, we "group" the data across each row, using the function ```rowwise()```.  We illustrate it's use with the previous data frame and using the pipe operator. We start from the data frame, then we group the data frame by rows, and finally we add the four columns using the previous functions.



```r
df %>%
  rowwise() %>%
  mutate(sum1 = suma1(x,y,z),sum2=suma2(c(x,y,z)),choose_one1 = choose_one1(x,y,z), choose_one2 = choose_one2(x,y,z))
#> # A tibble: 4 x 7
#> # Rowwise: 
#>       x     y     z  sum1  sum2 choose_one1 choose_one2
#>   <dbl> <dbl> <dbl> <dbl> <dbl>       <dbl>       <dbl>
#> 1     5     1     9    15    15           9           9
#> 2     6     2    10    18    18           2           6
#> 3     7     3    11    21    21           7           7
#> 4     8     4    12    24    24           8           4
```

Notice that in this case, all the operations worked well.  The last two columns are different because they were selected randomly. Nevertheless, each of them selects one element from each row.


## ungroup()
The ```rowwise()``` operator does not change the data, but it groups it. If we look carefully at the output, we see that the table says "# Rowwise" to let us know that the data is grouped by rows. To remove the grouping, we add the ```ungroup()``` operator.



```r
df %>%
  rowwise() %>%
  mutate(sum1 = suma1(x,y,z),sum2=suma2(c(x,y,z)),choose_one1 = choose_one1(x,y,z), choose_one2 = choose_one2(x,y,z)) %>%
  ungroup()
#> # A tibble: 4 x 7
#>       x     y     z  sum1  sum2 choose_one1 choose_one2
#>   <dbl> <dbl> <dbl> <dbl> <dbl>       <dbl>       <dbl>
#> 1     5     1     9    15    15           5           5
#> 2     6     2    10    18    18           2           2
#> 3     7     3    11    21    21           7           3
#> 4     8     4    12    24    24           8           4
```

Now the output of the table doesn't include "# Rowwise". 

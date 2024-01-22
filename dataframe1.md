# Introduction to Data Frame

## Data Frames

Data frames in R are two-dimensional data structures, similar to spreadsheets. They are used to store and manipulate datasets. The key points are:

1. *Structure*: A data frame is a collection of vectors of equal length, where each vector represents a column. Recall that all the elements of a vectors have the same type.

1. *Columns*: Different columns can have different data types (numeric, character, factor, etc.).

1. *Rows*: Each row corresponds to a separate observation or record in the dataset.


### Creating Data Frames
Data frames are created using the ```data.frame()``` function or by importing data sources.

One option is to define the column vectors and to combine them into a data frame using the ```data.frame()``` function as shown in this example:  

```r
name = c("John Smith","Jane Doe", "Mary Johnson")
a <- c(NaN,16,3)
b<- c(2,11,1)
df1 <- data.frame(name,a,b)
knitr::kable(df1)
```



|name         |   a|  b|
|:------------|---:|--:|
|John Smith   | NaN|  2|
|Jane Doe     |  16| 11|
|Mary Johnson |   3|  1|
We used the ```knitr::kable()``` function to have a nicer display.

Data frames can also be defined inside the data.frame() function directly, naming the columns and the values. 


```r
df2 <- data.frame(
  name = c("John Smith","Jane Doe","Mary Johnson","John Smith","Jane Doe","Mary Johnson"),
  treatment =c("a","a","a","b","b","b"),
  results = c(NaN,16,3,2,11,1)
)
knitr::kable(df2)
```



|name         |treatment | results|
|:------------|:---------|-------:|
|John Smith   |a         |     NaN|
|Jane Doe     |a         |      16|
|Mary Johnson |a         |       3|
|John Smith   |b         |       2|
|Jane Doe     |b         |      11|
|Mary Johnson |b         |       1|



Notice that the data frames df1 and df2 have the same information.  We will later learn that df2 is preferred over df1. We will learn how to convert a data frame like df1 into a data frame like df2. df2 has a "tidy" format that fits specially well with many libraries of R.

### Importing data sets from R, head(), and tail() {-}

R has several data sets that can be imported using the ```data()``` function.  If you write data() inside an R block and run it, you will see a list of all data sets. 

For illustration, we will import the iris data set that has 150 rows. We will use the ```head()``` function to see only the first 6 rows. The ```tail()``` function is similar, but it shows the last 6 rows by default. 


```r
data(iris)
head(iris) 
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
#> 1          5.1         3.5          1.4         0.2  setosa
#> 2          4.9         3.0          1.4         0.2  setosa
#> 3          4.7         3.2          1.3         0.2  setosa
#> 4          4.6         3.1          1.5         0.2  setosa
#> 5          5.0         3.6          1.4         0.2  setosa
#> 6          5.4         3.9          1.7         0.4  setosa
```

### Extracting columns from a data frame{-}

The syntax to extract columns is data_frame$column_name. Recall that the columns of a data frame are vectors.


```r
iris$Sepal.Length
#>   [1] 5.1 4.9 4.7 4.6 5.0 5.4 4.6 5.0 4.4 4.9 5.4 4.8 4.8
#>  [14] 4.3 5.8 5.7 5.4 5.1 5.7 5.1 5.4 5.1 4.6 5.1 4.8 5.0
#>  [27] 5.0 5.2 5.2 4.7 4.8 5.4 5.2 5.5 4.9 5.0 5.5 4.9 4.4
#>  [40] 5.1 5.0 4.5 4.4 5.0 5.1 4.8 5.1 4.6 5.3 5.0 7.0 6.4
#>  [53] 6.9 5.5 6.5 5.7 6.3 4.9 6.6 5.2 5.0 5.9 6.0 6.1 5.6
#>  [66] 6.7 5.6 5.8 6.2 5.6 5.9 6.1 6.3 6.1 6.4 6.6 6.8 6.7
#>  [79] 6.0 5.7 5.5 5.5 5.8 6.0 5.4 6.0 6.7 6.3 5.6 5.5 5.5
#>  [92] 6.1 5.8 5.0 5.6 5.7 5.7 6.2 5.1 5.7 6.3 5.8 7.1 6.3
#> [105] 6.5 7.6 4.9 7.3 6.7 7.2 6.5 6.4 6.8 5.7 5.8 6.4 6.5
#> [118] 7.7 7.7 6.0 6.9 5.6 7.7 6.3 6.7 7.2 6.2 6.1 6.4 7.2
#> [131] 7.4 7.9 6.4 6.3 6.1 7.7 6.3 6.4 6.0 6.9 6.7 6.9 5.8
#> [144] 6.8 6.7 6.7 6.3 6.5 6.2 5.9
```
### Some Data Frame Functions{-}

The following is a list of some of the most useful data frame functions. In the next section we will import the library dplyr. This library handles functions 9-14 in a nicer way.

1. head(): Displays the first few rows of a data frame.
2. tail(): Displays the last few rows of a data frame.
3. str(): Shows the structure of a data frame.
4. summary(): Provides summary statistics for each column.
5. nrow(): Returns the number of rows in a data frame.
6. ncol(): Returns the number of columns in a data frame.
7. colnames(): Returns or sets the column names.
8. rownames(): Returns or sets the row names.
9. subset(): Subsets a data frame based on conditions.
subset(df, condition)
10. select(): Chooses specific columns.
select(df, column1, column2)
11. filter(): Filters rows based on conditions.
filter(df, condition)
12. mutate(): Adds new variables or modifies existing ones.
mutate(df, new_column = expression)
13. arrange(): Sorts rows based on one or more columns.
arrange(df, column1, column2)
14. merge(): Combines two data frames by common columns.
merge(df1, df2, by = "common_column")


We will illustrate some of these functions with the data frame iris.


```r
str(iris) #str shows the structure of the data set.
#> 'data.frame':	150 obs. of  5 variables:
#>  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
#>  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
#>  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
#>  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
#>  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
```

```r
summary(iris) # summary provides summary statistical for each column.
#>   Sepal.Length    Sepal.Width     Petal.Length  
#>  Min.   :4.300   Min.   :2.000   Min.   :1.000  
#>  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600  
#>  Median :5.800   Median :3.000   Median :4.350  
#>  Mean   :5.843   Mean   :3.057   Mean   :3.758  
#>  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100  
#>  Max.   :7.900   Max.   :4.400   Max.   :6.900  
#>   Petal.Width          Species  
#>  Min.   :0.100   setosa    :50  
#>  1st Qu.:0.300   versicolor:50  
#>  Median :1.300   virginica :50  
#>  Mean   :1.199                  
#>  3rd Qu.:1.800                  
#>  Max.   :2.500
```


```r
nrow(iris) # nrwon finds the number of rows
#> [1] 150
```


```r
colnames(iris) # colnames finds the names of the columns
#> [1] "Sepal.Length" "Sepal.Width"  "Petal.Length"
#> [4] "Petal.Width"  "Species"
```












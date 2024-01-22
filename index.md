--- 
title: "R Projects for Math 3080"
author: "Alvaro Arias"
date: "2024-01-22"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  R projects for Intro to Probability
biblio-style: apalike
csl: chicago-fullnote-bibliography.csl
---
# Introduction to R

We start reviewing fundamental concepts of the R language. 
We assume that you have downloaded and installed RStudio.

Base R refers to the core functionality and built-in functions of the R programming language without any additional packages or libraries. It provides essential data structures (vectors, matrices, data frames, lists), control structures (loops, conditionals), and basic statistical functions.

Libraries, or packages, in R are extensions that add specialized functions and capabilities beyond what's available in base R. They are developed by the R community to address specific tasks or domains. These libraries enhance R's functionality and make it a powerful tool for a wide range of data analysis and statistical tasks. 

## Variables and Data Types

In R, variables store data and are assigned using ```<-```. There are various types, including numeric (real or complex numbers), character (strings, written between quotes), and logical or boolean (TRUE or FALSE)



```r
x <- 2
y <- "I am a string"
z <- TRUE

print(x)
#> [1] 2
print(y)
#> [1] "I am a string"
print(z)
#> [1] TRUE
```

## Logical Operators and Conditionals

The logical operators perform logical operations on boolean values. They include:

* AND that is represented by $\wedge$ in logic and by ```&``` in R.
* OR that is represented by $\vee$ in logic and by ```|``` in R.
* NOT that is represented by $\neg$ in logic and by ```!``` in R.

AND and OR are formally defined by the truth tables below. Note that AND is TRUE only when both values are true, and OR is TRUE when at least one value is true.

| $p$   | $q$   |$p\wedge q$| $p\vee q$ |
|-------|-------|---------|-------------|
| TRUE | TRUE   | TRUE   | TRUE  |
| TRUE | FALSE  | FALSE  | TRUE |
| FALSE  | TRUE | FALSE  | TRUE |
| FALSE | FALSE |FALSE   | FALSE |

NOT is defined in the obvious way: $\neg p$ is FALSE if $p$ is TRUE and it is TRUE if $p$ is FALSE.

In R, ```==``` is used to check if the objects are equal and ```!=``` is used to check if the objects aren't equal. For example, ```1==2``` is FALSE and ```1!=2``` is TRUE.  The comparison symbols ```<``` and ```>``` have the same meaning we use in math. Less than or equal $\leq$ is ```<=``` and greater than or equal $\geq$ is  ```>=```.

Conditionals in R are structures that allow us to execute different blocks of code based on specified conditions. The main conditional statement is the **if statement**, which can be extended to an **if else statement** and **else if clauses**. They have the following syntax:

```
if (condition) {
  # Code to execute when the condition is TRUE
}
```

```
if (condition) {
  # Code to execute when the condition is TRUE
} else {
  # Code to execute when the condition is FALSE
}
```

```
if (condition1) {
  # Code to execute when condition1 is TRUE
} else if (condition2) {
  # Code to execute when condition1 is FALSE and condition2 is TRUE
} else {
  # Code to execute when both condition1 and condition2 are FALSE
}
```

## Functions
The functions in R have the following syntax:
```
function_name <- function(arg1, arg2, ...) {
  # Code defining the function's behavior
  return(result)
}
```
The functions $f$ and $g$ below demonstrate simple examples. The function $g$ also illustrates the use of conditionals. 

$$f(x)=2x^2-3x-1$$
$$g(x)=\begin{cases}2x+1&\text{if }x<0\\x^2 &\text{otherwise} \end{cases}$$

```r
f <- function(x){
  return(2*x^2-3*x-1)
}

g <- function(x) {
  if (x < 0) {
    result <- 2 * x+1
  } else {
    result <- x^2
  }
  return(result)
}
```

We can evaluate the functions.

```r
f(0)
#> [1] -1
g(-2)
#> [1] -3
```
## Vectors and Operations

Vectors are fundamental data structures in R that provide a flexible way to work with and manipulate sequences of values.

In R, vectors are one-dimensional arrays that store elements of the *same data type*. Vectors are often created using the ```c( )``` function and they can be used to represent situations, like a box with 3 red balls, 2 yellow ones and 1 green.


```r
numeric_vector <- c(1,2,3,4,5)
box <- c("red","red","red","yellow","yellow","green")
```

The ***colon*** operator ``` : ``` is used to create sequences of numbers with steps of length 1 or -1. The basic syntax is ```start:end```. Here are a few examples:


```r
1:10
#>  [1]  1  2  3  4  5  6  7  8  9 10
20:12
#> [1] 20 19 18 17 16 15 14 13 12
3:8
#> [1] 3 4 5 6 7 8
```

## Indexing and Subsets

Elements in a vector are accessed using square brackets and indices. ***Indexing starts at 1 in R,*** which is different from most programming languages, where indexing typically starts at 0. 


```r
x <- box[3]
x
#> [1] "red"
```
We can also select a subset of elements using index vectors or boolean vectors.

```r
box[2:4]
#> [1] "red"    "red"    "yellow"
box[c(3,4,6)]
#> [1] "red"    "yellow" "green"
numbers <- 1:6
numbers[c(T,F,T,F,T,F)]
#> [1] 1 3 5
```

## Vectorized Operations

R supports *vectorized operations*, allowing us to perform operations on entire vectors without the need for explicit loops. Recall that ```numeric_vector <- c(1,2,3,4,5)```.


```r
numeric_vector*(-3)
#> [1]  -3  -6  -9 -12 -15
numeric_vector^3
#> [1]   1   8  27  64 125
numeric_vector+numeric_vector
#> [1]  2  4  6  8 10
```

Vectorized operation also supports logical operations 


```r
numeric_vector>2
#> [1] FALSE FALSE  TRUE  TRUE  TRUE
box == "yellow"
#> [1] FALSE FALSE FALSE  TRUE  TRUE FALSE
```
We can combine these features to select objects satisfying certain properties

:::{.example}
Suppose that ```x<-c(2,1,4,1,5,1,6,2)```. Find the elements that are larger than 2 
:::


```r
x<-c(2,1,4,1,5,1,6,2)
x
#> [1] 2 1 4 1 5 1 6 2
y <- x>2
y
#> [1] FALSE FALSE  TRUE FALSE  TRUE FALSE  TRUE FALSE
x[y]
#> [1] 4 5 6
```
If we want to know the indices that have values greater than 2, we use the ```which()``` function

```r
which(y)
#> [1] 3 5 7
```

We can select objects using more complicated expressions that involve logical operators.

:::{.example}
Suppose that ```x<-c(2,1,4,1,5,1,6,2)```. Find the elements that are larger than 1 and less than 6 
:::


```r
x
#> [1] 2 1 4 1 5 1 6 2
y <- (x>1)&(x<6) #Recall that & is the AND operator in R
y
#> [1]  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE
x[y]
#> [1] 2 4 5 2
```


### Vectorization of functions {#vectorize}
Since many built-in functions in R can be applied directly to vectors, functions like $f(x)=2x^2-3x-1$, that only use these operations, can be applied to vectors as well

```r
x <- c(-1,0,1,2)
f(x)
#> [1]  4 -1 -2  1
```
However, this does not work with functions that have conditionals, like $g(x)=\begin{cases}2x+1&\text{if }x<0\\x^2 &\text{otherwise.} \end{cases}$

```r
x <- c(-1,0,1,2)
g(x)
#> Warning in if (x < 0) {: the condition has length > 1 and
#> only the first element will be used
#> [1] -1  1  3  5
```
Notice that $g$ evaluated all values using $2x+1$.

R has several ways to vectorize functions like $g$. One such solution is the ```sapply()``` from base R.


```r
x <- c(-1,0,1,2)
sapply(x,g)
#> [1] -1  0  1  4
```
### Graphs of Functions
Vectorization in R simplifies the process of generating plots when creating visualizations in R.


```r
x <- -10:10
y <- f(x)
plot(x,y)
```

![](index_files/figure-epub3/unnamed-chunk-16-1.png)<!-- -->

```r
# The optional arguments "type" and "col" allow us to change the graph
plot(x,y,type="l", col="red")
```

![](index_files/figure-epub3/unnamed-chunk-16-2.png)<!-- -->






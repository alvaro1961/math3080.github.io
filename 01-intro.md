# Hello bookdown 

All chapters start with a first-level heading followed by your chapter title, like the line above. There should be only one first-level heading (`#`) per .Rmd file.


```r
x <- -10:10
y <- x**2
plot(x,y,type="l",col = "red")
```

![](01-intro_files/figure-epub3/unnamed-chunk-1-1.png)<!-- -->


```python
for i in [1,3,4,5]:
  print(i**3)
#> 1
#> 27
#> 64
#> 125
```




## A section

All chapter sections start with a second-level (`##`) or higher heading followed by your section title, like the sections above and below here. You can have as many as you want within a chapter.


```python
import numpy as np
x = np.array([1,2,3,4])
x
#> array([1, 2, 3, 4])
```



### An unnumbered section {-}

Chapters and sections are numbered by default. To un-number a heading, add a `{.unnumbered}` or the shorter `{-}` at the end of the heading, like in this section.

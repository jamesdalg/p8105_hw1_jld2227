---
title: "hw1"
author: "James Dalgleish"
date: "September 13, 2018"
output: html_document
---


Here, we're creating a tibble, essentially a data.frame, with multiple data types, numeric, logical, character, and factor making up four distinct columns.
what didn't work:
letters(1:5)
"letters" is a built in vector rather than a function.

```r
rand10_greaterthan2_letters_factor_tibl <- tibble::tibble( 
  ten_random_0_to_5 = runif(n = 10, min = 0, max=5),
  logical_greater_than_two = ten_random_0_to_5 > 2,
  first_ten_letters = letters[ 1:10 ],
greater_than_two_factor= as.factor(logical_greater_than_two)

)
head(rand10_greaterthan2_letters_factor_tibl)
```

```
## # A tibble: 6 x 4
##   ten_random_0_to_5 logical_greater_t~ first_ten_lette~ greater_than_two_~
##               <dbl> <lgl>              <chr>            <fct>             
## 1            3.43   TRUE               a                TRUE              
## 2            1.14   FALSE              b                FALSE             
## 3            2.91   TRUE               c                TRUE              
## 4            0.585  FALSE              d                FALSE             
## 5            0.0880 FALSE              e                FALSE             
## 6            3.92   TRUE               f                TRUE
```
Now, we'll attempt to take the mean of all the columns... we'll note that some
of these columns are
not numeric and therefore a mean would be difficult to calculate.

```r
#taking the mean of every column won't work because 
#you can't take the mean of a nonnumeric column.
 tibl_means <- sapply( rand10_greaterthan2_letters_factor_tibl, mean)
```

```
## Warning in mean.default(X[[i]], ...): argument is not numeric or logical:
## returning NA

## Warning in mean.default(X[[i]], ...): argument is not numeric or logical:
## returning NA
```

```r
mean( rand10_greaterthan2_letters_factor_tibl$ten_random_0_to_5 )
```

```
## [1] 2.506073
```

```r
mean( rand10_greaterthan2_letters_factor_tibl$logical_greater_than_two )
```

```
## [1] 0.6
```

```r
mean( rand10_greaterthan2_letters_factor_tibl$first_ten_letters )
```

```
## Warning in mean.default(rand10_greaterthan2_letters_factor_tibl
## $first_ten_letters): argument is not numeric or logical: returning NA
```

```
## [1] NA
```

```r
mean( rand10_greaterthan2_letters_factor_tibl$greater_than_two_factor )
```

```
## Warning in mean.default(rand10_greaterthan2_letters_factor_tibl
## $greater_than_two_factor): argument is not numeric or logical: returning NA
```

```
## [1] NA
```

```r
#print(tibl_means)
```
You can actually take the mean of a factor, but it's something to avoid. It is
possible to have a factor that looks like a list of numbers that is not
a numeric variable, Using some if logic, we find we can take just the means
of the columns that are amenable to means.


```r
 #putting in a check to determine if it's numeric.
tibl_means <- sapply( rand10_greaterthan2_letters_factor_tibl,
                     function(x)
                     if ( is.numeric( x ) ) {
                     return( 
                       mean( x )
                       )
                     } else {
                     return( "Incompatible type for mean" )
                     })
 print(tibl_means)
```

```
##            ten_random_0_to_5     logical_greater_than_two 
##           "2.50607258232776" "Incompatible type for mean" 
##            first_ten_letters      greater_than_two_factor 
## "Incompatible type for mean" "Incompatible type for mean"
```
Converting the logical, character, and factor columns to numeric:

```r
tibl_as_num_log_char_num <-
  sapply( rand10_greaterthan2_letters_factor_tibl,
  function(x)
  if (is.logical(x) |
  is.character(x) |
  is.factor(x)) {
  return(as.numeric(x))
  } else {
  return(NULL)
  })
  print( tibl_as_num_log_char_num )
```
The logical TRUEs have been converted to 1s and the logical FALSEs have been
converted to 0s. The letters are completely incompatible and result with 
empty values (NAs). The logical vector can be converted to numeric...
so this is something to watch out for and avoid if not intended.

Converting character variable from character to factor to numeric:







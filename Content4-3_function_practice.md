---
title: "Content 4-3: Practice writing functions"
date: "Monday, November 11, 2024"
output: 
  html_document:
    number_sections: false
    toc: true
    toc_depth: 1
    keep_md: yes
---


In Module 4, we are learning to write functions.  In Content 4-3, we will practice writing functions.

*Note:* Our class materials for module 4 are adapted from "R for Data Science" by Grolemund and Wickham, chapter 19.  You can find this chapter [here](https://r4ds.had.co.nz/functions.html#when-should-you-write-a-function).

# Some key information from previous weeks

We create a new function using the following format:

``` r
function_name <- function(argument_1, argument_2, ...) {
  
# function's body
# code to do the function's task using the variables argument_1 and argument_2 from above
  
}
```

We can specify a **default value** by setting the default value in the argument portion of the `function()` function, such as `function(argument_1, argument_2 = default)`.  In this case argument 2 has a default value but argument 1 does not.  So the user will have to specify a value/object for argument 1 but could omit specifying argument 2.

We can use **conditional execution** if we want to run code only if a specific condition is met:

``` r
if (condition) {
  # code executed when condition is TRUE
} else {
  # code executed when condition is FALSE
}
```

We can **chain** multiple `if` statements together if we want to check a bunch of conditions in sequence:

``` r
if (this condition) {
  # do something
} else if (that condition) {
  # do something else
} else {
  # do a third thing if neither of previous conditions are met
}
```

We can **check** whether the values given to the function are valid using an `if` statement and then providing an error message to the user with the `stop()` function:

``` r
  if (condition) {
    stop("error message")
  }
```

We can pass arguments (like `na.rm = TRUE`) through our function to embedded functions using the `...` argument in our function:

``` r
function_name <- function(argument_1, argument_2, ...) {

  mean(argument1, ...)
  
}
```

The **standard return rule** says that a function returns the last value that it computes.  We can explicitly specify what should be returned or force a value to return early by using `return(value)` within our function.

# Practice

1. Write a function `f_to_c()` that converts temperatures from Fahrenheit to Celsius. Note that the formula for conversion from the temperature in Fahrenheit ($F$) to temperature in Celsius ($C$) is: $C = (F - 32) \times 5/9$


2. Write a `temp_conversion()` function that will convert temperatures from Fahrenheit to Celsius OR from Celsius to Fahrenheit, depending on what the user specifies.  Make the default be conversion from Fahrenheit to Celsius.  Note that the formula for conversion from the temperature in Celsius ($C$) to temperature in Fahrenheit ($F$) is: $F = (C \times 9/5) + 32$


3. Write a `logit()` function that calculates the log odds, $\log(p/(1-p))$ of an input.  Be sure to check that the input is a valid probability between 0 and 1.


4. Write a `birthday` function that inputs a date in the format `"mm-dd"` and says “Happy birthday!” if it's your birthday and "It's not your birthday yet!" if it's not your birthday.


5. Write a function called `is_binary()` that takes a vector as an argument and returns `TRUE` if the vector contains only 0s and 1s (and possibly `NA` values) and returns `FALSE` if the vector contains any values other than 0/1/`NA`. 






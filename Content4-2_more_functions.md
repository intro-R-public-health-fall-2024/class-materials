---
title: "Content 4-2: More on functions"
date: "Monday, Nov 4, 2024"
output: 
  html_document:
    number_sections: false
    toc: true
    toc_depth: 1
    keep_md: yes
---


In Module 4, we are learning to write functions.  In Class 4-2, we will learn about function **arguments** and **return values** as well as **conditional execution**.

*Note:* These class activities are adapted from "R for Data Science" by Grolemund and Wickham, chapter 19.  You can find this chapter [here](https://r4ds.had.co.nz/functions.html#when-should-you-write-a-function).

# Recap from last week

There are three key steps to creating a new function using the following format:

``` r
function_name <- function(argument_1, argument_2, ...) {
# function's body
# code to do the function's task using the variables argument_1 and argument_2 from above
  
}
```

(1) You need to pick a **name** for the function. You should make the name meaningful to the task the function does.

(2) You list the inputs of the function, called **arguments**, inside `function()`. For a single argument, we use `function(x)`.  For multiple arguments, we would have something like `function(x, y, z)`.  It can be helpful to give your arguments meaningful names as well!

(3) You place the code that does the task in the **body** of the function, a block between `{` and `}` that immediately follows `function(...)`.

**Example:** From Mini-Assignment 4-1, we wrote a function to calculate a confidence interval for a mean based on a sample from the population.


``` r
mean_ci <- function(x) {
  
  xbar <- mean(x, na.rm=TRUE)
  n <- sum(!is.na(x))
  se <- sd(x, na.rm=TRUE) / sqrt(n)
  xbar + c(-2, 2) * se
  
}

x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4250708 0.5440011
```

Let's quickly review a couple of common mistakes I saw in what you turned in, since you'll be building on this work for this week's assignment. Here is an example that combines two issues:


``` r
mean_ci <- function(x) {
  x <- rnorm(100)

  xbar <- mean(x, na.rm=TRUE)
  n <- length(x)
  se <- sd(x, na.rm=TRUE) / sqrt(n)
  xbar + c(-2, 2) * se
}
```

What is the data that this function will be operating on? What do we want to do with missing values when we calculate the value for `n`?


# Conditional execution

Sometimes when we write a function, we only want some of the code to run if a specific condition is met.  We can  conditionally execute code using an `if` statement:

``` r
if (condition) {
  # code executed when condition is TRUE
} else {
  # code executed when condition is FALSE
}
```

An example of what this might look like in our `mean_ci()` function from above:

``` r
mean_ci <- function(x) {
  
  if (is.numeric(x)) {
    
    xbar <- mean(x, na.rm=TRUE)
    n <- sum(!is.na(x))
    se <- sd(x, na.rm=TRUE) / sqrt(n)
    xbar + c(-2, 2) * se
    
  } else{
    
    "Cannot calculate CI since x is not a numeric variable."
    
  }
  
}
```

What is the `if` statement doing in this case?  We are using it to check whether the input to the function is numeric meaning we can calculate a confidence interval from it.  If it's numeric, we calculate the confidence interval.  If it's not numeric, we print out a message telling the user what the problem is!

Try it!  (Remember you have to run the code chunk directly above to define this new version of the function.)

``` r
# using a numeric input
x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4873117 0.6025364
```

``` r
# using a non-numeric input
x <- c("Yes", "No", "Yes", "Yes", "No")
mean_ci(x)
```

```
## [1] "Cannot calculate CI since x is not a numeric variable."
```

In addition to demonstrating conditional execution, this function also demonstrates what's called the **standard return rule**: the function returns the last value that it computes.  In this case, the last thing computed depends on which branch of the `if` statement is used!

## Choosing a condition

The statement you use for the `condition` to the `if` statement must evaluate to either a single `TRUE` or single `FALSE` value.  If it doesn't, you'll either get a warning message or an error message.  Run each of the three statements below to see what the problem is in each case:

``` r
if (c(TRUE, FALSE)) {}

if (NA) {}

if(5) {}
```

In the first case, the statement has two logical values, `TRUE` and `FALSE`, and so only the first value will be used to determine if the condition is met.  In this case, the conditional would be considered true.

In the second case, the statement is a missing value instead of either a `TRUE` or a `FALSE`.

In the third case, there's a numeric value instead of a logical `TRUE` or `FALSE`.

We can combine multiple logical expressions using the operator `||` for *or* and the operator `&&` for *and*.  Notice that these are different from the symbols we used for *or* and *and* in the `dplyr` functions.  In `if` statements, you want to use the double `||` and `&&`.  The reason for this is nicely explained in "R for Data Science:"

> These operators are “short-circuiting”: as soon as `||` sees the first `TRUE` it returns `TRUE` without computing anything else. As soon as `&&` sees the first `FALSE` it returns `FALSE`. You should never use `|` or `&` in an `if` statement: these are vectorised operations that apply to multiple values (that’s why you use them in `filter()`). If you do have a logical vector, you can use `any()` or `all()` to collapse it to a single value.
>
> Section 19.4.1 in [R for Data Science](https://r4ds.had.co.nz/functions.html#conditions)

Remember than `any()` returns `TRUE` if at least one of the values is `TRUE` and `all()` returns `TRUE` if all of the values are `TRUE`.

The following are all examples of appropriate conditions to use in an `if` statement.  

**If the input is a single number:**

``` r
x <- 3.4
```

* Check whether the input is at least 5:

``` r
x > 5
```

```
## [1] FALSE
```
* Check whether the input is between 0 and 5:

``` r
x < 5 && x > 0
```

```
## [1] TRUE
```
* Check whether the input is greater than 5 or less than -5:

``` r
x > 5 || x < -5
```

```
## [1] FALSE
```
* Check whether the input is numeric:

``` r
is.numeric(x)
```

```
## [1] TRUE
```
* Check whether the input is exactly equal to 3.4:

``` r
identical(x, 3.4)
```

```
## [1] TRUE
```

**If the input is a vector:**

``` r
x <- c(3.4, 5.3)
```
* Check whether any element in the input is at least 5:

``` r
x > 5  # this one won't work since it returns more than a single TRUE/FALSE
```

```
## [1] FALSE  TRUE
```

``` r
any(x > 5) # this is the right way to do this!
```

```
## [1] TRUE
```
* Check whether all elements in the input are at least 5:

``` r
all(x > 5)
```

```
## [1] FALSE
```
* Check whether any element in the input is not numeric:

``` r
any(!is.numeric(x))
```

```
## [1] FALSE
```
* Check whether any element in the input is missing:

``` r
any(is.na(x))
```

```
## [1] FALSE
```

## Combining multiple conditions

You can chain multiple `if` statements together if you want to check a bunch of conditions in sequence:

``` r
if (this condition) {
  # do something
} else if (that condition) {
  # do something else
} else {
  # do a third thing if neither of previous conditions are met
}
```

Here's an example of what that might look like for a functions that combines multiple arithmetic operations:

``` r
arith_xy <- function(x, y, operation) {
  if (operation == "plus") {
    x + y
  } else if (operation == "minus") {
    x - y
  } else if (operation == "times") {
    x * y
  } else if (operation == "divide") {
    x / y
  } else {
    "Unknown operation!  Please use either 'plus', 'minus', 'times', or 'divide'."
  }
}
```

We can try out this function:

``` r
arith_xy(5, 3, "plus")
```

```
## [1] 8
```

``` r
arith_xy(5, 3, "minus")
```

```
## [1] 2
```

``` r
arith_xy(5, 3, "times")
```

```
## [1] 15
```

``` r
arith_xy(5, 3, "divide")
```

```
## [1] 1.666667
```

``` r
arith_xy(5, 3, "subtract")
```

```
## [1] "Unknown operation!  Please use either 'plus', 'minus', 'times', or 'divide'."
```

## Code format

In terms of code style, the `if` function should be followed by curly brackets `{}`, and the contents should be indented by two spaces (RStudio does this automatically for you!) This makes it easier to see the hierarchy in your code by skimming the left-hand margin.

An opening curly brace `{` should never go on its own line and should always be followed by a new line. A closing curly brace  `}` should always go on its own line, unless it’s followed by `else`. Always indent the code inside curly braces.


``` r
# good example
if (y < 0) {
  message("Y is negative")
}

# bad example
if (y < 0)
 message("Y is negative") 

# good example
if (y == 0) {
  log(x)
} else {
  y ^ x
}

# bad example
if (y == 0) {
  log(x)
} 
else {
  y ^ x
}
```

## Practice

1. Write a `is_it_hot` function. It takes a single number as input. If the number is greater than 85, it returns “hot”. If the number is less than or equal to 85, it returns "not hot".  Make sure you first write working code before you create the function.



2. Write a `thermostat` function. It takes a single number as input. If the number is less than or equal to 32, it returns “freezing”. If the number is more than 32 but less than or equal to 45, it returns "cold".  If it's more than 45 but less than or equal to 65, it returns "cool".  If more than 65 but less than or equal to 85, it returns "warm".  For greater than 85, it returns "hot".  Make sure you first write working code before you create the function.



# Function arguments

For functions that work with data, there are generally two types of arguments: ones that supply the data to compute on and ones that control the details of the computation to be done on that data.  For example:

* In `log()`, the data is `x`, and the detail is `base` which provides the base of the logarithm.

``` r
?log
```

* In `mean()`, the data is `x`, and the details are `trim`, which tells how much data to trim from the ends and `na.rm` which tells how to handle missing values.

``` r
?mean
```

* In `t.test()`, the data are `x` and `y`, and the details needed for the test are `alternative`, `mu`, `paired`, `var.equal`, and `conf.level`.

``` r
?t.test
```

Generally, data arguments should come first. Detail arguments should go on the end, and usually should have default values. 

## Default values for detail arguments

You specify a default value by setting the default value in the argument portion of the `function()` function.  For example, see this updated function for calculating a confidence interval:

``` r
mean_ci <- function(x, conf = 0.95) {
  
  xbar <- mean(x, na.rm=TRUE)
  n <- sum(!is.na(x))
  se <- sd(x, na.rm=TRUE) / sqrt(n)
  
  if (identical(conf, 0.95)) {
    xbar + c(-2, 2) * se
  } else if (identical(conf, 0.99)) {
    xbar + c(-2.6, 2.6) * se
  } else {
    "Confidence level not found; please use 0.95 or 0.99"
  }
}
```

Here I've added another arguement to the function, `conf`, that controls the confidence level for the interval.  Since people mostly want 95% confidence intervals, I've given this new argument a default value of 0.95.  However, I'm allowing this same function to *also* calculate 99% confidence intervals by changing this `conf` value.  The 99% interval is provided by using an `if` statement on the value of this `conf` argument.

Let's see it in action:


``` r
x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4170700 0.5352147
```

If no value is specified for `conf`, then the default value of 0.95 is used.  You see we get the same thing when we explicitly specify this default value:


``` r
mean_ci(x, conf = .95)
```

```
## [1] 0.4170700 0.5352147
```

But we can also request a 99% confidence interval:


``` r
mean_ci(x, conf = .99)
```

```
## [1] 0.3993483 0.5529364
```

And if we request a value other than 95% or 99%, we will get an informative error message:


``` r
mean_ci(x, conf = .9)
```

```
## [1] "Confidence level not found; please use 0.95 or 0.99"
```

A good rule of thumb is to choose the default value for the detail arguments to be the *most commonly used* value.  The one exception to this rule is when the most commonly used value might allow the user to not be aware of something important, such as the fact there are missing values.  So the default for `na.rm` arguments is usually `FALSE` so the user must be aware there are missing values and must make the choice to ignore them instead of ignoring them by default.

## Code style

When we call a function, we often omit the names of the data arguments because they are used so commonly. For example, since the first argument of the `mean()` function is called `x` and gives the data, we could write either of the following:

``` r
# Good
mean(x = 1:10)
```

```
## [1] 5.5
```

``` r
# Good
mean(1:10)
```

```
## [1] 5.5
```

If you override the default value of a detail argument, you should use the full name!  Otherwise, you have to be very sure you have put the value of the detail argument in the correct position!

``` r
# Good
mean(1:10, na.rm = TRUE)
```

```
## [1] 5.5
```

``` r
# Bad
mean(x = 1:10, , FALSE)
```

```
## [1] 5.5
```

``` r
mean(, TRUE, x = c(1:10, NA))
```

```
## [1] 5.5
```

Notice that when you call a function, it's good practice to place a space around `=` in function calls, and always put a space after a comma, not before (just like in standard English). Using whitespace makes it easier to skim the function for the important components.


``` r
# Good
average <- mean(feet / 12 + inches, na.rm = TRUE)

# Bad
average<-mean(feet/12+inches,na.rm=TRUE)
```

The names of the arguments are also important in functions, because meaningful argument names are easier for the user! Generally you should use longer, more descriptive names, that make it clear what the argument does.  Also, it can be helpful to match names of your arguments to those in existing `R` functions.  For example, use `na.rm` to determine whether missing values should be removed!

## Checking values

Another useful thing to include in your functions are checks on whether the inputs to your function from the user are valid.  For example, in our `mean_ci()` function, we may want to be sure the vector `x` that is provided is numeric so that a mean CI is appropriate!  We can make this constraint explicit using the an `if` statement and then provide an error message to the user with the `stop()` function.  These checks are usually done at the top of the function, right after the function declaration:


``` r
mean_ci <- function(x, conf = 0.95) {
  
  if (!is.numeric(x)) {
    stop("`x` must be a numeric vector")
  }
  
  xbar <- mean(x, na.rm=TRUE)
  n <- sum(!is.na(x))
  se <- sd(x, na.rm=TRUE) / sqrt(n)
  
  if (identical(conf, 0.95)) {
    xbar + c(2, -2) * se
  } else if (identical(conf, 0.99)) {
    xbar + c(2.6, -2.6) * se
  } else {
    "Confidence level not found; please use 0.95 or 0.99"
  }
}

x <- runif(100)
mean_ci(x)
```

```
## [1] 0.6046476 0.4974775
```

``` r
y <- c("hi", "there", "how", "are", "you", "?")
mean_ci(y)
```

```
## Error in mean_ci(y): `x` must be a numeric vector
```

We could also move a check of the confidence level to the top as well:

``` r
mean_ci <- function(x, conf = 0.95) {
  
  if (!is.numeric(x)) {
    stop("`x` must be a numeric vector")
  }
  if (!identical(conf, 0.95) && !identical(conf, 0.99)) {
    stop("Confidence level not found; please use `0.95` or `0.99`")
  }
  
  xbar <- mean(x, na.rm=TRUE)
  n <- sum(!is.na(x))
  se <- sd(x, na.rm=TRUE) / sqrt(n)
  
  if (identical(conf, 0.95)) {
    xbar + c(-2, 2) * se
  } else {
    xbar + c(-2.6, 2.6) * se
  } 
}

x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4406515 0.5572622
```

``` r
mean_ci(x, conf = .95)
```

```
## [1] 0.4406515 0.5572622
```

``` r
mean_ci(x, conf = .99)
```

```
## [1] 0.4231599 0.5747538
```

``` r
mean_ci(x, conf = .9)
```

```
## Error in mean_ci(x, conf = 0.9): Confidence level not found; please use `0.95` or `0.99`
```


If there are a lot of conditions you want to check at the beginning of your function, you can use the function `stopifnot()` that checks that each argument is `TRUE`, and produces a generic error message if not.  Note that when using `stopifnot()` you assert what should be TRUE rather than checking for what might be FALSE like we did in the `if` statement.

``` r
mean_ci <- function(x, conf = 0.95) {
  
  stopifnot(is.numeric(x))
  stopifnot(identical(conf, 0.95) || identical(conf, 0.99))
  
  xbar <- mean(x, na.rm=TRUE)
  n <- sum(!is.na(x))
  se <- sd(x, na.rm=TRUE) / sqrt(n)
  
  if (identical(conf, 0.95)) {
    xbar + c(-2, 2) * se
  } else {
    xbar + c(-2.6, 2.6) * se
  } 
}

x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4797958 0.5836103
```

``` r
mean_ci(x, conf = .95)
```

```
## [1] 0.4797958 0.5836103
```

``` r
mean_ci(x, conf = .99)
```

```
## [1] 0.4642237 0.5991825
```

``` r
mean_ci(x, conf = .9)
```

```
## Error in mean_ci(x, conf = 0.9): identical(conf, 0.95) || identical(conf, 0.99) is not TRUE
```

``` r
y <- c("hi", "there", "how", "are", "you", "?")
mean_ci(y)
```

```
## Error in mean_ci(y): is.numeric(x) is not TRUE
```


## Passing arguments through to other functions using `...`

Many functions in `R` can take an arbitrary number of inputs:

``` r
sum(1, 2, 3, 4, 5)
```

```
## [1] 15
```

``` r
sum(1, 2)
```

```
## [1] 3
```

These functions rely on a special argument pronounced dot-dot-dot: `...`  You can see this special argument in the help file for these functions:

``` r
?sum
```

This `...` argument captures any number of arguments that aren’t otherwise matched.  It’s a useful argument because you can then send those `...` arguments on to another function. This is a useful catch-all if your function includes other functions that have their own arguments that you might want to modify from within your function call. 

For example, in our original `mean_ci()` function, we used the `na.rm = TRUE` option for both the `mean()` and `sd()` functions that we used to calculate our confidence interval.  However, maybe we don't want the default to be `na.rm = TRUE` and instead want the user to be aware there is missing data!

We can use the `...` as an argument in our own function and then pass this `...` argument into the `mean()` and `sd()` functions we use in the body of our function:


``` r
mean_ci <- function(x, ...) {
  
  xbar <- mean(x, ...)
  n <- sum(!is.na(x))
  se <- sd(x, ...) / sqrt(n)
  xbar + c(-2, 2) * se
}
```


Now see what happens when we have a vector that includes a `NA` value?  The default will be to return `NA` as well, unless we explicitly specify `na.rm = TRUE` when we use the function:

``` r
x <- c(runif(99), NA)
mean_ci(x)
```

```
## [1] NA NA
```

``` r
mean_ci(x, na.rm = TRUE)
```

```
## [1] 0.4116006 0.5231785
```

## Practice

3. Update your `is_it_hot` function (starting from the code below) to check to be sure the value `temp` is numeric and is only a single value (of length one).

``` r
is_it_hot <- function(temp) {
  
  if (temp > 85) {
    "hot"
    } else {
      "not hot"
    }
  
}
```

4. Update the function below to allow for a `na.rm` argument:

``` r
mean_sd_ratio <- function(x) {
  mean_x <- mean(x)
  sd_x <- sd(x)
  
  mean_x/sd_x
  
}

mean_sd_ratio(x = c(1, 3, 4, NA))
```

```
## [1] NA
```

``` r
mean_sd_ratio(x = c(1, 3, 4, NA), na.rm = TRUE)
```

```
## Error in mean_sd_ratio(x = c(1, 3, 4, NA), na.rm = TRUE): unused argument (na.rm = TRUE)
```

# Function return values

We already talked about the **standard return rule**, which means the function returns the last value that it computes.  We can also use explicit return statements to specify what we want the function to return.  We do this using the `return()` function.  This can be helpful if you want to be very clear about what value is being returned:

``` r
mean_ci <- function(x, ...) {
  xbar <- mean(x, ...)
  n <- sum(!is.na(x))
  se <- sd(x, ...) / sqrt(n)
  ci <- xbar + c(-2, 2) * se
  return(ci)
}

x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4190938 0.5393888
```

This can be especially helpful if you have different branches of an `if` statement and want to make it clear what is being returned for each branch, since you can't just easily glance to the last line of code!

``` r
mean_ci <- function(x, conf = 0.95, ...) {
  stopifnot(is.numeric(x))
  stopifnot(identical(conf, 0.95) || identical(conf, 0.99))
  
  xbar <- mean(x, ...)
  n <- sum(!is.na(x))
  se <- sd(x, ...) / sqrt(n)
  
  if (identical(conf, 0.95)) {
    return(xbar + c(-2, 2) * se)
  } else {
    return(xbar + c(-2.6, 2.6) * se)
  } 
}

x <- runif(100)
mean_ci(x)
```

```
## [1] 0.4514406 0.5666353
```




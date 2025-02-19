---
title: "Content 5-1: Reports with R Markdown"
date: "Monday, November 18, 2024"
output: 
  html_document:
    number_sections: false
    toc: true
    toc_depth: 1
    keep_md: yes
---


In Module 5, we will learn more about using R Markdown to create data analysis reports and interactive dashboards.  

This week we will learn about creating reports.  Some of the material from this week's content is adapted from "R for Data Science" by Grolemund and Wickham, chapter 27.  You can find this chapter [here](https://r4ds.had.co.nz/r-markdown.html).

This document contains many things related to creating reports using R Markdown.  For reference on some of these details, check out the **R Markdown Cheat Sheet** and **R Markdown Reference Guide** that you can find [here](https://rstudio.com/resources/cheatsheets/).

There are a few packages that we will want to install for this module to help make pretty tables in our final report. You need to execute these lines by hand (by running the code below), since the `eval = FALSE` argument to the code chunk means these lines are not evaluated each time the document is compiled.  Once you've executed these commands you should comment them out (with `#`) so that you don't re-run them by accident in the future.

``` r
install.packages("knitr")         
install.packages("pander")         
```

Then we will load these packages, along with the `tidyverse`, so we can use them:

``` r
library(knitr)
library(pander)
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

We are also going to load our processed National Medical Expenditures Survey (NMES) data set again for this module.  Remember that the NMES data set is called `nmes.data`.

``` r
load("nmes2018.rda")
```

# Introduction to R Markdown

We've been using R Markdown files throughout the semester, but we haven't really talked much about how to use them other than running code chunks and knitting the file to get a complete document.  In general, an R Markdown file is a plain text file that has the extension `.Rmd` and has the following three types of content:

* An (optional) **YAML header** surrounded by `---`
* **Chunks** of R code surrounded by ``` marks
* **Text** mixed with simple text formatting like `# heading` and `_italics_`

# Text formatting with Markdown

Text in `.Rmd` files is written in the language Markdown, which is a set of rules for formatting plain text files. Markdown is designed to be easy to read, easy to write, and easy to learn. The guide below shows how to use the version of Markdown that R Markdown understands:


``` r
Text formatting 
------------------------------------------------------------

*italic*  or _italic_
**bold**   __bold__
`code`
superscript^2^ and subscript~2~

Headings
------------------------------------------------------------

# 1st Level Header

## 2nd Level Header

### 3rd Level Header

Lists
------------------------------------------------------------

*   Bulleted list item 1

*   Item 2

    * Item 2a

    * Item 2b

1.  Numbered list item 1

1.  Item 2. The numbers are incremented automatically in the output.

Links and images
------------------------------------------------------------

<http://example.com>

[linked phrase](http://example.com)

![optional caption text](path/to/img.png)

Tables 
------------------------------------------------------------

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell
```

The best way to learn the formatting for R Markdown is simply to try them out; eventually they become second nature and you don't need to think about them.  If you forget, you can always consult the **Markdown Quick Reference** guide mentioned above!



# Code chunks

We've already been using code chunks to run code within an R Markdown document.  Think of each code chunk like a function; a chunk should be relatively self-contained and focused on a single task.  You don't want just one code chunk for your entire project; instead you should break your project into meaningful pieces and give each its own chunk!

There are three ways to insert a code chunk:

* The keyboard shortcut `Cmd/Ctrl + Alt + I`

* The “Insert” button icon in the editor toolbar.

* By manually typing the chunk delimiters ` ```{r} ` and ` ``` `.

You can run the code in the code chunk in different ways as well:

* `Cmd/Ctrl + Enter` runs the current line of code or a set of highlighted lines of code.

* `Cmd/Ctrl + Shift + Enter` runs all of the code in the chunk. (You can also click on the green arrow at the top of the chunk to do this!)

* You can also run all of the code up to the current code chunk by clicking on the gray down arrow over the green line at the top of the chunk.  This can be helpful if you want to restart your code from the beginning of the document without re-running each individual code chunk!

## Code chunk headers

The code chunk header is the piece of code at the top of the chunk that consists of ```{r  followed by an optional chunk name, followed by comma separated options, followed by }.

* Chunk names: Code chunks can be given an optional name: ```{r by_name}. Naming your code chunks lets you easily navigate to specific chunks using the drop-down code navigator in the bottom left of the script editor.  Also, when you produce a graphic with a code chunk with a useful name, it's easier to refer to them elsewhere in your report.

* Chunk options: Chunk output can be customized with options, arguments supplied to chunk header. There are almost 60 options that you can use to customize your code chunks. You can see the full list at [http://yihui.name/knitr/options/](http://yihui.name/knitr/options/). Here are some of the ones you'll use the most frequently in writing a report:

  * `eval = FALSE` prevents code from being evaluated. (And obviously if the code is not run, no results will be generated). This is useful for displaying example code, or for disabling a large block of code without commenting each line.

  * `include = FALSE` runs the code, but doesn’t show the code or results in the final document. Use this for setup code that you don’t want cluttering your report.

  * `echo = FALSE` prevents code, but not the results, from appearing in the finished file. Use this when writing reports aimed at people who don’t want to see the underlying R code.

  * `message = FALSE` or `warning = FALSE` prevents messages or warnings from appearing in the finished file.

  * `results = 'hide'` hides printed output; `fig.show = 'hide'` hides plots.

  * `error = TRUE` causes the knit to continue even if code returns an error. This is rarely something you’ll want to include in the final version of your report, but can be very useful if you need to debug exactly what is going on inside your `.Rmd`. The default, `error = FALSE` causes knitting to fail if there is a single error in the document.

Try some of these options by looking at what happens with the different chunk options for the code chunk below:

* No options

``` r
x <- c(1, 2, 3, 4, 5)
y <- x^3+3
plot(x, y)
```

![](Content5-1_r_markdown_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

* Using `eval = FALSE`

``` r
x <- c(1, 2, 3, 4, 5)
y <- x^3+3
plot(x, y)
```

* Using `include = FALSE`



* Using `echo = FALSE`

![](Content5-1_r_markdown_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

## Global code options

If you look at the top of this .Rmd file, you'll see the code chunk named `setup`.  This code chunk contains the function `knitr::opts_chunk$set()` which allows you to set the global options for all code chunks in the document.  This means that if you don't specify anything for an individual chunk, those are the options you will get for your chunk.  (This first code chunk is *always* named `setup`.)  You can see that in this case, the default is `echo = TRUE`, which means that the code will always be included with the output.  If we specified something different, like `echo = FALSE`, in an individual code chunk, it would override the global option and not show the code for that chunk. What happens if you change to `echo = FALSE` in the setup chunk and reknit the file?  Try it!

## Making attractive tables

By default, R Markdown prints data frames and matrices as you’d see them in the console:

``` r
nmes.data %>% 
  group_by(eversmk) %>%
  summarize(n=n(), mean.exp = mean(totalexp), sd.exp = sd(totalexp))
```

```
## # A tibble: 2 × 4
##   eversmk     n mean.exp sd.exp
##   <fct>   <int>    <dbl>  <dbl>
## 1 no       2084    2083.  6794.
## 2 Yes      1994    2520.  8010.
```

If you prefer that data be displayed with additional formatting you can use the `kable()` function from the `knitr` package we installed and loaded earlier. (It's often easiest to first store the table as an object and then use `kable()` to print it.) 

``` r
exp_table <- nmes.data %>% 
  group_by(eversmk) %>%
  summarize(n=n(), mean.exp = mean(totalexp), sd.exp = sd(totalexp))

kable(exp_table, caption = "Table 1: Medical expenditures by smoking status")
```



Table: Table 1: Medical expenditures by smoking status

|eversmk |    n| mean.exp|   sd.exp|
|:-------|----:|--------:|--------:|
|no      | 2084| 2083.283| 6793.619|
|Yes     | 1994| 2520.110| 8009.922|

Look at `?kable` to see other ways you can customize your table.  For example, you can specify the number of digits you want in your output and change the column names:

``` r
kable(exp_table, 
      caption = "Table 1: Medical expenditures by smoking status", 
      digits = 1,
      col.names = c("Smoking status", "n", "Mean expenditures", "SD of expenditures"))
```



Table: Table 1: Medical expenditures by smoking status

|Smoking status |    n| Mean expenditures| SD of expenditures|
|:--------------|----:|-----------------:|------------------:|
|no             | 2084|            2083.3|             6793.6|
|Yes            | 1994|            2520.1|             8009.9|

The `pander()` function from the `pander` package also allows you to easily make nicely formatted tables in your document:


``` r
pander(exp_table)
```


------------------------------------
 eversmk    n     mean.exp   sd.exp 
--------- ------ ---------- --------
   no      2084     2083      6794  

   Yes     1994     2520      8010  
------------------------------------

``` r
pander(exp_table, 
      caption = "Table 1: Medical expenditures by smoking status", 
      digits = 1,
      col.names = c("Smoking status", "n", "Mean expenditures", "SD of expenditures"))
```


----------------------------------------------------------------
 Smoking status    n     Mean expenditures   SD of expenditures 
---------------- ------ ------------------- --------------------
       no         2084         2083                 6794        

      Yes         1994         2520                 8010        
----------------------------------------------------------------

Table: Table 1: Medical expenditures by smoking status

The nice thing about `pander()` is that it can make nice tables out of things that aren't data frames; things like model output.  Let's play around with a simulated data set to illustrate a few things we can do with captions and the function `pander()`.

``` r
ctl <- c(4.17, 5.58, 5.18, 6.11, 4.50, 4.61, 5.17, 4.53, 5.33, 5.14)
trt <- c(4.81, 4.17, 4.41, 3.59, 5.87, 3.83, 6.03, 4.89, 4.32, 4.69)
group <- gl(2, 10, 20, labels = c("Control","Treatment"))
weight <- c(ctl, trt)

lm.D9 <- lm(weight ~ group)
pander(lm.D9, 
       caption = paste0("Table 2: Linear model output for model weight ~ group, for n=", length(weight), " observations"))
```


------------------------------------------------------------------
       &nbsp;         Estimate   Std. Error   t value   Pr(>|t|)  
-------------------- ---------- ------------ --------- -----------
  **(Intercept)**      5.032       0.2202      22.85    9.547e-15 

 **groupTreatment**    -0.371      0.3114     -1.191      0.249   
------------------------------------------------------------------

Table: Table 2: Linear model output for model weight ~ group, for n=20 observations

And we can make it look even nicer:

``` r
pander(lm.D9, 
       caption = paste0("Table 2: Linear model output for model weight ~ group, for n=", length(weight), " observations"),
       col.names = c("Coefficient", "Standard Error", "t-statistic", "p-value"),
       row.names = c("Intercept", "Treatment")
)
```


------------------------------------------------------------------------
    &nbsp;       Coefficient   Standard Error   t-statistic    p-value  
--------------- ------------- ---------------- ------------- -----------
 **Intercept**      5.032          0.2202          22.85      9.547e-15 

 **Treatment**     -0.371          0.3114         -1.191        0.249   
------------------------------------------------------------------------

Table: Table 2: Linear model output for model weight ~ group, for n=20 observations


## In-line code (including code in within the text)

We can also embed R code directly into the text of an R Markdown document using: tick r. This can be very useful if you mention properties of your data in the text. 

For example, in the example above where we look at the relationship between weight and treatment, the coefficient for the treatment group was -0.371.

**If we don't want to type out this number, we could use in-line R code instead:** In our model, the coefficient for treatment was -0.371 and the p-value for this coefficient is 0.2490232.  There were a total of n=20 observations in our data set.

When the report is knit, the results of these computations are inserted into the text:

**In our model, the coefficient for treatment was -0.371 and the p-value for this coefficient is 0.2490232. There were a total of n=20 observations in our data set.**

When inserting numbers into text, `format()` is your friend. It allows you to set the number of digits so you don’t print to a ridiculous degree of accuracy.  We can also add `big.mark = ","` to make numbers easier to read:


``` r
format(3452345, digits = 2, big.mark = ",")
```

```
## [1] "3,452,345"
```

``` r
format(.12358124331, digits = 2, big.mark = ",")
```

```
## [1] "0.12"
```

We might re-write out code above as:

In our model, the coefficient for treatment was -0.371 and the p-value for this coefficient is 0.249.  There were a total of n=20 observations in our data set.


## Making attractive plots

We can add figure captions to our plots in R Markdown as well.  This code chunk creates a string that will be used below as a figure caption:

``` r
figcap.nmes = paste0("Figure 1: Scatter plot of log10 medical expenditures vs age for n=", nrow(nmes.data), " observations")
```

We can then add this figure caption to our code chunk header using the `fig.cap` argument in the code chunk header:

``` r
nmes.data %>%
  mutate(log10exp = log10(totalexp)) %>%
  ggplot(mapping = aes(x = age, y = log10exp)) + 
  geom_point() + 
  geom_smooth(se = FALSE, color = "red")
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

![Figure 1: Scatter plot of log10 medical expenditures vs age for n=4078 observations](Content5-1_r_markdown_files/figure-html/unnamed-chunk-16-1.png)

Note that we can use options like `include = FALSE`, `echo = FALSE`, and `message = FALSE` in our code chunk headers to control what is printed out in the actual report.  So if we just want to include the graph and not the code for either the graph or the caption we could set `include = FALSE` for the caption definition chunk and `echo = FALSE` for the plot chunk.  We can also suppress the message about `geom_smooth` by including `message = FALSE` in our chunk for the graph.

Then we **just** get the graph in our report while hiding all the code that went into creating it!



![Figure 1: Scatter plot of log10 medical expenditures vs age for n=4078 observations](Content5-1_r_markdown_files/figure-html/unnamed-chunk-17-1.png)

## Including equations

If you know the math typesetting language Latex, you can use the same commands to create equations with mathematical symbols in RMarkdown:

$$
\alpha + \beta = x
$$

$$
  \begin{eqnarray}
  \dot{x} + \bar{x} &=& 2\\
  \hat{y} &=& 400
  \end{eqnarray}
$$

You can also put math notation within the text, like $\bar{x} + \bar{y} = 5$.

# YAML header

You can control many other “whole document” settings by tweaking the parameters of the YAML header. You might wonder what YAML stands for: it’s “yet another markup language”, which is designed for representing hierarchical data in a way that’s easy for humans to read and write. R Markdown uses it to control many details of the output. A couple of the things you can do here are to add a table of contents or a bibliography.

## Table of contents

If you look at the YAML header of this document, you can see that I've added a table of contents to my document by specifying some options under the `output:` section of the header.  I want my document to knit to HTML, so I have the `html_document:` specified and then under that I list some options I want to see in the HTML document:

* `number_sections: true` gives each section that's designated with a header (using `#`) a number according to a numbering scheme.  You can change this to `false` and re-knit the document to see what it does.
* `toc: true` adds a clickable table of contents to the top of the document when it's knit.  Again, change this to `false` and see what happens!
* `toc_depth: 1` means the table of contents will only list items that are at the highest header setting, so it will include headings marked with `#` but not those marked with `##` or below.  Change this to `2` and re-knit to see what happens!

## Bibliographies

R Markdown can also automatically generate citations and a bibliography in a number of styles. I won't go into details of this here, but the basic idea is that you specify a bibliography file using the bibliography field in the YAML header using `bibliography: rmarkdown.bib`.   The field should contain a path from the directory that contains your .Rmd file to the file that contains the bibliography file.  You can use many common bibliography formats including BibLaTeX, BibTeX, endnote and medline. Then you can create a citation within your .Rmd file by using a key composed of ‘@’ + the citation identifier from the bibliography file, placing the citation in square brackets like: Blah blah [see @doe99, pp. 33-35; also @smith04, ch. 1].

We won't talk more about using bibliographies in this class, but it's useful to know this functionality exists!












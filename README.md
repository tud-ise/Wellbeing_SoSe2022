# Increasing Well-Being With Data Analytics

# Syllabus

- [R for Data Science](https://r4ds.had.co.nz)
- [Cheatsheet "Daten bändigen mit R"](./Schummelzettel%20–%20dplyr%20und%20tidyr.pdf)

## Helpful links

- [https://www.rdocumentation.org](https://www.rdocumentation.org)
- [https://www.tidyverse.org](https://www.tidyverse.org)
- [https://www.rstudio.com/resources/cheatsheets](https://www.rstudio.com/resources/cheatsheets)
- [https://www.r-bloggers.com](https://www.r-bloggers.com)
- [https://stackoverflow.com/questions/tagged/r](https://stackoverflow.com/questions/tagged/r)
- [https://towardsdatascience.com](https://towardsdatascience.com)

## 0. Prerequisites

Please prepare for the exercise by following these steps:

1. Download and install `R` from here: https://cloud.r-project.org/
1. Download and install `RStudio` from here: https://www.rstudio.com/products/rstudio/download/#download
1. Make sure that you can open RStudio and that it works by entering `sum(1, 1)` and pressing `Enter ⏎`. You should see an output of `2`.

Everything below will be covered during the exercise, but feel free to read ahead.

## 1. The most important skill you will need

First of all, the goal for this course is _not_ to teach you everything you will need to work with R. The goal here is to show you how some basics, give you an introduction to RStudio, and make you curious to explore by yourself. Depending on the wellness activity you choose, you will need different commands and approaches for the analysis. Therefore, we want to show you where to look for the answers and give you a glimpse into working with R. The goal is _most definitely not_ to make you memorize some random commands or functions because...

:trophy: **The single most important skill you need to use R is knowing how to use a search engine well.**

## 2. First steps in R

- Variables are usually in `snake_case`
- Variable assignments look like this: `days_in_year <- 365`
- Function calls look like this: `f(x, y)`. Example: `round(3.14158, digits = 2)`
- To inspect a variable type `days_in_year` and press `Enter ⏎`
- One important data structure is vectors, they look like this (for a string vector) (the `c` stands for `combine`): `c('Han Solo', 'Chewbacca')`
- Most commonly in our course, data is stored in `data.frame`s or `tibble`s. A `tibble` (or `tbl_df`) is [a modern reimagining of the `data.frame`](https://tibble.tidyverse.org), so fundamentally the same but better.
- The pipe: Instead of calling functions like this `f(x, y)`, you can also use a so-called pipe and put the first function parameter in front of it like this: `x %>% f(y)`. Example from above: `3.14157 %>% round(digits = 2)` is the same as `round(3.14157, digits = 2)`. Another example: `5 %>% sum(7)` is the same as `sum(5, 7)`.

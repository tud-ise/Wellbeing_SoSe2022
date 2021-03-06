Exercise 2 - Data Curation
================

## Christopher Diebel & Jan-Hendrik Schmidt - ISE Darmstadt

**Note**: Thanks are also due here to Gregor Albrecht and Jakob Lorz,
whose exercise session from the summer semester of 2021 also forms the
basis of this semester’s exercise.

-   :house_with_garden: [Home](./README.md)

-   :open_book: [`dplyr`
    Documentation](https://dplyr.tidyverse.org/reference/index.html)

-   :open_book: [`readr`
    Documentation](https://readr.tidyverse.org/reference/index.html)

-   :open_book: [`lubridate`
    Documentation](https://lubridate.tidyverse.org/reference/index.html)

-   :open_book: [`rmarkdown` Cheatsheet](./rmarkdown-cheatsheet-2.0.pdf)

-   :open_book: Further Reading [R for Data
    Science](https://r4ds.had.co.nz/), [R für
    Einsteiger](http://aproxy.ulb.tu-darmstadt.de:2058/book/index.cfm?bok_id=1993358)
    or [Einführung in
    R](https://methodenlehre.github.io/einfuehrung-in-R/index.html)

## Preparation

1.  Install the `weights` package by running
    `install.packages('weights')`

2.  Install the `tidyfst` package by running
    `install.packages('tidyfst')`

3.  Install the `rmarkdown` package by running
    `install.packages('rmarkdown')`

## Refresher: Star Wars Tasks

1\. Find all characters with yellow eyes.

``` r
> starwars %>%
+     select(name, eye_color) %>%
+     filter(eye_color == 'yellow')
```

2\. Remove all Gungans. How many characters are left?

``` r
> starwars %>%
+     filter(is.na(species)|species != 'Gungan')
```

3\. What is the average mass of all droids?

``` r
> starwars %>%
+     select(name, species, mass) %>%
+     filter(species == 'Droid') %>%
+     na.omit() %>%
+     summarise(mean(mass))
```

4\. Calculate the BMI for all humans (\`mass / ((height / 100) ^ 2)\`)

``` r
> starwars %>%
+     select(name, mass, height, species) %>%
+     filter(species == 'Human') %>%
+     filter(!is.na(mass)) %>%
+     filter(!is.na(height)) %>%
+     mutate(bmi = (mass / ((height / 100) ^ 2)))
```

5\. Which character has the longest name?

``` r
> starwars %>%
+     select(name) %>%
+     arrange(desc(str_length(name))) %>%
+     filter(row_number()==1)
```

6\. What is the earliest birth year for each species? (\`birth_year\` is
measured in BBY = Before Battle of Yavin, so high values = earlier)

``` r
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species) %>%
+     arrange(birth_year) %>%
+     summarise(max(birth_year))
```

7\. What are the most populated planets?

``` r
> starwars %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
```

8\. What is the average height of all female humans?

``` r
> starwars %>%
+     select(name, height, gender, species) %>%
+     filter(species == 'Human', gender == 'feminine') %>%
+     na.omit() %>%
+     summarise(mean(height))
```

9\. Which planet has the most droids?

``` r
> starwars %>%
+     filter(species == 'Droid') %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
```

10\. Who is the oldest character of each species?

``` r
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species, name) %>%
+     summarise(by = max(birth_year)) %>%
+     group_by(species) %>%
+     slice(which.max(by))
```

11\. Which is the most prevalent eye color on each planet?

``` r
> starwars %>%
+       group_by(homeworld, eye_color) %>%
+       mutate(rank = row_number()) %>%
+       group_by(homeworld) %>%
+       slice(which.max(rank)) %>%
+       summarise(homeworld, eye_color, rank)
```

12\. How many unique eye colors are there?

``` r
> starwars %>%
+       summarise(eye_color) %>%
+       distinct(eye_color)
```

## How to Import Data in R

There are [a lot of packages in the `tidyverse` for importing
data](https://www.tidyverse.org/packages/#import), but you should mostly
to only care about [`readr`](https://readr.tidyverse.org/)† and
[`readxl`](https://readxl.tidyverse.org/)‡.

-   `read_csv()`† for comma-separated values (csv) files
-   `read_csv2()`† for csv files that use semicolons as delimiters
-   `read_tsv()`† for csv files that use tabs as delimiters
-   `read_delim()`† for csv files that use anything else as delimiters
-   `read_excel()`‡ for Excel files

As you can see, there are a lot of different functions with very
specific use cases.

**Important**: There are also very similar functions from other (partly
the basic) packages, which are called e.g. `read.csv()`. Pay attention
to the correct notation/source package accordingly. That means: Remember
to load (and install if necessary) the necessary package to be able to
use the functions.

To read a file, it’s easiest if the file is in your current working
directory (CWD). To find out what your CWD is, use `getwd()`.

``` r
getwd()
```

### Necessary CSV File

Download the CSV file [friends.csv](./friends.csv) from GitHub to your
private computer. To get the file, you can either download everything as
`.zip` or create a new empty file and copy&paste the raw file content
(for those of you who know what that is, you can also, of course, clone
the repository).

-   **Option 1**: Download as `.zip`: Go to [the `wellbeing`
    repository](https://github.com/tud-ise/Wellbeing_SoSe2022), click on
    the green `Code` button and click on “Download ZIP”. Then put it
    into your CWD. Also see: [“How to download as
    ZIP?”](https://stackoverflow.com/questions/2751227/how-to-download-source-in-zip-format-from-github)

-   **Option 2**: Create an empty file and copy&paste: Open RStudio,
    type `write.csv('hello there', 'friends.csv')` and press `Enter`.
    You should now see a new file in RStudio (Section “files”) that you
    can open (Select the file and press “View File”) and replace the
    content with [the raw content of the file in the
    repository](https://raw.githubusercontent.com/tud-ise/Wellbeing_SoSe2022/main/Exercises/Exercise%202/friends.csv).
    Don’t forget to save the changes.

-   **Option 3**: Use `getwd()` to read your current working directory
    and copy it to your clipboard. Then open [the raw content of the
    file in the
    repository](https://raw.githubusercontent.com/tud-ise/Wellbeing_SoSe2022/main/Exercises/Exercise%202/friends.csv),
    use the `right mouse button` and click “`Save as`”. Save the csv
    file (pay attention to the file type) in your working directory.

After the file itself is created, we should also save it to a variable.

In the so created tibble, we can see that three observations in the
“vaccine” column have no entry (`NA`). For our first evaluation, we are
**not interested in which vaccine** a character received, but **only
whether he received one or not**. For this we can use dummy variables.

## Dummy Variables

To make our lives easier, it is possible in R to define our own
functions and then use them. This is very simple:

``` r
ourFunction <- function() {

}
```

Let’s go through these lines step by step. We see that we define an
object name, `ourFunction`, and this object is now assigned something,
`<-`. With the keyword `function` we say that `ourFunction` should be a
function. In our example, the function has no parameters, so there is
nothing in the braces, `()`. The curly braces form the `function body`,
which now contains all the statements that belong to the function.
Everything after the closing brace no longer belongs to the function.†

† [Source](https://r-coding.de/blog/funktionen-in-r/)

But now we want the function to do something specific, because it is
still empty and nothing is executed. Let’s build a function that
calculates a simple mathematical function:

``` r
ourFunction <- function(x) {
  result <- 4*x + 5
  return(result)
}

ourFunction(9)
```

    ## [1] 41

For our use case, we first need our own defined function
`has_been_vaccinated()` that returns `1` if the passed parameter is
defined and `0` if not. We can declare our own function as follows:

``` r
has_been_vaccinated <- function(vaccine) {
  is_vaccinated = ifelse(is.na(vaccine), 0, 1)
  return(is_vaccinated)
}
```

Now, we want to use this function to dummify the column `vaccine` into a
new column `is_vaccinated`. `1` means “vaccinated” and `0` means “not
vaccinated”. It **doesn’t matter which** vaccine the character has,
**only** **if** they have one.

``` r
friends %>% 
  mutate(is_vaccinated = has_been_vaccinated(vaccine))
```

    ## # A tibble: 11 × 3
    ##    name      vaccine     is_vaccinated
    ##    <chr>     <chr>               <dbl>
    ##  1 Luke      AstraZeneca             1
    ##  2 Leia      Moderna                 1
    ##  3 Han       BioNTech                1
    ##  4 Rey       <NA>                    0
    ##  5 Chewbacca J&J                     1
    ##  6 C-3PO     Microsoft               1
    ##  7 R2-D2     Microsoft               1
    ##  8 Obi-Wan   BioNTech                1
    ##  9 JarJar    <NA>                    0
    ## 10 Boba      Sputnik V               1
    ## 11 Bossk     <NA>                    0

**Now, we do want to know which vaccine** each character received. In
theory, we could do this by checking for each possibility separately:

But, as you can see, this is quite tedious. Luckily, there is the
function `dummify()` from the package `weights`. So load the package
with `library(weights)` (install if necessary with
`install.packages('weights')`and then let’s try to use `dummify()`:

``` r
library(weights)
dummify(friends$vaccine)
```

This will show you an error: `variable needs to be a factor`. But what
are factors?

> R uses factors to represent categorical variables that have a known
> set of possible values.†

† [Source](https://r4ds.had.co.nz/factors.html?q=fac#creating-factors)

So how do we create factors? In this case, it’s simple. First, we remove
all `NA` values:

``` r
friends <- friends %>% 
  mutate(vaccine = replace_na(vaccine, 'NONE'))
```

Then, we make the `vaccine` column a factor:

``` r
friends_factorized <- friends %>% 
  mutate(vaccine = as.factor(vaccine))
```

## Practice: Breakout Session

1.  Install the `tidyfst` package: `install.packages("tidyfst")`.
    Afterwards, load it with `library(tidyfst)`.

2.  Download the `starwars_dd.csv` file from GitHub. For ease of use:
    place it in the folder which is shown in the `Files` section of
    RStudio.

The following code was used to generate the `starwars_dd.csv` file, in
case you are curious (this is not part of this exercise).

``` r
> library(tidyverse)
> starwars %>% 
+       unnest(cols=films,keep_empty=TRUE) %>% 
+       unnest(cols=vehicles, keep_empty=TRUE) %>% 
+       unnest(cols=starships, keep_empty=TRUE) %>%
+       write.csv('./starwars_dd.csv')
```

3.  Import `starwars_dd.csv` into a variable named `starwars_dd`. The
    contents are different from the tidyverse-internal `starwars`
    dataset and simplify the next steps for you.

``` r
> read_csv('./starwars_dd.csv') -> starwars_dd
```

4.  Import the `tidyfst` package. Then generate dummies for all films
    and store them in the variable `starwars_films_dd`. Inspect them in
    RStudio (using the `View` function) afterwards. You may use the
    [:book:
    `dummy_dt`](https://www.rdocumentation.org/packages/tidyfst/versions/0.9.9/topics/dummy_dt)
    function from `tidyfst`. Make sure to exclude rows which hold no
    additional information, we are focusing on the different films here
    (Hint: Each character has multiple rows for each film, vehicle and
    starship)

``` r
> library(tidyfst)
> starwars_dd %>% 
+       distinct(name, films, .keep_all=TRUE) %>% 
+       dummy_dt(films) ->
+       starwars_films_dd
> starwars_films_dd <- as_tibble(starwars_films_dd)
> View(starwars_films_dd)
```

5.  How many characters starred in `A New Hope`?

``` r
> starwars_films_dd %>% 
+       filter(`films_A New Hope` == 1) %>% 
+       summarise(name, `films_A New Hope`)
# A tibble: 18 x 2
name                  `films_A New Hope`
<chr>                              <dbl>
1 Beru Whitesun lars                     1
2 Biggs Darklighter                      1
3 C-3PO                                  1
4 Chewbacca                              1
5 Darth Vader                            1
6 Greedo                                 1
7 Han Solo                               1
8 Jabba Desilijic Tiure                  1
9 Jek Tono Porkins                       1
10 Leia Organa                            1
11 Luke Skywalker                         1
12 Obi-Wan Kenobi                         1
13 Owen Lars                              1
14 R2-D2                                  1
15 R5-D4                                  1
16 Raymus Antilles                        1
17 Wedge Antilles                         1
18 Wilhuff Tarkin                         1 
```

6.  Which of those characters are female?

``` r
> starwars_films_dd %>% 
+       filter(`films_A New Hope` == 1) %>% 
+       filter(sex == 'female') %>% 
+       summarise(name, `films_A New Hope`)
# A tibble: 2 x 2
  name               `films_A New Hope`
  <chr>                           <dbl>
1 Beru Whitesun lars                  1
2 Leia Organa                         1
```

## Group Session: Working with Dates

Because working with dates can be cumbersome, the `tidyverse` contains a
very helpful package for that: `lubridate`.

``` r
library(lubridate)
```

    ## 
    ## Attache Paket: 'lubridate'

    ## Die folgenden Objekte sind maskiert von 'package:base':
    ## 
    ##     date, intersect, setdiff, union

If the date you are trying to work with contains a date like `4/14/22`,
you can tell `lubridate` that this is a date:

``` r
date <- parse_date_time('4/14/22', 'mdy')
```

The `'mdy'` represents `[m]onth [d]ay [year]`. The resulting `date` is a

“POSIXct date-time object” (you can verify that with `is.instant(date)`
or `class(date)`) that makes it very easy to work with it. For instance,
you can find out which week the date refers to:

``` r
week(date)
```

    ## [1] 15

We can also do some calculations with timestamps:

``` r
start <- now()

# wait...
Sys.sleep(3.14159265359)
end <- now()

elapsed <- start %--% end
# ... and find out how long you waited
as.duration(elapsed)
```

    ## [1] "3.20936703681946s"

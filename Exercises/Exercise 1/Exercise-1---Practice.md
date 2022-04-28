# Exercise Number 1: Introduction to R - Exercises

## Christopher Diebel & Jan-Hendrik Schmidt - ISE Darmstadt

## Preparation

1. Even if you work with R every day, it is (almost) impossible to remember all the functions. R has a very good built-in help system, but it is not very easy to understand for beginners. This is accessible in the Help section. If you want to search specifically, you can enter a term in the Help Viewer search window. If you simply want to find out what features are available in an installed package, you can search for a package in the Packages area and then click on it to see the help pages.
1. If you want to get quick help on a function in the console, you can proceed like this: `help(mean)` - This opens the help page for the mean function. Alternatively, you can also enter `?mean`. 
1. One can also search for help on a topic. For example, if you need help on rounding numbers up or down, you can type `help("round")`. More help on the `help()` function can be found like this: `help(help)`.


## Practice Exercises

### Operators and Numerical Functions
Calculate using RStudio:

1. <img src="https://latex.codecogs.com/png.image?\inline&space;\large&space;\dpi{120}{\color{Emerald}&space;\frac{1}{3}\frac{1&plus;3&plus;5&plus;7&plus;2}{3&space;&plus;&space;5&space;&plus;&space;4}}" title="https://latex.codecogs.com/png.image?\inline \large \dpi{120}{\color{Emerald} \frac{1}{3}\frac{1+3+5+7+2}{3 + 5 + 4}}" />
```R
(1/3)*((1+3+5+7+2)/(3+5+4))
```
2. How can you calculate <img src="https://latex.codecogs.com/png.image?\inline&space;\large&space;\dpi{120}{\color{Emerald}&space;e}" title="https://latex.codecogs.com/png.image?\inline \large \dpi{120}{\color{Emerald} e}" /> 
```R
exp(1)
```
3. <img src="https://latex.codecogs.com/png.image?\inline&space;\large&space;\dpi{120}{\color{Emerald}&space;\sqrt{2}}" title="https://latex.codecogs.com/png.image?\inline \large \dpi{120}{\color{Emerald} \sqrt{2}}" /> 
```R
sqrt(2)
```
4. <img src="https://latex.codecogs.com/png.image?\inline&space;\large&space;\dpi{120}{\color{Emerald}&space;\sqrt[3]{8}}" title="https://latex.codecogs.com/png.image?\inline \large \dpi{120}{\color{Emerald} \sqrt[3]{8}}" /> 
```R
8^(1/3)
```
5. <img src="https://latex.codecogs.com/png.image?\inline&space;\large&space;\dpi{120}{\color{Emerald}&space;log_2(8)}" title="https://latex.codecogs.com/png.image?\inline \large \dpi{120}{\color{Emerald} log_2(8)}" /> 
```R
log2(8)
```

### Statistical Functions

1. Create a sequence from 0 to 100 in steps of 5
```R
seq(0, 100, 5)
```
2. Calculate the mean value of the vector [1, 3, 4, 7, 11, 2].
```R
mean(c(1, 3, 4, 7, 11, 2))
```
3. Display the span <img src="https://latex.codecogs.com/png.image?\inline&space;\large&space;\dpi{120}{\color{Emerald}&space;x_{max}&space;-&space;x_{min}}" title="https://latex.codecogs.com/png.image?\inline \large \dpi{120}{\color{Emerald} x_{max} - x_{min}}" /> of this vector.
```R
range(c(1, 3, 4, 7, 11, 2))
```
4. Calculate the sum of this vector.
```R
sum(c(1, 3, 4, 7, 11, 2))
```

### Data Frames
One advantage of packages like `tidyverse` is that sample data is often included, making it particularly clear why the packages are useful. We use the tibble `starwars` from the `tidyverse` package.

#### Group Task

0. Get to know the dataset

```R
starwars
View(starwars)
```

1. Who is the shortest character?

2. Find all characters from  Tatooine.

#### Breakout Room Tasks

1. Who is the tallest character? (Hint: You may want to use the `desc()` function)

```R
starwars %>%
   arrange(desc(height))
```

2. Find all droids.

```R
starwars %>%
   filter(species == 'Droid')
```

3. How many droids are there?

```R
starwars %>%
   filter(species == 'Droid') %>%
   summarise(n())
```

4. What is the home world of Han Solo?

```R
starwars %>%
   select(name, homeworld) %>%
   filter(name == 'Han Solo')
```

5. How many characters are there per species? Sort by ascending count.

```R
starwars %>%
    group_by(species) %>%
    summarise(count = n()) %>%
    arrange(desc(count))
```

6. Change the height from `cm` to `m`.

```R
starwars %>%
   mutate(height = height / 100)
```

7. Filter out all characters that have missing values (`NA`) for their mass. How many are left?

```R
starwars %>%
   select(name, mass) %>%
   na.omit()
```
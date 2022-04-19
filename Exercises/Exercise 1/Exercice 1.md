# Exercise Number 1: Introduction to R 
## Christopher Diebel & Jan-Hendrik Schmidt - ISE Darmstadt

## Helpful Links

- :house_with_garden: [Home](https://github.com/tud-ise/Wellbeing_SoSe2022/blob/main/README.md)
- :open_book: [Information about R](https://www.r-project.org/)
- :open_book: [`dplyr` documentation](https://dplyr.tidyverse.org/reference/index.html)
- :open_book: Further Reading [R for Data Science](https://r4ds.had.co.nz/) or [R für Einsteiger](http://aproxy.ulb.tu-darmstadt.de:2058/book/index.cfm?bok_id=1993358)

## What is R? 
R is a free software environment for statistical analysis.

## Preparation

Please work through the [Prerequisites section](https://github.com/tud-ise/Wellbeing_SoSe2022/blob/main/README.md#0-prerequisites) to be prepared for the first exercise.

## Programming Fundamentals

### Operators

#### Arithmetic Operators

| Command | Meaning                                   | Example                 |
|---------|-------------------------------------------|-------------------------|
| +       | Addition                                  |                         |
| -       | Subtration                                |                         |
| *       | Multiplication                            |                         |
| /       | Division                                  |                         |
| ^ or ** | Exponentiation                            |                         |
| x %*% y | Matrix Multiplication                     | c(1,4) %*% c(3,5) == 23 |
| %/%     | Whole Number Division                     | 6 %/% 4 == 1            |
| %%      | Modulo (Remainder of a Division; x mod y) | 6 %% 4 == 2             |

#### Logic Operators

| Command  | Meaning                         | Example                    |
|----------|---------------------------------|----------------------------|
| ==       | Equal                           |                            |
| !=       | Unequal                         |                            |
| <        | Smaller than                    |                            |
| >        | Greater than                    |                            |
| <=       | Smaller equal                   |                            |
| >=       | Greater equal                   |                            |
| &        | Logical AND                     | (x & y)                    |
| &#124;   | Logical OR                      | (x &#124; y)               |
| !        | Logical NOT                     | !x                         |
| xor(x, y)| Exklusive OR                    | Either in x or y, not both |
| isTRUE(x)| Tests if x is true              |                            |

![Illustration of the Logical Operators](/MD_Images/transform-logical.png)

## RStudio Introduction
### Graphical User Interface


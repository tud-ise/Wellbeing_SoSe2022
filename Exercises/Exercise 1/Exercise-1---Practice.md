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

#### Further Star Wars Tasks

If you find time during the week, you can solve the following tasks. We will start the next exercise by discussing your solutions. 

##### Tasks
1. Find all characters with yellow eyes.

```R
> starwars %>%
+     select(name, eye_color) %>%
+     filter(eye_color == 'yellow')
# A tibble: 11 x 2
   name              eye_color
   <chr>             <chr>
 1 C-3PO             yellow
 2 Darth Vader       yellow
 3 Palpatine         yellow
 4 Watto             yellow
 5 Darth Maul        yellow
 6 Dud Bolt          yellow
 7 Ki-Adi-Mundi      yellow
 8 Yarael Poof       yellow
 9 Poggle the Lesser yellow
10 Zam Wesell        yellow
11 Dexter Jettster   yellow
```

2. Remove all Gungans. How many characters are left?

```R
> starwars %>%
+     filter(is.na(species)|species != 'Gungan')
# A tibble: 84 x 14
```

3. What is the average mass of all droids?

```R
> starwars %>%
+     select(name, species, mass) %>%
+     filter(species == 'Droid') %>%
+     na.omit() %>%
+     summarise(mean(mass))
# A tibble: 1 x 1
  `mean(mass)`
         <dbl>
1         69.8
```

4. Calculate the BMI for all humans (`mass / ((height / 100) ^ 2)`)

```R
> starwars %>%
+     select(name, mass, height, species) %>%
+     filter(species == 'Human') %>%
+     filter(!is.na(mass)) %>%
+     filter(!is.na(height)) %>%
+     mutate(bmi = (mass / ((height / 100) ^ 2)))
# A tibble: 22 x 5
   name                mass height species   bmi
   <chr>              <dbl>  <int> <chr>   <dbl>
 1 Luke Skywalker        77    172 Human    26.0
 2 Darth Vader          136    202 Human    33.3
 3 Leia Organa           49    150 Human    21.8
 4 Owen Lars            120    178 Human    37.9
 5 Beru Whitesun lars    75    165 Human    27.5
 6 Biggs Darklighter     84    183 Human    25.1
 7 Obi-Wan Kenobi        77    182 Human    23.2
 8 Anakin Skywalker      84    188 Human    23.8
 9 Han Solo              80    180 Human    24.7
10 Wedge Antilles        77    170 Human    26.6
# … with 12 more rows
```

5. Which character has the longest name?

```R
> starwars %>%
+     select(name) %>%
+     arrange(desc(str_length(name))) %>%
+     filter(row_number()==1)
# A tibble: 1 x 1
   name
   <chr>
 1 Jabba Desilijic Tiure

```

6. What is the earliest birth year for each species? (`birth_year` is measured in BBY = Before Battle of Yavin, so high values = earlier)

```R
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species) %>%
+     arrange(birth_year) %>%
+     summarise(max(birth_year))
# A tibble: 15 x 2
   species        `max(birth_year)`
   <chr>                      <dbl>
 1 Cerean                        92
 2 Droid                        112
 3 Ewok                           8
 4 Gungan                        52
 5 Human                        102
 6 Hutt                         600
 7 Kel Dor                       22
 8 Mirialan                      58
 9 Mon Calamari                  41
10 Rodian                        44
11 Trandoshan                    53
12 Twi'lek                       48
13 Wookiee                      200
14 Yoda's species               896
15 Zabrak                        54
```

7. What are the most populated planets?

```R
> starwars %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
# A tibble: 49 x 2
   homeworld     n
   <chr>     <int>
 1 Naboo        11
 2 Tatooine     10
 3 NA           10
 4 Alderaan      3
 5 Coruscant     3
 6 Kamino        3
 7 Corellia      2
 8 Kashyyyk      2
 9 Mirial        2
10 Ryloth        2
# … with 39 more rows
```

8. What is the average height of all female humans?

```R
> starwars %>%
+     select(name, height, gender, species) %>%
+     filter(species == 'Human', gender == 'feminine') %>%
+     na.omit() %>%
+     summarise(mean(height))
# A tibble: 1 x 1
  `mean(height)`
           <dbl>
1           160.
```

9. Which planet has the most droids?

```R
> starwars %>%
+     filter(species == 'Droid') %>%
+     group_by(homeworld) %>%
+     summarise(n = n()) %>%
+     arrange(desc(n))
# A tibble: 3 x 2
  homeworld     n
  <chr>     <int>
1 NA            3
2 Tatooine      2
3 Naboo         1
```

10. Who is the oldest character of each species?

```R
> starwars %>%
+     filter(!is.na(birth_year)) %>%
+     filter(!is.na(species)) %>%
+     group_by(species, name) %>%
+     summarise(by = max(birth_year)) %>%
+     group_by(species) %>%
+     slice(which.max(by))
`summarise()` has grouped output by 'species'. You can override using the `.groups` argument.
# A tibble: 15 x 3
# Groups:   species [15]
   species        name                     by
   <chr>          <chr>                 <dbl>
 1 Cerean         Ki-Adi-Mundi             92
 2 Droid          C-3PO                   112
 3 Ewok           Wicket Systri Warrick     8
 4 Gungan         Jar Jar Binks            52
 5 Human          Dooku                   102
 6 Hutt           Jabba Desilijic Tiure   600
 7 Kel Dor        Plo Koon                 22
 8 Mirialan       Luminara Unduli          58
 9 Mon Calamari   Ackbar                   41
10 Rodian         Greedo                   44
11 Trandoshan     Bossk                    53
12 Twi'lek        Ayla Secura              48
13 Wookiee        Chewbacca               200
14 Yoda's species Yoda                    896
15 Zabrak         Darth Maul               54
```

11. Which is the most prevalent eye color on each planet?

```R
> starwars %>%
+       group_by(homeworld, eye_color) %>%
+       mutate(rank = row_number()) %>%
+       group_by(homeworld) %>%
+       slice(which.max(rank)) %>%
+       summarise(homeworld, eye_color, rank)
# A tibble: 49 x 3
   homeworld      eye_color  rank
   <chr>          <chr>     <int>
 1 Alderaan       brown         3
 2 Aleen Minor    unknown       1
 3 Bespin         blue          1
 4 Bestine IV     blue          1
 5 Cato Neimoidia red           1
 6 Cerea          yellow        1
 7 Champala       blue          1
 8 Chandrila      blue          1
 9 Concord Dawn   brown         1
10 Corellia       brown         1
```

12. How many unique eye colors are there?

```R
> starwars %>%
+       summarise(eye_color) %>%
+       distinct(eye_color)
# A tibble: 15 x 1
   eye_color
   <chr>
 1 blue
 2 yellow
 3 red
 4 brown
 5 blue-gray
 6 black
 7 orange
 8 hazel
 9 pink
10 unknown
11 red, blue
12 gold
13 green, yellow
14 white
15 dark
```
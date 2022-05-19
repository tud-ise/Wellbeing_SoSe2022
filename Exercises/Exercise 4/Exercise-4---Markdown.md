Exercise 4
================

## Wellbeing - Test Data Set

For this exercise we will focus on a test data set, which is a simulated
combination of *RescueTime* data (anonymized by category - see Exercise
3), a wellness activity and survey data. Please be aware that the data
set contains no real case data, so that the results we receive in this
exercise are not to be taken seriously.

The data set contains 1,200 entries with 44 attributes:

**Data set attributes**

| Attribute                                         | Meaning                                             | Format                  |
|---------------------------------------------------|-----------------------------------------------------|-------------------------|
| id                                                | Unique user ID                                      | \-                      |
| sex                                               | Which gender does the participant have?             | Male (0) - Female (1)   |
| age                                               | What is the age of the participant?                 | Numeric                 |
| wellness_act                                      | Has the participant performed a wellness activity?  | No (0) - Yes (1)        |
| Date                                              | Time stamp                                          | M/DD/YYYY               |
| st\_*RescueTime category*\_\_numberOfApplications | Number of used application by category              | Numeric                 |
| st\_*RescueTime category*                         | Time spend with applications by category in seconds | Numeric                 |
| esm/items                                         | Please see survey examples (Github - Exercise 4)    | Likert skala 1-5 or 1-7 |

**Download**

Please download the [data set from our github
repository](https://github.com/tud-ise/Wellbeing_SoSe2022/blob/main/Exercises/Exercise%204/all_data_1.csv).

## Data Preparation

**Open the data set**

    data = read.csv("**Your Path** /all_data_1.csv")
    df = data

**Check the data structure**

    summary(df)
    str(df)

    summary(df[, 5:9]) # Life Satisfaction
    str(df[,11:22]) # RescueTime data

**Group items**

We had multiple items (questions) to measure specific characteristics.
To analyze the overall characteristic, we have to calculate the mean for
each characteristic using each item.

*Life Satisfaction*

      df$total_Life_satisfaction = (df$life_satisfaction_item_1 + df$life_satisfaction_item_2 + df$life_satisfaction_item_3 + df$life_satisfaction_item_4 + df$life_satisfaction_item_5) / 5
      

*Technostress*

    df$TS_CTRL = (df$esm_technostress_control_1 + df$esm_technostress_control_2)/2

    df$TS_NEG = (df$esm_technostress_negative_1 + df$esm_technostress_negative_2 
             + df$esm_technostress_negative_3 + df$esm_technostress_negative_4 + df$esm_technostress_negative_5)/5

    df$TS_POS = (df$esm_technostress_positive_1 + df$esm_technostress_positive_2 
             + df$esm_technostress_positive_3 + df$esm_technostress_positive_4 + df$esm_technostress_positive_5)/5

*Positive/Negative affects*

    df$distressed = df$esm_panas_na_distressed
    df$irritable = df$esm_panas_na_irritable
    df$jittery = df$esm_panas_na_jittery
    df$nervous = df$esm_panas_na_nervous
    df$upset = df$esm_panas_na_upset
    df$active = df$esm_panas_pa_active
    df$determined = df$esm_panas_pa_determined
    df$enthusiastic = df$esm_panas_pa_enthusiastic
    df$excited = df$esm_panas_pa_excited
    df$inspired = df$esm_panas_pa_inspired

    df$PA = (df$excited + df$enthusiastic + df$inspired + df$active + df$determined)/5
    df$NAS = (df$distressed + df$irritable + df$jittery + df$nervous + df$upset)/5

**Adjust *RecueTime* data**

The RescueTime data has ‘NA’ if an category is not used at all.
Regression models do often not handle NA values, because of that we will
replace the ’NA’s with zeros.

    df[is.na(df)] = 0 

**Format Columns**

The column `date` is currently loaded as character (`chr`). If we want
to work with a `time series` we need to format the date column into the
`date` format.

    library(lubridate)
    df$date = mdy(df$date)

Most regression libraries will expect an `factor` instead of `character`
or `numeric` as categorical variables. Therefore we transform the data
type of `id`, `sex` and `wellness_act` into `factors`:

    df$id = as.factor(df$id)
    df$sex = as.factor(df$sex)
    df$wellness_act = as.factor(df$wellness_act)

Test if all transformations were successful:

    str(df[,1:10])

**Create single user data frame**

It could be interesting to take a look on a single user as well (for
example your own data). The easiest way is to create a second data frame
which contains just one specific user.

    df_sgl = df[df$id == 2,] 
    summary(df_sgl)

## Data Analysis

### Descriptive Statistics

First, we should use descriptive statistics to describe and summarize
the data.

**Examples**

What are the average age of our participants?

    mean(df$age)

How are the genders distributed in our data set?

     table(df$sex)
     

What is your/the single users average time on social media?

    mean(df_sgl$st_Social.Networking_time, na.rm=TRUE) 

Have male participants used more time with networking apps than female
participants?

    mean(df[df$sex == 1, 18])

    mean(df[df$sex == 0, 18], na.rm=TRUE) #Social networking time = column 18 
    mean(df[df$sex == 1, 18], na.rm=TRUE) #Result is the same, that is not a misstake, it is because the data set was generated this way

**Graphs**

In addition graphs are strong tool to visualize and describe a data set.
Important R-functions are ‘hist()’ and ‘plot()’

How is the age of our participants distributed?

    hist(df$age)

Plot between time on social media and age.

    plot(df$age, df$st_Social.Networking_time)

**Your Turn**

Try to find some interesting numbers or insight using descriptive
statistics.

### Regression

Next, we take a look on inferential statistics to see if we have
statistical significant effects.

**OLS Regression**

We can use a simple OLS regression (Exercise 3) to analyse the data of
one specific group, such as “your own” data (df_sgl).

Lets test if we can find a significant effect of screen time per
category on your life satisfaction.

    wlb_ols = lm(df_sgl$total_Life_satisfaction ~ df_sgl$st_Communication...Scheduling_time + df_sgl$st_Entertainment_time + df_sgl$st_Shopping_time + df_sgl$st_Social.Networking_time + df_sgl$st_Software.Development_time + df_sgl$st_Uncategorized_time)

    summary(wlb_ols)

**Panel Regression**

To analyze the whole data set, we have to pick the correct model which
fits to the data structure. As we already learned, the data set is based
on panel data, which we can analyze with a panel regression. In this
exercise we will use the ‘plm()’ function from the ‘plm’ package to run
our panel regression.

*Install library*

    install.packages("plm")

    library("plm")

*Prepare data*

    E = pdata.frame(df, index=c("id","date"), drop.index=TRUE, row.names=TRUE)

    head(E)

    head(attr(E, "index"))

*Run regression*

    panel_reg = plm(PA ~ wellness_act + sex + age + total_Life_satisfaction + TS_POS + TS_NEG  + st_Entertainment_numberOfApplications + st_Entertainment_time, data = E, model = "random")

    summary(panel_reg)


    panel_reg = plm(NAS ~ wellness_act + sex + age + total_Life_satisfaction + TS_POS + TS_NEG  + st_Entertainment_numberOfApplications + st_Entertainment_time, data = E, model = "random")

    summary(panel_reg)

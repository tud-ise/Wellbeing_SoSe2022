Exercise 5
================

## Wellbeing - Test Data Set

In this exercise we will focus on a test data set, which is in the same
format as the final data set, you will receive for the report. We will
offer two kind of data sets. A minimized version
(wellbing_test_data.csv), which contains already summarized and labeled
columns which you could use for your analysis, and the full version
(wellbing_test_data_total), so that you have the the possibility to
analyze whatever you want. The test data set does not include data of
all user.

Please be aware that this exercise gives you some example how to
transform and handle the data set. You can use this for your analysis
but you should analyse the data with additional approaches using the
skills you have already learned in the exercises before.

The data set contains around 3,000 entries with 62 attributes:

**Data set attributes**

| Attribute                       | Meaning                                                                    | Format                                                                   |
|---------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------|
| Primarykey                      | Unique row id (generated via ID and Date)                                  |                                                                          |
| ID                              | User id                                                                    |                                                                          |
| Date                            | Timestamp                                                                  | YYYY-MM-DD                                                               |
| ST_Entertainment.Media          | Screen Time - Entertainment Media                                          | Seconds                                                                  |
| ST_Social.Media                 | Screen Time - Social Media                                                 | Seconds                                                                  |
| ST_Study.Work.Related           | Screen Time - Study Work Related Media                                     | Seconds                                                                  |
| ST_Communication.Technology     | Screen Time - Communication technologie                                    | Seconds                                                                  |
| ST_Uncategorized                | Screen Time - Uncategorized                                                | Seconds                                                                  |
| Total_Screentime                | Total Screen Time                                                          | Seconds                                                                  |
| Wellness Activity Check S101    | Did you successfully do the activity (meditation/screen time limit) today? | 1 No/ 2 Yes                                                              |
| Wellness Activity Planning S108 | Did you plan to do the activity today?                                     | 1 No/ 2 Yes                                                              |
|                                 | Posible dependent variables                                                |                                                                          |
| ”                               | cognitive_wellbeing_S201_01                                                | How positively do you feel about yourself today?                         |
| ”                               | positive_wellbeing_score                                                   | How do you feel today? (summarized - Happy/Exited/Enthusiastic/Inspired) |

. . . Please see excel file “legende.xlsx” for more information

**Download**

Please download the [data set from our github
repository](https://github.com/tud-ise/Wellbeing_SoSe2022/blob/main/Exercises/Exercise%205/wellbing_test_data.csv).

## Data Preparation

**Open the data set**

    data = read.csv("**Your Path** /wellbing_test_data.csv")
    df = data

**Check the data structure**

    summary(df)
    str(df)

**Format data**

First we have to transform some columns into the correct format.

Transform the timestamp column into a Date format. Please Note: The date
format has to be YYYY-MM-DD. If you open the file in excel, excel will
change it to an german format. If thats the case change the format in
excel using “Format cells-> Date -> Pick YYYY-MM-DD”. Thats also the
common problem if you receive any error if you try to change the format
into a date.

    df_input$Date = as.Date(df_input$Date)

Use a for loop to transform all numbers into the numeric format:

    for(i in 4:62){ #4 is the first column which representes screen time and 62 the last column with numeric survey data. 
      
      df_input[,i] = as.numeric(df_input[,i]) 
      
    }

Check data:

    str(df_input)

**add data**

Next we can add new columns which could be interesting for our analysis.
For example misfits:

    df_input$SM_fit = abs(df_input$Needs_SM_Perceived - df_input$Supplies_SM_Perceived)
    df_input$control_SM_fit = abs(df_input$Needs_SM_self_control - df_input$Supplies_SM_self_control)


    df_input$EM_fit = abs(df_input$Needs_EM_Perceived - df_input$Supplies_EM_Perceived)
    df_input$control_EM_fit = abs(df_input$Needs_EM_Abilities.Demandself_control - df_input$Supplies_EM_self_control)


    df_input$CT_fit = abs(df_input$Needs_CT_Perceived - df_input$Supplies_CT_Perceived)
    df_input$control_CT_fit = abs(df_input$Needs_CT_self_control - df_input$Supplies_CT_self_control)


    df_input$SM_fit = abs(df_input$Needs_Study.ICT_Perceived - df_input$Supplies_Study.ICT_Perceived)
    df_input$control_SM_fit = abs(df_input$Needs_Study.ICT_self_control - df_input$Supplies_Study.ICT_self_control)

**prepare data frames**

We have now one main data frame with all data in the correct format. We
can split our analysis into three interesting parts, first the effects
between groups over time for which we can use the main data set. Second,
the analysis of one single user (your own data) and at least the trends
and effects above all users. We can split the input data set into two
more data frames which is prepared for one individual purpose.

The following function creates an data frame which includes one row for
each date with aggregated values. So the user ID is not important
anymore, this data frame represents the complete course overall.

      df_overall = aggregate(x = df_input[,4:68], FUN = mean, by = list(Group.date = df_input$Date))

Create an data frame with data of one individual user - for example
yourself (I dont know this ID, it is picked randomly)

      df_single = df_input[df_input$ID == "11u9ixuw", ]

**Trend**

Plots are a create tool to look if there is a trend in the data, which
can be a first hint of an correlation or effect.

ggplot2 is the most used library for plots in r. It makes it easy to
handle timeseries data and you will find a lot of documentation in the
web about ways to use ggplot2.

Install ggplot2

      install.packages("ggplot2") 
      
      install.packages("dplyr") 

Load ggplot2

      library(ggplot2)
      
      library(dplyr)
      

Plot time series Note: Group.date is the Date column in df_overall. Your
can rename it, if you want to use another name.

Examples:

    p = ggplot(df_overall, aes(x=Group.date, y=cognitive_wellbeing_S201_01)) +
      geom_line() + 
      xlab("")
    p # show plot



    p = ggplot(df_overall, aes(x=Group.date, y= negative_wellbeing_score)) +
      geom_line() + 
      xlab("")
    p #show plot



    p = ggplot(df_single, aes(x=Date, y= Mindfulness_ALL)) +
      geom_line() + 
      xlab("")
    p #show plot

#Descriptive statistics You can use descriptive statistics like the
average and standard derivations to receive some interesting information
about yourself compared with others.

Example: How high was my average positive wellbeing score compared to
the whole course?

    mean(df_single$Mindfulness_SM)

    mean(df_overall$Mindfulness_SM)

Please take a look into Exercise 3 and 4 for more information.

### Regression

Next, we take a look on inferential statistics to see if we have
statistical significant effects.

**OLS Regression**

We can use a simple OLS regression (Exercise 3) to analyse the data of
one specific group, such as “your own” data (df_single).

Lets test if we can find a significant effect of the wellness activity,
exercise and mindfulness on the negative wellbeing score

    wlb_ols = lm(df_single$negative_wellbeing_score ~ df_single$Wellness.Activity.Check.S101 + df_single$Supplie_time_exercise_S403_02 + df_single$Mindfulness_SM)

    summary(wlb_ols)

Overall

    wlb_ols = lm(df_overall$negative_wellbeing_score ~ df_overall$Wellness.Activity.Check.S101 + df_overall$Supplie_time_exercise_S403_02 + df_overall$Mindfulness_SM)

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

    E = pdata.frame(df_input, index=c("ID","Date"), drop.index=TRUE, row.names=TRUE)

    head(E)

    head(attr(E, "index"))

*Run regression*

    panel_reg = plm(E$positive_wellbeing_score ~ Wellness.Activity.Check.S101 + ST_Entertainment.Media + E$Needs_EM_Perceived + E$work_exhausting_S204_01, data = E, model = "random")

    summary(panel_reg)

---
title: "W04 Task: Clean and Reformat (aka Tidy) Stock Data"
author: "Maddy McGreer"
date: "June 24, 2023"
output:
  html_document:  
    keep_md: true
    toc: true
    toc_float: true
    code_folding: hide
    fig_height: 6
    fig_width: 12
    fig_align: 'center'
---






```r
# Use this R-Chunk to import all your datasets!

# Read the .RDS file from GitHub
url <- "C:/Users/maddy/Downloads/Dart_Expert_Dow_6month_anova.RDS"
data <- read_rds(url)
```

## Background

_The Dow Jones Industrial Average is a group of 30 stocks for large companies with stable earnings. This “index” of stocks is one of the oldest, most closely watched indices in the world and is designed to serve as a proxy, or indicator, of the United States economy in general. We want to look at the returns for each six-month period of the year in which the returns were reported. _

## Data Wrangling



```r
# Use this R-Chunk to clean & wrangle your data!

# Create month_end and year_end columns from contestant_period
tidy_data <- data %>%
  separate(contest_period, into = c("month_start", "month_end"), sep = "-")%>%
  separate(month_end, into = c("month_end","year_end"),sep="(?<=\\D)(?=\\d)",remove=TRUE)



# Save tidy data as .rds object
saveRDS(tidy_data, "tidy_data.rds")

# View the resulting tidy data
tidy_data
```

```
## # A tibble: 300 × 5
##    month_start   month_end year_end variable value
##    <chr>         <chr>     <chr>    <chr>    <dbl>
##  1 January       June      1990     PROS      12.7
##  2 February      July      1990     PROS      26.4
##  3 March         August    1990     PROS       2.5
##  4 April         September 1990     PROS     -20  
##  5 May           October   1990     PROS     -37.8
##  6 June          November  1990     PROS     -33.3
##  7 July          December  1990     PROS     -10.2
##  8 August1990    January   1991     PROS     -20.3
##  9 September1990 February  1991     PROS      38.9
## 10 October1990   March     1991     PROS      20.2
## # ℹ 290 more rows
```

## Data Visualization


```r
# Use this R-Chunk to plot & visualize your data!

# Create plot of six-month returns by year
plot_data <- tidy_data %>%
  group_by(year_end) %>%
  summarize(sum_variable = sum(value))

ggplot(plot_data, aes(x = year_end, y = sum_variable)) +
  geom_bar(stat = "identity", fill = "blue") +
  labs(title = "Sum Variable by Year",
       x = "Year",
       y = "Sum Variable")
```

![](w4task_tidydata_files/figure-html/plot_data-1.png)<!-- -->

```r
# Create a table of variable sums using pivot_wider()
table_data <- tidy_data %>%
  select(month_end, year_end, value) %>%
  pivot_wider(names_from = year_end, values_from = value, values_fn = sum) 


# Arrange the table by month in chronological order
#month_order <- c("01", "02", "03", "04", "05", "06", "07", "08", "09", "10", "11", "12")
table_data <- table_data %>%
  arrange(match(month_end, month.name))

# Print the table
table_data
```

```
## # A tibble: 14 × 10
##    month_end `1990` `1991` `1992`  `1993` `1994` `1995` `1996` `1997` `1998`
##    <chr>      <dbl>  <dbl>  <dbl>   <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 January     NA    -58.4  31.8    0.900  57.5    25     40.7  32       5  
##  2 February    NA     47.4  71.5   -8.6    10.7    NA     56.1  92.3    18.3
##  3 March       NA     47.2  33.5   52.9    -2.9    -3.7   70.4 -17.3    26.4
##  4 April       NA    140.    4.2  -11.7    -4.6    26.3   50    -8      29.4
##  5 May         NA    101.   23.9   41.2    -6.8    39.3   22.5   2.40   39.1
##  6 June        15.2   53.9  -9.7    5.8   -28     100.    -4.3  54.4    42.6
##  7 July        39.7   69.9   6.6  -10.2     1.60   66.6   -4.7  36.2    36.3
##  8 August     -14.1  115.    0.8    2      25.6    53.4   26.6  29.1   -52.3
##  9 September  -36.4   21.8 -14     -9.9    35.6    92.3   22.3 106.    -66.1
## 10 October    -62.6   54.8  -4.4    5.8    44.4    38.4   16.2  62.1    NA  
## 11 November   -73.5  -23.5  -1.00 -23.9    29.9    67.9   26.4   6.8    NA  
## 12 December   -42     23    16.1   NA      -6.9    17     63.6  -8.3    NA  
## 13 Dec.        NA     NA    NA     52.4    NA      NA     NA    NA      NA  
## 14 Febuary     NA     NA    NA     NA      NA      20.5   NA    NA      NA
```

## Conclusions

_The first plot displays the sum of a variable over six-month periods by year using a bar chart. It provides a visual representation of how the sum variable changes across different years. The second part of the code creates a table showing the sum of the variable for each month and year. The table is organized in chronological order by month, allowing for easy comparison of variable sums over time._

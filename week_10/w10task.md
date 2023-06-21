---
title: "W10 Task: My Investment is Better Than Yours"
author: "Maddy McGreer"
date: "June 20, 2023"
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

# Define the stock symbols
your_stocks <- c("GOOGL", "META", "MCD")
friend_stocks <- c("AAPL", "AMZN", "NFLX")

# Get price performance data
your_prices <- try(tq_get(your_stocks,
                          from = "2022-10-01",
                          to = "2023-06-20"))

friend_prices <- try(tq_get(friend_stocks,
                            from = "2022-10-01",
                            to = "2023-06-20"))
```


## Background

_The stock market is overflowing with data. There are many packages in R that allow us to get quick access to information on publicly traded companies. Imagine that you and a friend each purchased about $1,000 of stock in three different stocks at the start of October last year, and you want to compare your performance up to this week. Use the stock shares purchased and share prices to demonstrate how each of you fared over the period you were competing (assuming that you did not change your allocations)._

_I chose Google, Facebook, McDonalds against Apple, Amazon, and Netflix._

## Data Wrangling


```r
# Use this R-Chunk to clean & wrangle your data!


# Calculate cumulative returns
your_returns <- cumprod(1 + your_prices$adjusted) - 1
friend_returns <- cumprod(1 + friend_prices$adjusted) - 1



# Combine returns data into a single data frame
portfolio_returns <- data.frame(Date = your_prices$date,
                                Your_Returns = your_returns,
                                Friend_Returns = friend_returns)
```

## Data Visualization


```r
# Use this R-Chunk to plot & visualize your data!

melted_returns <- reshape2::melt(portfolio_returns,
                                 id.vars = "Date",
                                 variable.name = "Competitor",
                                 value.name = "Returns")

ggplot(melted_returns, aes(x = Date, y = Returns, color = Competitor)) +
  geom_line() +
  labs(x = "Date", y = "Cumulative Returns", color = "Competitor") +
  scale_color_manual(values = c("Your_Returns" = "blue", "Friend_Returns" = "red")) +
  theme_minimal()
```

![](w10task_files/figure-html/plot_data-1.png)<!-- -->

## Conclusions

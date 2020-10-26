Galaxies
--------

Unwin, Ch. 3, \#1
(10 points) The dataset galaxies in the package MASS contains the velocities of 82 planets. (a) Draw a histogram, a boxplot, and a density estimate of the data. What information can you get from each plot?

``` r
library(tidyverse)
library(MASS)

df <- data.frame(galaxies)

ggplot(df, aes(x = galaxies)) +
  geom_histogram(color = "black", fill = "#7192E3", binwidth = 1000) +
  xlab("Velocity") + ylab("Count") + 
  ggtitle("Histogram of Velocities of Galaxies")
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-1-1.png)

**The velocity of most of the galaxies is between 15000 and 28000. There are some values which are far away from this range. For example, the velocity of some galaxies are around 1000 , and that of some galaxies are around 33000.**

``` r
ggplot(df, aes(y = galaxies)) + 
  geom_boxplot() +
  ylab("Velocity") + 
  ggtitle("Boxplot Estimation of Velocities of Galaxies") 
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-2-1.png)

**Clearly, there are outliers above and below the box. These outliers have the velocity with values around 10000 or 33000. This may suggest that there can be some errors in data collection. However, they may also represnt the true values. Further investigation is required to reach to some conclusion regarding these outliers. Median of the velocity is around 21000.**

``` r
ggplot(df, aes(x = galaxies)) +
  geom_density(fill = "#7192E3") +
  xlab("Velocity") + ylab("Density") + 
  ggtitle("Density Estimation of Velocities of Galaxies")
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-3-1.png)

**Density curves can be used to analyse the distribution of the data. The velocity data is slighly right skewed, and some data points**

b)Experiment with different binwidths for the histogram and different bandwidths for the density estimates. Which choices do you think are best for conveying the information in the data?

``` r
library(gridExtra)

g1 <- ggplot(df, aes(galaxies)) + 
  geom_histogram(color = "black", fill = "#7192E3", binwidth = 500) + 
  xlab("Velocity") + ylab("Count") +
  ggtitle("Histogram with Bandwidth 500")

g2 <- ggplot(df, aes(galaxies)) + 
  geom_histogram(color = "black", fill = "#7192E3", binwidth = 1000) +
  xlab("Velocity") + ylab("Count") +
  ggtitle("Histogram with Bandwidth 1000")

g3 <- ggplot(df, aes(galaxies)) + 
  geom_histogram(color = "black", fill = "#7192E3", binwidth = 2000) +
  xlab("Velocity") + ylab("Count") +
  ggtitle("Histogram with Bandwidth 2000")

g4 <- ggplot(df, aes(galaxies)) + 
  geom_histogram(color = "black", fill = "#7192E3", binwidth = 3000) +
  xlab("Velocity") + ylab("Count") +
  ggtitle("Histogram with Bandwidth 4000")

grid.arrange(g1, g2, g3, g4, nrow = 2)
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-4-1.png)

``` r
g1 <- ggplot(df, aes(galaxies)) +
  geom_density(fill = "#7192E3", bw = 200) +
  xlab("Velocity") + ylab("Density") +
  ggtitle("Density of Galaxies with BW 200")

g2 <- ggplot(df, aes(galaxies)) +
  geom_density(fill = "#7192E3", bw = 500) +
  xlab("Velocity") + ylab("Density") +
  ggtitle("Density of Galaxies with BW 500")

g3 <- ggplot(df, aes(galaxies)) + 
  geom_density(fill = "#7192E3", bw = 1000) + 
  xlab("Velocity") + ylab("Density") +
  ggtitle("Density of Galaxies with BW 1000")

g4 <- ggplot(df, aes(galaxies)) + 
  geom_density(fill = "#7192E3", bw = 2000) + 
  xlab("Velocity") + ylab("Density") +
  ggtitle("Density of Galaxies with BW 2000")

grid.arrange(g1, g2, g3, g4, nrow = 2)
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-5-1.png)

**In the bandwidth used above to plot histogram and density plot, histogram with bandwith 1000 and density with bandwidth 500 represents the data well and convey most of the information without focusing too much on individual data points.**

1.  How many plots do you think you need to present the information? Which one(s)?

**Boxplots and histograms present different information about the data: boxplots display individual outliers and summary statistics, whereas histograms group the data and draw bars from each interval, and it also roughly demonstrates the distribution of the data if bandwidth is selected appropriately.**

Zuni educational funding
------------------------

Unwin, Ch. 3, \#5

(10 points)

1.  Are the lowest and highest 5% of the revenue values extreme? Do you prefer a histogram or a boxplot for showing this?

``` r
library("lawstat")
data(zuni)
boxplot(zuni$Revenue)
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-6-1.png)

``` r
hist(zuni$Revenue)
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-6-2.png)

``` r
quantile <- quantile(zuni$Revenue, c(0.05, 0.95))
boxplot(zuni$Revenue)
abline(h = quantile[1], col = "red", lty = 2)
abline(h = quantile[2], col = "red", lty = 2)
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-6-3.png) **The boxplot is prefered. From the boxplot, we can easily see the top 5% is extreme because they are outliers, while the lowest 5% is not.**

1.  Having removed the lowest and highest 5% of the cases, draw a density estimate of the remaining data and discuss whether the resulting distribution looks symmetric.

``` r
zuni.remain <-
  zuni[zuni$Revenue > quantile[1] & zuni$Revenue < quantile[2], ]

plot(density(zuni.remain$Revenue),
     main = "Density Estimate",
     xlab = "Revenue")
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-7-1.png)

**The resulting distribution does not look symmetric and is skewed to the right.**

1.  Draw a Q-Q plot for the data after removal of the 5% at each end and comment on whether you would regard the remaining distribution as normal.

``` r
qqnorm(zuni.remain$Revenue)
qqline(zuni.remain$Revenue, col = "red")
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-8-1.png)

**Deviation in the upper and lower tails is large, so I would not regard the remaining distribution as normal.**

Non-detectable
--------------

Unwin, Ch. 3, \#6 a) with the 0 values?

``` r
library(mi)
data(CHAIN)
log_virus_0 <- CHAIN$log_virus[!is.na(CHAIN$log_virus)]
df_log_virus <- data.frame(log_virus_0)

ggplot(df_log_virus, aes(x = log_virus_0)) +
  geom_histogram(color = "black", fill = "#7192E3", binwidth = 1) +
  xlab("log_virus") + ylab("Count") + 
  ggtitle("Log of viral load level with 0")
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-9-1.png)

1.  without the 0 values?

``` r
log_virus_no_0 <- log_virus_0[log_virus_0 != 0]
df_log_virus_updated <- data.frame(log_virus_no_0)

ggplot(df_log_virus_updated, aes(x = log_virus_no_0)) +
  geom_density(bw = 0.45, fill = "#7192E3") +
  xlab("log_virus") + ylab("Density Estimate") + 
  ggtitle("Log of viral load level without 0")
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-10-1.png)

Base Salary
-----------

(20 points) For this question, we will use NYC salary data found here: <https://data.cityofnewyork.us/City-Government/> Citywide-Payroll-Data-Fiscal-Year-/k397-673e Click the Export tab then CSV.

1.  List the 10 agencies (Agency Name) with the highest median base salaries (Base Salary) in descending order by median base salary.

``` r
df <- read_csv("Citywide_Payroll_Data__Fiscal_Year_.csv")
top_med_salary <- df %>%
  group_by(`Agency Name`) %>%
  summarize(median = median(`Base Salary`)) %>%
  top_n(10) %>%
  arrange(desc(median))
```

``` r
top_med_salary
```

    ## # A tibble: 10 x 2
    ##    `Agency Name`                   median
    ##    <chr>                            <dbl>
    ##  1 DISTRICTING COMMISSION         176099 
    ##  2 FINANCIAL INFO SVCS AGENCY     105875 
    ##  3 OFFICE OF COLLECTIVE BARGAININ  99000 
    ##  4 BRONX COMMUNITY BOARD #3        98489 
    ##  5 DEPT OF ED PEDAGOGICAL          82900 
    ##  6 INDEPENDENT BUDGET OFFICE       82012 
    ##  7 OFFICE OF THE MAYOR             79000 
    ##  8 BRONX COMMUNITY BOARD #1        78190 
    ##  9 CONFLICTS OF INTEREST BOARD     78000 
    ## 10 BRONX COMMUNITY BOARD #5        77106.

1.  Use ridgeline plots to compare the distributions of base salary among the top 10 agencies from part a). The agencies should be sorted from lowest median base salary on the bottom of the graph to highest median base salary on the top of the graph.

``` r
library(ggridges)
topdf <- df %>%
  filter(`Agency Name` %in% top_med_salary$`Agency Name`)

ggplot(topdf, aes(`Base Salary`,
                  fct_reorder(`Agency Name`, `Base Salary`, median))) +
  geom_density_ridges()
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-13-1.png)

1.  Describe patterns that you observe.

**The graph of some agencies has one peak and some has two peaks. When we order the agency by median salary from high to low, the data of first two agencies skew to the left. Except some of the density curve such as "Dept of Ed Pedagogical" , most of the density curves are not normal**

1.  What information is missing or not clear from the ridgeline plots? Plot the data with another chart form that provides additional information not available in the ridgeline plots. What do you learn?

The number of values in each agency are very different, as is clear from frequency histograms or an uneven width boxplot

``` r
ggplot(topdf, aes(`Base Salary`)) + 
  geom_histogram() + facet_wrap( ~ `Agency Name`, ncol = 2)
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-14-1.png)

``` r
ggplot(topdf, aes(fct_reorder(`Agency Name`, `Base Salary`, median),
                  `Base Salary`)) + 
  geom_boxplot() + coord_flip()
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-15-1.png)

A table of counts confirms this observation:

``` r
topdf %>% group_by(`Agency Name`) %>% 
  summarize(median = median(`Base Salary`), count = n()) %>% 
  arrange(desc(median))
```

    ## # A tibble: 10 x 3
    ##    `Agency Name`                   median  count
    ##    <chr>                            <dbl>  <int>
    ##  1 DISTRICTING COMMISSION         176099       3
    ##  2 FINANCIAL INFO SVCS AGENCY     105875    2361
    ##  3 OFFICE OF COLLECTIVE BARGAININ  99000      95
    ##  4 BRONX COMMUNITY BOARD #3        98489      10
    ##  5 DEPT OF ED PEDAGOGICAL          82900  531294
    ##  6 INDEPENDENT BUDGET OFFICE       82012     213
    ##  7 OFFICE OF THE MAYOR             79000    3296
    ##  8 BRONX COMMUNITY BOARD #1        78190      11
    ##  9 CONFLICTS OF INTEREST BOARD     78000     134
    ## 10 BRONX COMMUNITY BOARD #5        77106.     14

Wine
----

\[18 points\]

Data: `wine` dataset in **pgmm** package

Recode the `Type` variable to human readable names.

1.  Create a Cleveland dot plot showing the mean value for each of the 27 chemical and physical properties of the wines.

``` r
library(tidyverse)
library(pgmm)
data(wine)
theme_dotplot <- theme_bw(18) +
    theme(axis.text.y = element_text(size = rel(.75)),
          axis.ticks.y = element_blank(),
          axis.title.x = element_text(size = rel(.75)),
          panel.grid.major.x = element_blank(),
          panel.grid.major.y = element_line(size = 0.5),
          panel.grid.minor.x = element_blank())
```

``` r
tidy_wine <- wine %>% 
  mutate(Type = fct_recode(factor(Type), Barolo = "1", Grignolio = "2", Barbera = "3")) %>%
  mutate(Type = fct_relevel(Type, "Barbera", "Barolo", "Grignolio")) %>%
  gather(key = "var", value, -Type)

tidy_wine %>% 
  group_by(var) %>% 
  summarize(mean = mean(value)) %>% 
  ggplot(aes(mean, fct_reorder(var, mean))) +
  geom_point() + theme_dotplot
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-18-1.png)

**For (b) and (c), use only `dplyr` and `tidyr` for data manipulation -- no \[ \] and $ should appear in your code.**

1.  Create a Cleveland dot plot *with multiple dots* to show mean value of each of the 27 properties by `Type`. That is, each property--such as `Ash`--should have three different colored dots, one for each `Type`. The properties should be sorted (highest on top) by the mean value for the `Grignolino` `Type`.

``` r
tidy_wine %>% 
  group_by(Type, var) %>% 
  summarize(mean = mean(value)) %>% 
  ggplot(aes(mean, fct_reorder2(var, Type, -mean), color = Type)) + 
  geom_point() + theme_dotplot
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-19-1.png)

![Caption for the picture.](NY.png) (c) Standardize the values in the wine dataset by subtracting the mean and dividing by the standard deviation *for each property separately.* Redo the plot from from part b with the transformed data, sorted (highest on top) by the standardized mean value for the `Grignolino` `Type`.

``` r
std_wine <- tidy_wine %>% 
  group_by(var) %>% 
  mutate(sdvalue = (value - mean(value))/sd(value))

std_wine %>% 
  group_by(Type, var) %>% 
  summarize(sdmean = mean(sdvalue)) %>% 
  ggplot(aes(sdmean, fct_reorder2(var, Type, -sdmean), color = Type)) + 
  geom_point() + theme_dotplot
```

![](assignment_r_files/figure-markdown_github/unnamed-chunk-20-1.png)

1.  Is the order of the properties the same or different in the graphs for parts (b) and (c)? Why or why not?

**No. Standardizing removes the effect of the units and provides a means for relative rather than absolute comparisons. For example, the highest property in part (b), `Potassium`, drops to the middle in part (c) since the means of the three groups aren't very different.**

1.  Is the sum of the three standardized means (one per `Type`) for each property equal to zero? Why or why not?

**No. The groups are different sizes so their means won't sum to zero. However, if we take a weighted average based on group size, they will.**

Sum of standardized means for Potassium:

``` r
potassium <- std_wine %>% 
  filter(var == "Potassium") %>% 
  group_by(Type) %>% 
  summarize(count = n(), sdmean = mean(sdvalue)) 

potassium
```

    ## # A tibble: 3 x 3
    ##   Type      count  sdmean
    ##   <fct>     <int>   <dbl>
    ## 1 Barbera      48  0.0946
    ## 2 Barolo       59  0.158 
    ## 3 Grignolio    71 -0.195

``` r
potassium %>% summarize(sum(sdmean))
```

    ## # A tibble: 1 x 1
    ##   `sum(sdmean)`
    ##           <dbl>
    ## 1        0.0573

Sum of weighted means:

``` r
potassium %>% summarize(sum((count/sum(count))*sdmean))
```

    ## # A tibble: 1 x 1
    ##   `sum((count/sum(count)) * sdmean)`
    ##                                <dbl>
    ## 1               0.000000000000000236

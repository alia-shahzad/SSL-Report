SSL
================
Alia Shahzad
5/14/2020

FOR FINAL DOC As the CPD still has not released the details of this
algorithm — we have the scores, but we don’t know how they got there —
we’re forced to take them at their
    word.

    ## ── Attaching packages ─────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.0     ✓ purrr   0.3.3
    ## ✓ tibble  3.0.0     ✓ dplyr   0.8.5
    ## ✓ tidyr   1.0.2     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    ## here() starts at /home/ashahzad/R/Homework/hw6

    ## Installing package into '/home/ashahzad/R/x86_64-redhat-linux-gnu-library/3.6'
    ## (as 'lib' is unspecified)

``` r
#Builds local file path & avoids using the dreaded setwd() by using Socrata package
#to read directly from a URL. Find more info on Socrata.  [here](https://github.com/Chicago/RSocrata/blob/master/README.md).

ssl <- RSocrata::read.socrata("https://data.cityofchicago.org/Public-Safety/Strategic-Subject-List/4aki-r3np/data") %>%
  write_csv(path = here("data", "ssl.csv")) %>% #writing data into right folder
  select(ssl_score, 
         predictor_rat_age_at_latest_arrest,
         predictor_rat_gang_affiliation, 
         race_code_cd, 
         age_curr) %>% #selecting vars of interest
  rename(age_arr = 'predictor_rat_age_at_latest_arrest',
         gang_aff = 'predictor_rat_gang_affiliation', 
         race = 'race_code_cd') #shortening var names
```

``` r
#this chunk splits the ssl scores into new classes for the next graph 
#it's in a different chunk so I can hide its very ugly product when it print

mylabels <- c("Low Risk", "High Risk", "Very High Risk") 
cut_ssl <- cut(ssl$ssl_score, breaks=c(0, 250, 450, 500), labels=mylabels)  
```

SSL Scores Overview by Risk of Violence

Overview of SSL Scores. SSL Scores are placed on a scale ranging from 0
(extremely low risk of violence) to 500 (extremely high risk violence).
A score above 250
[earns](https://medium.com/equal-future/how-strategic-is-chicagos-strategic-subjects-list-upturn-investigates-9e5b4b235a7c)
“heightened police scrutiny,” and score above 450 is high-risk for
becoming party to violence. The vast majority of scores are between 0 -
450, and within this group, more scores are high risk (250-350) than low
risk (0-250). Very few scores are very high risk (above 450).

``` r
#plotting data visualization
ssl_overview <- ssl %>%
  #mapping ssl_scores & cut ssl_scores separately to show score groups in visualization
  ggplot(mapping = aes(ssl_score, fill = cut_ssl)) + 
  geom_area(stat = "bin") +
  labs(title = "SSL Score Distribution", 
       x = "SSL Score",
       y = "Count",
       fill = "Risk of Becoming Party to Violence",
       caption = "(Source: Chicago Data Portal)") +
  theme(legend.position = "top")
  theme_minimal() 

ssl_overview
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](images/ssl%20scores%20overview%20graph-1.png)<!-- -->

SSL Scores by Age

Most SSL Subjects were under the age of 40 when they were most recently
arrested. The most represented age group here is 20-30, which is
unsurprising considering this is also the age group most represented in
committing crime. It’d be interesting to compare these figures to age
groups of SSL subjects when they were first arrested, but unfortunately
the CPD did not release this data. I suspect we’d see a significantly
larger bar for the under 20 age group in this case.

``` r
age_arr_mut <- ssl %>%
  #using forcats:fct_relevel to re-order levels to desired order along x axis
  mutate(age_arr = fct_relevel(age_arr, 
                               c("less than 20",
                                 "20-30",
                                 "30-40",
                                 "40-50",
                                 "50-60",
                                 "60-70",
                                 "70-80"))) %>% 
  #using forcats:fct_collapse to combine older age groups in over fifty
  mutate(age_arr = fct_collapse(age_arr,
                                'over fifty' = c("50-60", 
                                                 "60-70", 
                                                 "70-80"))) 
age_arr_bar <- age_arr_mut %>%
  #dropping na's before graphing
  drop_na() %>%
  ggplot(mapping = aes(age_arr)) +
  #adding a fun color
  geom_bar(fill = "#FF6666") +
  labs(title = "SSL Subjects by Age Last Arrested", 
             x = "Age Group",  
             y = "Count",
             caption = "(Source: Chicago Data Portal)") +
  theme_minimal()


age_arr_bar
```

![](images/scores%20by%20age-1.png)<!-- -->

What proporation of people who the list claims is at risk or extremely
at risk of becoming party to violence are *actually* likely of becoming
party to violence? One way of answering this question is to ask: how
many people on the list are too old to commit violence? (Aging out
indicates inaccuracies in the list, old people commit very few violent
crimes).

More background: very possible list is inaccurate because it was not
updated very frequently, it kept people on the list who had last been
arrested when they were in their 20s – 40 years ago\!)

``` r
ssl_oldies <- ssl %>%
  group_by(ssl_score) %>%
  #count total number of SSL scores per score
  mutate(total = n()) %>%
  #count (oldies) for old high risk SSL subjects
  filter(age_curr == "50-60" | 
           age_curr == "60-70" | 
           age_curr == "70-80" & 
           ssl_score > 250) %>% 
  mutate(oldies = n()) %>% 
  #create percentage
  mutate(percent = oldies/total) %>% 
  #graph
  ggplot(mapping = aes(x = ssl_score, y = percent)) +
  geom_line() +
  # print the y-axis labels using percentages rather than proportions
  scale_y_continuous(labels = scales::percent) +
  # label our graph
  labs(
    title = "Percent of Old Subjects (Age 50+)  per SSL Score",
    x = "SSL Score",
    y = "Percent of Age 50+ Subjects",
    caption = "Source: Chicago Data Portal"
  )

ssl_oldies
```

![](images/oldies%20visualization-1.png)<!-- -->

Results are pretty surprising. Looks like the SSL is pretty accurate in
terms of factoring age into SSL Score; people over 50 make up the
significant majority of subjects assessed at a low risk of becoming
party to violence. It seems like age is perhaps one of the biggest
factors in deciding SSL scores – let’s examine that variance further
with a boxplot.

Variance in SSL score by age group

``` r
ssl_age_curr <- ssl %>% 
  #using forcats:fct_relevel to re-order levels to desired order along x axis
  mutate(age_curr = fct_relevel(age_curr, 
                               c("less than 20",
                                 "20-30",
                                 "30-40",
                                 "40-50",
                                 "50-60",
                                 "60-70",
                                 "70-80"))) %>% 
  #using forcats:fct_collapse to combine older age groups in over fifty
  mutate(age_curr = fct_collapse(age_curr,
                                'over fifty' = c("50-60", 
                                                 "60-70", 
                                                 "70-80")))
ssl_age_group <- ssl_age_curr %>%
  #get rid of na's before graphing
  drop_na() %>%
  ggplot(mapping = aes(x = ssl_score, fill = age_curr, na.rm = FALSE)) +
  geom_density(stat = "bin") +
   labs(title = "SSL Subjects by Current Age Group", 
             x = "SSL Score",  
             y = "Density",
             fill = "Age Group", 
             caption = "(Source: Chicago Data Portal)") +
  theme_minimal()
  

ssl_age_faceted <- ssl_age_curr %>%
  #get rid of na's before graphing
  drop_na() %>%
  ggplot(mapping = aes(x = ssl_score, na.rm = FALSE)) +
  geom_density() +
  facet_wrap(~age_curr) +
  labs(title = "SSL Subjects by Current Age Group, Faceted", 
             x = "SSL Score",  
             y = "Density",
             caption = "(Source: Chicago Data Portal)") +
  theme_minimal()
  

ssl_age_group
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](images/ssl%20scores%20by%20age%20group-1.png)<!-- -->

``` r
ssl_age_faceted
```

![](images/ssl%20scores%20by%20age%20group-2.png)<!-- -->

Highest scores are for age group less than 20\! Lines up with age
demographics of violent crime offenders. Also wondering what that extra
pink lump is. It got smaller after I got rid of NA’s, but even after I
did, it’s still there. Are there people who are older than 80 on the
list? No, there aren’t any people older than 80 on the list.

Now let’s check out gang affiliations by age group.

``` r
ssl_age_gang <- ssl_age_curr %>%
  #get rid of na's before graphing
  drop_na() %>%
  #reordering by bar height and filling by gang affiliation
  ggplot(mapping = aes(x = age_curr, 
                       fill = (gang_aff == TRUE), 
                       na.rm = FALSE)) +
  geom_histogram(stat = "count") +
   labs(title = "SSL Subjects Gang Affiliation by Age Group", 
             x = "Age Group",  
             y = "Count of Subjects",
             fill = "Gang Affiliation", 
             caption = "(Source: Chicago Data Portal)") +
  #manually customizing colors and legend text
  scale_fill_manual(values = c("#E69F00", "#56B4E9"), 
                       name = "Gang Affiliation",
                       breaks = c("TRUE", "FALSE"),
                       labels = c("Affiliated", "Not Affiliated")) 
```

    ## Warning: Ignoring unknown parameters: binwidth, bins, pad

``` r
#next creating a boxplot of gang affiliations and ssl scores  
#first creating a list for factor labels
  gang_labels <- list(
  '0' = "No Gang Affiliation",
  '1' = "Gang Affiliation"
)
#creating a function to push labels (I know this is extra work but trying
#to make up for the lack of tidying I had to do).

gang_labeller <- function(variable,value){
  return(gang_labels[value])
}

ssl_age_gang_box <- ssl_age_curr %>%
  #get rid of na's before graphing
  drop_na() %>%
  #reordering by bar height and filling by gang affiliation
  ggplot(mapping = aes(x = ssl_score, 
                      na.rm = FALSE)) +
  geom_boxplot() +
  #faceting around gang affilliation 
  facet_wrap(~gang_aff,
             labeller = gang_labeller) +
  labs(title = "SSL Subjects Gang Affiliation by Age Group", 
             x = "SSL Score",  
             caption = "(Source: Chicago Data Portal)")
```

    ## Warning: The labeller API has been updated. Labellers taking `variable` and
    ## `value` arguments are now deprecated. See labellers documentation.

``` r
ssl_age_gang
```

![](images/age%20&%20gang%20affiliation-1.png)<!-- -->

``` r
ssl_age_gang_box
```

![](images/age%20&%20gang%20affiliation-2.png)<!-- -->

Median SSL score a bit higher for people with gang affiliations, but
maybe not as much as we might anticipate.

By race – we observe no significant differences in median SSL score by
race. We do see that significantly more black men are subject on the
list than any other race, which is not proportionate to their
perctentage of the city’s population,
[30%](https://www.census.gov/quickfacts/chicagocityillinois).

``` r
ssl_race_data <- ssl %>% 
  #using forcats:fct_relevel to re-order levels to desired order along x axis
  mutate(race = fct_collapse(race,
                             'Other' = c("I", "U", "WBH"),
                             'Black' = c("BLK"), 
                             'White' = c("WHI"),
                             'Hispanic' = c("WWH")))


ssl_race_bar <- ssl_race_data %>% 
  #reordering by bar height and filling by gang affiliation
  ggplot(mapping = aes(x = race, fill = fct_infreq(race))) +
  geom_bar() +
   labs(title = "SSL Subject Counts by Race", 
             x = "Race Group",  
             y = "Counts",
             caption = "(Source: Chicago Data Portal)") +
  theme_minimal() +
  theme(legend.title = element_blank())

ssl_race_box <- ssl_race_data %>% 
  #reordering by bar height and filling by gang affiliation
  ggplot(mapping = aes(x = ssl_score)) +
  geom_boxplot() +
  facet_wrap(~race)
   labs(title = "SSL Subject Counts Faceted by Race", 
             x = "Race Group",  
             y = "Count of Subjects",
             caption = "(Source: Chicago Data Portal)") +
  theme_minimal() +
  theme(legend.title = element_blank())
```

    ## NULL

``` r
ssl_race_bar
```

![](images/by%20race-1.png)<!-- -->

``` r
ssl_race_box
```

![](images/by%20race-2.png)<!-- -->

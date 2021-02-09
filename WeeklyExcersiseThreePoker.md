---
title: 'Weekly Exercises #3'
author: "Michael Poker"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday data
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>% 
  mutate(day = wday(date, label = TRUE)) %>% 
  group_by(vegetable, day) %>% 
  summarize(total_wt = sum(weight)) %>% 
  pivot_wider(names_from = day,
              values_from = total_wt)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"156","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"20","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"186","3":"30","4":"50","5":"12","6":"212","7":"NA","8":"NA"},{"1":"beans","2":"2136","3":"2952","4":"1990","5":"1539","6":"692","7":"868","8":"1852"},{"1":"beets","2":"172","3":"305","4":"72","5":"5394","6":"11","7":"146","8":"83"},{"1":"broccoli","2":"NA","3":"372","4":"NA","5":"NA","6":"75","7":"571","8":"321"},{"1":"carrots","2":"1057","3":"395","4":"160","5":"1213","6":"970","7":"1332","8":"2523"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"8"},{"1":"cilantro","2":"17","3":"NA","4":"2","5":"NA","6":"33","7":"NA","8":"NA"},{"1":"corn","2":"597","3":"344","4":"330","5":"NA","6":"1564","7":"661","8":"2405"},{"1":"cucumbers","2":"4373","3":"2166","4":"4557","5":"1500","6":"3370","7":"1408","8":"2407"},{"1":"edamame","2":"2127","3":"NA","4":"636","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"571","4":"64","5":"NA","6":"NA","7":"NA","8":"31"},{"1":"jalapeño","2":"684","3":"2519","4":"249","5":"102","6":"587","7":"119","8":"218"},{"1":"kale","2":"676","3":"938","4":"128","5":"127","6":"173","7":"375","8":"280"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"191","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"597","3":"1115","4":"416","5":"1112","6":"817","7":"665","8":"538"},{"1":"onions","2":"868","3":"231","4":"321","5":"273","6":"33","7":"118","8":"NA"},{"1":"peas","2":"1294","3":"2102","4":"938","5":"1541","6":"425","7":"933","8":"490"},{"1":"peppers","2":"627","3":"1146","4":"655","5":"322","6":"152","7":"228","8":"1108"},{"1":"potatoes","2":"1271","3":"440","4":"NA","5":"5376","6":"1697","7":"NA","8":"2073"},{"1":"pumpkins","2":"42043","3":"13662","4":"14450","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"105","3":"89","4":"43","5":"67","6":"88","7":"37","8":"NA"},{"1":"raspberries","2":"242","3":"59","4":"152","5":"131","6":"259","7":"NA","8":"NA"},{"1":"rutabaga","2":"3129","3":"NA","4":"NA","5":"NA","6":"1623","7":"8738","8":"NA"},{"1":"spinach","2":"118","3":"67","4":"225","5":"106","6":"89","7":"221","8":"97"},{"1":"squash","2":"25502","3":"11038","4":"8377","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"77","3":"217","4":"NA","5":"40","6":"221","7":"37","8":"NA"},{"1":"Swiss chard","2":"333","3":"487","4":"32","5":"1012","6":"280","7":"566","8":"412"},{"1":"tomatoes","2":"15933","3":"5213","4":"22113","5":"15657","6":"38590","7":"34296","8":"26429"},{"1":"zucchini","2":"1549","3":"5532","4":"7470","5":"15708","6":"8492","7":"5550","8":"926"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>% 
  group_by(vegetable, variety) %>% 
  summarize(tot_harvest_lb = weight*0.0022) %>% 
  left_join(plant_date_loc,
            by = c("vegetable", "variety"))
```

```
## Error in is.data.frame(y): object 'plant_date_loc' not found
```
Theres missing data and therefore there is a missing error and an unknown object. The inner join function would fix this.


  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
  This can be done by calculating how many seeds were used for each vegetable in the garden harvest set and pairing it with the supply costs data set. This will give us the specific prices on supplies.

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>%
  mutate(variety = fct_reorder(variety, date, min)) %>% 
  group_by(variety) %>%
  summarize(tot_harvest_lb = sum(weight*0.0022),
            min_date = min(date)) %>% 
  ggplot(aes(x = tot_harvest_lb, y = fct_rev(variety))) +
  geom_col(fill = "tomato4")+
  labs(title = "Tomato Varieties and Respective Harvest Weight 
       From Earliest to Latest First Harvest Date",
       y = "",
       x = "total pounds")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(lowercase = str_to_lower(variety),
         length = str_length(variety)) %>%
  group_by(vegetable, variety) %>% 
  summarize(length = mean(length)) %>%
  arrange(vegetable, length)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["length"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"unknown","3":"7"},{"1":"asparagus","2":"asparagus","3":"9"},{"1":"basil","2":"Isle of Naxos","3":"13"},{"1":"beans","2":"Bush Bush Slender","3":"17"},{"1":"beans","2":"Chinese Red Noodle","3":"18"},{"1":"beans","2":"Classic Slenderette","3":"19"},{"1":"beets","2":"leaves","3":"6"},{"1":"beets","2":"Sweet Merlin","3":"12"},{"1":"beets","2":"Gourmet Golden","3":"14"},{"1":"broccoli","2":"Yod Fah","3":"7"},{"1":"broccoli","2":"Main Crop Bravado","3":"17"},{"1":"carrots","2":"Bolero","3":"6"},{"1":"carrots","2":"Dragon","3":"6"},{"1":"carrots","2":"greens","3":"6"},{"1":"carrots","2":"King Midas","3":"10"},{"1":"chives","2":"perrenial","3":"9"},{"1":"cilantro","2":"cilantro","3":"8"},{"1":"corn","2":"Dorinny Sweet","3":"13"},{"1":"corn","2":"Golden Bantam","3":"13"},{"1":"cucumbers","2":"pickling","3":"8"},{"1":"edamame","2":"edamame","3":"7"},{"1":"hot peppers","2":"thai","3":"4"},{"1":"hot peppers","2":"variety","3":"7"},{"1":"jalapeño","2":"giant","3":"5"},{"1":"kale","2":"Heirloom Lacinto","3":"16"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"17"},{"1":"lettuce","2":"reseed","3":"6"},{"1":"lettuce","2":"Tatsoi","3":"6"},{"1":"lettuce","2":"mustard greens","3":"14"},{"1":"lettuce","2":"Lettuce Mixture","3":"15"},{"1":"lettuce","2":"Farmer's Market Blend","3":"21"},{"1":"onions","2":"Delicious Duo","3":"13"},{"1":"onions","2":"Long Keeping Rainbow","3":"20"},{"1":"peas","2":"Magnolia Blossom","3":"16"},{"1":"peas","2":"Super Sugar Snap","3":"16"},{"1":"peppers","2":"green","3":"5"},{"1":"peppers","2":"variety","3":"7"},{"1":"potatoes","2":"red","3":"3"},{"1":"potatoes","2":"purple","3":"6"},{"1":"potatoes","2":"Russet","3":"6"},{"1":"potatoes","2":"yellow","3":"6"},{"1":"pumpkins","2":"saved","3":"5"},{"1":"pumpkins","2":"New England Sugar","3":"17"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"21"},{"1":"radish","2":"Garden Party Mix","3":"16"},{"1":"raspberries","2":"perrenial","3":"9"},{"1":"rutabaga","2":"Improved Helenor","3":"16"},{"1":"spinach","2":"Catalina","3":"8"},{"1":"squash","2":"delicata","3":"8"},{"1":"squash","2":"Red Kuri","3":"8"},{"1":"squash","2":"Blue (saved)","3":"12"},{"1":"squash","2":"Waltham Butternut","3":"17"},{"1":"strawberries","2":"perrenial","3":"9"},{"1":"Swiss chard","2":"Neon Glow","3":"9"},{"1":"tomatoes","2":"grape","3":"5"},{"1":"tomatoes","2":"Big Beef","3":"8"},{"1":"tomatoes","2":"Jet Star","3":"8"},{"1":"tomatoes","2":"Better Boy","3":"10"},{"1":"tomatoes","2":"Black Krim","3":"10"},{"1":"tomatoes","2":"Bonny Best","3":"10"},{"1":"tomatoes","2":"Brandywine","3":"10"},{"1":"tomatoes","2":"Old German","3":"10"},{"1":"tomatoes","2":"volunteers","3":"10"},{"1":"tomatoes","2":"Amish Paste","3":"11"},{"1":"tomatoes","2":"Cherokee Purple","3":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"15"},{"1":"zucchini","2":"Romanesco","3":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  mutate(has_er_ar = str_detect(variety, "er|ar")) %>%
  filter(has_er_ar == TRUE) %>% 
  distinct(vegetable, variety)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"radish","2":"Garden Party Mix"},{"1":"lettuce","2":"Farmer's Market Blend"},{"1":"peas","2":"Super Sugar Snap"},{"1":"chives","2":"perrenial"},{"1":"strawberries","2":"perrenial"},{"1":"asparagus","2":"asparagus"},{"1":"lettuce","2":"mustard greens"},{"1":"raspberries","2":"perrenial"},{"1":"beans","2":"Bush Bush Slender"},{"1":"beets","2":"Sweet Merlin"},{"1":"hot peppers","2":"variety"},{"1":"tomatoes","2":"Cherokee Purple"},{"1":"tomatoes","2":"Better Boy"},{"1":"peppers","2":"variety"},{"1":"tomatoes","2":"Mortgage Lifter"},{"1":"tomatoes","2":"Old German"},{"1":"tomatoes","2":"Jet Star"},{"1":"carrots","2":"Bolero"},{"1":"tomatoes","2":"volunteers"},{"1":"beans","2":"Classic Slenderette"},{"1":"pumpkins","2":"Cinderella's Carraige"},{"1":"squash","2":"Waltham Butternut"},{"1":"pumpkins","2":"New England Sugar"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){300px}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){300px}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data-Small.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(x = sdate))+
  geom_density()+
  labs(title = "Distribution of Bike Rentals by Date",
       x = "",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  This shows bike rentals as time moves on. The peak time period was between late October to the start of November. Also as the weather got bad the rentals slowed down from december to january.
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(hour = hour(sdate),
         minute = minute(sdate),
         time = (hour + (minute/60))) %>% 
  ggplot(aes(x = time))+
  geom_density()+
  labs(title = "Distribution of Bike Rentals by Time of Day",
       x = "hour of the day",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  This plot shows bike rentals to the time of day. the bike renatls see a spike when everyone is awake early in the morning and a decrease when the work day is usually done around 5:30pm.
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(wday = wday(sdate, label = TRUE)) %>% 
  ggplot(aes(y = fct_rev(wday)))+
  geom_bar()+
  labs(title = "Bike Rentals by Day of the Week",
       x = "",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  This bar Plot shows bike rentals compared to every day of the week. the most popular day for bike rentals is Friday. Also the weekdays all have more rentals than Saturday and Sunday.
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>% 
  mutate(hour = hour(sdate),
         minute = minute(sdate),
         time = (hour + (minute/60)),
         wday = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time))+
  facet_wrap(vars(wday))+
  geom_density()+
   labs(title = "Distribution of Bike Rentals by Time of Day",
       x = "",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  This data plot combines two of the plots above and shows on each individual day the distribution of when rentals are purchased on that given day. Saturday and Sunday just have one spike which is around Midday and each weekday there is two spikes one for the morning rush and one for the afternoon rush for people on the way home.
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>% 
  mutate(hour = hour(sdate),
         minute = minute(sdate),
         time = (hour + (minute/60)),
         wday = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time, fill = client))+
  facet_wrap(vars(wday))+
  geom_density(alpha = .5)+
   labs(title = "Distribution of Bike Rentals by Time of Day
        and Type of Client",
       x = "",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>% 
  mutate(hour = hour(sdate),
         minute = minute(sdate),
         time = (hour + (minute/60)),
         wday = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time, fill = client))+
  facet_wrap(vars(wday))+
  geom_density(alpha = .5, position = position_stack())+
   labs(title = "Distribution of Bike Rentals by Time of Day
        and Type of Client",
       x = "",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  This is much better with making conclusions because there is less overlapping and more definitive lines. Overall, it tells a much better story in a much clearer way.
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(hour = hour(sdate),
         minute = minute(sdate),
         time = (hour + (minute/60)),
         day_of_week = wday(sdate, label = TRUE),
         day_type = ifelse(wday(sdate) %in% c(1,7), "weekend", "weekday")) %>% 
  ggplot(aes(x = time, fill = client))+
  facet_wrap(vars(day_type))+
  geom_density(alpha = .5,)+
   labs(title = "Distribution of Bike Rentals by Time of Day, Type of Day,
        and Type of Client",
       x = "",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>% 
  mutate(hour = hour(sdate),
         minute = minute(sdate),
         time = (hour + (minute/60)),
         day_of_week = wday(sdate, label = TRUE),
         day_type = ifelse(wday(sdate) %in% c(1,7), "weekend", "weekday")) %>% 
  ggplot(aes(x = time, fill = day_type))+
  facet_wrap(vars(client))+
  geom_density(alpha = .5,)+
   labs(title = "Distribution of Bike Rentals by Time of Day, Type of Day,
        and Type of Client",
       x = "",
       y = "")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
  This graph facets on clients and fills like weekday unlike the other. Its clear in both to see the density distributions.
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Trips %>% 
  count(sstation) %>% 
  inner_join(Stations,
             by = c("sstation" = "name")) %>% 
  ggplot(aes(x = long, y = lat, color = n))+
  geom_point()+
  labs(title = "Total Number of Departures From Each Station
       by Latitude and Longitude",
       x = "longitude",
       y = "latitude")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
Trips %>% 
  group_by(sstation) %>% 
  summarize(tot_dept = n(), 
            prop_casual = mean(client == "Casual")) %>% 
  left_join(Stations,
            by = c("sstation" = "name")) %>% 
  ggplot(aes(x = long, y = lat, color = prop_casual))+
  geom_point()+
  labs(title = "Areas With Stations with a Higher %
  of Departures by Casual Users by Latitude and Longitude",
       x = "longitude",
       y = "latitude")
```

![](WeeklyExcersiseThreePoker_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
  
  There is a main cluster of points between -77.1 and -77.0 that ranges from 38.8 and 39.0. There is also a tiny cluster around 39.1 and around -77.15. Finally due to their shades of blue their proportion is less than 0.3.
  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

```r
top_trip <- Trips %>%
  mutate(sdate = as_date(sdate)) %>% 
  count(sstation, sdate) %>% 
  slice_max(n = 10, order_by = n, with_ties = FALSE)

top_trip
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["sstation"],"name":[1],"type":["chr"],"align":["left"]},{"label":["sdate"],"name":[2],"type":["date"],"align":["right"]},{"label":["n"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7"},{"1":"14th & V St NW","2":"2014-11-07","3":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

```r
Trips %>% 
  mutate(sdate = as_date(sdate)) %>% 
  inner_join(top_trip, 
             by = c("sstation", "sdate"))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["duration"],"name":[1],"type":["chr"],"align":["left"]},{"label":["sdate"],"name":[2],"type":["date"],"align":["right"]},{"label":["sstation"],"name":[3],"type":["chr"],"align":["left"]},{"label":["edate"],"name":[4],"type":["dttm"],"align":["right"]},{"label":["estation"],"name":[5],"type":["chr"],"align":["left"]},{"label":["bikeno"],"name":[6],"type":["chr"],"align":["left"]},{"label":["client"],"name":[7],"type":["chr"],"align":["left"]},{"label":["n"],"name":[8],"type":["int"],"align":["right"]}],"data":[{"1":"0h 12m 43s","2":"2014-10-02","3":"Columbus Circle / Union Station","4":"2014-10-02 08:36:00","5":"14th St & New York Ave NW","6":"W20228","7":"Registered","8":"7"},{"1":"0h 7m 57s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 11:42:00","5":"21st St & Constitution Ave NW","6":"W20032","7":"Registered","8":"8"},{"1":"2h 10m 20s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 14:45:00","5":"Ohio Dr & West Basin Dr SW / MLK & FDR Memorials","6":"W01221","7":"Casual","8":"9"},{"1":"0h 30m 6s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 12:28:00","5":"Jefferson Memorial","6":"W20256","7":"Casual","8":"9"},{"1":"0h 7m 41s","2":"2014-10-01","3":"Massachusetts Ave & Dupont Circle NW","4":"2014-10-01 22:09:00","5":"14th & Belmont St NW","6":"W20200","7":"Registered","8":"7"},{"1":"0h 20m 32s","2":"2014-10-16","3":"New Hampshire Ave & T St NW","4":"2014-10-16 07:25:00","5":"North Capitol St & G Pl NE","6":"W21470","7":"Registered","8":"7"},{"1":"0h 13m 0s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 06:21:00","5":"Maryland & Independence Ave SW","6":"W00733","7":"Registered","8":"11"},{"1":"0h 23m 24s","2":"2014-10-02","3":"Columbus Circle / Union Station","4":"2014-10-02 09:42:00","5":"17th & K St NW / Farragut Square","6":"W00281","7":"Registered","8":"7"},{"1":"0h 13m 36s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 13:56:00","5":"Jefferson Memorial","6":"W00924","7":"Casual","8":"9"},{"1":"0h 3m 41s","2":"2014-10-06","3":"17th St & Massachusetts Ave NW","4":"2014-10-06 12:49:00","5":"Massachusetts Ave & Dupont Circle NW","6":"W01470","7":"Registered","8":"7"},{"1":"0h 11m 15s","2":"2014-10-16","3":"New Hampshire Ave & T St NW","4":"2014-10-16 09:16:00","5":"17th & G St NW","6":"W20852","7":"Registered","8":"7"},{"1":"0h 13m 38s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 22:21:00","5":"Jefferson Memorial","6":"W20283","7":"Casual","8":"8"},{"1":"0h 24m 26s","2":"2014-10-25","3":"Georgetown Harbor / 30th St NW","4":"2014-10-25 18:26:00","5":"Georgetown Harbor / 30th St NW","6":"W21957","7":"Casual","8":"7"},{"1":"0h 17m 49s","2":"2014-10-25","3":"Georgetown Harbor / 30th St NW","4":"2014-10-25 16:36:00","5":"Harvard St & Adams Mill Rd NW","6":"W00842","7":"Registered","8":"7"},{"1":"0h 8m 8s","2":"2014-10-02","3":"Columbus Circle / Union Station","4":"2014-10-02 06:16:00","5":"8th & D St NW","6":"W21071","7":"Registered","8":"7"},{"1":"0h 6m 31s","2":"2014-10-01","3":"Massachusetts Ave & Dupont Circle NW","4":"2014-10-01 08:56:00","5":"New Hampshire Ave & 24th St NW","6":"W21010","7":"Registered","8":"7"},{"1":"0h 17m 0s","2":"2014-10-25","3":"Georgetown Harbor / 30th St NW","4":"2014-10-25 16:36:00","5":"Harvard St & Adams Mill Rd NW","6":"W20600","7":"Casual","8":"7"},{"1":"0h 17m 45s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 14:01:00","5":"Smithsonian / Jefferson Dr & 12th St SW","6":"W01464","7":"Casual","8":"8"},{"1":"0h 11m 5s","2":"2014-10-06","3":"17th St & Massachusetts Ave NW","4":"2014-10-06 21:44:00","5":"10th & U St NW","6":"W20675","7":"Registered","8":"7"},{"1":"0h 57m 4s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 10:44:00","5":"Smithsonian / Jefferson Dr & 12th St SW","6":"W01059","7":"Casual","8":"9"},{"1":"0h 9m 23s","2":"2014-11-07","3":"14th & V St NW","4":"2014-11-07 20:03:00","5":"18th & M St NW","6":"W21318","7":"Registered","8":"6"},{"1":"0h 20m 5s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 18:50:00","5":"14th St & New York Ave NW","6":"W20890","7":"Casual","8":"9"},{"1":"0h 53m 48s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 10:44:00","5":"Smithsonian / Jefferson Dr & 12th St SW","6":"W00653","7":"Casual","8":"9"},{"1":"0h 19m 21s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 11:35:00","5":"Maryland & Independence Ave SW","6":"W20232","7":"Casual","8":"9"},{"1":"0h 12m 26s","2":"2014-10-06","3":"17th St & Massachusetts Ave NW","4":"2014-10-06 19:25:00","5":"New Jersey Ave & N St NW/Dunbar HS","6":"W20069","7":"Registered","8":"7"},{"1":"0h 28m 33s","2":"2014-10-25","3":"Georgetown Harbor / 30th St NW","4":"2014-10-25 17:44:00","5":"Lincoln Memorial","6":"W21439","7":"Casual","8":"7"},{"1":"0h 20m 47s","2":"2014-10-01","3":"Massachusetts Ave & Dupont Circle NW","4":"2014-10-01 23:07:00","5":"North Capitol St & F St NW","6":"W21122","7":"Registered","8":"7"},{"1":"0h 14m 5s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 16:06:00","5":"Maryland & Independence Ave SW","6":"W00022","7":"Casual","8":"9"},{"1":"0h 8m 52s","2":"2014-11-07","3":"14th & V St NW","4":"2014-11-07 17:50:00","5":"17th St & Massachusetts Ave NW","6":"W00595","7":"Registered","8":"6"},{"1":"0h 11m 48s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 07:54:00","5":"L'Enfant Plaza / 7th & C St SW","6":"W20307","7":"Registered","8":"11"},{"1":"0h 36m 38s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 11:48:00","5":"Maryland & Independence Ave SW","6":"W21644","7":"Casual","8":"9"},{"1":"0h 16m 10s","2":"2014-10-06","3":"17th St & Massachusetts Ave NW","4":"2014-10-06 09:03:00","5":"25th St & Pennsylvania Ave NW","6":"W20141","7":"Registered","8":"7"},{"1":"0h 38m 22s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 16:28:00","5":"Jefferson Memorial","6":"W21946","7":"Casual","8":"9"},{"1":"0h 8m 44s","2":"2014-10-16","3":"New Hampshire Ave & T St NW","4":"2014-10-16 08:28:00","5":"19th & K St NW","6":"W20787","7":"Registered","8":"7"},{"1":"0h 25m 15s","2":"2014-10-25","3":"Georgetown Harbor / 30th St NW","4":"2014-10-25 14:08:00","5":"Constitution Ave & 2nd St NW/DOL","6":"W21629","7":"Casual","8":"7"},{"1":"0h 11m 54s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 13:59:00","5":"Smithsonian / Jefferson Dr & 12th St SW","6":"W01384","7":"Casual","8":"8"},{"1":"0h 9m 45s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 08:28:00","5":"Potomac Ave & 8th St SE","6":"W01369","7":"Registered","8":"11"},{"1":"0h 10m 52s","2":"2014-11-07","3":"14th & V St NW","4":"2014-11-07 07:49:00","5":"New York Ave & 15th St NW","6":"W01452","7":"Registered","8":"6"},{"1":"0h 9m 21s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 16:13:00","5":"Washington & Independence Ave SW/HHS","6":"W20584","7":"Registered","8":"9"},{"1":"0h 9m 10s","2":"2014-10-16","3":"New Hampshire Ave & T St NW","4":"2014-10-16 11:55:00","5":"19th St & Pennsylvania Ave NW","6":"W01078","7":"Registered","8":"7"},{"1":"0h 6m 41s","2":"2014-10-06","3":"17th St & Massachusetts Ave NW","4":"2014-10-06 08:06:00","5":"14th & R St NW","6":"W00684","7":"Registered","8":"7"},{"1":"0h 7m 41s","2":"2014-11-07","3":"14th & V St NW","4":"2014-11-07 16:49:00","5":"Massachusetts Ave & Dupont Circle NW","6":"W20094","7":"Registered","8":"6"},{"1":"0h 10m 17s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 10:12:00","5":"11th & F St NW","6":"W00766","7":"Casual","8":"11"},{"1":"0h 9m 45s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 20:17:00","5":"13th & H St NE","6":"W20481","7":"Registered","8":"11"},{"1":"0h 14m 48s","2":"2014-11-07","3":"14th & V St NW","4":"2014-11-07 08:14:00","5":"17th & G St NW","6":"W01215","7":"Registered","8":"6"},{"1":"0h 3m 9s","2":"2014-10-06","3":"17th St & Massachusetts Ave NW","4":"2014-10-06 19:26:00","5":"15th & P St NW","6":"W00409","7":"Registered","8":"7"},{"1":"0h 10m 33s","2":"2014-11-07","3":"14th & V St NW","4":"2014-11-07 14:32:00","5":"8th & H St NW","6":"W21189","7":"Registered","8":"6"},{"1":"0h 7m 51s","2":"2014-10-01","3":"Massachusetts Ave & Dupont Circle NW","4":"2014-10-01 18:29:00","5":"Calvert St & Woodley Pl NW","6":"W21466","7":"Registered","8":"7"},{"1":"0h 3m 33s","2":"2014-10-02","3":"Columbus Circle / Union Station","4":"2014-10-02 09:22:00","5":"Constitution Ave & 2nd St NW/DOL","6":"W00490","7":"Registered","8":"7"},{"1":"0h 14m 35s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 20:04:00","5":"Smithsonian / Jefferson Dr & 12th St SW","6":"W20974","7":"Casual","8":"9"},{"1":"0h 18m 14s","2":"2014-10-02","3":"Columbus Circle / Union Station","4":"2014-10-02 14:27:00","5":"New York Ave & 15th St NW","6":"W00759","7":"Casual","8":"7"},{"1":"1h 32m 53s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 18:24:00","5":"14th & D St NW / Ronald Reagan Building","6":"W20284","7":"Casual","8":"8"},{"1":"0h 1m 48s","2":"2014-10-01","3":"Massachusetts Ave & Dupont Circle NW","4":"2014-10-01 19:29:00","5":"21st & M St NW","6":"W00928","7":"Registered","8":"7"},{"1":"0h 5m 33s","2":"2014-10-25","3":"Georgetown Harbor / 30th St NW","4":"2014-10-25 11:17:00","5":"34th & Water St NW","6":"W20311","7":"Casual","8":"7"},{"1":"0h 3m 57s","2":"2014-10-06","3":"17th St & Massachusetts Ave NW","4":"2014-10-06 18:34:00","5":"15th & P St NW","6":"W21041","7":"Registered","8":"7"},{"1":"0h 25m 37s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 12:19:00","5":"Iwo Jima Memorial/N Meade & 14th St N","6":"W21089","7":"Casual","8":"9"},{"1":"0h 6m 28s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 15:08:00","5":"11th & H St NE","6":"W01357","7":"Registered","8":"11"},{"1":"0h 27m 4s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 08:15:00","5":"8th & Eye St SE / Barracks Row","6":"W21619","7":"Registered","8":"8"},{"1":"1h 22m 31s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 14:20:00","5":"New York Ave & 15th St NW","6":"W21494","7":"Casual","8":"9"},{"1":"0h 21m 16s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 12:23:00","5":"Lincoln Memorial","6":"W01177","7":"Casual","8":"9"},{"1":"0h 41m 18s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 21:35:00","5":"New York Ave & 15th St NW","6":"W20605","7":"Registered","8":"9"},{"1":"0h 8m 49s","2":"2014-10-02","3":"Columbus Circle / Union Station","4":"2014-10-02 17:32:00","5":"3rd St & Pennsylvania Ave SE","6":"W21214","7":"Registered","8":"7"},{"1":"0h 11m 33s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 18:18:00","5":"3rd & G St SE","6":"W21695","7":"Registered","8":"11"},{"1":"0h 6m 54s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 17:42:00","5":"11th & H St NE","6":"W00229","7":"Registered","8":"11"},{"1":"0h 49m 1s","2":"2014-10-05","3":"Lincoln Memorial","4":"2014-10-05 14:51:00","5":"Maryland & Independence Ave SW","6":"W20927","7":"Registered","8":"9"},{"1":"0h 23m 6s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 14:58:00","5":"Smithsonian / Jefferson Dr & 12th St SW","6":"W21407","7":"Casual","8":"11"},{"1":"0h 48m 48s","2":"2014-12-27","3":"Jefferson Dr & 14th St SW","4":"2014-12-27 14:40:00","5":"19th St & Constitution Ave NW","6":"W21453","7":"Casual","8":"9"},{"1":"0h 3m 58s","2":"2014-10-16","3":"New Hampshire Ave & T St NW","4":"2014-10-16 08:35:00","5":"Massachusetts Ave & Dupont Circle NW","6":"W21089","7":"Registered","8":"7"},{"1":"0h 19m 16s","2":"2014-10-01","3":"Massachusetts Ave & Dupont Circle NW","4":"2014-10-01 21:02:00","5":"Calvert St & Woodley Pl NW","6":"W20884","7":"Registered","8":"7"},{"1":"0h 5m 53s","2":"2014-10-25","3":"Georgetown Harbor / 30th St NW","4":"2014-10-25 18:19:00","5":"New Hampshire Ave & 24th St NW","6":"W21709","7":"Registered","8":"7"},{"1":"0h 8m 8s","2":"2014-10-16","3":"New Hampshire Ave & T St NW","4":"2014-10-16 16:37:00","5":"14th & Harvard St NW","6":"W21623","7":"Registered","8":"7"},{"1":"0h 23m 8s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 12:40:00","5":"Jefferson Dr & 14th St SW","6":"W00851","7":"Casual","8":"8"},{"1":"1h 20m 27s","2":"2014-10-09","3":"Lincoln Memorial","4":"2014-10-09 16:37:00","5":"Lincoln Memorial","6":"W00006","7":"Casual","8":"8"},{"1":"0h 2m 3s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 18:22:00","5":"3rd & H St NE","6":"W20792","7":"Registered","8":"11"},{"1":"0h 13m 19s","2":"2014-10-02","3":"Columbus Circle / Union Station","4":"2014-10-02 17:46:00","5":"14th & D St SE","6":"W21573","7":"Registered","8":"7"},{"1":"0h 10m 43s","2":"2014-11-12","3":"Columbus Circle / Union Station","4":"2014-11-12 14:47:00","5":"Eastern Market Metro / Pennsylvania Ave & 7th St SE","6":"W01158","7":"Registered","8":"11"},{"1":"0h 2m 49s","2":"2014-10-16","3":"New Hampshire Ave & T St NW","4":"2014-10-16 09:19:00","5":"Massachusetts Ave & Dupont Circle NW","6":"W00066","7":"Registered","8":"7"},{"1":"0h 11m 31s","2":"2014-10-01","3":"Massachusetts Ave & Dupont Circle NW","4":"2014-10-01 13:25:00","5":"37th & O St NW / Georgetown University","6":"W21080","7":"Registered","8":"7"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.


```r
Trips %>% 
  mutate(sdate = as_date(sdate)) %>% 
  inner_join(top_trip, by = c("sstation", "sdate")) %>%
  mutate(day_of_week = wday(sdate, label = TRUE)) %>% 
  group_by(client, day_of_week) %>% 
  summarize(trips_day = n()) %>% 
  group_by(client) %>% 
  mutate(prop = trips_day/sum(trips_day)) %>% 
  pivot_wider(id_cols = day_of_week,
              names_from = client,
              values_from = prop)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["day_of_week"],"name":[1],"type":["ord"],"align":["right"]},{"label":["Casual"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Registered"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"Sun","2":"0.24137931","3":"0.04081633"},{"1":"Wed","2":"0.06896552","3":"0.32653061"},{"1":"Thu","2":"0.24137931","3":"0.30612245"},{"1":"Sat","2":"0.44827586","3":"0.06122449"},{"1":"Mon","2":"NA","3":"0.14285714"},{"1":"Fri","2":"NA","3":"0.12244898"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

For days of the weekend, the proportion of casual riders is greater than the proportion of registered riders. On weekdays except for Friday and Monday, which isn't measured above, the proportion of registered is greater. This can be explained by regular work commuters during the week compared  recreational fun on the weekends.

**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

[Github Link](https://github.com/mpoker2/Weekly-Excersie-Three)


## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  

  I have no clue very sorry.
  


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**

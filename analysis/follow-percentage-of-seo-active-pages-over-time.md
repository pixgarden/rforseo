# Track SEO active pages percentage over time x

### What are active pages? and Why would you want to track them?

An active page is a page which generates at least one SEO visit over a period.  If a page has at least one visit it means that its indexed and 'Google" doesn't think it's a useless page. It is a good indicator of the  SEO health of a website.

To make things even more interesting we will grab google search console data and compare them to the number of pages submitted in the XML sitemap file.

### step 1: Counting active URLs using Search Console data

\( see article about [grabbing Search Console data](../apis/searchconsoler-x.md)\)



```r
library(searchConsoleR)
library(googleAuthR)
scr_auth()

# Load
sc_websites <- list_websites()

# and display the list
View(sc_websites)

# pitck the one
hostname <- "https://www.rforseo.com/"
require(lubridate)

#  we want data between now and 2 months ago
now <- lubridate::today()-3
month(beforedate) <- month(now) - 2
day(beforedate) <- days_in_month(beforedate)

# we ask for data with dates and pages


gsc_all_queries <- search_analytics(hostname,
                                    beforedate,now,
                                    c("date", "page"), rowLimit = 80000)



```



```r
library(dplyr)

# we count url with clicks
gsc_all_queries_clicks <- gsc_all_queries %>%
  filter(clicks != 0) %>%
  group_by(date) %>%
  tally()

colnames(gsc_all_queries_clicks) <- c("date","clicks")

# we count url with impressions
gsc_all_queries_impr <- gsc_all_queries %>%
  filter(impressions != 0) %>%
  group_by(date) %>%
  tally()

colnames(gsc_all_queries_impr) <- c("date","impr")

# we merge those two
gsc_all_queries_stats <- merge(gsc_all_queries_clicks, gsc_all_queries_impr)




```



```r
# we scrape the url count from github csv
urls <- read.csv(url("https://raw.githubusercontent.com/pixgarden/scrape-automation/main/data/xml_url_count.csv"))

# rename columns
colnames(urls)  <- c("date","urls")

# transform string date into real dates
urls$date <- as.Date(urls$date)

# merge with google search console data
# because column names match the merge function dont need arguments
gsc_all_queries_merged <- merge(gsc_all_queries_stats, urls)



```



```r
# we count url with no but with impression
gsc_all_queries_merged$impr <-gsc_all_queries_merged$impr - gsc_all_queries_merged$clicks
# we count url with no impression and no clicks
gsc_all_queries_merged$urls <-gsc_all_queries_merged$urls - gsc_all_queries_merged$impr

# rename columns
colnames(gsc_all_queries_merged) <- c("date", "url-with-clics","url-only-impr","url-no-impr")


```





```r
require(tidyr)
test <- gather(gsc_all_queries_merged, urls, count, 2:4)
esquisse::esquisser(test)

ggplot(test) +
  aes(x = date, fill = urls, weight = count) +
  geom_bar() +
  scale_fill_hue() +
  theme_minimal()

library(ggplot2)

ggplot(test) +
 aes(x = date, fill = urls, weight = count) +
 geom_bar() +
 scale_fill_hue() +
 theme_minimal()

```


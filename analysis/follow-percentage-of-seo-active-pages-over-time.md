# Track SEO active pages percentage over time x

### What are active pages? and Why would you want to track them?

An active page is a page which generates at least one SEO visit over a period.  If a page has at least one visit it means that its indexed and 'Google" doesn't think it's a useless page. It is a good indicator of the  SEO health of a website.









```r
library(searchConsoleR)
install.packages("searchConsoleR")

library(googleAuthR)
scr_auth()
# Load
sc_websites <- list_websites()
# and display the list
View(sc_websites)

hostname <- "https://www.rforseo.com/"
require(lubridate)
beforedate <- lubridate::today()
month(beforedate) <- month(beforedate) - 2
day(beforedate) <- days_in_month(beforedate)

gsc_all_queries <- search_analytics(hostname,
                                    beforedate, tree_days_ago,
                                    c("date", "page"), rowLimit = 80000)



```



```r
library(dplyr)

gsc_all_queries_clicks <- gsc_all_queries %>%
  filter(clicks != 0) %>%
  group_by(date) %>%
  tally()

colnames(gsc_all_queries_clicks) <- c("date","clicks")

gsc_all_queries_impr <- gsc_all_queries %>%
  filter(impressions != 0) %>%
  group_by(date) %>%
  tally()

colnames(gsc_all_queries_impr) <- c("date","impr")

gsc_all_queries_stats <- merge(gsc_all_queries_clicks, gsc_all_queries_impr)




```



```r
urls <- read.csv(url("https://raw.githubusercontent.com/pixgarden/scrape-automation/main/data/xml_url_count.csv"))

colnames(urls)  <- c("date","urls")

urls$date <- as.Date(urls$date)


gsc_all_queries_merged <- merge(gsc_all_queries_stats, urls)



```



```r
gsc_all_queries_merged$impr <-gsc_all_queries_merged$impr - gsc_all_queries_merged$clicks
gsc_all_queries_merged$urls <-gsc_all_queries_merged$urls - gsc_all_queries_merged$impr

colnames(gsc_all_queries_merged) <- c("date", "url-clics","url-only-impr","url-without-impr")


```


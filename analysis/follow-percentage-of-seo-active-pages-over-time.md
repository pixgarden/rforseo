# Tracking SEO active pages percentage over time

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

urls <- read.csv(url("https://raw.githubusercontent.com/pixgarden/scrape-automation/main/data/xml_url_count.csv"))

colnames(urls)  <- c("date","urls")

urls$date <- as.Date(urls$date)

View(merge(gsc_all_queries_stats, urls))


```


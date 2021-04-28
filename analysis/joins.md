---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# How to join datasets x

If you have crawled your website, it might be quite nice to check how of these pages got some SEO traffic. To do that we'll need to link the two dataset and as you will see, its quite simple

### Crawl data

Using [rcrawler](../crawl/rcrawler.md), we've collected our pages

```r
library(Rcrawler)
Rcrawler(Website = "https://www.rforseo.com/")
```

we now have a dataset \(dataframe\) of url associated to their crawl depht called `INDEX`

```r
View(INDEX)
```

![second column is the url](../.gitbook/assets/screenshot-2021-04-21-at-11.11.18-pm.png)

### Google analytics data

Using [googleAnalyticsR](https://code.markedmondson.me/googleAnalyticsR/) package we grab Google Analytics SEO Landing page

```text
ga <- google_analytics(ga_id, 
    date_range = c("2021-01-01", "2021-02-01"),
    metrics = "sessions",
    dimensions = c("date","medium","landingPagePath"),
    anti_sample = TRUE)View(extract)
View(ga)
```

### **Fusion** Dance

tbc




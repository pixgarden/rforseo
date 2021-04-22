---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# What's crawling and why is it useful? X

### What is crawling?

It's fetching the contents of a web page using an app or a script. This is what Google is doing when its bot explores the web and analyze webpage content.

### How crawling is useful for SEO?

As someone doing SEO you need to know what you are showing to Google. What your website looks like from a \(Google\) bot perspective. You need the [check the quality of your XML sitemap](download-xml-sitemaps.md) if you are submitting one. You need to [check the website webpages](rcrawler.md). Checking the web server logs is also a good idea, to know what Google bot is doing on your website.

You can also respectfully crawl your competitors' website to better understand their SEO strategy.

### Crawling is also interesting to grab data. 

There are some great public datasets out there, even wikipedia is a great source. Let's take this nice recap of [Hundred Years' War battles](https://en.wikipedia.org/wiki/List_of_Hundred_Years%27_War_battles) for example, it can be crawled   


```r
library(dplyr)
library(rvest)
url <- "https://en.wikipedia.org/wiki/List_of_Hundred_Years%27_War_battles"
battles <- url %>%
  read_html() %>%
  html_nodes(xpath='//*[@id="mw-content-text"]/div[1]/center/table') %>%
  html_table() %>%
  as.data.frame()

battles$Year <- as.numeric(battles$Year)
```

and displayed on a plot

```r
devtools::install_github('bbc/bbplot')
library(ggplot2)
library(bbplot)

battles %>%
 filter(Victor %in% c("England", "France")) %>%
 ggplot() +
 aes(x = Year, fill = Victor) +
 geom_bar() +
 scale_fill_hue() +
 bbc_style()
```

et voila

![](../.gitbook/assets/rplot%20%281%29.png)

It's not really SEO, but it can be useful. I've also been using it to check the quality of the data on the sites, like product prices, image availability, etc.

Again, Screamingfrog or another crawler might be a better choice, it depends on how integrated you want that to be.

#### 




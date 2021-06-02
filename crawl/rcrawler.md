---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Website Crawling and SEO extraction with Rcrawler

This section is relying on a package called [Rcrawler](https://cran.r-project.org/web/packages/Rcrawler/Rcrawler.pdf) by Salim Khalil. It’s a very handy crawler with some nice functionalities.

### Installation

After [R](https://www.r-project.org/) is being installed and [rstudio](https://www.rstudio.com/) launched, same as always, we’ll install and load our package:

```r
#install to be run once 
install.packages("Rcrawler")
# and loading
library(Rcrawler)
```

### Crawl an entire website with Rcrawler <a id="1-crawl-an-entire-website"></a>

To launch a simple website analysis, you only need this line of code:

```r
Rcrawler(Website = "https://www.gokam.co.uk/")
```

It will crawl the entire website and provide you with the data

![Less than 30s to crawl a small website](https://www.gokam.fr/wp-content/uploads/2020/03/V0goQ3vuzC.gif)

\_\_

After the crawl is being done, you’ll have access to:

**The INDEX variable**

it’s a data frame, if don’t know what’s a data frame, it’s like an excel file. Please note that it will be overwritten every time so [export it](https://www.gokam.co.uk/export-your-data-from-r/) if you want to keep it!

To take a look at it, just run

```r
View(INDEX)
```

![INDEX data frame](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-09-18.02.35-1024x476.png)

\_\_

Most of the columns are self-explanatory. Usually, the most interesting ones are ‘**Http Resp**‘ and ‘**Level**‘

The Level is what SEOs call “crawl depth” or “page depth”. With it, you can easily check how far from the homepage some webpages are.

Quick example with [BrightonSEO](https://www.brightonseo.com/) website, let’s do a quick ‘ggplot’ and we’ll be able to see pages 

![page count by level](https://www.gokam.co.uk/wp-content/uploads/2020/08/brightonSEO_crawl_depht_2020.png)

```r
#here the code to run to see the plot
 
# install ggplot plot library to be run once
install.packages("ggplot2")
# Loading library
library(ggplot2)
# Convert Level to number
INDEX$Level <- as.integer(INDEX$Level)
 
# Make plot
# 1 define dimensions (only 'Level')
# 2 set up the plot type
# 3 customise the x scale, easier to read
ggplot(INDEX, aes(x=Level))+
       geom_bar()+
       scale_x_continuous(breaks=c(1:10))
 
 
#alternative command to count webpages per Level
table(INDEX$Level)
 
# Should display something like that:
# 0  1   2  3   4   5  6  7  8   9  10
# 1 32 306 91 116 127 61 54 90 149 255 
```

**HTML Files**

By default, the rcrawler function also store HTML files in your ‘working directory’. Update location by running setwd\(\) function

![](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-09-19.05.35-1024x386.png)

Let’s go deeper into options by replying to the most commons questions:

### So how to extract metadata while crawling? <a id="2-extract-metadata-while-crawling"></a>

It’s possible to extract any elements from webpages, using a CSS or XPath selector. We’ll have to use 2 new parameters

* **PatternsNames** to name the new parameters
* **ExtractXpathPat** or **ExtractCSSPat** to setup where to grab it in the web page

Let’s take an example:

```r
#what we want to extract
CustomLabels <- c("title",
                 "h1",
                 "canonical tag",
                 "meta robots",
                 "hreflang",
                 "body class")
 
# How to grab it
 CustomXPaths <- c("///title",
           "///h1",
           "//link[@rel='canonical']/@href",
           "//meta[@rel='robots']/@content",
           "//link[@rel='alternate']/@hreflang",
           "//body/@class")
 
 Rcrawler(Website = "https://www.brightonseo.com/",
       ExtractXpathPat = CustomXPaths, PatternsNames = CustomLabels)
```

You can access the scraped data in two ways:

* **option 1 =** **DATA** – it’s an environment variable that you can directly access using the console. A small warning, it’s a ‘list’ a little less easy to read

![View\(DATA\) will display something like that](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-15-16.14.38-1024x670.png)

If you want to convert it to a data frame, easier to deal with, here the code:

```r
NEWDATA <- data.frame(matrix(unlist(DATA), nrow=length(DATA), byrow=T))
```

* **option 2 =** **extracted\_data.csv**  It’s a CSV file that has been saved inside your working directory along with the HTML files.

It might be useful to merge INDEX and NEWDATA files, here the code

```r
MERGED <- cbind(INDEX,NEWDATA)
```

As an example, let’s try to collect webpage type using scraped body class

![Seems that the first word is the page type](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-15-16.35.00.png)

Let’s extract the first word and feed it inside a new column

```r
MERGED$pagetype <- str_split_fixed(MERGED$X7, " ", 2)[,1]
```

A little bit a cleaning to make the labels easier to read

```r
MERGED$pagetype_short <- str_replace(MERGED$pagetype, "-default", "")
MERGED$pagetype_short <- str_replace(MERGED$pagetype_short, "-template", "")

#it's basically deleting "-default" and "-template" from strings 
#as it doesn't help that much understanding data
```

![the 3 steps being displayed](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-15-19.29.37.png)

And then a quick ggplot

```r
#loading graphic library
library(ggplot2)


p <- ggplot(MERGED, aes(x=Level, fill=pagetype_short))+
   geom_histogram(stat="count")+
   scale_x_continuous(breaks=c(1:10))
   
p

```

![Count of Pagetype per level](https://www.gokam.co.uk/wp-content/uploads/2020/08/brightonSEO_plot_pagetype_2020.png)

Want to see something even cooler?

### An interactive graph

```r
#install package plotly the first time
#install.packages("plotly")
library(plotly) 
ggplotly(p, tooltip = c("count","pagetype_short"))
```

![An interactive graph](https://www.gokam.co.uk/wp-content/uploads/2020/08/1Qf42NR6sd.gif)

This is a static HTML file that can be store anywhere, even on [my shared hosting](https://www.gokam.co.uk/pagetype.html)

### Explore Crawled Data with rpivottable <a id="4-explore-crawled-data-with-rpivottable"></a>

```r
#install package rpivottable the first time
#install.packages("rpivottable")
# And loading 
library(rpivottable)
# launch 
toolrpivotTable(MERGED)
```

![This create a drag &amp; drop pivot explorer](https://www.gokam.co.uk/wp-content/uploads/2020/08/LgfVsFu6NL.gif)

![It&#x2019;s also possible make some quick data viz](https://www.gokam.co.uk/wp-content/uploads/2020/08/UmtYC25Kdh.gif)

Full [DEMO – see by yourself](https://www.gokam.co.uk/rpivottable.html)

### Extract more data without having to recrawl <a id="4-extract-more-data-without-having-to-recrawl"></a>

All the HTML files are stored in your hard drive, so if you need more data extracted, it’s entirely possible.

You can list your recent crawl by using ListProjects\(\) function,

![it displays 2 recent crawling projects](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-24-21.32.24.png)

First, we’re going to load the crawling project HTML files:

```r
LastHTMLDATA <- LoadHTMLFiles("gokam.co.uk-242115", type = "vector")
# or to simply grab the last one:
LastHTMLDATA <- LoadHTMLFiles(ListProjects()[1], type = "vector")
```

```r
LastHTMLDATA <- as.data.frame(LastHTMLDATA)
colnames(LastHTMLDATA) <- 'html'
LastHTMLDATA$html <- as.character(LastHTMLDATA$html)
```

Let’s say you forgot to grab h2’s and h3’s you can extract them again using the ContentScraper\(\) also included inside rcrawler package.

```r
for(i in 1:nrow(LastHTMLDATA)) {
   LastHTMLDATA$title[i] <- ContentScraper(HTmlText = LastHTMLDATA$html[i] ,XpathPatterns = "//title")
   LastHTMLDATA$h1[i] <- ContentScraper(HTmlText = LastHTMLDATA$html[i] ,XpathPatterns = "//h1")
   LastHTMLDATA$h2[i] <- ContentScraper(HTmlText = LastHTMLDATA$html[i] ,XpathPatterns = "//h2")
   LastHTMLDATA$h3[i] <- ContentScraper(HTmlText = LastHTMLDATA$html[i] ,XpathPatterns = "//h3")
 }
```

![](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-25-22.46.42-1024x427.png)

### Categorize URLs using Regex <a id="5-categorize-urls-using-regex"></a>

For those not afraid of regex, here is a complimentary script to categorize URLs. Be careful the regex order is important, some values can overwrite others. Usually, it’s a good idea to place the home page last

```r
# define a default category
 
INDEX$UrlCat <- "Not match"
 
 
 
# create category name
 
category_name <- c("Category", "Dates", "author page", "Home page")
 
 
# create category regex, must be the same length
 
category_regex <- c("category", "2019", "author","example\.com.\/$")
 
 
 
# categorize
 
for(i in 1:length(category_name)){
 
# display a dot to show the progress
  cat(".")
# run regex test and update value if it matches
# otherwise leave the previous value
  INDEX$UrlCat <- ifelse(grepl(category_regex[i], INDEX$Url, ignore.case = T), category_name[i], INDEX$UrlCat)
 
}
 
 
# View variable to debug
 
View(INDEX)
```

### What if I want to follow robots.txt rules? <a id="4-what-if-i-want-to-follow-robotstxt-rules"></a>

just had **Obeyrobots** parameter

```r
#like that
Rcrawler(Website = "https://www.gokam.co.uk/", Obeyrobots = TRUE)
```

### What if I want to limit crawling speed? <a id="3-limit-crawling-speed"></a>

By default, this crawler is rather quick and can grab a lot of webpage in no times. To every advantage an inconvenience, it’s fairly easy to wrongly detected as a DOS. To limit the risks, I suggest you use the parameter **RequestsDelay**. it’s the time interval between each round of parallel HTTP requests, in seconds. Example

```r
# this will add a 10 secondes delay between
Rcrawler(Website = "https://www.example.com/", RequestsDelay=10)
```

Other interesting limitation options:

**no\_cores**: specify the number of clusters \(logical cpu\) for parallel crawling, by default it’s the numbers of available cores.

**no\_conn**: it’s the number of concurrent connections per one core, by default it takes the same value of no\_cores.

### What if I want to crawl only a subfolder? <a id="7-what-if-i-want-to-crawl-only-a-subfolder"></a>

2 parameters help you do that. _crawlUrlfilter_ will limit the crawl, _dataUrlfilter_ will tell from which URLs data should be extracted

```r
Rcrawler(Website = "http://www.glofile.com/sport/", dataUrlfilter ="/sport/", crawlUrlfilter="/sport/" )
```

### How to change user-agent? <a id="6-how-to-change-user-agent"></a>

```r
#as simply as that
Rcrawler(Website = "http://www.example.com/", Useragent="Mozilla 3.11")
```

### What if my IP is banned? <a id="6-what-if-my-ip-is-banned"></a>

**option 1: Use a VPN on your computer**

**Option 2: use a proxy**

Use the **httr** package to set up a proxy and use it

```r
# create proxy configuration
proxy <- httr::use_proxy("190.90.100.205",41000)
# use proxy configuration
Rcrawler(Website = "https://www.gokam.co.uk/", use_proxy = proxy)
```

Where to find proxy? It’s been a while I didn’t need one so I don’t know.

### Where are the internal Links? <a id="4-where-are-the-internal-links"></a>

By default, RCrawler doesn’t save internal links, you have to ask for them explicitly by using **NetworkData** option, like that:

```r
Rcrawler(Website = "https://www.gokam.co.uk/",  NetworkData = TRUE)
```

Then you’ll have two new variables available at the end of the crawling:

* **NetwIndex** var that is simply all the webpage URLs. The row number are the same than locally stored HTML files, so row n°1 = homepage = 1.html

![](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-15-20.14.14.png)**NetwIndex** data frame

* **NetwEdges** with all the links. It’s a bit confusing so let me explain:

![](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-15-20.16.20.png)**NetwEdges** data frame

Each row is a link. **From** and **To** columns indicate “from” which page “to” which page are each link.  
  
On the image above:  
row n°1 is a link from homepage \(page n°1\) to homepage  
row n°2 is a link from homepage to webpage n°2. According to NetwIndex variable, page n°2 is the article about [rvest](https://www.gokam.co.uk/crawling-with-r-using-rvest-package/).  
etc…

**Weight** is the Depth level where the link connection has been discovered. All the first rows are from the homepage so Level 0.  
  
**Type** is either 1 for internal hyperlinks or 2 for external hyperlinks

### Count Links <a id="6-count-links"></a>

I guess you guys are interested in counting links. Here is the code to do it. I won’t go into too many explanations, it would be too long. if you are interested \(and motivated\) go and check out the [dplyr](https://dplyr.tidyverse.org/) package and specifically [Data Wrangling functions](https://rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)

**Count outbound links**

```r
count_from <- NetwEdges[,1:2] %>%
#grabing the first two columns
     distinct() %>%
# if there are several links from and to the same page, the duplicat will be removed.
     group_by(From) %>%
     summarise(n = n()) 
# the counting
View(count_from)
# we want to view the results
```

![the homepage \(n&#xB0;1\) has 13 outbound links](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-17-23.22.18.png)

To make it more readable let’s replace page IDs with URLs

```r
count_from$To <- NetwIndex
View(count_from)
```

![using website URLs](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-23-22.48.11.png)

**Count inbound links**

The same thing but the other way around

```r
count_to -> NetwEdges[,1:2] %>%
#grabing the first two columns
     distinct() %>%
# if there are several links from and to the same page, the duplicat will be removed.
     group_by(To) %>%
     summarise(n = n())
# the counting
View(count_to)
 
# we want to view the results
```

![count of inbound links](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-17-23.25.06.png)

Again to make it more readable

```r
count_to$To <- NetwIndexView(count_to)
```

![using website URLs](https://www.gokam.fr/wp-content/uploads/2020/03/Screenshot-2020-03-23-22.29.18.png)

So the useless ‘[author page](https://www.gokam.co.uk/author/gokam/)‘ has 14 links pointing at it, as many as the homepage… Maybe I should fix this one day.

### Compute ‘Internal Page Rank’ <a id="4-compute-internal-page-rank"></a>

[section moved here](https://www.rforseo.com/analysis/page-ranks)

### What if a website is using a JavaScript framework like React or Angular? <a id="5-what-if-my-website-is-using-a-javascript-framework-like-react-or-angular"></a>

RCrawler handly includes **Phantom JS**, the classic headless browser.  
Here is how to to use

```r
# Download and install phantomjs headless browser
# takes 20-30 seconds usually
install_browser()
 
# start browser process 
br <-run_browser()
```

After that, reference it as an option

```r
Rcrawler(Website = "https://www.example.com/", Browser = br)
 
# don't forget to stop browser afterwards
stop_browser(br)
```

It’s fairly possible to run 2 crawls, one with and one without, and compare the data afterwards

This _Browser_ option can also be used with the other Rcrawler functions.

⚠️ Rendering webpage means every Javascript files will be run, including **Web Analytics tags**. If you don’t take the necessary precaution, it’ll change your Web Analytics data

### So what’s the catch? <a id="6-perform-automatic-browser-tests-with-selenium"></a>

Rcrawler is a great tool but it’s far from being perfect. SEO will definitely miss a couple of things like there is no internal dead links report, It doesn’t grab nofollow attributes on Links and there is always a couple of bugs here and there, but overall it’s a great tool to have.  
  
Another concern is the [git repo](https://github.com/salimk/Rcrawler) which is quite inactive. This is it. I hope you did find this article useful, reach to me for slow support, bugs/corrections or ideas for new articles. Take care.

ref:  
_Khalil, S., & Fakir, M. \(2017\). RCrawler: An R package for parallel web crawling and scraping. SoftwareX, 6, 98-106._


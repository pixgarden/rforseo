---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Hunt down keyword cannibalization

### What is keyword cannibalization?

if you put a lot of articles out there, at some point, some articles will compete with one another for the same keywords in Google result pages. it’s what SEO people call ‘keyword cannibalization’.

### Does it matter SEO-wise?

Sometimes it’s perfectly normal. I hope, for your sake, that several of your web pages show up when someone is typing your brand name in Google.

Sometimes it’s not. Let me give an example:

💭 Imagine you run an e-commerce website, with various page type: products, FAQ’s, blog posts, …

At some point, Google decided to make a switch:  a couple of Google search queries that were sending traffic to product pages, now display a blog post of yours.

Inside Google Analytics, SEO sessions count is the same. Your ‘Rank Tracking’ software will not bring you up any position changes.

And yet, these blog post pages will be able to convert much less, and at the end of the month, this will result in a decrease in sales.

Sometimes it can't be fixed, the search intent is now different but sometimes it just because you neglected your product pages. Either way, it's good to know what's happening.

### How to check for keyword cannibalization?

There are several ways to do it. Of course, SEO tools people want you to use their tools, the [method from ahref](https://ahrefs.com/blog/keyword-cannibalization/) is definitely useful. Unfortunately, this kind of tool can be sometimes imprecise, it doesn’t take into account what’s really happening.

So let do it using Google Search Console and R’. Once set up, you’ll be able to check big batches of keywords in minutes. 

### step 0: install R & rstudio

see [Getting started](../classic-r-operations.md)

### step 1: install the necessary packages

First, we’ll load `searchConsoleR` package by [Mark Edmondson](https://github.com/MarkEdmondson1234).  
This will allow us to send requests to Google ‘Search Console API’ very easily.

```r
install.packages("searchConsoleR")
library(searchConsoleR)
```

Then let’s load _tidyverse_.  For those who don’t know about it, it’s a very popular master package that will allow us to work with data frames and in a graceful way.

```r
install.packages("tidyverse")
library(tidyverse)
```

and finally, something to help to deal with Google Account Authentication \(still by Mark Edmondson\). It will spare the pain of having to set up an API Key.

```r
install.packages("googleAuthR")
library(googleAuthR)
```

### step 2 – gather DATA

Let’s initiate authentification. This should open a new browser window, asking you to validate access to your GSC account. The script will be allowed to make requests for a limited period of time.

```r
scr_auth()
```

This will create a **sc.oauth** file inside your working directory. It stores your temporary Access tokens. If you wish to switch between Google accounts, just delete the file, re-run the command and log in with another account.

Let’s list all websites we are allowed to send requests about:

```r
sc_websites <- list_websites()View(sc_websites)
```

and pick one

```r
hostname <- "https://www.example.com/"
```

_don’t forget to update this with your hostname_

As you may know, Search Console data is not available right away. That’s why we want to request data for the last _available_ 2 months, so between 3 days ago and 2 months before that… again using a little useful package!

```r
install.packages("lubridate")
require(lubridate)
tree_days_ago <- lubridate::today()-3
beforedate <- tree_days_ago
month(beforedate) <- month(beforedate) - 2
day(beforedate) <- days_in_month(beforedate)
```

and **now the actual request \(at last!\)**

```r
gsc_all_queries <- search_analytics(hostname,
                                beforedate, tree_days_ago,
                                c("query", "page"), rowLimit = 80000)
```

We are requesting ‘query’ and ‘page’ dimensions. If you wish, it’s possible to restrict request to some type of user device, like ‘desktop only’. See function [documentation.](https://www.rdocumentation.org/packages/searchConsoleR/versions/0.3.0/topics/search_analytics)

There is no point in asking for a longer time period. We want to know if our web pages currently compete with one another now.

_rowLimit_ is a bit of a big random number, this should be enough. If you have a popular website, with a lot of long tail traffic. You might need to increase it.

API respond is store inside _gbr\_all\_queries_ variable as a data frame.

![](https://www.gokam.fr/wp-content/uploads/2019/03/google_search_r.png)

If you happen to have several domains/subdomains that compete with each other for the same keywords, this process should be repeated.  The results will have to be aggregated, [_bind\_rows_](https://dplyr.tidyverse.org/reference/bind.html) function will help you bind them together. This is how to use it :

```r
bind_rows(gsc_queries_1,gsc_queries_2)
```

### step 3 – clean up

First, we’ll filter out queries that are not on the 2 first SERPs and that doesn’t generate any click. There is no point of making useless time-consuming calculations.

We’ll also remove branded search queries using a regex. As said earlier, having several positions for your brand name is pretty classic and shouldn’t be seen as a problem.

```r
gsc_queries_filtered <-gsc_all_queries %>%
    filter(position<=20) %>%
    filter(clicks!=0) %>%
    filter(!str_detect(query, 'brandname|brand name'))
```

_update this with your brand name_

### step 4 – computations

We want to know for one query, what percentage of clicks are going to each landing page.

First, we’ll create a new column **clicksT** with the aggregated number of clicks for each search query.  
Then, using this value to calculate what we need inside a new **per** column.

```r
gsc_queries_computed <- gsc_queries_filtered %>%
                                group_by(query) %>%
                                mutate(clicksT= sum(clicks)) %>%
                                group_by(page, add=TRUE) %>%
                                mutate(per=round(100*clicks/clicksT,2)) 

View(gsc_queries_computed)
```

A **per** column value of 100 means that all clicks go the same URL.

Last final steps, we will sort rows

```r
gsc_queries_final <- gsc_queries_computed %>%
                                arrange(desc(clicksT))
```

\[edit : \] It could also make sense fo remove rows where cannibalization is not significant. Where **per** column value is not very high. \[end of edit\]

Removing now useless columns: click, impression and total click per query group

```r
gsc_queries_final <-gsc_queries_final[,c(-3,-4,-7)]
```

Now it’s your choice to display it inside rstudio

```r
View(gsc_queries_final)
```

Or write a CSV file to open it elsewhere

```r
write.csv(gsc_queries_final,"./gsc_queries_final.csv")
```

Here is my rstudio view \(anonymized sorry 🙊\)

![](https://www.gokam.fr/wp-content/uploads/2019/03/Screenshot-2019-03-19-19.23.34-copy.png)

### step 5 – analysis

You should check data inside each “query pack”. Everything is sorted using the total number of clicks, so, first rows are critical, bottoms rows not so much.

To help you deal with this, let’s check the first one’s

![](https://www.gokam.fr/wp-content/uploads/2019/03/seqrch-query-1.jpg)

_**For Search query 1:**_  
97% of click are going to the same page. Their is no Keyword cannibalization here. It’s interesting to notice that the ‘second’ landing page, only earn 1,4% of clicks, even though, it got an average position of 1,5. Users really don’t like the second ‘Langing page’. Page metadata probably sucks.

Check if the first landing page is the right one and we should move on.

![](https://www.gokam.fr/wp-content/uploads/2019/03/seqrch-query-2.jpg)

_**For Search query 2:**_  
63% of clicks are going to the first landing page. 36% to the second page. This is Keyword cannibalization.  
It could make sense to adapt internal linking between involved landing pages to influence which one should rank before the other one’s, depending on your goals, pages bounce rates, etc.

And so on…

This is it, I hope you’ll find it be useful.


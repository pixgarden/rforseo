---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# searchConsoleR x

## install the necessary packages

First, we’ll load _searchConsoleR_,  an awesome 🌈 package by [Mark Edmondson](https://github.com/MarkEdmondson1234).  
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

We are requesting ‘query’ and ‘page’ dimensions. If you wish, it’s possible to restrict the request to some type of user device, like ‘desktop only’. See function [documentation.](https://www.rdocumentation.org/packages/searchConsoleR/versions/0.3.0/topics/search_analytics)

There is no point in asking for a longer time period. We want to know if our web pages currently compete with one another now.

_`rowLimit`_ is a bit of a big random number, this should be enough. If you have a popular website, with a lot of long-tail traffic. You might need to increase it.

API respond is store inside _gbr\_all\_queries_ variable as a data frame.

![](https://www.gokam.fr/wp-content/uploads/2019/03/google_search_r.png)

If you happen to have several domains/subdomains that compete with each other for the same keywords, this process should be repeated.  The results will have to be aggregated, [_bind\_rows_](https://dplyr.tidyverse.org/reference/bind.html) function will help you bind them together. This is how to use it :

```r
bind_rows(gsc_queries_1,gsc_queries_2)
```


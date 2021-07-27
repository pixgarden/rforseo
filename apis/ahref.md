---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Grab 'ahrefs' API data x

### What is ahref?

Ahrefs  is an [All-in-one SEO toolset](https://ahrefs.com/#seo-toolset) that allows to 

* Optimize your website Site Audit 
* Analyze your competitors 
* Study what your customers are searching
* Learn from your industry’s top performing content
* Track your ranking progress

We will be using  a package called [RAhrefs](https://github.com/Leszek-Sieminski/RAhrefs) by [Leszek Siemiński](https://twitter.com/leszek_sieminsk) use ahref with R

### Installation

```r
# main version on CRAN:
install.packages("RAhrefs")

# development version:
# install.packages("devtools")
# devtools::install_github("Leszek-Sieminski/RAhrefs")
```

### Authentication

```r
library("RAhrefs")
api_key <- "012345"
RAhrefs::rah_auth(api_key)
# will return "API authorized" if success
```

### Checking available reports

To check what Ahrefs data are available in R through API, you need to check provided help dataset:

```r
library("RAhrefs")
View(ahrefs_reports) # view dataset in a new tab (RStudio)
print(head(ahrefs_reports, 5)) # see first 5 reports in the console

# >         report_name          function_name                                                                                   short_description                                             url_address
# > 1        ahrefs_rank        rah_ahrefs_rank                                                                 Contains the URLs and the rankings.        https://ahrefs.com/api/documentation/ahrefs-rank
# > 2            anchors            rah_anchors Contains the anchor text and the num of backlinks, referring pages and referring domains that has it.            https://ahrefs.com/api/documentation/anchors
# > 3 anchors_refdomains rah_anchors_refdomains                               Contains the num of anchors and backlinks with that anchor, per domain. https://ahrefs.com/api/documentation/anchors-refdomains
# > 4          backlinks          rah_backlinks           Contains the backlinks and details of the referring pages, such as anchor and page title.          https://ahrefs.com/api/documentation/backlinks
# > 5 backlinks_new_lost rah_backlinks_new_lost                              Contains the new or lost backlinks and details of the referring pages. https://ahrefs.com/api/documentation/backlinks-new-lost
```

### Checking available reports

To check what Ahrefs data are available in R through API, you need to check provided help dataset:

```r
library("RAhrefs")
View(ahrefs_reports) # view dataset in a new tab (RStudio)
print(head(ahrefs_reports, 5)) # see first 5 reports in the console

# >         report_name          function_name                                                                                   short_description                                             url_address
# > 1        ahrefs_rank        rah_ahrefs_rank                                                                 Contains the URLs and the rankings.        https://ahrefs.com/api/documentation/ahrefs-rank
# > 2            anchors            rah_anchors Contains the anchor text and the num of backlinks, referring pages and referring domains that has it.            https://ahrefs.com/api/documentation/anchors
# > 3 anchors_refdomains rah_anchors_refdomains                               Contains the num of anchors and backlinks with that anchor, per domain. https://ahrefs.com/api/documentation/anchors-refdomains
# > 4          backlinks          rah_backlinks           Contains the backlinks and details of the referring pages, such as anchor and page title.          https://ahrefs.com/api/documentation/backlinks
# > 5 backlinks_new_lost rah_backlinks_new_lost                              Contains the new or lost backlinks and details of the referring pages. https://ahrefs.com/api/documentation/backlinks-new-lost
```

### Checking available metrics

To check what metrics can be choosen, you need to check provided help dataset:

```r
library("RAhrefs")
View(ahrefs_metrics) # view dataset in a new tab (RStudio)
print(head(ahrefs_metrics, 5)) # see first 5 metrics in the console

# >         metric   type use_where? use_having?                                  description
# > 1      url_from string       TRUE        TRUE URL of the page where the backlink is found.
# > 2        url_to string       TRUE        TRUE URL of the page the backlink is pointing to.
# > 3   ahrefs_rank    int       TRUE        TRUE            URL Rating of the referring page.
# > 4 domain_rating    int      FALSE        TRUE       Domain Rating of the referring domain.
# > 5    ahrefs_top    int      FALSE        TRUE            Ahrefs Rank of the target domain.
```

However, different functions can accept different metrics for experimental `where` & `having` conditions. To find out which ones are available for a particular function, check that function's documentation.

### Creating conditions

Ahrefs API can use `where`, `having` and `order_by` parameters. However, behaviour of `where` and `having` may change in further updates.

```r
# first, create all needed conditions in single form:
cond_1 <- RAhrefs::rah_condition(
  column_name = "first_seen",
  operator = "GREATER_THAN",
  value = "2018-01-01",
  is_date = TRUE)

cond_2 <- RAhrefs::rah_condition(
  column_name = "backlinks",
  operator = "GREATER_THAN",
  value = "10")

# next, create a set of conditions from them:
final_condition_set <- RAhrefs::rah_condition_set(cond_1, cond_2)

# finally, use the set of conditions to download choosen results:
result <- RAhrefs::rah_anchors(
  target = "ahrefs.com", 
  limit = 1000, 
  where = final_condition_set)
```

### Usage

```r
# library ----------------------------
library("RAhrefs")

# authentication ---------------------
api_key <- "012345"
RAhrefs::rah_auth(api_key)

# downloading data -------------------
ahrefs_data <- RAhrefs::rah_anchors(
  target = "ahrefs.com",
  mode = "domain",
  limit = 2,
  where   = RAhrefs::rah_condition_set(
    RAhrefs::rah_condition(
      column_name = "backlinks",
      operator = "GREATER_THAN",
      value = "10"),
    RAhrefs::rah_condition(
      column_name = "refpages",
      operator = "GREATER_THAN",
      value = "20")),
  order_by = "refpages:asc")
  
print(ahrefs_data)
# >      anchor backlinks refpages refdomains          first_seen        last_visited
# > 1.21 driver        42       21          1 2018-06-06 07:16:28 2019-01-05 10:37:13
# >    a href's        21       21          1 2015-11-22 14:30:18 2015-11-22 14:30:18

str(ahrefs_data)
# > 'data.frame':	2 obs. of  6 variables:
# >  $ anchor      : chr  "1.21 driver" "a href's"
# >  $ backlinks   : int  42 21
# >  $ refpages    : int  21 21
# >  $ refdomains  : int  1 1
# >  $ first_seen  : POSIXct, format: "2018-06-06 07:16:28" "2015-11-22 14:30:18"
# >  $ last_visited: POSIXct, format: "2019-01-05 10:37:13" "2015-11-22 14:30:18"
```








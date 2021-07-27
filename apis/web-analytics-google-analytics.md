---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Grab Google Analytics Data x

[googleAnalyticsR](https://code.markedmondson.me/googleAnalyticsR/) is an amazing package by Mark Edmondson

```text
## setup
library(googleAnalyticsR)

## authenticate
ga_auth()

## get your accounts
account_list <- ga_account_list()

## account_list will have a column called "viewId"
account_list$viewId

## View account_list and pick the viewId you want to extract data from
ga_id <- 123794729

## simple query to test connection
## simple query to test connection
extract <- google_analytics(ga_id, 
                         date_range = c("2021-01-01", "2021-02-01"), 
                         metrics = "sessions", 
                         dimensions = c("date","medium","landingPagePath"),                                        
                        anti_sample = TRUE)
View(extract)
```


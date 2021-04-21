---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Grab 'ahrefs' API data

### What is ahref?

Ahrefs  is an [All-in-one SEO toolset](https://ahrefs.com/#seo-toolset) that allows to 

* Optimize your website Site Audit 
* Analyze your competitors 
* Study what your customers are searching
* Learn from your industry’s top performing content
* Track your ranking progress

We will be using  a package called [RAhrefs](https://github.com/Leszek-Sieminski/RAhrefs) by Leszek Siemiński use ahref with R

### Installation

```text
# main version on CRAN:
install.packages("RAhrefs")

# development version:
# install.packages("devtools")
# devtools::install_github("Leszek-Sieminski/RAhrefs")
```

### Authentication

```text
library("RAhrefs")
api_key <- "012345"
RAhrefs::rah_auth(api_key)
# will return "API authorized" if success
```










---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Download XML sitemaps ✔️

It's not required to submit an XML sitemap to have a successful website but it's definitely a SEO nice to have. 

Nevertheless, if you do submit one, it's best to make sure it's error-free and as you will see its is quite straightforward using R

### Install xsitemap R’ Package \(to be done once\) and Load

```r
install.packages("devtools")
library(devtools)
install_github("pixgarden/xsitemap")
library(xsitemap)

```

### Find and fetch XML sitemaps

```r
xsitemap_urls <- xsitemapGet("https://www.nationalarchives.gov.uk/")
```

This function will first search for XML sitemap urls. It will check the robots.txt file to see if an XML sitemap url is explicitly declared.

if not the script will do some random guess \(‘sitemap.xml’, ‘sitemap\_index.xml’ , …\) most of the time, it will find the XML sitemap url, if not everything end’s here.

Then, the XML sitemap URL is fetched and the URLs extracted.

If it’s a classic XML sitemap, a data frame \(special kind of array\) will be produced and return.

If it’s an index XML sitemap, the process will get back from the start with every XML sitemaps inside.

This will produce a data frame with all the information extracted. This works for index XML sitemaps too.

### \(optional\) Check URLs HTTP code

Another interesting function allows you to crawl the sitemap URLs and verify if your web pages send proper 200 HTTP codes \(HEAD Requests\)

It can take some time depending on the number of URLs. It took several hours for [https://www.gov.uk/](https://www.gov.uk/) for example.

```text
xsitemap_urls_http <- xsitemapCheckHTTP(xsitemap_urls)
```

It will add a dedicated column with the HTTP code filled in. You can check data inside rstudio by using 

```r
View(xsitemap_urls_http)
```

or if you prefer, [generate a CSV](../export-data/send-and-read-seo-data-to-excel.md#export-your-data-into-a-csv) 


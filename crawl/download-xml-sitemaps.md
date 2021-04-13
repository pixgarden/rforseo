# Download XML sitemaps ✔️

this one is quite straightforward

### Install xsitemap R’ Package \(to be done once\)

```r
install.packages("devtools")
install_github("pixgarden/xsitemap")

```

### Load Packages

```r
library(devtools)
library(xsitemap)
```

#### Find and fetch XML sitemaps

```r
xsitemap_urls <- xsitemapGet("https://www.nationalarchives.gov.uk/")
```

This function will first search for XML sitemap urls. It will check the robots.txt file to see if an XML sitemap url is explicitly declared.

if not the script will do some random guess \(‘sitemap.xml’, ‘sitemap\_index.xml’ , …\) most of the time, it will find the XML sitemap url, if not everything end’s here.

Then, the XML sitemap URL is fetched and the URLs extracted.

If it’s a classic XML sitemap, a data frame \(special kind of array\) will be produced and return.

If it’s an index XML sitemap, the process will get back from the start with every XML sitemaps inside.

This will produce a data frame with all the information extracted. This works for index XML sitemaps too.

#### \(optional\) Check submitted URLs

Another interesting function allows you to crawl the sitemap URLs and verify if your web pages send proper 200 HTTP codes.

It can take some time depending on the number of URLs. It took several hours for [https://www.gov.uk/](https://www.gov.uk/) for example.

```text
xsitemap_urls_http <- xsitemapCheckHTTP(xsitemap_urls)
```

It will add a dedicated column with the HTTP code filled in. You can check data inside rstudio by using 

```r
View(xsitemap_urls_http)
```

or if you prefer, [generate a CSV](../export-data/send-and-read-seo-data-to-excel.md#export-your-data-into-a-csv) 


# URL categorization



```r
# default category
INDEX$UrlCat <- "Not match"

# create category name
category_name <- c("Category", "Dates", "Page Auteur", "Page d'accueil")

# create category regex, must be the same length
category_regex <- c("category", "2019", "author","example\.com.\/$")

# categorize
for(i in 1:length(category_name)){
  cat(".")
  INDEX$UrlCat <- ifelse(grepl(category_regex[i], INDEX$Url, ignore.case = T), 
                          category_name[i], INDEX$UrlCat)
}

# debug
View(INDEX)
```


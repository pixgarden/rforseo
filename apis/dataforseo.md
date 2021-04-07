---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# DataForSeo

### What is DataForSeo?

[DataForSEO](https://dataforseo.com/) is a paid API that provides SEO data

### How to launch the API?

```text
require(httr)

# those can be found here https://app.dataforseo.com/api-dashboard
username <- "APILOGIN"
password <- "APIPWD"

headers = c(
  `Authorization` = paste('Basic',base64_enc(paste0(username,":",password))),
  `Content-Type` = 'application/json'
)

data = '[{"keyword":"r for seo", "location_code":2826, "language_code":"en", "device":"desktop", "os":"windows"}]'

res <- httr::POST(url = 'https://api.dataforseo.com/v3/serp/google/organic/live/advanced', httr::add_headers(.headers=headers), body = data)


res_text <- content(res, "text")
require(jsonlite)
res_json <- fromJSON(res_text, flatten = TRUE)
View(res_json)

```


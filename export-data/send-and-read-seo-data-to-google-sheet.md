---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Send and read  SEO data to Google Sheet x



```text
install.packages("googlesheets4")
library(googlesheets4)



gs4_auth()
(demo <- gs4_create("demo", sheets = list(flowers = head(iris))))


class(demo)

read_sheet(demo)

gs4_get(demo)


```

This package is also  

```text

gs4_deauth()

salary <- read_sheet("https://docs.google.com/spreadsheets/d/1rGCKXIKt-7l5gX06NAwO3pjqEHh-oPXtB8ihkp0vGWo/view", skip = 1)


```


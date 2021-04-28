---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Grab Google Custom search API Data x

```text
library(httr)
query="https://www.googleapis.com/customsearch/v1?key=API_KEY&cx=ENGINE_ID&q=SEARCH_TERM"
content(GET(query))
```


---
description: ⚠️ THIS IS A WORK IN PROGRESS
---

# Count words, n-grams, shingles x

```text
library(stringr)


top_words <- all_full_txt %>%
  unnest_tokens(word, txt) %>%
  anti_join(get_stopwords()) %>%
  filter(!str_detect(word, "[0-9]+") == TRUE) %>%
  group_by(url) %>%
  count(word, sort = F) %>%
  View()
```


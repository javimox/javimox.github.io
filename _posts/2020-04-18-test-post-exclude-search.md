---
title:  "Exclude Post from Search Index"
toc: false
search: false
categories: 
  - misc
last_modified_at: 2020-04-18T00:54:53+02:00
---

This is a test that should not appear in the search index because it has the following YAML Front Matter

```yaml
search: false
```

**Note:** `search: false` only works to exclude posts when using **Lunr** as a search provider.
{: .notice--info}

To exclude files when using **Algolia** as a search provider add an array to `algolia.files_to_exclude` in your `_config.yml`. For more configuration options be sure to check their [full documentation](https://community.algolia.com/jekyll-algolia/options.html).

```yaml
algolia:
  # Exclude more files from indexing
  files_to_exclude:
    - index.html
    - index.md
    - excluded-file.html
    - _posts/2017-11-28-post-exclude-search.md
    - subdirectory/*.html
```

Date generated using: `date --iso-8601=seconds`

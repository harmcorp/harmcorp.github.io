
``` shell
# getting-started
# https://squidfunk.github.io/mkdocs-material/getting-started/

# Installation
docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material new .

# Previewing as you write
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material

# Building your site
docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material build

# configuration
# https://squidfunk.github.io/mkdocs-material/creating-your-site/#advanced-configuration

# tutorial (useful!)
# https://squidfunk.github.io/mkdocs-material/tutorials/#blogs
# setting-up-a-blog
# https://squidfunk.github.io/mkdocs-material/setup/setting-up-a-blog/
# blog
# https://squidfunk.github.io/mkdocs-material/plugins/blog/

# tag icon
# https://squidfunk.github.io/mkdocs-material/setup/setting-up-tags/#tag-icons-and-identifiers-tag-icon
```

# migration jekyll to mkdoc

``` shell
# in search 

# file to include:
docs/blog/posts/2019
# search:
layout:.*
# replace
# none

# search:
date: (.*)
# replace
date:
  created: $1



```
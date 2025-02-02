
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

# setting-up-a-blog
# https://squidfunk.github.io/mkdocs-material/setup/setting-up-a-blog/
# blog
# https://squidfunk.github.io/mkdocs-material/plugins/blog/
```
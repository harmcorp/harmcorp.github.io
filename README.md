
``` shell
# https://squidfunk.github.io/mkdocs-material/getting-started/

# Installation
docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material new .

# Previewing as you write
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material

# Building your site
docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material build
```
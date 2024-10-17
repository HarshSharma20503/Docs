## Running Setup Locally

1. Create python environment and activate it
```bash
python3 venv env-name
source env-name/bin/activate
```
2. install `mkdocs` and `mkdocs-material` and some extensions and plugins
```bash
pip3 install mkdocs mkdocs-material mkdocs-minify-plugin
```
3. Run the website via following command
```bash
mkdocs serve
```
4. change the config file to the following 
```yml
# # Project Information
site_name: XOXO Docs
site_url: https://docs.xoxoharsh.in/
site_author: Harsh Sharma
site_description: >-
  My Personal Documentation Website for all my projects and blogs.

# Repository
repo_name: harshsharma20503/docs
repo_url: https://github.com/harshsharma20503/docs

# Copyright
copyright: Copyright &copy; 2024 Harsh Sharma

# Theme
theme:
  name: material
  logo: https://e7.pngegg.com/pngimages/392/294/png-clipart-documents-logo-docs-logo-documents-documentation.png
  favicon: https://e7.pngegg.com/pngimages/392/294/png-clipart-documents-logo-docs-logo-documents-documentation.png
  features:
    - announce.dismiss
    # - content.action.edit
    # - content.action.view
    - content.code.annotate
    - content.code.copy
    # - content.code.select
    # - content.footnote.tooltips
    # - content.tabs.link
    - content.tooltips
    # - header.autohide
    # - navigation.expand
    - navigation.footer
    - navigation.indexes
    # - navigation.instant
    # - navigation.instant.prefetch
    # - navigation.instant.progress
    # - navigation.prune
    # - navigation.sections
    # - navigation.tabs
    # - navigation.tabs.sticky
    - navigation.top
    - navigation.tracking
    - search.highlight
    - search.share
    - search.suggest
    - toc.follow
    # - toc.integrate
  palette:
    - media: "(prefers-color-scheme)"
      toggle:
        icon: material/brightness-auto
        name: Switch to light mode
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: black
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: Switch to system preference
  font:
    text: Roboto
    code: Roboto Mono
  language: en
# # Plugins
plugins:
  # - blog
  - search:
      separator: '[\s\u200b\-_,:!=\[\]()"`/]+|\.(?!\d)|&[lg]t;|(?!\b)(?=[A-Z][a-z])'
  - minify:
      minify_html: true

extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/harshsharma20503
      name: GitHub
    - icon: fontawesome/brands/linkedin
      link: https://www.linkedin.com/in/harshsharma20503/
      name: LinkedIn
  generator: false
markdown_extensions:
  - abbr
  - admonition
  - attr_list
  - def_list
  - footnotes
  - md_in_html
  - toc:
      permalink: true
  - pymdownx.arithmatex:
      generic: true
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.details
  - pymdownx.emoji:
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
      emoji_index: !!python/name:material.extensions.emoji.twemoji
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.keys
  - pymdownx.magiclink:
      normalize_issue_symbols: true
      repo_url_shorthand: true
      user: squidfunk
      repo: mkdocs-material
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.snippets:
      auto_append:
        - includes/mkdocs.md
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.tabbed:
      alternate_style: true
      combine_header_slug: true
      slugify: !!python/object/apply:pymdownx.slugs.slugify
        kwds:
          case: lower
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tilde
```
5. Open the docs folder as vault in Obsidian
6. Write the docs using Obsidian

## Deploying the setup

```bash
mkdocs gh-deploy
```
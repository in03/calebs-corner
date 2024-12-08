+++
authors = ["Caleb Trevatt"]
title = "Deploying Zola to GitHub Pages"
description = "The guide in the Zola docs is out of date, so here's a better one. For now, at least."
date = 2024-12-08

[extra]
disclaimer = """
Let me waste no time in telling you that I did not write this.
Direct all your credit and attention here 🫱 [Vanja Ćosić](https://vanjacosic.com/posts/deploy-zola-github-pages/)
"""

featured = true

[taxonomies]
tags = ["Snippet", "CI/CD", "zola", "GitHub"]
+++

## Context
In case you weren't aware, this whole site is built with [Zola](https://www.getzola.org/).
It's a fast, static site generator written in Rust. I left Hugo for it 💔

Also, shoutout to the theme creator, [Daudix](https://codeberg.org/daudix/duckquill) for making this theme.
He doesn't seem like a GitHub fan, so I didn't find much on deploying to Pages in his otherwise very comprehensive docs.


## Prerequisites
You should be able to find plenty of up-to-date resources for setting up GitHub pages and GitHub actions elsewhere.
I won't create any unnecessary surface area for breaking changes. I'll leave you to find those.

---

## Deployment
**In a nutshell, you can either:**

1. Push the workflow from the repo
2. Add it directly to the repo from GitHub Actions. 

{% alert(caution=true) %}
Make sure you update the Zola URL and version as needed, and keep an eye out for deprecation warnings. Most of this should be pretty easy to debug from the **GitHub Actions** UI.
{% end %}


```yaml
name: Deploy to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
        ZOLA_VERSION: "0.19.2"
    steps:
      - name: Install Zola
        run: |
          set -x
          wget -O - \
            "https://github.com/getzola/zola/releases/download/v${ZOLA_VERSION}/zola-v${ZOLA_VERSION}-x86_64-unknown-linux-gnu.tar.gz" \
          | sudo tar xzf - -C /usr/local/bin          
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Build with Zola
        run: |
                    zola build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v4
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```


{% crt() %}
```

Remember...
                                             
 _____     _                        _        _       _             
|     |___| |_ ___    ___ ___ ___ _| |   ___| |_ ___|_|___ ___ ___ 
| | | | .'| '_| -_|  | . | . | . | . |  |  _|   | . | |  _| -_|_ -|
|_|_|_|__,|_,_|___|  |_  |___|___|___|  |___|_|_|___|_|___|___|___|
                     |___|                                         


```
{% end %}

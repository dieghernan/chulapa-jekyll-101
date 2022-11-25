# chulapa-jekyll-101

This repo is the source of <https://dieghernan.github.io/chulapa-jekyll-101/>, that is a website deployed with GH Pages and Jekyll 4.0, using a GitHub Action.

Uses the `chulapa-jekyll` gem (`">= 1.0.1"`)

## Setup

### [_config.yml](_config.yml)


```yaml
theme: chulapa-jekyll
github: [metadata]
 
repository: dieghernan/chulapa-jekyll-101   
...

plugins:
  - jekyll-github-metadata
  - jekyll-paginate
  - jekyll-include-cache
  - jekyll-sitemap
```


### [Gemfile](Gemfile)

```ruby
source 'https://rubygems.org'

gem "chulapa-jekyll"
gem "jekyll", "~> 4"

gem "jekyll-github-metadata"
```


### [.github/workflows/chulapa-gh-pages.yml](.github/workflows/chulapa-gh-pages.yml)


```yaml
name: build-chulapa-gh-pages

on:
  push:
    branches:
      - master
      - main
  workflow_dispatch:

jobs:
  build-chulapa-gh-pages:
    runs-on: ubuntu-latest
    env:
      JEKYLL_GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    steps:
    - uses: actions/checkout@v3

    # Use GitHub Actions' cache to shorten build times and decrease load on servers
    - uses: actions/cache@v3
      with:
        path: vendor/bundle
        key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile') }}
        restore-keys: |
          ${{ runner.os }}-gems-

    # Standard usage
    - uses:  helaili/jekyll-action@v2
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
        target_branch: 'gh-pages'
        keep_history: true
```

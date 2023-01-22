# CI/CD Tools

Collections for CI deploy utils.
 - deploy with github actions. 
 - Working with github API 

## Quic Start 

This is sample, how to collect release changes in github actions.

`.github/workflows/collect_release_changes.yml`:
```yaml
name: Collect Release Changes

on:
  workflow_dispatch:
  push:
    branches:
      - 'release/**'
      - 'releases/**'

jobs:
  tests:
    name: Collect Release Changes
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
          lfs: true

      - name: set up Python 3
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      - name: Download CI Tools
        uses: actions/checkout@v3
        with:
          repository: klee0kai/ci_tools
          path: 'ci_tools'
          ref: 'master'

      - name: Install CI Tools Requirements
        run:  pip install -r ./ci_tools/requirements.txt

      - name: Collect changes
        env:
          SECRETS_GH_API_TOKEN: ${{ env.SECRETS_GH_API_TOKEN }}
        run: |
          python ./ci_tools/ci_tools.py -d --summary -w changes.md release_diff

      - name: Upload changes artifacts
        uses: actions/upload-artifact@v3
        with:
          name: changes
          path: |
            ./changes.md
```

## Using Env

 - SECRETS_GH_API_TOKEN - github api token 

## License

```
Copyright (c) 2023 Andrey Kuzubov
```

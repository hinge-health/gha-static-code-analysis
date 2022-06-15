# GitHub Actions - Static Code Analysis 

This repository will contain the set of Github Action workflows pertaining to static code analysis.

## Using the Static Code Analysis workflow

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, edited, reopened, synchronize]
    push:
        branches: [ main ]

jobs:
  static-code-analysis-scanner:
    uses: hinge-health/gha-static-code-analysis/.github/workflows/static-code-analysis.yml@main
```

It is also a good idea to pin reference to a particular tag instead of `@main` so that if a breaking change is made in the future it will not break your deploy.

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, edited, reopened, synchronize]
    push:
        branches: [ main ]

jobs:
  static-code-analysis-scanner:
    uses: hinge-health/gha-static-code-analysis/.github/workflows/static-code-analysis.yml@v1.0.0
```

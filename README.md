# GitHub Actions - Static Code Analysis 

This repository will contain the set of Github Action workflows pertaining to static code analysis.

## Static Code Analysis Workflow

The `static-code-analysis.yml` workflow uses SonarSources [scanner action](https://github.com/SonarSource/sonarqube-scan-action) to scan the code base of a repository based on the calling workflows `on` action, and sends the static code analysis and code coverage data to a SonarQube/SonarCloud instance.

For more information on what SonarQube is please refer to this [guru](https://fixme.com) document.

This workflow will `NOT` cause builds or pr's to fail if static code analysis scanner fails to pass the quality profile set for the underlying repository. This reusable action is only meant to display PR decorations and send static code analysis data to the companys SonarQube instance.

### How to use the `static-code-analysis.yml` workflow

#### Preqrequistes

- A SonarQube project has been created and binded to the working repository

- The working repository contains a GHA workflow that generates a coverage report and uploads the artifact using the [upload-artifact](https://github.com/actions/upload-artifact) action.
  - For repositories that generate multiple coverage reports the coverage reports `must` combine to a single file before uploading the artifact.
  - Coverage report artifacts `must` follow the naming conventions based on the underlying source code language of the repository. SonarQubes code coverage propeties looks for specific naming conventions for the coverage reports.
    - `typescript|javascript|react|react-native` : must be set to lcov.info 
    - `python` : must be set to coverage.xml
    - `ruby`   : must be set to either [ .resultset.json | coverage.json ] read the [Example workflow for `ruby` source code repositories](#example-workflow-ruby) for information on what file to choose from.


### [Example workflow for `typescript|javascript|react|react-native` source code repositories](#example-workflow-nodejs)

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [ main ]

jobs:
  # Job to generate and upload coverage reports goes here
  # be sure to upload the coverage report with the file name lcov.info

  static_code_analysis:
    uses: hinge-health/gha-static-code-analysis/.github/workflows/static-code-analysis.yml@main
    with:
      coverage_artifact: lcov.info
    secrets: inherit
    if: always()
    needs: [FIXME] # Job name that generates coverage report 
```

### [Example workflow for `python` source code repositories](#example-workflow-python)

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [ main ]

jobs:
  # Job to generate and upload coverage reports goes here
  # be sure to upload the coverage report with the file name coverage.xml

  static_code_analysis:
    uses: hinge-health/gha-static-code-analysis/.github/workflows/static-code-analysis.yml@main
    with:
      coverage_artifact: coverage.xml
    secrets: inherit
    if: always()
    needs: [FIXME] # Job name that generates coverage report 
```
### [Example workflow for `ruby` source code repositories](#example-workflow-ruby)

For ruby repositories teams will need to use the [simplecov](#https://github.com/simplecov-ruby/simplecov) dependency to generate the coverage report file
and set the `coverage_artifact` variable to the filename.

- For simplecov <= v0.20.0 set `coverage_artifact` field to .resultset.json
- simplecov >= v0.20.0 set `coverage_artifact` field to coverage.json

#### Simplecov versions <= v0.20.0

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [main]

jobs:
  # Job to generate and upload coverage reports goes here
  # be sure to upload the coverage report with the file name .resultset.json

  static_code_analysis:
    uses: hinge-health/gha-static-code-analysis/.github/workflows/static-code-analysis.yml@main
    with:
      coverage_artifact: .resultset.json
    secrets: inherit
    if: always()
    needs: [FIXME] # Job name that generates coverage report 
```

#### Simplecov versions <= v0.20.0

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [main]

jobs:
  # Job to generate and upload coverage reports goes here
  # be sure to upload the coverage report with the file name .resultset.json

  static_code_analysis:
    uses: hinge-health/gha-static-code-analysis/.github/workflows/static-code-analysis.yml@main
    with:
      coverage_artifact: coverage.json
    secrets: inherit
    if: always()
    needs: [FIXME] # Job name that generates coverage report 
```

It is also a good idea to pin reference to a particular tag instead of `@main` so that if a breaking change is made in the future it will not break your deploy.

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [ main ]

jobs:
  static-code-analysis-scanner:
    uses: hinge-health/gha-static-code-analysis/.github/workflows/static-code-analysis.yml@v1
    with:
      coverage_artifact: lcov.info
    secrets: inherit
```

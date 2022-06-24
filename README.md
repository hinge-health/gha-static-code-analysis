# GitHub Actions - Static Code Analysis 

This repository will contain the set of Github Action workflows pertaining to static code analysis.

## Static Code Analysis Workflow

The `static-code-analysis.yml` workflow uses SonarSources [scanner action](https://github.com/SonarSource/sonarqube-scan-action) to scan the code base of a repository based on a calling workflow `on` action, and sends the static code analysis and code coverage data to a SonarQube/SonarCloud instance.

For more information on what SonarQube is please read over this guru card: [SonarQube - Static Code Analysis Tool](https://app.getguru.com/card/ca87zp4i/SonarQube-Static-Code-Analysis-Tool-)

This workflow will `NEVER` cause builds or pull requests to fail if the action reports failures based on the quality profile set for the repository. This reusable action is only meant to display pull request decorations and send static code analysis data to the companies SonarQube instance.

### How to use the `static-code-analysis.yml` workflow

#### Preqrequistes

- A SonarQube project has been created and binded to the working repository

- The working repository contains a GHA workflow that generates a coverage report and uploads the artifact using the [upload-artifact](https://github.com/actions/upload-artifact) action.
  - For repositories that generate multiple coverage reports the coverage reports `must` combine to a single file before uploading the artifact.
  - Coverage report artifacts `must` follow specific naming convention guidelines based on the underlying source code language of the repository. If the naming convention is incorrect, then the SonarQube scanner will not detect the coverage report.
    - `typescript|javascript|react|react-native` : must be set to `lcov.info` 
    - `python` : must be set to `coverage.xml`
    - `ruby`   : must be set to either `[ .resultset.json | coverage.json ]` read the [Example workflow for `ruby` source code repositories](#example-workflow-for-ruby-source-code-repositories) for more information on what file name to choose from.


### Example workflow for `typescript|javascript|react|react-native` source code repositories

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [main]

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

### Example workflow for `python` source code repositories

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [main]

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
### Example workflow for `ruby` source code repositories

For ruby repositories teams will need to use the [simplecov](https://github.com/simplecov-ruby/simplecov) dependency to generate the coverage report file
and set the `coverage_artifact` variable to the filename.

- For simplecov versions before v0.18 set `coverage_artifact` field to the `.resultset.json`
- For simplecov versions after v0.18 use the simplecov [JSON formatter](https://github.com/simplecov-ruby/simplecov#json-formatter) to format the coverage report and set the `coverage_artifact` field to `coverage.json`

#### Simplecov versions <= v0.18.0

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

#### Simplecov versions > v0.18.0

```yml
name: Static Code Analysis

on: 
    pull_request:
        types: [opened, synchronize]
    push:
        branches: [main]

jobs:
  # Job to generate and upload coverage reports goes here
  # be sure to upload the coverage report with the file name coverage.json

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

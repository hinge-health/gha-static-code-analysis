# Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
although this project (might not) adhere to [Semantic Versioning](http://semver.org/).

<!-- TEMPLATE
## [Unreleased] - yyyy-mm-dd

Here we write upgrading notes for brands. It's a team effort to make them as
straightforward as possible.

### Added
 - [INFRA-1337](https://hingehealth.atlassian.net/browse/INFRA-1337)
  Ticket title goes here.

### Changed
 - [INFRA-1337](https://hingehealth.atlassian.net/browse/INFRA-1337)
  Ticket title goes here.
-->

## [2] - 2022-06-15

### Feature
- [QE-159](https://hingehealth.atlassian.net/browse/QE-159)
- GHA `static-code-analysis.yml` has inputs and secrets that need to be injected.
- GHA `static-code-analysis.yml` has a download artifact and checkout step.

## [1.0.1] - 2022-06-15

### Fix
- [QE-159](https://hingehealth.atlassian.net/browse/QE-159)
- Fix GHA action `static-code-analysis.yml` to include checkout step before running scanner.

## [1] - 2022-06-15

### Feature
- [QE-159](https://hingehealth.atlassian.net/browse/QE-159)
- GHA action `static-code-analysis.yml` for SonarQube code scans

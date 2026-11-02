# Continuous Integration Pipeline

## Objective
Automate code validation, testing, and model quality checks using GitHub Actions.

## Workflow Configuration
Workflow file: `.github/workflows/ci.yml`

The pipeline runs on:
- Push to `main` and `develop`
- Pull requests targeting `main` and `develop`

## CI Jobs

### 1. build-and-lint
- Uses Python 3.11.
- Installs project dependencies.
- Compiles Python files.
- Runs Flake8 lint checks on `src/`.

### 2. test-suite
- Runs after `build-and-lint`.
- Collects and preprocesses the Iris dataset.
- Generates features.
- Executes the pytest test suite.
- Generates test and coverage reports.
- Uploads `test-results.xml` and `coverage.xml` as artifacts.

### 3. model-quality-gate
- Runs after `test-suite`.
- Prepares the Iris dataset.
- Executes `tests/test_model_quality.py`.
- Validates model quality requirements.

## Branch Protection
The `main` branch is protected by a GitHub ruleset:
- Pull requests are required before merging.
- The following status checks are required:
  - `build-and-lint`
  - `test-suite`
  - `model-quality-gate`

## CI Verification
A feature branch named `feature/ci-verification-test` was created.
A README update was committed and pushed.
A pull request was created, CI checks passed, and the pull request was merged.

## Regression Testing
A temporary branch named `feature/simulate-regression` was used.
The model accuracy threshold was increased from `0.90` to `0.999`.

Observed results:
- `build-and-lint`: Passed
- `test-suite`: Failed
- `model-quality-gate`: Skipped

This demonstrated that the CI pipeline detects model quality regressions and prevents dependent jobs from running after a failed test suite.

The regression pull request was closed without merging.

## Reports
The workflow uploads these artifacts:
- `test-results.xml`
- `coverage.xml`

## Conclusion
The GitHub Actions CI pipeline automates linting, testing, and model quality validation. Branch protection and required status checks help prevent changes that fail the CI pipeline from being merged into `main`.

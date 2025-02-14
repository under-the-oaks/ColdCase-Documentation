# Continuous Integration (CI) Pipeline for ColdCase

We automate the process of building, testing, and analyzing code for the ColdCase project through a pipeline that uses
GitHub actions to run. It ensures that every push to the `development` branch and every pull request triggers a
structured verification process.

## Triggering Conditions

The pipeline runs automatically when code is pushed to the `development` branch or when a pull request is opened against
any branch:

```yaml
on:
  pull_request:
    branches:
      - '*'
  push:
    branches:
      - development
```  

## Build and Test Process

The job executes on an Ubuntu-based runner:

```yaml
jobs:
  build-and-test:
    runs-on: ubuntu-latest
```  

It begins by checking out the repository to ensure the latest code is available:

```yaml
- name: Check out the code
  uses: actions/checkout@v3
```  

Next, it sets up Java 21 using the Temurin distribution:

```yaml
- name: Set up JDK 21
  uses: actions/setup-java@v3
  with:
    java-version: '21'
    distribution: 'temurin'
```  

To improve performance, Gradle dependencies are cached, reducing build times for subsequent runs:

```yaml
- name: Cache Gradle dependencies
  uses: actions/cache@v3
  with:
    path: |
      ~/.gradle/caches
      ~/.gradle/wrapper
    key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
    restore-keys: |
      ${{ runner.os }}-gradle-
```  

Since Gradle's wrapper script needs execution permissions, the pipeline grants them:

```yaml
- name: Grant execute permission for gradlew
  run: chmod +x ./gradlew
```  

The project is then built using Gradle:

```yaml
- name: Build the project
  run: ./gradlew build
```  

Unit tests are executed to verify functionality:

```yaml
- name: Run tests
  run: ./gradlew test
```  

## Test and Coverage Reports

To track test results, the pipeline uploads test reports regardless of success or failure:

```yaml
- name: Upload test report
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: test-report
    path: '**/build/test-results/test/*.xml'
```  

Similarly, it generates and uploads a **JaCoCo** code coverage report:

```yaml
- name: Upload code coverage report
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: code-coverage-report
    path: '**/build/reports/jacoco/test/html'
```  

A summary of the test results is added to improve visibility:

```yaml
- name: Summarize tests results
  uses: jeantessier/test-summary-action@v1
  if: ${{ always() }}
```  

## Code Coverage Analysis

The pipeline analyzes JaCoCo coverage and posts a comment in the pull request with the coverage details. It enforces a
minimum overall code coverage of 40% and requires at least 60% coverage for changed files:

```yaml
- name: Jacoco Report to PR
  id: jacoco
  uses: madrapps/jacoco-report@v1.7.1
  with:
    paths: '**/build/reports/jacoco/test/jacocoTestReport.xml'
    token: ${{ secrets.GITHUB_TOKEN }}
    min-coverage-overall: 40
    min-coverage-changed-files: 60
    title: Code Coverage
    update-comment: true
```
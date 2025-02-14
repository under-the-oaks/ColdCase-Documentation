# Continuous Integration

The CI pipeline for the **ColdCase-Client** automates the processes of building, testing, and analyzing code. It is implemented using **GitHub Actions** and is triggered by:

- Every push to the `development` branch
- Every pull request

This ensures that all changes undergo a structured verification process before integration.

## Additional Pipelines

In the **Client** and **Documentation** repositories, you will find additional `.yml` files defining separate pipelines:

- **Documentation Repository:**
    - Builds the **writerside** documentation
    - Publishes it directly to **GitHub Pages**

- **Client Repository:**
    - Builds the **Javadoc**
    - Publishes it to **GitHub Pages**

These pipelines ensure that both the documentation and code-related resources are automatically generated and deployed.

## ColdCase-Client CI Pipeline

### Setting up the Environment

The pipeline starts by checking out the project repository and setting up JDK 21 using the Temurin distribution. To
optimize build performance, it caches Gradle dependencies, reducing redundant downloads across workflow runs. The Gradle
wrapper is granted execute permissions to ensure compatibility with the project's Gradle setup.

```yaml
      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          java-version: '21'
          distribution: 'temurin'

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

### Building and Testing

Once the environment is ready, the project is built using Gradle. After a successful build, the test suite is executed,
and the results are stored. The workflow then uploads test reports and code coverage reports as artifacts, allowing
us to review them later.

```yaml
      - name: Check out the code
        uses: actions/checkout@v3

      - name: Build the project
        run: ./gradlew build

      - name: Run tests
        run: ./gradlew test

      - name: Upload test report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-report
          path: '**/build/test-results/test/*.xml'
```

### Code Coverage and Reporting

The pipeline uses Jacoco to analyze test coverage. It requires an overall minimum coverage of 40% and at
least 60% coverage for changed files in a pull request. The coverage report is automatically posted as a comment in the
pull request, providing us with immediate feedback on the quality of the changes.

```yaml
      - name: Upload code coverage report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: code-coverage-report
          path: '**/build/reports/jacoco/test/html'

      - name: Summarize tests results
        uses: jeantessier/test-summary-action@v1
        if: ${{ always() }}

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

## ColdCase-Server CI Pipeline

The server pipeline is designed to automate testing for the Deno-based Server. It runs on every push to the main
and development branches, as well as for all pull requests.

The pipeline starts by checking out the repository to ensure the latest code is available. It then sets up Deno using
the official action.

```yaml
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Deno
        uses: denoland/setup-deno@v1
        with:
          deno-version: v2.x

      - name: Install dependencies
        run: deno cache ./server/main.ts
```

Once Deno is installed, it preloads dependencies by caching the modules referenced in `server/main.ts`, which helps
speed up subsequent builds. Finally, the pipeline executes the test suite using `deno test --allow-net`, allowing tests
that require network access to run. This ensures that the project’s functionality is validated before changes are merged
into the main or development branches.

```yaml
      - name: Run tests
        run: deno test --allow-net
```
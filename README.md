# New Relic Java Telemetry SDK 
The New Relic Java Telemetry SDK is an easy way to send data to New Relic.
The SDK currently supports sending the MELT telemetry data types 
(Metrics, Events, Logs, and Traces) 
via the following APIs:

* [Metric API](https://docs.newrelic.com/docs/data-ingest-apis/get-data-new-relic/metric-api/report-metrics-metric-api) - for dimensional metrics 
* [Trace API](https://docs.newrelic.com/docs/understand-dependencies/distributed-tracing/trace-api/introduction-trace-api) - for building up traces with spans
* [Events API](https://docs.newrelic.com/docs/insights/insights-data-sources/custom-data/introduction-event-api) - for custom events
* [Log API](https://docs.newrelic.com/docs/logs/new-relic-logs/log-api/introduction-log-api) - for log data

Why is this cool?

Send data to New Relic! No agent required. 

Our [Telemetry SDK](https://docs.newrelic.com/docs/data-ingest-apis/get-data-new-relic/new-relic-sdks/telemetry-sdks-send-custom-telemetry-data-new-relic) tries to be helpful, so your job of sending telemetry data to New Relic can be done in the right way, easily. We've covered all of the basics for you so you can focus on writing feature code directly related to your business need or interest.

Why would you want to use the telemetry SDK?

We imagine you (or your customers) are interested in the telemetry data, generated by your tool, framework, or code, in New Relic. You can write an exporter to do so! Check out the [telemetry_examples](telemetry_examples) module to get started.

For the most recently published version, see [Releases](https://github.com/newrelic/newrelic-telemetry-sdk-java/releases)

## Get started

In order to send metrics or spans to New Relic, you will need an [Insights Insert API Key](https://docs.newrelic.com/docs/apis/getting-started/intro-apis/understand-new-relic-api-keys#user-api-key).

Maven dependencies:

```
    <dependency>
      <groupId>com.newrelic.telemetry</groupId>
      <artifactId>telemetry</artifactId>
      <version>0.6.1</version>
    </dependency>
    <dependency>
      <groupId>com.newrelic.telemetry</groupId>
      <artifactId>telemetry-http-okhttp</artifactId>
      <version>0.6.1</version>
    </dependency>
```

Gradle dependencies: 

```
compile("com.newrelic.telemetry:telemetry:0.6.1")
compile("com.newrelic.telemetry:telemetry-http-okhttp:0.6.1")
```

Take a look at the example code in the [telemetry_examples](telemetry_examples) module. 
We recommend looking at the [TelemetryClientExample](telemetry_examples/src/main/java/com/newrelic/telemetry/examples/TelemetryClientExample.java)
first.

Note: If you do not want to include `okhttp` as a transitive depenedency, you will need to provide a custom implementation of the 
`com.newrelic.telemetry.http.HttpPoster` interface, rather than using the `com.newrelic.telemetry:telemetry-http-okhttp` library.

## Logging
The Telemetry SDK uses slf4j for all logging. Having a slf4j implementation in your application is required in order to see log information.
See [the slf4j documentation](http://www.slf4j.org/manual.html#swapping) for details.

### Enabling audit logging
The various builders for the Telemetry SDK components include an option to `enableAuditLogging`. Enabling this option will cause the
SDK to product additional logging at `DEBUG` level. This logging includes the details of every payload sent to the New Relic APIs, and the responses from those APIs.

*WARNING*: If you enable audit logging, all the data in your spans and metrics will be sent to your logging system. It is recommended that you only enable
audit logging when absolutely necessary.

## For developers: 
### Requirements

* Java 8 or greater
* For IDEA:
* Docker & docker-compose must be installed for integration testing

## Find and use your data

Tips on how to find and query your data in New Relic:
- [Find metric data](https://docs.newrelic.com/docs/data-ingest-apis/get-data-new-relic/metric-api/introduction-metric-api#find-data)
- [Find trace/span data](https://docs.newrelic.com/docs/understand-dependencies/distributed-tracing/trace-api/introduction-trace-api#view-data)

For general querying information, see:
- [Query New Relic data](https://docs.newrelic.com/docs/using-new-relic/data/understand-data/query-new-relic-data)
- [Intro to NRQL](https://docs.newrelic.com/docs/query-data/nrql-new-relic-query-language/getting-started/introduction-nrql)


### Building
CI builds are run on Github Actions: 
![build badge](https://github.com/newrelic/newrelic-telemetry-sdk-java/workflows/main%20build/badge.svg)

The project uses gradle 6 for building, and the gradle wrapper is provided.

To compile, run the tests and build the jars:

`$ ./gradlew build`

### Integration testing

End-to-end integration tests are included. 
They are implemented with the testcontainers library; [mock-server](https://github.com/jamesdbloom/mockserver) provides the backend.

There are two modes to run the integration tests.
* Run with gradle: `$ ./gradlew integration_test:test`
* Run the integration test classes in IDEA directly. It should "just work".

### Code style
This project uses the [google-java-format](https://github.com/google/google-java-format) code style, and it is 
easily applied via an included [gradle plugin](https://github.com/sherter/google-java-format-gradle-plugin):

`$ ./gradlew googleJavaFormat verifyGoogleJavaFormat`

Please be sure to run the formatter before committing any changes. There is a `pre-commit-hook.sh` which can 
be applied automatically before commits by moving it into `.git/hooks/pre-commit`.

### Module structure:

#### `telemetry_core`
This is the core module for sending dimensional metrics and spans to New Relic. The library is published under maven coordinates:

`com.newrelic.telemetry:telemetry-core`

In order to send metrics and spans to New Relic, you will also need an Insights Insert API Key. 
Please see [New Relic Api Keys](https://docs.newrelic.com/docs/apis/getting-started/intro-apis/understand-new-relic-api-keys#user-api-key)
for more information.

#### `telemetry`
This module contains code for using all New Relic telemetry modules, gathered in one place, as well as what we 
consider "best practice" implementations of how to interact with the lower-level modules.

The `telemetry` library is published under the maven coordinates:

`com.newrelic.telemetry:telemetry`

#### `telemetry-http-java11`
This is an implementation of the required http client interface using Java 11 JDK classes as the underlying library.
The `telemetry-http-java11` library is published under the maven coordinates:

`com.newrelic.telemetry:telemetry-http-java11`

#### `telemetry-http-okhttp`
This is an implementation of the required http client interface using okhttp as the underlying library.
The `telemetry-http-okhttp` library is published under the maven coordinates:

`com.newrelic.telemetry:telemetry-http-okhttp`

#### `telemetry_examples`
Example code for using the metrics and telemetry APIs.

#### `integration_test`
Integration test module. Uses docker-compose based tests to test the SDK end-to-end.

### Retries

As [described here](https://github.com/newrelic/newrelic-telemetry-sdk-specs/blob/master/communication.md#graceful-degradation),
the `TelemetryClient` makes use of a default backoff strategy when it encounters data ingest errors.
Specifically, this strategy tries again after 1 second, then doubles the wait after each attempt
until a max wait of 15 seconds is reached.  After 10 failed attempts, data is dropped and a message
is logged. 

The backoff strategy is not currently pluggable.  Please file an issue or submit a pull request 
if you need greater control over this behavior. 


### Licensing
The New Relic Java Telemetry SDK is licensed under the Apache 2.0 License.

The New Relic Java Telemetry SDK also uses source code from third party libraries. 
Full details on which libraries are used and the terms under which they are licensed can be found in the 
third party notices document.

### Contributing
Full details are available in our [CONTRIBUTING.md](CONTRIBUTING.md) file. 
We'd love to get your contributions to improve the Java Telemetry SDK! Keep in mind when you submit your pull request, you'll need to sign the CLA via the click-through using CLA-Assistant. You only have to sign the CLA one time per project.
To execute our corporate CLA, which is required if your contribution is on behalf of a company, or if you have any questions, please drop us an email at opensource@newrelic.com. 

### Release Process

#### Publish to Staging Repo

To stage the release simply submit and merge a PR to update the [build.gradle.kts](build.gradle.kts) file with the version to be released (e.g. `version := "0.6.0"`).

Results of the job can be viewed here: https://github.com/newrelic/newrelic-telemetry-sdk-java/actions
After the staging release job has run successfully it will publish the new artifact to a staging repository on Sonatype at: https://oss.sonatype.org/#stagingRepositories.

#### Manually Release Staging Repo

1. Find the staging repo on Sonatype, which should be named similar to `comnewrelic-nnnn`, and validate that the contents and version look correct.
2. If the contents look correct, select the staging repo and choose `close`, leaving a comment such as `releasing 0.6.0`.
3. When the staging repo is finished closing, select the staging repo and choose `release`, keeping the `Automatically Drop` checkbox checked, and leave a comment such as `releasing 0.6.0`.
4. Verify that the artifacts were published on Maven Central at: https://repo1.maven.org/maven2/com/newrelic/telemetry/telemetry 

#### Post Release

Submit and merge a PR with the following:
* Update the [build.gradle.kts](build.gradle.kts) file with to a snapshot version of a potential future release (e.g. `version  := "0.6.1-SNAPSHOT"`).
* Update the [CHANGELOG](CHANGELOG.md) with details of the new release:
  ```markdown
  ## [0.6.0]
  - Miscellaneous bug fixes and tweaks
  ```
* Update the [Usage](#usage) example in the [README](README.md) with the newly released version (e.g. `implementation("com.newrelic.telemetry:telemetry:0.6.0")`).

[javadoc-image]: https://www.javadoc.io/badge/com.newrelic.telemetry/telemetry.svg
[javadoc-url]: https://www.javadoc.io/doc/com.newrelic.telemetry/telemetry

### Limitations
The New Relic Telemetry APIs are rate limited. Please reference the documentation for [New Relic Metric API](https://docs.newrelic.com/docs/introduction-new-relic-metric-api) and [New Relic Trace API requirements and limits](https://docs.newrelic.com/docs/apm/distributed-tracing/trace-api/trace-api-general-requirements-limits) on the specifics of the rate limits.

# Allogy Maven OSS Parent POM

Shared Maven parent POM for Allogy open-source projects. It provides common
build defaults and a `sign-artifacts` profile for publishing signed releases to
Maven Central through the Central Portal.

## Requirements

- Java 17 or newer
- Maven 3.9.0 or newer
- A GPG key available for signing release artifacts
- A Sonatype Central Portal publishing token

The POM enforces the Maven and Java version requirements with Maven Enforcer.

## Usage

Use the current parent snapshot while developing a project alongside this
repository:

```xml
<parent>
    <groupId>com.allogy</groupId>
    <artifactId>allogy-maven-pom-oss</artifactId>
    <version>3.0.0-SNAPSHOT</version>
</parent>
```

Install the parent locally before building downstream projects:

```bash
mvn install -DskipTests
```

## Publishing to Maven Central

Configure the Central Portal token in `~/.m2/settings.xml` using the shared
`central` server ID:

```xml
<settings>
    <servers>
        <server>
            <id>central</id>
            <username>${env.CENTRAL_USERNAME}</username>
            <password>${env.CENTRAL_PASSWORD}</password>
        </server>
    </servers>
</settings>
```

Run the release profile when deploying a release:

```bash
mvn -Psign-artifacts deploy
```

The profile attaches source and Javadoc jars, signs artifacts with GPG, and
uploads the deployment through `central-publishing-maven-plugin`. Deployments
are not automatically published; review and publish them in the Central Portal.

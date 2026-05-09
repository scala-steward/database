Upgrade this project's dependencies.

It's important that you respect the following requirements to the letter!

## How-to find latest versions

To update the project's dependencies, first execute this command:

```bash
make dependency-updates-ci
```

This will generate these reports with dependency updates that you can review:
- For the Gradle project(s):
  - `./build/dependencyUpdates/report.html
  - `./delayedqueue-jvm/build/dependencyUpdates/report.html`
- For the Scala project:
  - `./target/dependency-updates.txt`
  - `./project/target/dependency-updates.txt` (sbt plugins)
  - `./delayedqueue-scala/target/dependency-updates.txt`

To also update Scala's sbt (build tool), you can find the latest release
by querying GitHub (and then edit `./project/build.properties`):

```
curl -s https://api.github.com/repos/sbt/sbt/releases/latest \
  | jq -r .tag_name
```

You can also upgrade Gradle, but you need to take care of any deprecations
(we need a clean build, always up to date):

```
./gradlew wrapper --gradle-version latest
```

The real dependency and build tool versions are declared in these files, so
inspect and update them as appropriate:
- `gradle/libs.versions.toml`
- `settings.gradle.kts`
- `gradle/wrapper/gradle-wrapper.properties`
- `build.sbt`
- `project/plugins.sbt`
- `project/build.properties`

## Hard Requirements

* Only consider stable, minor versions (semver), concern being API compatibility
  * Testing dependencies, build tools or plugins can be upgraded to any stable version
* The Scala project must support both Scala 2.13.x and Scala 3.x.x
  so only minor versions should be considered (e.g., 2.13.x -> 3.x.x is a false positive)
* Fix code breakage, but carefully, we MUST NOT break public API compatibility
* Respect AGENTS.md
* Work is not over until `make check-all` passes! (acceptance criteria)

## Communication requirements

In case there are updates, create a PR such that:
- Title of PR MUST BE "Upgrade dependencies: <list>"
- Description of PR MUST CONTAIN:
  - the list of dependencies that were upgraded
  - the list of dependencies that couldn't be upgraded
    - runtime/library major versions skipped for API compatibility
  - other details you deem relevant (like code fixes)

In case there are no updates, such that no PR can be created, then a comment with the status on this issue (this is the way you communicate with the project's maintainers): https://github.com/funfix/database/issues/52

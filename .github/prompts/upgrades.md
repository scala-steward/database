To update the project's dependencies, first execute this command:

```bash
make dependency-updates-ci
```

This will generate these reports:
- For the Gradle project(s):
  - `./build/dependencyUpdates/report.html
  - `./delayedqueue-jvm/build/dependencyUpdates/report.html`
- For the Scala project:
  - `./target/dependency-updates.txt`
  - `./project/target/dependency-updates.txt` (sbt plugins)
  - `./delayedqueue-scala/target/dependency-updates.txt`

Requirements for upgrades:
  * Only consider stable, minor versions (semver), concern being API compatibility
    * Testing dependencies, build tools or plugins can be upgraded to any stable version
  * The Scala project must support both Scala 2.13.x and Scala 3.x.x
    so only minor versions should be considered (e.g., 2.13.x -> 3.x.x is a false positive)
  * In case no PR can be created, the run being a NO-OP, create
    a comment with the status on this issue:
    https://github.com/funfix/database/issues/52
  * Title of the PR, if created, should be "Upgrade dependencies"
  * The status of the run (PR description or issue comment, see below) must contain:
    * List of dependencies that were upgraded, with their versions (if any)
    * List of updates available with major versions (if any)
    * Other details you deem relevant
  * Code can be updated if needed
    (deprecated APIs, breaking changes, reformatting, idiomatic uses of updated APIs)
  * Acceptance criteria: `make check-all` passes

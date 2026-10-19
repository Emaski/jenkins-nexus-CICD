# vprofile CI/CD Pipeline

A Jenkins declarative pipeline that builds, tests, analyzes, and publishes a Java web application (vprofile) to a Nexus Repository Manager. The project was built as part of a hands-on DevOps course and extended with [your additions, e.g., "a quality gate that blocks bad code from being published"].

## Pipeline Stages

| # | Stage | What it does | Tooling |
|---|-------|--------------|---------|
| 1 | Build | Compiles the project and packages it as a WAR file, then archives it | Maven |
| 2 | Unit Test | Runs the unit test suite | Maven, JUnit |
| 3 | Integration Test | Runs integration tests via `mvn verify` | Maven |
| 4 | Checkstyle | Generates a code style report | Checkstyle |
| 5 | SonarQube Analysis | Scans the code and waits for the quality gate. The pipeline aborts if the gate fails | SonarQube, SonarScanner |
| 6 | Publish | Uploads the WAR and POM to a Nexus release repository | Nexus 3, Nexus Artifact Uploader plugin |

## Architecture

```
Git push → Jenkins → Maven build & tests → Checkstyle → SonarQube (quality gate) → Nexus
```

## Prerequisites

**Jenkins plugins:** Pipeline, Git, Maven Integration, SonarQube Scanner, Nexus Artifact Uploader, and Pipeline Utility Steps (for `readMavenPom`).

**Jenkins configuration:**

| Item | Name / ID | Purpose |
|------|-----------|---------|
| Maven tool | (optional, see Jenkinsfile) | Build tool |
| SonarScanner tool | `sonarscanner4` | Runs the code analysis |
| SonarQube server | `sonar-pro` | Analysis target and quality gate |
| Credentials | `nexuslogin` (username/password) | Authenticates uploads to Nexus |
| Environment variable or secret | `NEXUS_URL` | Address of the Nexus server |

Nothing sensitive is stored in this repository. Hostnames and credentials are configured in Jenkins.

## Configuration

The pipeline's main settings are in the `environment` block of the `Jenkinsfile`:

- `NEXUS_VERSION`, `NEXUS_PROTOCOL`: Nexus version and protocol (`nexus3`, `http`)
- `NEXUS_REPOSITORY`: Target Nexus repository (`vprofile-release`)
- `NEXUS_CREDENTIAL_ID`: ID of the Jenkins credential used for uploads
- `ARTVERSION`: Artifact version, set to the Jenkins build number so every build publishes a unique version

## Running the Pipeline

1. Create a Pipeline job in Jenkins and point it at this repository.
2. Set the job's definition to *Pipeline script from SCM* and the script path to `Jenkinsfile`.
3. Complete the Jenkins configuration above.
4. Click **Build Now**.

## Lessons Learned

[Optional, but recruiters like this section. Add 2 to 3 real problems you hit and how you solved them, for example:]

- Configuring credential scoping so Jenkins could reach SonarQube
- Diagnosing why the quality gate timed out
- Understanding how Nexus handles versioned artifacts

## Possible Improvements

- Move the build and analysis into Docker agents for reproducible environments
- Add a deployment stage to push the artifact to AWS
- Replace the hardcoded artifact version with a Git tag or semantic version

## Author

[Your name] · [LinkedIn] · [Email]

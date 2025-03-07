# LHM Actions

TEST

This repository contains and provides actions to

- Build Maven projects and deploy them to Maven Central
- Build npm projects and deploy them to npmjs
- Build Docker images and push them to GitHub Container Registry
- Create GitHub Releases
- Build vitepress Documentation and deploy it to GitHub Pages
- You can use CodeQL to identify vulnerabilities and errors in your code. The results are shown as code scanning alerts in GitHub.

## Usage

### action-build-docs

Action to build a vitepress docs project.

Executes the following steps:

1. Enables GitHub Pages
2. Build vitepress docs project
3. Uploads the build output as an artifact.<br/>
   The uploaded artifact can be used to deploy the docs to a web page (see [action-deploy-docs](#action-deploy-docs))

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-build-docs
  with:
    # Path to vitepress docs project
    # Default: ./docs
    docs-path: "./docs"

    # Node version
    # Default: 22
    node-version: "22"

    # You can change build command, e.g. if using vuepress
    # Default: build
    build-cmd: "build"

    # Vitepress output path that will be uploaded as artifact
    # Default: .vitepress/dist
    dist-path: ".vitepress/dist"
```

### action-build-image

Action to build a Docker image and push it to a registry.

Executes the following steps:

1. Checkout code
2. Login to Registry
3. Extract metadata (tags, labels) for Docker
4. Build and push image to a registry

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-build-image
  with:
    # Image registry to push image to
    # Default: ghcr.io
    registry: "ghcr.io"

    # Username to authenticate against image registry
    registry-username: ${{ github.actor }}

    # Password to authenticate against image registry
    registry-password: ${{ secrets.GITHUB_TOKEN }}

    # Tags to tag image with
    # Default: type=raw,value=latest
    image-tags: type=raw,value=latest

    # Labels to add to image  
    # Default: org.opencontainers.image.description=See ${{ github.server_url }}/${{ github.repository }}
    # Optional
    image-labels: |
      org.opencontainers.image.description=See ${{ github.server_url }}/${{ github.repository }}

    # Path to the Dockerfile to build image from
    path: ${{ github.event.inputs.app-path }}

    # Name to give the image
    image-name: ${{ github.event.inputs.app-path }}

    # Name of the artifact to download
    artifact-name: ${{ needs.release-maven.outputs.ARTIFACT_NAME }}
```

### action-checkout

Action to wrap [GitHub Action Checkout](https://github.com/actions/checkout)
using current branch with default values.

Executes the following steps:

1. Checkout current branch

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-checkout
```

### action-codeql

Action to scan a repository using provided CodeQL language, buildmode and query scan set

Executes the following steps:

1. Checkout repository
2. Setup JDK Version
3. Initialize CodeQL for language type
4. Build using Autobuild
5. Perform CodeQL analysis for language type

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-codeql
  with:
    # CodeQL language name to scan with (e.g java-kotlin, javascript-typescript, python, ...)
    codeql-language: "java-kotlin"
    
    # Build mode to use when scanning the source code (e.g. none, autobuild, manual)
    # Default: none
    codeql-buildmode: "autobuild"

    # Query set to use when analyzing the source code (e.g. default, security-extended, security-and-quality)
    # Default: security-and-quality
    codeql-query: "security-and-quality"

    # Temurin JDK version to use for autobuild (only when codeql-language is java-kotlin and codeql-build is set to autobuild)
    # Default: 21
    java-version: "21"

    # Path to scan files in
    # Default: .
    path: "."
```

### action-create-github-release

Action to create a GitHub Release.

Executes the following steps:

1. Download a single artifact
2. Create a GitHub Release

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-create-github-release
  with:
    # Name of the artifact to download
    artifact-name: my-artifact

    # Name of a tag (e.g. sps-1.0.0 or myproject-1.0.0)
    tag-name: myrepo-1.0.0

    # Path to the artifacts (e.g. ./target/*.jar)
    artifact-path: ./target/*.jar
```

### action-dependency-review

The dependency review action scans your pull requests for dependency changes, and will raise an error if any vulnerabilities or invalid licenses are being introduced.
It will always use the baseline configuration in <https://github.com/it-at-m/.github/blob/main/workflow-configs/dependency_review.yaml>.

Executes the following steps:

1. Checkout repository
2. Execute dependency review check

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-dependency-review
  with:
    # Additional comma separated string of packages to be ignored by the dependency check (see https://github.com/package-url/purl-spec for more information)
    # Default: ""
     allow-dependencies-licenses: "pkg:maven/com.github.spotbugs/spotbugs-annotations, pkg:maven/com.h3xstream.findsecbugs:findsecbugs-plugin"
```

### action-deploy-docs

Action to deploy a docs artifact to a web page.

Executes the following steps:

1. Deploy docs to a web page

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-deploy-docs
  with:
    # Name of the artifact to deploy
    # Default: github-pages
    artifact_name: "github-pages"

    # Branch to deploy docs from
    # Default: main
    deploy-branch: "docs"
```

### action-maven-build

Action to build a Maven project.

Executes the following steps:

1. Checkout repository
2. Setup Java version
3. Execute Maven Build
4. Upload build artifact

Output parameters:

1. `artifact-name`: Name of the uploaded artifact

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-maven-build
  with:
    # Java Version to use
    # Default: 21
    java-version: "21"

    # Path to pom.xml
    app-path: "."
```

### action-maven-release

Action to create a Maven Release and deploy it to Maven Central.

Executes the following steps:

1. Checkout repository
2. Setup Java version
3. Execute Maven Release and deploy it to Maven Central
4. Upload release artifact

Output parameters:

1. `MVN_ARTIFACT_ID`: Artifact name of pom.xml
2. `artifact-name`: Name of the uploaded artifact

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-maven-release
  with:
    # Java Version to use
    # Default: 21
    java-version: "21"

    # Path to pom.xml
    app-path: "."

    # Version which will be released
    releaseVersion:

    # Next snapshot version
    developmentVersion:

    # Skip deployment to maven central
    # Default: true
    skipDeployment: "false"

    # Environment variable for GPG private key passphrase
    SIGN_KEY_PASS: ${{ secrets.gpg_passphrase }}

    # Environment variable for Maven central username
    CENTRAL_USERNAME: ${{ secrets.sonatype_username }}

    # Env variable for Maven central token
    CENTRAL_PASSWORD: ${{ secrets.sonatype_password }}

    # Value of the GPG private key to import
    GPG_PRIVATE_KEY: ${{ secrets.gpg_private_key }}
```

### action-npm-build

Action to build an npm project.

Executes the following steps:

1. Checkout repository
2. Setup Node.js
3. Install dependencies
4. Run lint
5. Run test
6. Run build
7. Upload artifact

Outputs:

1. `artifact-name`: Name of the uploaded artifact

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-npm-build
  with:
    # Node Version to use
    # Default: 22.11.0
    node-version: "22.11.0"

    # Path to package.json
    app-path: "."

    # Controls the execution of the 'npm run lint' script
    # Default: true
    run-lint: "true"

    # Controls the execution of the 'npm run test' script
    # Default: true
    run-test: "true"
```

### action-npm-release

Action to create an npm release and deploy it to Node.js.

Executes the following steps:

1. Checkout repository
2. Setup Node.js version
3. Bump version and create Git tag
4. Run npm build
5. Deploy npm artifact to Node.js

Output parameters:

1. `ARTIFACT_NAME`: Name of artifact
2. `ARTIFACT_VERSION`: Version of the uploaded artifact

### action-pr-checklist

Action to enforce ticking of all checklist items inside a PR (useful for PR templates)

<!-- prettier-ignore -->
```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-pr-checklist
  with:
    # Whether the action should fail if the PR contains no checklist
    # Default: false
    fail-missing: "false"
```

Testline

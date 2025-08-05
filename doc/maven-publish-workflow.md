# Maven Publish Workflow Documentation

## Overview

The `maven-publish.yml` workflow is a reusable GitHub Actions workflow designed to publish Maven artifacts to Maven Central via Sonatype Central API. This workflow automates the process of downloading artifacts from GitHub Packages and uploading them to Maven Central for public distribution.

**Purpose**: This workflow is specifically designed for publishing ReportPortal commons libraries (e.g., `commons-dao`, `commons-bom`, `commons-model`, etc.) to Maven Central, making them available for public consumption.

## Workflow Structure

### Reusable Workflow
- **File**: `.github/.github/workflows/maven-publish.yml`
- **Type**: Reusable workflow (can be called by other workflows)
- **Trigger**: `workflow_call`

### Caller Workflow
- **File**: `{commons-library}/.github/workflows/maven-publish.yml`
- **Type**: Manual workflow dispatch
- **Trigger**: `workflow_dispatch`
- **Usage**: Each commons library (e.g., `commons-dao`, `commons-bom`, `commons-model`, etc.) can have its own caller workflow

## Inputs

### Required Inputs
- **`version`**: Release version (e.g., "5.14.4")
  - Description: The version number to publish
  - Type: string
  - Required: true

### Optional Inputs
- **`artifact`**: Artifact name (e.g., "commons-dao", "commons-bom")
  - Description: The name of the artifact to publish
  - Type: string
  - Default: empty string
  - Required: false

- **`package_suffixes`**: Comma-separated list of package suffixes to download
  - Description: File extensions to download from GitHub Packages
  - Type: string
  - Default: empty string
  - Required: false
  - Example: `-javadoc.jar,-sources.jar,.jar,.pom`

## Secrets

### Required Secrets
- **`SONATYPE_USER`**: Sonatype Central username
- **`SONATYPE_PASSWORD`**: Sonatype Central password

### Automatic Secrets
- **`GITHUB_TOKEN`**: Automatically provided by GitHub (no need to pass explicitly)

## Environment Variables

```yaml
REPOSITORY_URL: 'https://maven.pkg.github.com'
PACKAGE: 'com.epam.reportportal'
SONATYPE_CENTRAL_API: 'https://central.sonatype.com/api/v1/publisher'
```

## Workflow Steps

### 1. Checkout Code
- Uses `actions/checkout@v4`
- Checks out the repository code

### 2. Setup Java Environment
- Uses `actions/setup-java@v4`
- Sets up JDK 11 with Temurin distribution

### 3. Get Variables
- Sets up environment variables for the workflow:
  - `PACKAGE_PATH`: Converts package name to path format (e.g., `com/epam/reportportal`)
  - `VERSION`: The release version
  - `ARTIFACT`: The artifact name
  - `ARTIFACT_NAME`: Full artifact name with version
  - `PACKAGE_SUFFIXES`: Package suffixes to download

### 4. Create Authentication Token
- Creates base64 encoded authentication token for Sonatype Central API
- Format: `base64(username:password)`

### 5. Download Artifacts from GitHub Packages
- Downloads all artifacts specified in `package_suffixes`
- Uses GitHub Packages API with authentication
- Downloads files like:
  - JAR files (main, sources, javadoc)
  - POM files
  - Signature files (.asc)
  - Checksum files (.md5, .sha1)

### 6. Create Deployment Bundle
- Creates a ZIP bundle with proper Maven repository structure
- Organizes files in the structure: `{package_path}/{artifact}/{version}/`
- Includes all downloaded artifacts in the bundle

### 7. Upload Bundle to Sonatype Central
- Uploads the ZIP bundle to Sonatype Central API
- Uses the new Central API endpoint
- Sets publishing type to `USER_MANAGED`
- Extracts deployment ID from the response

### 8. Monitor Deployment Status
- Polls the deployment status every 30 seconds
- Maximum 30 attempts (15 minutes total)
- Handles different deployment states:
  - `PENDING`: Waiting to start
  - `VALIDATING`: Validating the bundle
  - `VALIDATED`: Bundle validated, ready to publish
  - `PUBLISHING`: Currently publishing
  - `PUBLISHED`: Successfully published
  - `FAILED`: Deployment failed

### 9. Success Notification
- Runs only on successful completion
- Displays success message with package information

## Usage Example

### Calling the Workflow
```yaml
jobs:
  publish:
    uses: ./.github/.github/workflows/maven-publish.yml
    with:
      version: "5.14.4"
      artifact: "{library-name}"  # e.g., "commons-dao", "commons-bom", "commons-model"
      package_suffixes: "-javadoc.jar,-javadoc.jar.asc,-sources.jar,-sources.jar.asc,.jar,.jar.asc,.pom,.pom.asc,.pom.md5,.pom.sha1,.jar.md5,.jar.sha1,-javadoc.jar.md5,-javadoc.jar.sha1,-sources.jar.md5,-sources.jar.sha1"
    secrets:
      SONATYPE_USER: ${{ secrets.SONATYPE_USER }}
      SONATYPE_PASSWORD: ${{ secrets.SONATYPE_PASSWORD }}
```

### Example for Different Commons Libraries

#### commons-dao
```yaml
jobs:
  publish:
    uses: ./.github/.github/workflows/maven-publish.yml
    with:
      version: "5.14.4"
      artifact: "commons-dao"
      package_suffixes: "-javadoc.jar,-javadoc.jar.asc,-sources.jar,-sources.jar.asc,.jar,.jar.asc,.pom,.pom.asc,.pom.md5,.pom.sha1,.jar.md5,.jar.sha1,-javadoc.jar.md5,-javadoc.jar.sha1,-sources.jar.md5,-sources.jar.sha1"
```

#### commons-bom
```yaml
jobs:
  publish:
    uses: ./.github/.github/workflows/maven-publish.yml
    with:
      version: "5.14.4"
      artifact: "commons-bom"
      package_suffixes: ".pom,.pom.asc,.pom.md5,.pom.sha1"
```

#### commons-model
```yaml
jobs:
  publish:
    uses: ./.github/.github/workflows/maven-publish.yml
    with:
      version: "5.14.4"
      artifact: "commons-model"
      package_suffixes: "-javadoc.jar,-javadoc.jar.asc,-sources.jar,-sources.jar.asc,.jar,.jar.asc,.pom,.pom.asc,.pom.md5,.pom.sha1,.jar.md5,.jar.sha1,-javadoc.jar.md5,-javadoc.jar.sha1,-sources.jar.md5,-sources.jar.sha1"
```

### Manual Trigger
1. Go to the specific commons library repository's Actions tab
2. Select "Publish to Maven Central" workflow
3. Click "Run workflow"
4. Enter the version number
5. Click "Run workflow"

**Note**: Each commons library repository should have its own workflow file at `{library}/.github/workflows/maven-publish.yml` that calls the reusable workflow with the appropriate `artifact` parameter.

## Error Handling

### Common Issues
1. **Missing artifacts**: If required files are not found in GitHub Packages
2. **Authentication failures**: Invalid Sonatype credentials
3. **Validation errors**: Bundle doesn't meet Maven Central requirements
4. **Timeout**: Deployment takes longer than 15 minutes

### Troubleshooting
- Check GitHub Packages for required artifacts
- Verify Sonatype credentials
- Review deployment logs for specific error messages
- Ensure bundle structure follows Maven repository conventions

## Security Considerations

- Sonatype credentials are stored as repository secrets
- `GITHUB_TOKEN` is automatically provided by GitHub
- Authentication tokens are created securely using base64 encoding
- No sensitive information is logged in workflow output

## Dependencies

- **GitHub Packages**: Source of artifacts to publish
- **Sonatype Central API**: Target for publishing
- **JDK 11**: Required for Maven operations
- **curl**: Used for API calls
- **jq**: Used for JSON parsing
- **zip**: Used for creating deployment bundles 
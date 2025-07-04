# Tag-Based Release Pipeline

This repository now includes automated pipelines that trigger when you push a git tag. Here's how to use them:

## Available Workflows

### 1. Release Workflow (`.github/workflows/release.yml`)
- **Triggers:** On any tag push
- **Actions:** 
  - Builds the project
  - Runs tests
  - Creates a GitHub release
  - Uploads multiple build artifacts as release assets:
    - Plugin ZIP (for OpenSearch installation)
    - Plugin JAR (compiled plugin)
    - Sources JAR (source code)
    - Javadoc JAR (API documentation)
- **Permissions:** Requires `contents: write` permission

### 2. GitHub Packages Workflow (`.github/workflows/publish-packages.yml`)
- **Triggers:** On any tag push
- **Actions:**
  - Builds the project
  - Publishes Maven artifacts to GitHub Packages
- **Permissions:** Requires `packages: write` permission

## How to Trigger the Pipeline

### Step 1: Create and Push a Tag

For example, to create and push tag `2.5.0.0`:

```bash
# Create a signed tag
git tag -s -a 2.5.0.0 -m "Release 2.5.0.0"

# Verify the tag
git tag -v 2.5.0.0

# Push the tag to trigger the pipeline
git push origin 2.5.0.0
```

### Step 2: Monitor the Pipeline

1. Go to the **Actions** tab in your GitHub repository
2. You should see the workflows running for your tag
3. The release workflow will create a new release in the **Releases** section

## Version Configuration

The plugin version is configured in `gradle.properties`. Make sure to update it to match your tag:

```properties
version = 2.5.0.0
```

**Note:** The current code is compatible with OpenSearch 2.18.0. If you want to build for OpenSearch 2.5.0, you'll need to either:
1. Use a compatible codebase for OpenSearch 2.5.0, or
2. Use the current version (2.18.0.0) and update your tag accordingly

## Example Usage

To create a release for version 2.18.0.0:

```bash
# Update version in gradle.properties if needed
echo "version = 2.18.0.0" > gradle.properties

# Create and push the tag
git tag -s -a 2.18.0.0 -m "Release 2.18.0.0"
git push origin 2.18.0.0
```

This will:
1. Trigger both workflows
2. Create a GitHub release with multiple build artifacts:
   - Plugin ZIP (for installation)
   - Plugin JAR (compiled plugin)
   - Sources JAR (source code)
   - Javadoc JAR (API documentation)
3. Publish the Maven artifacts to GitHub Packages

## Installation

Once the release is created, users can install the plugin using:

```bash
./bin/opensearch-plugin install https://github.com/schtinkafinga/prometheus-exporter-plugin-for-opensearch/releases/download/2.18.0.0/prometheus-exporter-2.18.0.0.zip
```

## Troubleshooting

- **Permission denied:** Make sure your repository has the necessary permissions enabled in Settings → Actions → General
- **Build fails:** Check that the version in `gradle.properties` matches the OpenSearch version compatibility
- **Tag already exists:** Delete the existing tag locally and remotely before recreating it

## Manual Release Process

If you prefer the manual process, you can still follow the steps in `RELEASE_PROCESS.md`.
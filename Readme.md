
# Publish .NET Package

This GitHub Action builds, restores dependencies, and publishes a .NET package with dynamic versioning based on the Git reference (branches or tags).

## Features
- Sets up the required .NET SDK version.
- Restores project dependencies.
- Builds the .NET project in `Release` configuration.
- Dynamically generates package names and versions based on the current branch or tag.
- Creates and uploads the NuGet package as an artifact.

## Inputs

| Name             | Description                                | Required | Default  |
|------------------|--------------------------------------------|----------|----------|
| `dotnet-version` | The version of the .NET SDK to use.        | Yes      | `8.0.x`  |

## Outputs

| Name              | Description                                  |
|-------------------|----------------------------------------------|
| `package-id`      | The ID of the generated package.             |
| `package-version` | The version of the generated package.        |

## Package Versioning Logic
- **Tags (`refs/tags/*`)**: The package version matches the tag name, provided the tag is created on the `master` branch.
- **Development Branch (`refs/heads/development`)**: The package version is derived from the GitHub run ID.
- **Release Branch (`refs/heads/release`)**: The package version includes the commit hash of the latest commit.

## Example Workflow

```yaml
name: Publish .NET Package

on:
  push:
    branches:
      - development
      - release
    tags:
      - '*'

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Publish .NET Package
        uses: yashmahi88/publish-.net@v1
        with:
          dotnet-version: '8.0.x'
```

## How It Works
1. **Setup .NET**: Ensures the specified .NET SDK version is installed.
2. **Restore Dependencies**: Runs `dotnet restore` to install dependencies.
3. **Build Project**: Compiles the project using `dotnet build --configuration Release`.
4. **Set Package Name and Version**:
   - Determines the package ID and version based on the current Git reference.
   - Supports `tags`, `development`, and `release` branches.
5. **Pack Project**: Creates the NuGet package using `dotnet pack` with the computed package name and version.
6. **Upload Package**: Uploads the generated package as an artifact for download.

## Outputs Example
After running the workflow, you'll see the following outputs:
- **Package ID**: Example: `nexus-git-release-123abc`.
- **Package Version**: Example: `release-123abc`.



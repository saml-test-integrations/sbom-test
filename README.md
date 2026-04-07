# SBOM Generation Workflow (Syft + CycloneDX)

This Section provides a reusable GitHub Actions workflow for
generating a CycloneDX Software Bill of Materials (SBOM) using Anchore
Syft.

The workflow is designed to be:
``` 
- Reusable across repositories
- Centrally managed
- Lightweight for consumers 
```
------------------------------------------------------------------------

## What This Workflow Does

When invoked, this workflow:
```
1.  Checks out the repository
2.  Generates an SBOM using Syft
3.  Outputs the SBOM in CycloneDX JSON format
4.  Uploads the SBOM:
    -   As a GitHub Actions artifact
    -   As a GitHub release asset (when triggered via release)
```
------------------------------------------------------------------------

## Output

The workflow produces a single SBOM file:
```
`<sbom_name>`{=html}.cyclonedx.json

Example: v1.2.3.cyclonedx.json
```
------------------------------------------------------------------------

## Reusable Workflow Location
```
.github/workflows/syft-release-sbom.yml
```
------------------------------------------------------------------------

## How to Use (Consumer Repos)

On Release Published:
```yaml
name: syft-sbom-ci

on:
  release:
    types: [published]

jobs:
  sbom:
    uses: your-org/your-central-repo/.github/workflows/syft-release-sbom.yml@main
    with:
      sbom_name: ${{ github.event.release.tag_name }}
```
------------------------------------------------------------------------

## Run Manually

You can trigger this workflow manually for testing or troubleshooting:

```yaml
name: syft-sbom-ci

on:
  workflow_dispatch:

jobs:
  sbom:
    uses: your-org/your-central-repo/.github/workflows/syft-release-sbom.yml@main
    with:
      sbom_name: manual
```
-----------------------------------------------------------------------------

## Run on Specific Branch

You can configure the workflow to run on pushes to a specific branch:

```yaml
name: syft-sbom-ci

on:
  push:
    branches:
      - main
jobs:
  sbom:
    uses: your-org/your-central-repo/.github/workflows/syft-release-sbom.yml@main
    with:
      sbom_name: ${{ github.ref_name }}
```
## Input Parameters

  | Name      | Required | Description                                      |
|-----------|----------|--------------------------------------------------|
| sbom_name | Yes      | Name of the generated SBOM file (release tag)    |


## Versioning Strategy

This workflow uses a pinned version of the SBOM action:
```
anchore/sbom-action@v0.24.0

```
------------------------------------------------------------------------
This ensures:
Stability across all repositories
No unexpected breaking changes

## Summary
This workflow provides centralized SBOM generation with minimal setup
and future-ready supply chain visibility.
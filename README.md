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

## Recommended Usage (Policy)

> ⚠️ This workflow is required to run on **release publish** as part of our SBOM policy.

All repositories must configure the workflow as follows:

```yaml
name: syft-sbom-ci

on:
  release:
    types: [published]

jobs:
  sbom:
    uses: saml-test-integrations/ssdlc-actions/.github/workflows/syft-release-sbom.yml@main
    with:
      sbom_name: ${{ github.event.release.tag_name }}
```
------------------------------------------------------------------------
This ensures:
```
- SBOMs are generated for every release
- SBOM naming aligns with release versions 
- Compatibility with future Dependency-Track ingestion
```
------------------------------------------------------------------------

---
# 🧪 Optional (Testing / Troubleshooting)

⚠️ These configurations are for testing only and should not replace the required release-based workflow.

```markdown
## Optional: Manual or Branch Testing

For testing or troubleshooting, you may temporarily run the workflow manually or on a branch.

### Manual trigger

```yaml
on:
  workflow_dispatch:
```
Branch-based testing

```yaml
on:
  push:
    branches:
      - main
```
Example:

```yaml
name: syft-sbom-ci
on:
  workflow_dispatch:
jobs:
  sbom:
    uses: saml-test-integrations/ssdlc-actions/.github/workflows/syft-release-sbom.yml@main
    with:
      sbom_name: manual
```
------------------------------------------------------------------------
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
-   Stability across all repositories
-   No unexpected breaking changes

## Summary
This workflow provides centralized SBOM generation with minimal setup
and future-ready supply chain visibility.
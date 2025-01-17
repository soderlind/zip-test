# Create file attestation

This repository contains content used to test the [manually-build-zip.yml](.github/workflows/manually-build-zip.yml) GitHub Actions workflow. The workflow generates a zip file with the repository’s content, uploads it to the [release page](https://github.com/soderlind/zip-test/releases/tag/2.0.0), and creates an [attestation](https://github.com/soderlind/zip-test/attestations/4475606) for the zip file.

## Actions performed by the workflow:

- Create a zip file with the repository’s content ([thedoctor0/zip-release](https://github.com/thedoctor0/zip-release?tab=readme-ov-file#zip-release-))
  ```yml
  - name: Create zip archive
    uses: thedoctor0/zip-release@0.7.6
    with:
      type: 'zip'
      filename: ${{ github.event.inputs.zip }}
      exclusions: '*.git* .editorconfig composer* *.md vendor/*/test* vendor/*/docs'
  ```
- Upload the zip file to the release page ([softprops/action-gh-release](https://github.com/softprops/action-gh-release?tab=readme-ov-file#--action-gh-release))
  ```yml
  - name: Upload to release
    id: upload
    uses: softprops/action-gh-release@v2
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    with:
      files: ${{ github.event.inputs.zip }}
      tag_name: ${{ github.event.inputs.tag }}
  ```
- Create an attestation for the zip file ([johnbillion/action-wordpress-plugin-attestation](https://github.com/johnbillion/action-wordpress-plugin-attestation?tab=readme-ov-file#wordpress-plugin-attestation))
  ```yml
  - name: Generate attestation
    uses: johnbillion/action-wordpress-plugin-attestation@0.7.0
    with:
      zip-path: ${{ github.event.inputs.zip }}
      zip-url: ${{ steps.asset_url.outputs.final_url }}
  ```

## [manually-build-zip.yml](.github/workflows/manually-build-zip.yml):

```yml
name: Manually Build Release Zip

on:
  workflow_dispatch:
    inputs:
      tag:
        description: 'Release tag (e.g. v1.0.0)'
        required: true
        type: string
      zip:
        description: 'Output zip filename (e.g. my-plugin.zip)'
        required: true
        type: string

jobs:
  create-release:
    name: Create Release Package
    runs-on: ubuntu-latest
    permissions:
      attestations: write
      contents: write
      id-token: write
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Build plugin # Remove or modify this step as needed
        run: |
          composer install --no-dev

      - name: Create zip archive
        uses: thedoctor0/zip-release@0.7.6
        with:
          type: 'zip'
          filename: ${{ github.event.inputs.zip }}
          exclusions: '*.git* .editorconfig composer* *.md vendor/*/test* vendor/*/docs'

      - name: Upload to release
        id: upload
        uses: softprops/action-gh-release@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          files: ${{ github.event.inputs.zip }}
          tag_name: ${{ github.event.inputs.tag }}

      - name: Get final asset URL
        id: asset_url
        run: |
          URL="$(curl -s -I -o /dev/null -w "%{redirect_url}" "${{ fromJson(steps.upload.outputs.assets)[0].browser_download_url }}")"
          echo "final_url=${URL}" >> "${GITHUB_OUTPUT}"

      - name: Generate attestation
        uses: johnbillion/action-wordpress-plugin-attestation@0.7.0
        with:
          zip-path: ${{ github.event.inputs.zip }}
          zip-url: ${{ steps.asset_url.outputs.final_url }}
```

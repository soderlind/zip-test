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

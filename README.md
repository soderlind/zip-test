# Create file attestation

This repository contains content used to test the [manually-build-zip.yml](.github/workflows/manually-build-zip.yml) GitHub Actions workflow. The workflow generates a zip file with the repository’s content, uploads it to the [release page](https://github.com/soderlind/zip-test/releases/tag/2.0.0), and creates an [attestation](https://github.com/soderlind/zip-test/attestations/4475606) for the zip file.

## Actions performed by the workflow:

- Create a zip file with the repository’s content
  - [thedoctor0/zip-release](https://github.com/thedoctor0/zip-release)
- Upload the zip file to the release page
  - [softprops/action-gh-release](https://github.com/softprops/action-gh-release)
- Create an attestation for the zip file
  - [johnbillion/action-wordpress-plugin-attestation](https://github.com/johnbillion/action-wordpress-plugin-attestation)

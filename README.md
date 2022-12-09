# archiving-upload-artifact

In our builds we noticed that [actions/upload-artifact](https://github.com/marketplace/actions/upload-a-build-artifact) becomes really slow when trying to upload large numbers of files.

For comaprison:

|          Content          | File Count | Total Size | Upload time |
|---------------------------|------------|------------|-------------|
| Android APKs              | 6          | ~88MB      | 8s          |
| Android Unit Test Results | 1192       | ~13MB      | 2m 25s      |

This action attempts to reduce the upload time by putting all matched files into an archive before uploading.

## Inputs
The inputs of the action mirror [actions/upload-artifact](https://github.com/marketplace/actions/upload-a-build-artifact) except for path stripping:

`upload-artifact` strips away the directory hierarchy that is common between all `path` inputs. (See [Upload using Multiple Paths and Exclusions]([actions/upload-artifact](https://github.com/marketplace/actions/upload-a-build-artifact)))

To replicate the same behavior with this action, make sure to set the `directory` parameter.

| Parameter        |  Description |
|------------------|--------------|
| `name`           | The name under which the artifact will be uploaded |
| `directory`      | The base directory for all `path` values. Defaults to '`.`' | 
| `path`           | 'A file, directory or wildcard pattern that describes what to upload. These must be relative to the `directory` input parameter.' |
| `retention-days` | Duration after which artifact will expire in days. 0 means using default retention. Minimum 1 day. Maximum 90 days unless changed from the repository settings page. |
 
## Example
```yaml
- name: Archive and upload Build Artifacts
  uses: RockLobster/archiving-upload-artifact@v0.1.0
  with:
    name: android-test-results
    directory: android/libs
    path: |
      */build/test-results/**
      */build/reports/**
      */build/cucumber-reports/**
```
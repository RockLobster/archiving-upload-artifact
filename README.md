# archiving-upload-artifact

In our builds we noticed that [actions/upload-artifact](https://github.com/actions/upload-artifact) becomes really slow when trying to upload large numbers of files.

For comaprison:

|          Content          | File Count | Total Size | Upload time |
|---------------------------|------------|------------|-------------|
| Android APKs              | 6          | ~88MB      | 8s          |
| Android Unit Test Results | 1192       | ~13MB      | 2m 25s      |

This action attempts to reduce the upload time by putting all matched files into an archive before uploading.

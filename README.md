# Make Pear Apps :pear:

GitHub Action to build Pear apps on Linux, macOS, and Windows, with code signing and artifact upload.

## Inputs

### macOS

| Input | Description | Required |
|-------|------------|----------|
| `macos_certificate_base64` | Base64 Apple distribution certificate (P12) | Required on macOS |
| `macos_p12_password` | Password for the P12 certificate | Required on macOS |
| `macos_provisioning_profile_base64` | Base64 macOS provisioning profile | Required on macOS |
| `macos_keychain_password` | Temporary keychain password | Required on macOS |
| `macos_api_key_base64` | Base64 Apple API key (.p8) | Required on macOS |
| `macos_api_key_id` | Apple API key ID | Required on macOS |
| `macos_api_key_issuer` | Apple API key issuer ID | Required on macOS |
| `macos_codesign_identity` | Code signing identity | Required on macOS |

### Windows

| Input | Description | Required |
|-------|------------|----------|
| `windows_subject` | Windows code signing certificate subject | Required on Windows |
| `windows_thumbprint` | Windows code signing certificate thumbprint | Required on Windows |

### Linux

Linux builds require no inputs.

## Outputs

- All files in the `out/make` folder are uploaded as artifacts.  
- Each file is uploaded individually using its basename.  
- Artifacts can be downloaded from the workflow run summary.

## Usage

### Linux

```yaml
jobs:
  build-linux:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: RaisinTen/make-pear-app
```

### macOS

```yaml
jobs:
  build-macos:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4
      - uses: RaisinTen/make-pear-app
        with:
          macos_certificate_base64: ${{ secrets.APPLE_DISTRIBUTION_CERTIFICATE }}
          macos_p12_password: ${{ secrets.APPLE_P12_PASSWORD }}
          macos_provisioning_profile_base64: ${{ secrets.APPLE_PROVISIONING_PROFILE }}
          macos_keychain_password: ${{ secrets.APPLE_KEYCHAIN_PASSWORD }}
          macos_api_key_base64: ${{ secrets.APPLE_APIKEY }}
          macos_api_key_id: ${{ secrets.APPLE_APIKEY_ID }}
          macos_api_key_issuer: ${{ secrets.APPLE_APIKEY_ISSUER }}
          macos_codesign_identity: ${{ secrets.MAC_CODESIGN_IDENTITY }}
```

### Windows

```yaml
jobs:
  build-win32:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v4
      - uses: RaisinTen/make-pear-app
        with:
          windows_subject: ${{ secrets.WIN_SUBJECT }}
          windows_thumbprint: ${{ secrets.WIN_THUMBPRINT }}
```

# Make Pear Apps :pear:

GitHub Action to build Pear apps on Linux, macOS, and Windows, with code signing and artifact upload.

## Inputs

### Build Inputs

| Input | Description | Required |
|-------|------------|----------|
| `channel` | Channel name (e.g. `preview`, `experimental`, `staging`) | Yes |
| `upgrade_key` | Upgrade key (e.g. `pear://jj7jywoj83pswtcf5asywbm4ngro3xikgg1zcaqq3kdyhghats6o`) | No |

### macOS

| Input | Description | Required |
|-------|------------|----------|
| `macos_certificate_base64` | Base64 Apple distribution certificate (P12) | Required on macOS |
| `macos_p12_password` | Password for the P12 certificate | Required on macOS |
| `macos_codesign_identity` | Code signing identity | Required on macOS |
| `macos_notarization_method` | Notarization method to use: `appstore_connect` or `apple_id_password` | Required on macOS |
| `macos_apple_id` | Apple ID email for notarization | Required on macOS if `macos_notarization_method` is `apple_id_password` |
| `macos_apple_password` | Apple app-specific password | Required on macOS if `macos_notarization_method` is `apple_id_password` |
| `macos_apple_team_id` | Apple Developer Team ID | Required on macOS if `macos_notarization_method` is `apple_id_password` |
| `macos_api_key_base64` | Base64-encoded App Store Connect API key (like AuthKey_ABCD123456.p8) | Required on macOS if `macos_notarization_method` is `appstore_connect` |
| `macos_api_key_id` | 10-character alphanumeric ID string of the App Store Connect (like ABCD123456) | Required on macOS if `macos_notarization_method` is `appstore_connect` |
| `macos_api_issuer` | UUID that identifies the API key issuer | Required on macOS if `macos_notarization_method` is `appstore_connect` |

### Windows

| Input | Description | Required |
|-------|------------|----------|
| `windows_subject` | Windows code signing certificate subject | Required on Windows |
| `windows_thumbprint` | Windows code signing certificate thumbprint | Required on Windows |

### Linux

Linux builds require no inputs.

## Outputs

| Output | Description |
|-------|------------|
| `artifact-links` | Links to the uploaded artifacts from the `out/make` folder. Each file is uploaded individually and the link can be used in workflow notifications (e.g., Slack, Teams). |

## Usage

### Linux

```yaml
jobs:
  build-linux:
    runs-on: ubuntu-latest
    outputs:
      artifact-links: ${{ steps.build.outputs.artifact-links }}
    steps:
      - uses: actions/checkout@v4
      - uses: holepunchto/make-pear-app
        id: build
        with:
            channel: experimental
            upgrade_key: pear://jj7jywoj83pswtcf5asywbm4ngro3xikgg1zcaqq3kdyhghats6o
```

### macOS

```yaml
jobs:
  build-macos:
    runs-on: macos-latest
    outputs:
      artifact-links: ${{ steps.build.outputs.artifact-links }}
    steps:
      - uses: actions/checkout@v4
      - uses: holepunchto/make-pear-app
        id: build
        with:
          channel: preview
          macos_certificate_base64: ${{ secrets.APPLE_DISTRIBUTION_CERTIFICATE }}
          macos_p12_password: ${{ secrets.APPLE_P12_PASSWORD }}
          macos_codesign_identity: ${{ secrets.MAC_CODESIGN_IDENTITY }}
          macos_notarization_method: apple_id_password
          macos_apple_id: ${{ secrets.APPLE_ID }}
          macos_apple_password: ${{ secrets.APPLE_PASSWORD }}
          macos_apple_team_id: ${{ secrets.APPLE_TEAM_ID }}
```

### Windows

```yaml
jobs:
  build-win32:
    runs-on: self-hosted
    outputs:
      artifact-links: ${{ steps.build.outputs.artifact-links }}
    steps:
      - uses: actions/checkout@v4
      - uses: holepunchto/make-pear-app
        id: build
        with:
          channel: stage
          windows_subject: ${{ secrets.WIN_SUBJECT }}
          windows_thumbprint: ${{ secrets.WIN_THUMBPRINT }}
```

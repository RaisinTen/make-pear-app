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

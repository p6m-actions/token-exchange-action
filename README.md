# P6M Token Exchange Action

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/token-exchange-action?style=flat-square&label=Latest%20Release&color=blue)

## Description

Exchanges a GitHub Actions OIDC token for a P6M YBOR installation token. This action handles the token exchange flow needed to authenticate with P6M platform services from GitHub Actions workflows.

## Usage

```yaml
jobs:
  example:
    runs-on: ubuntu-latest
    permissions:
      id-token: write  # Required for OIDC token
      contents: read
    steps:
      - name: Get P6M Token
        id: token
        uses: p6m-actions/token-exchange-action@v1

      - name: Use token
        env:
          GH_TOKEN: ${{ steps.token.outputs.github_token }}
        run: |
          echo "Token status: ${{ steps.token.outputs.status }}"
```

## Inputs

None. The action uses the following environment variables which must be configured at the organization level:

| Environment Variable | Description |
|---------------------|-------------|
| `PLATFORM_INSTALLATION_ID` | The P6M platform installation ID |
| `PLATFORM_TOKEN_EXCHANGE_URL` | The URL of the token exchange service |

## Outputs

| Output | Description |
|--------|-------------|
| `github_token` | The P6M YBOR installation token |
| `status` | The status of the token exchange |

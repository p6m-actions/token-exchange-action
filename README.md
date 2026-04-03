# P6M Token Exchange

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/token-exchange?style=flat-square&label=Latest%20Release&color=blue)

## Description

Exchanges a GitHub Actions OIDC token for a P6M YBOR installation token. This action handles the token exchange flow needed to authenticate with P6M platform services from GitHub Actions workflows.

When `set_env` is enabled (the default), the action overwrites `GITHUB_TOKEN` and `GH_TOKEN` in the environment with the exchanged token. The original workflow token is preserved in `__GITHUB_TOKEN__` and `__GH_TOKEN__`, and is always available via `${{ github.token }}` which is immutable.

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
        uses: p6m-actions/token-exchange@v2

      - name: Use token
        run: |
          # GITHUB_TOKEN and GH_TOKEN are automatically set to the exchanged token
          gh repo list
```

### Accessing the original token

After the exchange, the original workflow token is available in three ways:

```yaml
      - name: Use original token
        run: |
          # Via the preserved environment variables
          echo "$__GITHUB_TOKEN__"
          echo "$__GH_TOKEN__"

      - name: Use original token via context
        env:
          ORIGINAL: ${{ github.token }}
        run: |
          # ${{ github.token }} always retains its original value
          echo "$ORIGINAL"
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `platform_installation_id` | The P6M platform installation ID | No | Resolved from GitHub context |
| `platform_token_exchange_url` | The URL of the token exchange service | No | `https://auth.p6m.dev/api/github/actions/token-exchange` |
| `set_env` | Whether to set `GITHUB_TOKEN` and `GH_TOKEN` in the environment | No | `true` |
| `show_summary` | Whether to output token exchange results to `GITHUB_STEP_SUMMARY` | No | `true` |

## Outputs

| Output | Description |
|--------|-------------|
| `github_token` | The P6M YBOR installation token |
| `status` | The status of the token exchange |

## Environment Variables

After the action runs with `set_env: true`:

| Variable | Value |
|----------|-------|
| `GITHUB_TOKEN` | Exchanged P6M installation token |
| `GH_TOKEN` | Exchanged P6M installation token |
| `__GITHUB_TOKEN__` | Original workflow token |
| `__GH_TOKEN__` | Original workflow token |

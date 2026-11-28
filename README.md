# August Cyber GitHub Action (`augstai/august-action`)

Autonomous pull-request authorization, API boundary, and MCP security gate powered by [August Cyber](https://august.so) (`august` CLI `v0.6.0`).

Diffs routes and MCP tool handlers changed in a pull request, probes cross-account access across two test actors against a local build or a scope-locked August Sandbox, emits inline GitHub annotations, and fails the check (`exit 2`) if real cross-tenant exposure is reproduced.

## Features

- **Route and MCP tool diffing:** Maps changed API routes, auth checks, row-level security policies, and Model Context Protocol (MCP) tool handlers from the pull request diff.
- **Proof-driven verdicts:** Enforces `fixed`, `reproduced`, or `inconclusive` verdicts using real HTTP traffic across two actors so scanner noise never blocks merges and broken controls never pass silently.
- **Local or cloud sandbox execution:** Run against a local loopback server (`address: 127.0.0.1:3000`) or an authorized staging URL inside a domain-verified, scope-locked August Sandbox (`sandbox`).
- **Zero data retention:** Customer prompts, source code, and findings are never used to train models.

## Usage

```yaml
name: August Cyber Gate
on:
  pull_request:
    branches: [main]

jobs:
  red-team-gate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: augstai/august-action@v1
        with:
          api-key: ${{ secrets.AUGUST_API_KEY }}
          base: origin/main
          address: 127.0.0.1:3000
          actor-a: "Authorization: Bearer ${{ secrets.STAGING_USER_A }}"
          actor-b: "Authorization: Bearer ${{ secrets.STAGING_OWNER_B }}"
```

## Links

- Website: [august.so](https://august.so)
- SOC 2 Penetration Testing: [august.so/soc2-pentest](https://august.so/soc2-pentest)
- MCP & AI Agent Security: [august.so/mcp-security](https://august.so/mcp-security)
- CLI Installer: `curl -fsSL https://api.august.so/install | sh`

# GitHub Action for Tailscale ACLs

This GitHub action lets you manage your [Tailscale ACL
rules](https://tailscale.com/kb/1018/acls/) using a
[GitOps](https://about.gitlab.com/topics/gitops/) workflow. With this GitHub
action you can automatically manage your Tailscale ACLs using a git repository
as your source of truth. 

## Inputs

### `tailnet`

### `api-key`

### `policy-file`

### `action`

## Example Usage

```yaml
name: Tailscale ACL syncing

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  acls:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Deploy ACL
        if: github.event_name == 'push'
        env:
          TS_API_KEY: ${{ secrets.TS_API_KEY }}
          TS_TAILNET: ${{ secrets.TS_TAILNET }}
        run: |
          ~/go/bin/gitops-pusher --policy-file ./policy.hujson apply
      - name: ACL tests
        if: github.event_name == 'pull_request'
        env:
          TS_API_KEY: ${{ secrets.TS_API_KEY }}
          TS_TAILNET: ${{ secrets.TS_TAILNET }}
        run: |
          ~/go/bin/gitops-pusher --policy-file ./policy.hujson test
```

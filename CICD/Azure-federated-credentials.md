# Workflow az login with federated credentials

Azure applications lets you use a federated credential to authenticate with the app within a github workflow

Example subject: `repo:bortington/github-common:environment:test`

Example workflow:
```yaml
permissions:
  id-token: write
  contents: read
  issues: write
  pull-requests: write
  
jobs:
  build:
    environment: prod
    runs-on: ubuntu-latest
    steps:
      - name: Azure Login
        uses: azure/login@v1
        env:
          ARM_USE_OIDC: true
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
```

Not use of `ARM_USE_OIDC` and `permissions.id-token`. Those are necessary.

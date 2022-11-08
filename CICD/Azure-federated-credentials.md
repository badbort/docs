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

Note use of `ARM_USE_OIDC` and `permissions.id-token` are necessary.

The three secrets are also non-sensitive.

`client-id` is assigned to the azure application (client) id found in the application overview
`tenant-id` can also be found in the application overview

If you just created an application and service principle you'll also need to provide it with access to your subscription to manage resources. Otherwise you will get the following error:
```
Error: : No subscriptions found for ***.

Error: Az CLI Login failed. Please check the credentials and make sure az is installed on the runner. For more information refer https://aka.ms/create-secrets-for-GitHub-workflows
```

For simplicity you can add the `Contributor` role to your service principle on your subscription. This is done within `Access control (IAM)` in the sub blade.

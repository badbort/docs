# Pulumi

I use pulumi with an azure backend, with a keyvault key as the secret provider. This can be achieved with a github action like so:

```yaml
      - name: Pulumi refresh
        uses: pulumi/actions@v6
        with:
          command: ${{ github.event_name == 'pull_request' && 'preview' || 'up' }}
          stack-name: azurekeyvault://kv-badbort-pulumi-pw.vault.azure.net/keys/<example-key>
          cloud-url: azblob://${{vars.BACKEND_CONTAINER_NAME }}?storage_account=${{vars.BACKEND_STORAGE_ACCOUNT }}
          secrets-provider: ${{ env.PULUMI_SECRETS_PROVIDER }}
          config-map: |
            data_dir:
              value: ${{ github.workspace }}/data
            resource_group_name:
              value: ${{ vars.FRONTDOOR_RESOURCE_GROUP }}
            endpoint_name:
              value: ${{ vars.FRONTDOOR_ENDPOINT_NAME }}
          comment-on-pr: true
          comment-on-summary: true
          always-include-summary: true
          diff: true
          upsert: true
```

For key creation you only need key_ops `["encrypt", "decrypt"]`

Note how configuration is performed in the above with a yaml string value passed to `config-map`.

To get started with a new pulumi infra repo I have to do the following:
- Set `$env:AZURE_KEYVAULT_AUTH_VIA_CLI="true"` - This is necessary for pulumi to use az cli for keyvault operations, otherwise it fails and it'll revert to passphrase.
- Run `pulumi stack init main --secrets-provider azurekeyvault://kv-badbort-pulumi-pw.vault.azure.net/keys/<key>`. You, and the CICD identity need `Key Vaults Crypto User`
- Commit and push the new stack file. e.g. Pulumi.main.yaml`.
- This will allow the pulumi action to succeed.

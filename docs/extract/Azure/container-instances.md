# Azure Container Instances

[Azure Container Instances](https://azure.microsoft.com/en-us/products/container-instances) is a platform to run serverless containers with hypervisor isolation.

| Property      | Value                          |
| ----------- | ------------------------------------ |
| Credentials delivery mechanism       | [Served through link-local metadata service](../types-of-credentials-delivery.md#served-through-link-local-metadata-service)  |
| Protection type       | Header requires hardcoded value |
| Exploitation primitives    | `Query arbitrary URL with arbitrary HTTP header`|

## Extracting credentials

You can call the metadata service at `169.254.169.254`. It requires the `Metadata` HTTP header to be set to `true`.

### Extracting credentials for a system-assigned managed identity

```bash
curl -H "Metadata: true" \
    "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.core.windows.net"
```

### Extracting credentials for a user-assigned managed identity

In this case, you need to know the client ID associated with the user-assigned managed identity. The application needs this value to function, so it should typically be available as an environment variable or in a configuration file.

```bash
CLIENT_ID=...

```bash
curl -H "Metadata: true" \
    "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.core.windows.net&client_id=$CLIENT_ID"
```

### Selecting the `resource` parameter to use

The choice of the `resource` parameter value to use depends on the subsequent APIs you want to try calling. The metadata service returns a JWT with the audience (`aud`) set to this `resource` parameter.

Known valid values and their associated API documentation:

| Resource URL      | Description                           |
| ----------- | ------------------------------------ |
| `https://management.azure.com/`       |  [Azure REST API](https://learn.microsoft.com/en-us/rest/api/azure/) (AzureRM) |
| `https://management.core.windows.net/`       | [Azure REST API](https://learn.microsoft.com/en-us/rest/api/azure/) (Azure Classic) |
| `https://vault.azure.net/`    | [Azure Key Vault REST API](https://learn.microsoft.com/en-us/rest/api/keyvault/?view=rest-keyvault-secrets-7.4) |
| `https://servicebus.windows.net/`    | [Service Bus REST API (data plane)](https://learn.microsoft.com/en-us/rest/api/servicebus/service-bus-runtime-rest?view=rest-servicebus-controlplane-2024-01-01) |
| `https://batch.core.windows.net/`    | [Azure Batch REST API (data plane)](https://learn.microsoft.com/en-us/rest/api/batchservice/?view=rest-servicebus-controlplane-2024-01-01) |
| `https://storage.azure.com`    | [Azure Storage REST API (data plane)](https://learn.microsoft.com/en-us/rest/api/storageservices/) |
| `https://managedhsm.azure.net/` | [Azure Managed HSMs REST API](https://learn.microsoft.com/en-us/azure/key-vault/general/about-keys-secrets-certificates) |
| `https://graph.microsoft.com`    | [Microsoft Graph API](https://learn.microsoft.com/en-us/graph/overview) |
| `https://graph.windows.net/`    | [Azure AD Graph API](https://learn.microsoft.com/en-us/graph/migrate-azure-ad-graph-overview) (deprecated) |

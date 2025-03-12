# How to use Google Cloud tokens

| Credentials type      | Description                          |
| ----------- | ------------------------------------ |
| [Service account keys](#long-lived-service-account-keys)       | Keys manually created on a service account |
| [Short-lived OAuth 2.0 access token](#short-lived-oauth-20-access-token)      | Access key ID (`AKIA*`), Secret access key  |

## Long-lived service account keys

Service account keys are JSON files that embed a private cryptographic key.

```json
{
  "type": "service_account",
  "project_id": "project",
  "private_key_id": "8766d7e8e4bdb54bd124446411e0383216db8c17",
  "private_key": "-----BEGIN PRIVATE KEY-----SNIP-----END PRIVATE KEY-----\n",
  "client_email": "service-account@project.iam.gserviceaccount.com",
  "client_id": "113171359947474921431",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/service-account%40sproject.iam.gserviceaccount.com",
  "universe_domain": "googleapis.com"
}
```

### Using a long-lived service account key with `gcloud`

To use a service account key, you can set the 

```bash
export CLOUDSDK_AUTH_CREDENTIAL_FILE_OVERRIDE=key.json
```

Alternatively, you can use [`gcloud auth activate-service-account`](https://cloud.google.com/sdk/gcloud/reference/auth/activate-service-account) to store it on disk persistently

```bash
gcloud auth activate-service-account service-account@project.iam.gserviceaccount.com --key-file key.json --project project
```

You can then use any `gcloud` command.

### Manually using a long-lived service account key

You can also use a long-lived service account key to manually perform requests to the [Google Cloud API](https://cloud.google.com/apis). As this involved generating a specific JWT signed by the private key embedded in the long-lived service account key file, the easiest is to leverage existing functionality in `gcloud auth print-access-token`:

```bash
export CLOUDSDK_AUTH_CREDENTIAL_FILE_OVERRIDE=key.json
gcloud auth print-access-token
```

You can then refer to the [Short-lived OAuth 2.0 access token](#short-lived-oauth-20-access-token) for information on how to use the resulting access token to call the Google Cloud API.

## Short-lived OAuth 2.0 access token

Google Cloud temporary access tokens are a 60-character strings that start with `ya29`. They are **opaque** tokens that you cannot directly decode.

### Verifying the validity of a short-lived access token

You can use the `tokeninfo` endpoint to retrieve information about a temporary access token:

```bash
curl "https://oauth2.googleapis.com/tokeninfo?access_token=$TOKEN"
```

This will return information about the token, including the service account it was generated for.

<div class="annotate" markdown>
```json
{
  "azp": "113143471284683414939",
  "aud": "113143471284683414939",
  "scope": "https://www.googleapis.com/auth/cloud-platform ...",  // (1)!
  "exp": "1741814018", // (2)!
  "expires_in": "1780",
  "email": "111111111111-compute@developer.gserviceaccount.com", // (3)!
  "email_verified": "true",
  "access_type": "online"
}
```
</div>

1.  Authorization [scopes](https://developers.google.com/identity/protocols/oauth2/scopes) limiting effective permission
2.  Expiration timestamp
3.  Email of the associated service account. Here, we're looking at the [Compute Engine default service account](https://cloud.google.com/compute/docs/access/service-accounts#default_service_account). `111111111111` is the Google Cloud project's numerical ID.

### Using a short-lived access token

You can use the access token in manual requests to the [Google Cloud API](https://cloud.google.com/apis) by inserting it in the `Authorization` HTTP header.

For instance, you can [list Cloud Storage buckets](https://cloud.google.com/storage/docs/json_api/v1/buckets/list) in the project by running:

```bash
PROJECT=111111111111 # Project name or numerical ID, that you can retrieve through the 'tokeninfo' endpoint
curl "https://storage.googleapis.com/storage/v1/b?project=$PROJECT" -H "Authorization: Bearer $TOKEN"
```

You can use the `$CLOUDSDK_AUTH_ACCESS_TOKEN` environment variable to use the `gcloud` CLI:

```bash
export CLOUDSDK_AUTH_ACCESS_TOKEN=$TOKEN

# Specify the project ID or project name (you only need one)
export CLOUDSDK_CORE_PROJECT_ID=111111111111
export CLOUDSDK_CORE_PROJECT=my-project
```

You can then use any `gcloud` command.

## References

- https://cloud.google.com/docs/authentication/token-types#access
- https://securitylabs.datadoghq.com/articles/google-cloud-default-service-accounts/
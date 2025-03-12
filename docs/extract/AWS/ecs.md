# Amazon ECS

Amazon ECS is a managed container orchestration platform.

| Property      | Value                          |
| ----------- | ------------------------------------ |
| Credentials delivery mechanism       | [Served through link-local metadata service](../types-of-credentials-delivery.md#served-through-link-local-metadata-service)  |
| Protection type       | URL path requires value from environment |
| Exploitation primitives    | `Query arbitrary URL` and `Read environment variables` |

## Extracting credentials

The metadata service is available at `169.254.170.2`, and the URL path to query is in the environment variable `$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI`.

```bash
curl http://169.254.170.2$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI
```

## References

- https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html
- https://docs.aws.amazon.com/AmazonECS/latest/developerguide/security-iam-roles.html#security-iam-task-role
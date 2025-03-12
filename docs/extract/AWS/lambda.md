# AWS Lambda

AWS Lambda is a serverless service that can run containers and traditional applications.

| Property      | Value                          |
| ----------- | ------------------------------------ |
| Credentials delivery mechanism       | [Injected at runtime through environment variables](../types-of-credentials-delivery.md#injected-at-runtime-through-environment-variables)  |
| Protection type       | N/A |
| Exploitation primitives    | `Read environment variables` |



## Extracting credentials

When the Lambda runtime invokes a function, it assumes the Lambda execution role attached to the function, then injects `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN` into environments variables of the function.

## References

- https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html
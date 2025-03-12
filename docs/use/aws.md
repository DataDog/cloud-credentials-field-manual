# How to use AWS credentials

| Credentials type      | Components                          |
| ----------- | ------------------------------------ |
| [Long-lived IAM user credentials](#long-lived-iam-user-credentials)      | Access key ID (`AKIA*`), Secret access key  |
| [Short-lived STS credentials](#short-lived-sts-credentials)       | Access key ID (`ASIA*`), Secret access key, Session token |

## Long-lived IAM user credentials

Export the credentials in your environment:

```bash
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=...
```

You can then use any tool using the AWS SDKs, such as the AWS CLIs or most of existing offensive tooling.

## Short-lived STS credentials

Export the credentials in your environment:

```bash
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=...
export AWS_SESISON_TOKEN=...
```

You can then use any tool using the AWS SDKs, such as the AWS CLIs or most of existing offensive tooling.

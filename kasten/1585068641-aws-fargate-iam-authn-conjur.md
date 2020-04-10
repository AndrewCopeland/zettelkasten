# 1585068641 aws-fargate-iam-authn-conjur
#authn-iam #conjur #ecs #fargate

How to obtain the Access Key, Secret Key and Token is mentioned here: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html

The container should have an environment variable set called `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI`


## Retrieving access key, secret access key and token
To obtain the AWS credentials perform the following command from the ECS instance:

### Bash
```bash
curl 169.254.170.2$AWS_CONTAINER_CREDENTIALS_RELATIVE_URI
# below is the response
{
    "AccessKeyId": "ACCESS_KEY_ID",
    "Expiration": "EXPIRATION_DATE",
    "RoleArn": "TASK_ROLE_ARN",
    "SecretAccessKey": "SECRET_ACCESS_KEY",
    "Token": "SECURITY_TOKEN_STRING"
}
```


## Links
- [1584721584-conjur-authn-iam.md](1584721584-conjur-authn-iam.md)

# Get Terraform to assume a role

In terraform, you need to give it AWS credentials.

Ieally it should have its own role to use which has the relevent permissions. However if you're using your own AWS account to run Terraform and/or you need to assume into a role, this is how to get Terraform to assume into that role.

## 1) Generate a token

Use the command below to generate an AWS token.

### Not 2FA

```console
aws sts get-session-token --duration-seconds 012345
```

### Have 2FA?

```console
aws sts get-session-token <arn of 2fa device> --token-code 012345 --duration-seconds 012345
```

## Output

Will look something like this:

```json
{
    "Credentials": {
        "AccessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
        "SessionToken": "AQoEXAMPLEH4aoAH0gNCAPyJxz4BlCFFxWNE1OPTgk5TthT+FvwqnKwRcOIfrRh3c/LTo6UDdyJwOOvEVPvLXCrrrUtdnniCEXAMPLE/IvU1dYUg2RVAJBanLiHb4IgRmpRV3zrkuWJOgQs8IZZaIv2BXIa2R4OlgkBN9bkUDNCJiBeb/AXlzBBko7b15fjrBs2+cTQtpZ3CYWFXG8C5zqx37wnOE49mRl/+OtkIKGO7fAE",
        "Expiration": "2020-05-19T18:06:10+00:00"
    }
}
```

## 2) Give these to Terraform

Fill in the blanks using your prefered method from the output from above.

`role_arn` is the ARN of the role you want to assume into. `session_name` will appear in Cloudtrail event logs if you need to chase down Terraforms actions.

```terraform
provider "aws" {
    region = us-east-1
    access_key = "xx"
    secret_key = "xx"

    token      = "xx"

    assume_role {
        role_arn     = "xx"
        session_name = "xx"
    }
}
```

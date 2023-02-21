## Terraform Introduction

### Setup

- Create a new user in [IAM](https://us-east-1.console.aws.amazon.com/iamv2/home?region=ap-southeast-2#/users)
- Select "Attach policies directly" and select "AdministratorAccess"
- Selected the created user and go to the "Security credentials" tab. 
- Under "Access Keys" select "Create access key"
- Select "Command Line Interface (CLI)"
- Scroll to the bottom of the page and check "I understand" and select next
- Give it a name
- Setup aws credentials file with the generated key under a profile for devops:

`~/.aws/credentials`

```
[devops]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

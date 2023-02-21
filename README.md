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

- Run `terraform init` to install the providers in `providers.tf`

### What Terraform does

```
                               xxxxxxxxx
                             xxxx       xxx
                             x           xxx
                           xxxx S3 Bucket   xx
                           x                x
                           xxxxx     xxxxxxx
                               xxxxx▲
                                    │
                                    │
                          ┌─────────┴─────────┐
                          │                   │
                          │ Provider eg. AWS  │
                          │                   │
                          └───────▲──┬────────┘
                                  │  │
                                  │  │
    ┌─────────────────┐    ┌──────┴──▼────────┐     ┌─────────┐
    │                 │    │                  │     │         │
    │  Configuration  │    │                  ├─────►         │
    │                 ├────►    Terraform     │     │  State  │
    │   HCL/JSON      │    │                  ◄─────┤         │
    │                 │    │                  │     │         │
    └─────────────────┘    └──────────────────┘     └─────────┘

```


### Providers

The plugins which allows terraform to work with resources, eg. AWS S3 bucket



### HCL

Hashicorp Configuration Language makes it quicker to write configuration. The following are equivalent:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}
```

```json
{
  "terraform": [
    {
      "required_providers": [
        {
          "aws": [
            {
              "source": "hashicorp/aws",
              "version": "~> 4.0"
            }
          ]
        }
      ]
    }
  ]
}
```

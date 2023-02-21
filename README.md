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

The plugins which allows terraform to work with resources, eg. AWS S3 bucket.
You can see documentation for most providers in the [Terraform Registry](https://registry.terraform.io/)

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

### Resources

A resource has a type "aws_s3_bucket" and a unique name eg "devops"

```hcl
resource "aws_s3_bucket" "devops" {
   bucket = "my-super-duper-unique-bucket-name"
}
```

- Add this to a filename you like eg. `resources.tf`
- Run `terraform plan` to see the changes
- Run `terraform apply` to see the changes
- It will create an S3 bucket
- Delete the lines above and run `terraform apply`
- It will delete the bucket

### State

How does Terraform "remember" what it needs to add/remove when applying the configuration?
It stores the current state in a state file, which is a JSON representation of what it has created.
By default it creates a local state file called `terraform.tfstate` but there are other backends eg. S3 bucket for when you work in a team.

### Variables

You can make your code more configurable by adding variables:

```hcl
variable "bucket_name" {
  type = string
  default = "my-super-duper-unique-bucket-name"
}

resource "aws_s3_bucket" "devops" {
   bucket = var.bucket_name
}
```

You can now override the value from the command line eg.

```bash
terraform apply -var="bucket_name=bucket-name-set-from-the-cli"
```

### Outputs

Resources have outputs which can be used in other resources eg. `aws_s3_bucket.devops.arn`.
This resource writes a file to the local directory
Note: you will need to run `terraform init` to download the "local_file" provider

```hcl
resource "local_file" "bucket_name" {
  content  = "The bucket arn is: ${aws_s3_bucket.devops.arn}"
  filename = "bucketarn.txt"
}
```


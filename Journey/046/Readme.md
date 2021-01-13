
# Provisioning AWS VPC Using Terraform

## Introduction

Today I provisioned an AWS VPC using Terraform. Practice makes perfect!

## Prerequisite

I used the aws configure process (as recommended by the official Terraform tutorial) so that I was not hard coding credentials into the my code. I suggest you do the same as well.

## Terraform Code

```
provider "aws" {
            region = "us-east-1"
        }

resource "aws_vpc" "terraformvpc" {
    cidr_block = "192.160.0.0/16"
    instance_tenancy = "default"
    tags = {
        Name = "terraformvpc"
        Location = "WestbrookCT"
    }
}

resource "aws_subnet" "terraformvpcsubnet1" {
    vpc_id = "${aws_vpc.terraformvpc.id}"
    cidr_block = "192.160.1.0/24"
    tags = {
        Name = "terraformvpcsubnet1"
    }
}
```

## Next Steps

1) Land a cloud role (top tier priority)
    - review cloud technical interview questions daily, practice better answers, get ready for the next interview rounds
    - contact hiring managers and send out resumes daily
    - continue Aaron Brooks' free devops bootcamp assignments
2) Earn meaningful professional certs (mid tier priority)
    - Terraform associate (1hr practice per day)
    - CKA (postponed until step one is complete)
3) Daily Practice (mid tier priority)
    - daily python
    - #100DaysOfCloud daily documenting
4) Complete AWS projects (mid tier)
    - serverless
    - ansible w/ ec2
5) Complete ci/cd pipeline proj (low tier)
    - with my current technical interview experience, no questions were asked about ci/cd or jenkins. This is now a low tier priority.

## Social Proof

[Tweet](https://twitter.com/lrnallday/status/1349329241628942343)

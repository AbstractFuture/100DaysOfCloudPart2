
# Provisioning AWS VPC & S3 Bucket With Terraform

## Introduction

Today I added an additional snippet of terraform code which provisions the default VPC for AWS. Simple but cool. Looking forward to more complex projects down the line.

## Prerequisite

I used the aws configure process (as recommended by the official Terraform tutorial) so that I was not hard coding credentials into the my code.

## Try yourself

Below is the Terraform code I used to provision my S3 bucket and VPC. 

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.70"
    }
  }
}

provider "aws" {
  profile = "default"
  region  = "us-east-1"
}

resource "aws_s3_bucket" "production-bucket-test" {
  bucket    = "production-bucket-test"
  acl       = "private"
  versioning {
      enabled = true
  }
  tags      = {
      Name        = "production_bucket_test"
      Environment = "Prod"
  }
}

resource "aws_default_vpc" "default" {}
```


## Next Steps

1) Land a cloud role (top tier priority)
    - review cloud technical interview questions daily, practice better answers, get ready for the next interview rounds
    - contact hiring managers and send out resumes daily
    - begin Aaron Brooks' free devops bootcamp assignments
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

[Tweet]()
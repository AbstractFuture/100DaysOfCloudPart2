
# Provisioning AWS S3 Bucket With Terraform

## Introduction

Practice makes perfect. Today, like the title explicitly says, I provisioned an AWS s3 bucket using terraform on my command line.

## Prerequisites

I used the aws configure process (as recommended by the official Terraform tutorial) so that I was not hard coding credentials into the my code.

## Try It Yourself

Below is the Terraform code I used to provision my S3 bucket. 

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
  region  = "us-west-2"
}

resource "aws_s3_bucket" "b" {
  bucket    = "terraform-test-bucket"
  acl       = "private"
  versioning {
      enabled = true
  }
  tags = {
      Name        = "My_tf_bucket"
      Environment = "Dev"
  }
}
```

## Next Steps

1) Land a cloud role (top tier priority)
    - review cloud technical interview questions daily, practice better answers, get ready for the next interview rounds
    - contact hiring managers and send out resumes daily
    - begin Aaron Brooks' free devops bootcamp
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
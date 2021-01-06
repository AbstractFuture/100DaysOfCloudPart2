
# Terraform Provisioning: Web Server & Application Load Balancer On AWS

## Introduction

I've added onto the previous day and have successfully provisioned an nginx web server on AWS. Still working through getting the ALB properly configured.

## Prerequisite

I used the aws configure process (as recommended by the official Terraform tutorial) so that I was not hard coding credentials into the my code.

## Try yourself

Full disclosure: I'm still working through the subnet ALB configuration. 

That being said, if you want to provision the web server without the application load balancer then simply comment that code out and then apply this terraform configuration.

If you manage to get this ALB running then please let me know which settings you've altered! 

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

# provisions private AWS S3 bucket
resource "aws_s3_bucket" "production-bucket-test" {
  bucket    = "production-bucket-test"
  acl       = "private"
  versioning {
      enabled = true
  }
  tags = {
      Name        = "production_bucket_test"
      Environment = "Prod"
  }
}

# use the default VPC
resource "aws_default_vpc" "default" {}

# creates security group for web server
# allows http and https traffic inbound and outbound
resource "aws_security_group" "prod-web" {
  name        = "prod-web"
  description = "This is going to allow http and https ports inbound and all outbound"
  
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 443
    to_port   = 443
    protocol  = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    "Terraform" : "true"
  }
}

# provisions nginx web server
resource "aws_instance" "prod-web" {
  ami                    = "ami-0708a0921e5eaf65d"
  instance_type          = "t2.nano"
  
  vpc_security_group_ids = ["${aws_security_group.prod-web.id}"]
  
  tags = {
    "Terraform" : "true"
  }
}

# provisions elastic IP for web server
resource "aws_eip" "prod-web" {
  instance = "${aws_instance.prod-web.id}"
  tags = {
    "Terraform" : "true"
  }
}


# declaring default subnets as a resource
# from https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_subnet
# unsure if I need to specify an additional default subnet so that terraform can provision the AWS ALB

resource "aws_default_subnet" "default_az1" {
  availability_zone = "us-east-1a"

  tags = {
    Name = "Default subnet for us-east-1a"
  }
}

# application load balancer 
# from https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lb

resource "aws_lb" "prod-web" {
  name               = "prod-web"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.prod-web.id]
  subnets            = aws_subnet.public.*.id # what does this param do?

  enable_deletion_protection = true

  #access_logs { # optional param
  #  bucket  = aws_s3_bucket.lb_logs.bucket
  #  prefix  = "test-lb"
  #  enabled = true
  #}

  tags = {
    "Terraform" = "true"
  }
}
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

[Tweet](https://twitter.com/lrnallday/status/1346797936227848192)


# Provisioning AWS VPC w/ 4 Subnets Using Terraform

## Introduction

Last time on day46 I provisioned a basic vpc on AWS using terrform but today I decided to expand on my previous attempt and set up a vpc with 4 subnets.

I first manually set this up in the AWS console so I could recall all the settings and then googled for the correct TF syntax.

The code works! If you choose to run this code please remember to destroy your resources afterwards.

## Terraform Code

```
# provider block
provider "aws" {
region     = "us-east-1"
}


# creating vpc
resource "aws_vpc" "terraform_vpc" {
  cidr_block       = "10.10.0.0/16"
  tags = {
    Name = "terraform_vpc"
  }
}

# create internet gateway 
resource "aws_internet_gateway" "terraform_igw" {
 vpc_id = "${aws_vpc.terraform_vpc.id}"
 tags = {
    Name = "terraform_igw"
 }
}

# create elastic ip
resource "aws_eip" "eip" {
  vpc=true
}

# create subnet
# data block reference: https://www.terraform.io/docs/configuration/data-sources.html
data "aws_availability_zones" "azs" {
  state = "available"
}

# create public subnets
resource "aws_subnet" "terraform-public-subnet-1a" {
  availability_zone = "${data.aws_availability_zones.azs.names[0]}"
  cidr_block        = "10.10.20.0/24"
  vpc_id            = "${aws_vpc.terraform_vpc.id}"
  map_public_ip_on_launch = "true"
  tags = {
   Name = "terraform-public-subnet-1a"
   }
}

resource "aws_subnet" "terraform-public-subnet-1b" {
  availability_zone = "${data.aws_availability_zones.azs.names[1]}"
  cidr_block        = "10.10.21.0/24"
  vpc_id            = "${aws_vpc.terraform_vpc.id}"
  map_public_ip_on_launch = "true"
  tags = {
   Name = "terraform-public-subnet-1b"
   }
}

# create private subnets
resource "aws_subnet" "terraform-private-subnet-1a" {
  availability_zone = "${data.aws_availability_zones.azs.names[0]}"
  cidr_block        = "10.10.30.0/24"
  vpc_id            = "${aws_vpc.terraform_vpc.id}"
  tags = {
   Name = "terraform-private-subnet-1a"
   }
}

resource "aws_subnet" "terraform-private-subnet-1b" {
  availability_zone = "${data.aws_availability_zones.azs.names[1]}"
  cidr_block        = "10.10.31.0/24"
  vpc_id            = "${aws_vpc.terraform_vpc.id}"
  tags = {
   Name = "terraform-private-subnet-1b"
   }
}

# create nat gateway
resource "aws_nat_gateway" "terraform-ngw" {
  allocation_id = "${aws_eip.eip.id}"
  subnet_id = "${aws_subnet.terraform-public-subnet-1b.id}"
  tags = {
      Name = "terraform nat gateway"
  }
}

# routing
resource "aws_route_table" "terraform-public-route" {
  vpc_id =  "${aws_vpc.terraform_vpc.id}"
  route {
      cidr_block = "0.0.0.0/0"
      gateway_id = "${aws_internet_gateway.terraform_igw.id}"
  }

   tags = {
       Name = "terraform-public-route"
   }
}

resource "aws_default_route_table" "terraform-default-route" {
  default_route_table_id = "${aws_vpc.terraform_vpc.default_route_table_id}"
  tags = {
      Name = "terraform-default-route"
  }
}

# subnet associations
resource "aws_route_table_association" "arts1a" {
  subnet_id = "${aws_subnet.terraform-public-subnet-1a.id}"
  route_table_id = "${aws_route_table.terraform-public-route.id}"
}

resource "aws_route_table_association" "arts1b" {
  subnet_id = "${aws_subnet.terraform-public-subnet-1b.id}"
  route_table_id = "${aws_route_table.terraform-public-route.id}"
}

resource "aws_route_table_association" "arts-p-1a" {
  subnet_id = "${aws_subnet.terraform-private-subnet-1a.id}"
  route_table_id = "${aws_vpc.terraform_vpc.default_route_table_id}"
}

resource "aws_route_table_association" "arts-p-1b" {
  subnet_id = "${aws_subnet.terraform-private-subnet-1b.id}"
  route_table_id = "${aws_vpc.terraform_vpc.default_route_table_id}"
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

[Tweet](https://twitter.com/lrnallday/status/1350855087409815552)

# Launching EC2 Instance In Custom Private Subnet

## Introduction

This is a follow up to the previous day's code. 

## Terraform Code Snippet

Add the following code to day49's configuration to launch an instance in the specified private subnet.

```
# launch ec2 instance
# https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance
resource "aws_instance" "terraform-ec2" {
  ami = "ami-0708a0921e5eaf65d"
  instance_type          = "t2.nano"
  #vpc_id = "${aws_vpc.terraform_vpc.id}"
  subnet_id = "${aws_subnet.terraform-private-subnet-1a.id}"

  tags = {
    Name = "terraform ec2 instance"
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

[Tweet](https://twitter.com/lrnallday/status/1351306883999674368)

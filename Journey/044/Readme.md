
# Moving Local Terraform State To AWS S3 Bucket

## Introduction

So how exactly does a team of infrastructure engineers know they're working with the latest infrastructure if they're all distributed? A centralized backend. 

In this example, we'll use Terraform to provision an AWS s3 bucket, then use the newly created s3 bucket to store the local terraform state file on our computer, set up a dynamodb table lock to prevent several people overwriting the file at once, and then destroy the resources so you aren't billed. 

Sound good? Good. 

## Prerequisite

I used the aws configure process (as recommended by the official Terraform tutorial) so that I was not hard coding credentials into the my code. I suggest you do the same as well.

## Creating The S3 Bucket

Create a new folder and new file for this configuration:

```
provider "aws" {
    region = "us-east-1"
}

resource "aws_s3_bucket" "terraform-test-bucket1" {
    bucket = "terraform-test-bucket1"
    acl    = "private"
    # line below will be uncommented when you want to
    # destroy the s3 bucket (holding the state) and its contents
    # and the dynamodb table
    #force_destroy = true
    
    versioning {
        enabled = true
    }
    tags = {
        Name        = "My tf-backend bucket"
        Environment = "Dev"
    }
}

```

Then;
```
$ terraform init
$ terraform apply
```

Congrats! Your s3 bucket is provisioned

## Using S3 To Hold State File

Now that the s3 bucket has been provisioned we can tell terraform to use the s3 bucket to hold the terraform state file. Add the following code snippet to the end of your configuration file.

```
terraform {
    backend "s3" {
        bucket = "terraform-test-bucket1"
        encrypt = true
        key = "terraform.tfstate"
        region = "us-east-1"
        # below line of code to be used once 
        # the dynamodb table was created
        # it is the last step, until then, keep commented out
        #dynamodb_table = "terraform-state-lock-dynamo"
    }
}
```

Whenever we make a change to the backend we need to initialize terraform again.

```
$ terraform init
$ terraform plan
$ terraform apply
```

Great! Now we have a centralized backend that any infrastructure engineer can refer to. 

## Creating The Lock Table

In the same directory as the above configuration, create a new file (I named mine ```create-dynamodb-lock-table.tf``` but choose anything so long as it is descriptive) and paste in the following configuration.

It will set up a dynamodb table that will prevent multiple engineers from overwriting the state of your infrastructure (which would be very bad for you!). 

```
resource "aws_dynamodb_table" "dynamodb-terraform-state-lock" {
    name            = "terraform-state-lock-dynamo"
    hash_key        = "LockID"
    read_capacity   = 20
    write_capacity  = 20
    attribute {
        name = "LockID"
        type = "S"
    }
}
```

Then;
```
$ terraform plan
$ terraform apply
```
Great! You've provisioned the lock table.

## Reconfigure Backend

Remember that line in the backend that I commented out? Uncomment the following line;

```
dynamodb_table = "terraform-state-lock-dynamo"
```

```
$ terraform init # need to init everytime we alter the backend
$ terraform plan
$ terraform apply
```

Great! On to the last step

## Destroying The Resources

In our resource section, uncomment the following line

```
force_destroy = true
```

```
$ terraform plan
$ terraform apply
$ terraform destroy
```

Wait until terraform confirms the resources are destroyed and then verify in you aws console that they are gone.

## Conclusion

You've just completed a real world terraform project. Congrats!

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

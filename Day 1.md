
# Mastering Terraform. Simplifying Cloud Infrastructure Management Across AWS

- CNT(Cloud Native tool) for Aws : CloudFormation
- CNT for azure : Arm templates/Bicep
- CNT for GCP : Deployment manager

## Why Terraform why not Cloud Native tools ?

- In above tools all the configurations need to be dumped in the same file.(Eg: Need to include all the services like ec2,s3,vpc whatever the infra we have we need to use the same file)
- Debugging is bit difficult here as all services are in same file.
- Learning JSON,YAML was bit difficult.
- Importing of resources is complex in aws and this option itself is not there in azure, Gcp.
- There is no  module,workspace concept in aws,azure,gcp..
- Performing dry checks in azure is not there.

## About
- Terraform was owned by Hashicorp
- Along with Terraform Hashicorp has below other tools
- Packer: for image automation
-  vault: Password management

## Practical hands-on

- main.tf: Through this file we are telling terraform to which cloud we are deploying through provider block.

```bash
# Configure the AWS Provider
provider "aws" {
  region = "us-east-1"
}
```
- The lines that are present inside {} are known as arguments.
  


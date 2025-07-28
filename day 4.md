# Terraform EC2 Demo: Static vs Dynamic (`for_each`) Approach

This guide demonstrates two approaches for launching multiple EC2 instances using Terraform:

- **Set 1**: Manual resource blocks
- **Set 2**: Dynamic configuration using `for_each`



## Set 1: WITHOUT `for_each` (Manual Approach)
- Define each EC2 instance separately using individual `aws_instance` resource blocks.

### Folder Structure

```bash
static/
├── main.tf
├── variables.tf
└── outputs.tf
```


- main.tf

```bash
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web1" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "web1"
  }
}

resource "aws_instance" "web2" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.small"
  tags = {
    Name = "web2"
  }
}
```
- variables.tf

```bash
variable "region" {
  default = "us-east-1"
}
```
- outputs.tf

```bash
output "web1_instance_id" {
  value = aws_instance.web1.id
}

output "web2_instance_id" {
  value = aws_instance.web2.id
}

```
- **Disadvantages**
- Repetition: Code is duplicated for each instance.

- Scalability issue: Hard to manage when you need 5, 10, or more instances.

- Maintenance burden: Updating configurations for multiple instances is time-consuming.

## Set 2: WITH for_each (Dynamic Approach)

- Loop over this map and for each key-value pair, create a separate EC2 instance with the specified configuration

Folder Structure

```bash

foreach/
├── main.tf
├── variables.tf
└── outputs.tf
```
- main.tf

```bash
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "ec2" {
  for_each      = var.ec2_config
  ami           = each.value.ami_id
  instance_type = each.value.instance_type

  tags = {
    Name = each.key
  }
}
```
- variables.tf
```bash
variable "ec2_config" {
  description = "Map of EC2 instance configurations"
  type = map(object({
    instance_type = string
    ami_id        = string
  }))
  default = {
    "web1" = {
      instance_type = "t2.micro"
      ami_id        = "ami-0c55b159cbfafe1f0"
    },
    "web2" = {
      instance_type = "t2.small"
      ami_id        = "ami-0c55b159cbfafe1f0"
    }
  }
}
```
- outputs.tf

```bash
output "instance_ids" {
  description = "IDs of the created EC2 instances"
  value = {
    for name, inst in aws_instance.ec2 : name => inst.id
  }
}
```
**Advantages**

- Scalable: Easily add or remove instances by modifying the map.

- Clean & DRY: No code duplication; centralized configuration.

- Dynamic properties: Configure instance types, AMIs, and tags per instance with ease.

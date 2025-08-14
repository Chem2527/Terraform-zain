# Terraform AWS Security Group

## Use Case: Why Use Dynamic Blocks?

In the static approach, each ingress rule must be manually defined, which is fine for few rules but not scalable. Dynamic blocks let you define a list of rules as variables and generate ingress blocks automatically, useful for many similar rules or reusable modules.

---

## Without Dynamic Blocks

In this version, ingress rules are explicitly declared inside the resource. This is simple but not flexible when you want to add or change rules frequently.

### main.tf

```bash
resource "aws_security_group" "sg" {
  name        = "dev-SG"
  description = "Example security group"
  vpc_id      = var.vpcid

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name        = "dev-sg"
    Environment = "development"
  }
}
```

- variables.tf
```bash
variable "vpcid" {
  description = "The ID of the VPC where the security group will be created."
  type        = string
  default     = "vpc-0b4fd04d01c1b83fa"
}
```
- outputs.tf
```bash
output "sg_name" {
  value = aws_security_group.sg.tags_all["Name"]
}
```
# With Dynamic Blocks
- This version uses a variable list of ingress rules and a dynamic block inside the resource. It allows easy scaling and reuse of the security group with different ingress rules.

```bash
resource "aws_security_group" "sg" {
  name        = var.tags["Name"]
  description = "Example security group"
  vpc_id      = var.vpcid

  dynamic "ingress" {
    for_each = var.ingress_rules
    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }

  tags = var.tags
}
```
- variables.tf

```bash
variable "vpcid" {
  description = "VPC ID"
  type        = string
  default     = "vpc-0b4fd04d01c1b83fa"
}

variable "ingress_rules" {
  type = list(object({
    from_port   = number
    to_port     = number
    protocol    = string
    cidr_blocks = list(string)
  }))
  default = [
    { from_port = 80,  to_port = 80,  protocol = "tcp", cidr_blocks = ["0.0.0.0/0"] },
    { from_port = 443, to_port = 443, protocol = "tcp", cidr_blocks = ["0.0.0.0/0"] },
    { from_port = 22,  to_port = 22,  protocol = "tcp", cidr_blocks = ["0.0.0.0/0"] }
  ]
}

variable "tags" {
  type = map(string)
  default = {
    Name        = "dev-sg"
    Environment = "development"
  }
}
```
- outputs.tf

```bash
output "sg_name" {
  value = aws_security_group.sg.tags_all["Name"]
}
```

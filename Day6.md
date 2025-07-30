# Understanding Terraform Import and its importance 
- 1. Prepare your Terraform configuration
- main.tf
```bash
resource "aws_instance" "example" {
  
}
```
- initialize
```bash
terraform init
```
- Import
```bash
terraform import aws_instance.example <instance-id>
```
- Verify the imported state
```bash
terraform state list
```
- View details
```bash
terraform state show aws_instance.example
```

- Match the existing infra configs and do the check using
```bash
terraform plan
```




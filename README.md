# Terraform-Training

## Phase 1
 ###  Module 1: Introduction & Setup
-	What is Infrastructure as Code (IaC)?
-	Why Terraform?
-	Install Terraform (Windows/macOS/Linux)
-	AWS CLI and credentials setup
-	Terraform CLI basics: init, plan, apply, destroy
-	Provider block: AWS
   - Exercise: Provision a basic AWS S3 bucket

  ### Module 2: Core Terraform Concepts
-	Resources (e.g., aws_instance, aws_s3_bucket)
-	Variables (var)
-	Output values
-	Data sources (e.g., aws_ami)
-	Local values
-	Formatting: terraform fmt
  - Exercise: Create a variable based EC2 instance with a dynamic AMI lookup

 ### Module 3: Language Features
-	Built-in types: string, list, map, object
-	Expressions & conditionals
-	Meta-arguments: count, for_each, depends_on
-	Basic error handling (nullable, validation)
  - Exercise: Provision multiple EC2 instances using for_each and map variables

## Phase 2: State Management & Project Structuring (Intermediate Level)


 ### Module 4: State Management
-	What is the .tfstate file?
-	Local vs remote state
-	Configure backend with:
-	S3 bucket for storage
-	DynamoDB table for locking
-	Workspace usage (terraform workspace)
    - Exercise: Setup remote backend (S3 + DynamoDB) and create dev/prod workspaces

### Module 5: Writing Reusable Modules
-	Module folder structure: main.tf, variables.tf, outputs.tf
-	Input/output flow
-	Module usage in root config
-	DRY approach for common AWS infra (e.g., VPC module)
  -  Exercise: Build a reusable VPC module and use it in a parent project

 ### Module 6: Project Organization
-	Folder structure:
  - /environments/dev
  - /environments/prod
  - /modules/vpc
  - /modules/ec2
-	Environment-specific .tfvars
-	Remote backend per environment
  -  Exercise: Setup a dev/staging/prod folder-based layout with reusable modules

## Phase 3: Real AWS Infrastructure Projects (Practical Level)

### Module 7: Networking with Terraform
-	VPC
-	Subnets (public/private)
-	Route Tables
-	NAT Gateway, IGW
-	Security Groups, NACLs
   - Project: Create a production-grade VPC with public/private subnets and NAT Gateway

### Module 8: Compute & Load Balancing
-	EC2 instances (user_data for provisioning)
-	Auto Scaling Groups (ASG)
-	Launch Templates
-	Elastic Load Balancer (ALB)
   - Project: Deploy an EC2-based web tier behind an ALB using ASG

### Module 9: Storage & Databases
-	S3 bucket (with versioning, encryption)
-	EBS volume attachment
-	RDS (MySQL/Postgres) with subnet group, security
-	Parameter group, final snapshot, backup windows
   - Project: Create an RDS-backed database layer in private subnets

### Module 10: IAM & Secrets
-	IAM roles, policies, users
-	EC2 instance profile
-	SSM Parameters
-	Secrets Manager for secrets
   -  Project: Store DB credentials in Secrets Manager and fetch via Terraform

## Phase 4: Advanced Concepts 
 ### Module 11: Advanced Language & Lifecycle
-	Complex variable structures (nested objects, lists of maps)
-	dynamic blocks (e.g., dynamic security group rules)
-	Resource lifecycle: prevent_destroy, create_before_destroy
-	Dependency graph visualization
   - Exercise: Build a dynamic security group module

 ### Module 12: Terraform Internals
-	Execution plan and graph (DAG)
-	Targeting resources: -target
-	Resource tainting and replacement
-	Import existing AWS resources (terraform import)
    - Exercise: Import an existing EC2 or S3 resource into Terraform management

## Phase 5: Security, Collaboration & CI/CD

### Module 13: Secrets & Sensitive Data
-	Secure use of variables
-	Avoid hardcoding secrets
-	Use AWS SSM & Secrets Manager
-	Encrypt backend state (S3 with SSE or KMS)

### Module 14: Code Quality & Testing
-	terraform fmt, validate
-	Static analysis: TFLint, Checkov, Tfsec
-	Pre-commit hooks
-	Optional: Intro to Terratest
  - Exercise: Add security scanning to a project via pre-commit and Checkov

### Module 15: Terragrunt
 - What is Terragrunt?, Installation & Setup, Basic Terragrunt Workflow, Terragrunt Configuration Basics, 
 - Managing Multiple Environments with Terragrunt
 - Remote State Management with Terragrunt
 -  Reusable Modules with Terragrunt
 -  Managing Dependencies Between Modules
 -  

### Module 16: CI/CD with GitHub Actions
-	Plan and apply via GitHub Actions
-	Store credentials securely
-	Setup plan-only workflow on PR
-	Add manual approval for apply
  - Project: Create a complete Terraform GitHub Actions pipeline


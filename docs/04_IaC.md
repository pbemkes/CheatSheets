# Infrastructure as Code (IaC)

---

## Terraform

### Terraform Installation on Ubuntu/Debian:

- Commands:

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update && sudo apt install terraform
```

### AWS CLI Installation on Ubuntu:

- Commands:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install
```

### Kubectl Installation on Ubuntu:

- Commands:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

### Step-by-Step Configuration Guide with AWS EC2 Instance

#### 1. Main Terraform Configuration: 

:file_folder: main.tf

- Terraform Configuration Example:

```tf
terraform { 
  required_providers { 
  aws = {
    source = "hashicorp/aws" 
    version = "~> 4.16"
    }
  }
  required_version = ">= 1.2.0"
}

provider "aws" { 
  region = "us-west-2"
}

resource "aws_instance" "app_server" { 
  ami = "ami-08d70e59c07c61a3a" 
  instance_type = "t2.micro"
  tags = {
    Name = var.instance_name
  }
}
```

#### 2. Input Variables: 

:file_folder: variables.tf

- Example: hcl

```hcl
variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type = string
  default = "ExampleAppServerInstance" }
```

#### 3. Output Values:

:file_folder: outputs.tf:

- Example: hcl

```hcl
output "instance_id" {
  description = "ID of the EC2 instance"
  value = aws_instance.app_server.id
}

output "instance_public_ip" {
  description = "Public IP address of the EC2 instance" 
  value = aws_instance.app_server.public_ip
}
```

#### 4. Running the Configuration:

- Initialize Terraform:

```bash
terraform init
```

- Apply the Configuration:

```bash
terraform apply
```

!!! note
    Confirm by typing ***yes*** when prompted.

- Inspect Output Values:

```bash
terraform output
```

- Destroy the Infrastructure:

```bash
terraform destroy
```

### Terraform Advanced Configuration Use Cases

#### 1. Provider Configuration:

```tf
provider "aws" { 
  region = "us-west-2"
}
```

#### 2. Resource Creation:

```tf
resource "aws_instance" "example" { 
  ami = "ami-0c55b159cbfafe1f0" 
  instance_type = "t2.micro"
  tags = {
    Name = "ExampleInstance"
  }
}
```

#### 3. Variable Management:

```tf
variable "region" { 
  default = "us-west-2"
}

provider "aws" { 
  region = var.region
}
```

#### 4. State Management:

- Example for using remote state in S3:
 
```hcl
terraform {
  backend "s3" {
    bucket = "my-tfstate-bucket"
    key = "terraform/state"
    region = "us-west-2"
    encrypt = true
    dynamodb_table = "terraform-locks"
  }
}
```

#### 5. Modules:

```tf
module "vpc" {
    source = "terraform-aws-modules/vpc/aws"
    name = "my-vpc"
    cidr = "10.0.0.0/16"
    azs = ["us-west-2a", "us-west-2b"]
    public_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
    private_subnets = ["10.0.3.0/24", "10.0.4.0/24"] }
```

### Terraform Commands Cheat Sheet

- `terraform init` - Initializes the Terraform configuration.
- `terraform fmt` - Formats configuration files.
- `terraform validate` - Validates the configuration files.
- `terraform plan` - Previews changes to be applied.
- `terraform apply` - Applies the changes to reach the desired state.
- `terraform destroy` - Destroys the infrastructure and removes it from the state.
- `terraform show` - Displays the current state of resources.
- `terraform state list` - Lists resources in the current state.
- `terraform taint <resource>` - Marks a resource for recreation.
- `terraform import <resource> <resource_id>` - Imports existing resources into Terraform.
- `terraform providers` - Lists the providers used in the configuration.

### Terraform Best Practices

- Use **Version Control** to manage your Terraform code.
- Break your code into **Modules** for reusability.
- Use **Remote State** (e.g., AWS S3, Terraform Cloud) to store state files.
- Always run **terraform plan** before **terraform apply**.
- Use **terraform fmt** & **terraform validate** to ensure code correctness.
- Avoid **hardcoding secrets**; use environment variables or secret management tools.
- Keep configurations **modular** and **well-documented**.

---

## CloudFormation (stacks, templates)

### 1. CloudFormation Concepts

- **Stack** - A group of AWS resources defined in a template.
- **Template** - /JSON file defining resources and configurations.
- **StackSet** - Deploys stacks across multiple accounts and regions.
- **Change Set** - Previews updates before applying changes.
- **Rollback** - Automatic stack rollback if an error occurs.
- **Drift Detection** - Identifies manual changes made outside CloudFormation.

### 2. CloudFormation Template Example

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Basic AWS CloudFormation Example"
Resources:
  MyBucket:
    Type: "AWS::S3::Bucket"
  MyEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      InstanceType: "t2.micro"
      ImageId: "ami-0abcdef1234567890"
Outputs:
  InstanceID:
    Description: "The Instance ID"
    Value: !Ref MyEC2Instance
```

### 3. CloudFormation CLI Commands

- Stack Operations

```bash
aws cloudformation create-stack --stack-name my-stack --template-body file://template
```
```bash
aws cloudformation update-stack --stack-name my-stack --template-body file://template
```
```bash
aws cloudformation delete-stack --stack-name my-stack
```

- Viewing Stack Details

```bash
aws cloudformation describe-stacks --stack-name my-stack
```
```bash
aws cloudformation list-stack-resources --stack-name my-stack
```
```bash
aws cloudformation describe-stack-events --stack-name my-stack
```

- Change Set (Preview Changes)

```bash
aws cloudformation create-change-set --stack-name my-stack --template-body file://template. --change-set-name my-change-set
```
```bash
aws cloudformation describe-change-set --stack-name my-stack --change-set-name my-change-set
```
```bash
aws cloudformation execute-change-set --stack-name my-stack --change-set-name my-change-set
```

- Drift Detection

```bash
aws cloudformation detect-stack-drift --stack-name my-stack
```
```bash
aws cloudformation describe-stack-drift-detection-status--stack-name my-stack
```

### 4. CloudFormation Best Practices

- Use Parameters for Flexibility
- Define parameters to make templates reusable:

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: "t2.micro"
    AllowedValues: ["t2.micro", "t2.small", "t2.medium"]
```

- Use Mappings for Region-Specific Configurations

```yaml
Mappings:
  RegionMap:
    us-east-1:
      AMI: "ami-12345678"
    us-west-1:
      AMI: "ami-87654321"
```

- Use Conditions for Conditional Resource Creation

```yaml
Conditions:
  IsProd: !Equals [!Ref "Environment", "prod"]
Resources:
  MyDatabase:
    Type: "AWS::RDS::DBInstance"
    Condition: IsProd
```

- Use Outputs to Export Values

```yaml
Outputs:
  S3BucketName:
    Description: "Name of the created S3 bucket"
    Value: !Ref MyBucket
    Export:
      Name: MyBucketExport
```

- Use Nested Stacks for Large Templates

!!! note
    Break large stacks into smaller, reusable nested stacks.

### 5. CloudFormation Troubleshooting

- **Stack creation fails** - Check describe-stack-events for error details.
- **Parameter validation error** - Ensure correct parameter types and values.
- **Rollback triggered** - Check logs and describe-stack-events to debug.
- **Resources stuck in DELETE_FAILED** - Manually delete dependencies before retrying.
- **Template validation error** - Use aws cloudformation validate-template --template-body file://template to validate the template.

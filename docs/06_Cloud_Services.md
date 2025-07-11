# Cloud Services

- [AWS](#aws)
- [Azure](#azure)
- [GCP](#gcp)

---

## AWS

### EC2 (Elastic Compute Cloud)

```bash title="List all instances"
aws ec2 describe-instances
```
```bash title="Start an instance"
aws ec2 start-instances --instance-ids <id>
```
```bash title="Stop an instance"
aws ec2 stop-instances --instance-ids <id>
```
```bash title="Terminate an instance"
aws ec2 terminate-instances --instance-ids <id>
```
```bash title="Create a key pair"
aws ec2 create-key-pair --key-name <name>
```
```bash title="List security groups"
aws ec2 describe-security-groups
```

### S3 (Simple Storage Service)

```bash title="List buckets"
aws s3 ls
```
```bash title="Create a bucket"
aws s3 mb s3://<bucket>
```
```bash title="Upload a file"
aws s3 cp <file> s3://<bucket>/
```
```bash title="Delete a file"
aws s3 rm s3://<bucket>/<file>
```
```bash title="Delete a bucket"
aws s3 rb s3://<bucket> --force
```
```bash title="Sync local and S3"
aws s3 sync <local-dir> s3://<bucket>/
```

### IAM (Identity and Access Management)

```bash title="List IAM users"
aws iam list-users
```
```bash title="Create a user"
aws iam create-user --user-name <name>
```
```bash title="Attach a policy"
aws iam attach-user-policy --user-name <name> --policy-arn <policy>
```
```bash title="List IAM roles"
aws iam list-roles
```
```bash title="Create a role"
aws iam create-role --role-name <name> --assume-role-policy-document file://policy.json
```
```bash title="List policies"
aws iam list-policies
```

### VPC (Virtual Private Cloud)

```bash title="List VPCs"
aws ec2 describe-vpcs
```
```bash title="Create a VPC"
aws ec2 create-vpc --cidr-block <CIDR>
```
```bash title="Delete a VPC"
aws ec2 delete-vpc --vpc-id <id>
```
```bash title="Create a subnet"
aws ec2 create-subnet --vpc-id <id> --cidr-block <CIDR>
```
```bash title="List security groups"
aws ec2 describe-security-groups
```
```bash title="List internet gateways"
aws ec2 describe-internet-gateways
```

### Lambda (Serverless Computing)

```bash title="List all Lambda functions"
aws lambda list-functions
```
```bash title="Create a function"
aws lambda create-function --function-name <name> --runtime <runtime> --role <role> --handler <handler>
```
```bash title="Update function code"
aws lambda update-function-code --function-name <name> --zip-file fileb://<file>.zip
```
```bash title="Delete a function"
aws lambda delete-function --function-name <name>
```
```bash title="Invoke a function"
aws lambda invoke --function-name <name> output.json
```

### Amazon EKS (Elastic Kubernetes Service)

```bash title="List EKS clusters"
aws eks list-clusters
```
```bash title="Describe an EKS cluster"
aws eks describe-cluster --name my-cluster
```
```bash title="Create an EKS cluster"
aws eks create-cluster --name my-cluster --role-arn arn:aws:iam::account-id:role/EKSRole --resources-vpc-config subnetIds=subnet-xxxxxxx,securityGroupIds=sg-xxxxxxx
```
```bash title="Configure kubectl to use the EKS cluster"
aws eks update-kubeconfig --name my-cluster
```
```bash title="Check worker nodes"
kubectl get nodes
```
```bash title="List running pods"
kubectl get pods -A
```

### Amazon ECS (Elastic Container Service)

```bash title=" List ECS clusters"
aws ecs list-cluster
```
```bash title="List ECS services"
aws ecs list-services --cluster my-cluster
```
```bash title="Describe an ECS service"
aws ecs describe-services --cluster my-cluster --services my-service
```

### AWS CloudFormation

```bash title="List all stacks"
aws cloudformation list-stacks
```
```bash title="Create a stack"
aws cloudformation create-stack --stack-name my-stack --template-body file://template.yml
```

### CI/CD (CodePipeline, CodeBuild, CodeDeploy)

#### AWS CodePipeline

```bash title="List all pipelines"
aws codepipeline list-pipelines
```
```bash title="Start a pipeline execution"
aws codepipeline start-pipeline-execution --name my-pipeline
```

#### AWS CodeBuild

```bash title="List all CodeBuild projects"
aws codebuild list-projects
```
```bash title="Start a build"
aws codebuild start-build --project-name my-project
```

#### AWS CodeDeploy

```bash title="List all CodeDeploy applications"
aws deploy list-applications
```
```bash title="Deploy an application"
aws deploy create-deployment --application-name MyApp --deployment-group-name MyDeploymentGroup --s3-location bucket=my-bucket,key=app.zip,bundleType=zip
```

### Security & Compliance

```bash title="List CloudTrail logs"
aws cloudtrail describe-trails
```
```bash title="List CloudTrail logs"
aws secretsmanager list-secrets
```
```bash title="Retrieve a secret"
aws secretsmanager get-secret-value --secret-id my-secret
```

### Monitoring & Logging (CloudWatch)

```bash title="List available metrics"
aws cloudwatch list-metrics
```
```bash title="Create a CloudWatch alarm"
aws cloudwatch put-metric-alarm --alarm-name cpu-high --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 80 --comparison-operator GreaterThanThreshold --dimensions Name=InstanceId,Value=i-xxxxxxxxxx --evaluation-periods 2 --alarm-actions arn:aws:sns:region:account-id:my-topic
```
```bash title="List all log groups"
aws logs describe-log-groups
```
```bash title=" Retrieve log events"
aws logs get-log-events --log-group-name my-log-group --log-stream-name my-log-stream
```

### Networking (VPC, ELB, Route 53)

#### Amazon VPC

```bash title="List all VPCs"
aws ec2 describe-vpcs
```
```bash title="List all subnets"
aws ec2 describe-subnets
```

#### Elastic Load Balancer (ELB)

```bash title="List all load balancers"
aws elbv2 describe-load-balancers
```

#### Amazon Route 53

```bash title="List hosted zones"
aws route53 list-hosted-zones
```

### Amazon ECR (Elastic Container Registry)

```bash title="Authenticate Docker with ECR"
aws ecr get-login-password | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```
```bash title="List all ECR repositories"
aws ecr list-repositories
```

### AWS Systems Manager (SSM)

```bash title="List managed instances"
aws ssm describe-instances
```

```bash title="Run a command on an EC2 instance"
aws ssm send-command --document-name "AWS-RunShellScript" --targets "Key=instanceIds,Values=i-xxxxxxxxxx" --parameters commands="sudo apt update"
```

### AWS Auto Scaling

```bash title="List Auto Scaling groups"
aws autoscaling describe-auto-scaling-groups
```
```bash title="Update the desired capacity"
aws autoscaling update-auto-scaling-group --auto-scaling-group-name my-asg --desired-capacity 3
```
```bash title="Manually scale an Auto Scaling group"
aws autoscaling set-desired-capacity --auto-scaling-group-name my-asg --desired-capacity 2
```

### AWS Elastic Beanstalk

```bash title="List all environments"
aws elasticbeanstalk describe-environments
```
```bash title="Create an application"
aws elasticbeanstalk create-application --application-name my-app
```
```bash title="Deploy a new version"
aws elasticbeanstalk update-environment --environment-name my-env --version-label new-version
```

### AWS Step Functions

```bash title="List all state machines"
aws stepfunctions list-state-machines
```
```bash title="Start a state machine execution"
aws stepfunctions start-execution --state-machine-arn arn:aws:states:region:account-id:stateMachine:MyStateMachine
```
```bash title="Get execution details"
aws stepfunctions describe-execution --execution-arn arn:aws:states:region:account-id:execution:MyStateMachine:MyExecution
```

### AWS Glue (ETL Service)

```bash title="List all Glue databases"
aws glue get-databases
```
```bash title="List all tables in a database"
aws glue get-tables --database-name my-database
```
```bash title="Start a Glue job"
aws glue start-job-run --job-name my-glue-job
```

### AWS SNS (Simple Notification Service)

```bash title="List all SNS topics"
aws sns list-topics
```
```bash title="Publish a message to an SNS topic"
aws sns publish --topic-arn arn:aws:sns:region:account-id:MyTopic --message "Test Message"
```

### AWS SQS (Simple Queue Service)

```bash title="List all SQS queues"
aws sqs list-queues
```
```bash title="Send a message"
aws sqs send-message --queue-url https://sqs.region.amazonaws.com/account-id/my-queue --message-body "Hello World"
```

### AWS Outposts(Managed on-prem cloud/On-premises AWS)

#### List and Describe Outposts

```bash title="List all AWS Outposts"
aws outposts list-outposts
```
```bash title="Get details of a specific Outpost"
aws outposts get-outpost --outpost-id <outpost-id>
```
```bash title="List instance types in an Outpost"
aws outposts get-outpost-instance-types --outpost-id <outpost-id>
```

#### Manage Outpost Resources

```bash title="List all Outpost sites"
aws outposts list-sites
```
```bash title="List Outpost orders"
aws outposts list-orders
```
```bash title="List EC2 instances in an Outpost"
aws outposts list-outpost-instances --outpost-id <outpost-id>
```

#### Deploy and Configure Outposts

```bash title="Order an Outpost"
aws outposts create-order --line-items "[{\"catalogItemId\": \"item-id\", \"quantity\": 1}]" --outpost-id <outpost-id>
```
```bash title="Update Outpost configuration"
aws outposts update-outpost --outpost-id <outpost-id> --name <new-name>
```

#### Networking and Storage on Outposts

```bash title="List network devices in an Outpost"
aws outposts list-outpost-network-devices --outpost-id <outpost-id>
```
```bash title="List S3 buckets on an Outpost"
aws s3 ls --outpost-id <outpost-id>
```
```bash title="Create an S3 bucket in Outpost"
aws s3 mb s3://<bucket-name> --outpost-id <outpost-id>
```

#### Deploy EC2 Instances in Outposts

```bash title="Launch an EC2 instance in Outpost"
aws ec2 run-instances --image-id <ami-id> --instance-type <type> --subnet-id <outpost-subnet-id>
```

#### Automate Outpost Deployments with CloudFormation

```bash title="Deploy Outpost resources using CloudFormation"
aws cloudformation deploy --template-file outpost-config.yml --stack-name my-outpost-stack
```

#### Monitor Outposts with CloudWatch

```bash title="List Outpost-related CloudWatch metrics"
aws cloudwatch list-metrics --namespace AWS/Outposts
```
```bash title="Monitor CPU usage of Outpost instances"
aws cloudwatch get-metric-data --metric-name CPUUtilization --namespace AWS/Outposts
```

#### Integrate Outposts with CI/CD

```bash title="List all CI/CD pipelines"
aws codepipeline list-pipelines
```
```bash title="Start a deployment pipeline for Outposts"
aws codepipeline start-pipeline-execution --name <pipeline-name>
```
```bash title="Start a build process for Outposts workloads"
aws codebuild start-build --project-name <build-project>
```
```bash title="Deploy an application to an Outpost"
aws deploy create-deployment --application-name <app-name> --deployment-group-name <group-name> --s3-location bucket=<bucket-name>,key=<app.zip>,bundleType=zip
```

#### Security and Compliance for Outposts

```bash title="Create an IAM role for Outpost management"
aws iam create-role --role-name <role-name> --assume-role-policy-document file://policy.json
```
```bash title="List stored secrets for Outposts"
aws secretsmanager list-secrets
```
```bash title="Retrieve a stored secret"
aws secretsmanager get-secret-value --secret-id <secret-name>
```
```bash title="List AWS CloudTrail logs for security auditing"
aws cloudtrail describe-trails
```
```bash title="Detect security threats related to Outposts"
aws guardduty list-findings
```

#### Delete or Deactivate an Outpost

```bash title="Delete an Outpost (must be empty)"
aws outposts delete-outpost --outpost-id <outpost-id>
```
```bash title="Cancel an Outpost order before delivery"
aws outposts cancel-order --order-id <order-id>
```

---

## Azure

### Azure Virtual Machines (VMs)

```bash title="Create a VM"
az vm create --resource-group <rg> --name <vm-name> --image <image>
```
```bash title="List all VMs"
az vm list -o table
```
```bash title="Stop a VM"
az vm stop --name <vm-name> --resource-group <rg>
```
```bash title="Start a VM"
az vm start --name <vm-name> --resource-group <rg>
```
```bash title="Delete a VM"
az vm delete --name <vm-name> --resource-group <rg>
```
```bash title="Resize a VM"
az vm resize --name <vm-name> --resource-group <rg> --size <vm-size>
```
```bash title="Show VM details"
az vm show --name <vm-name> --resource-group <rg>
```
```bash title="Open a port on a VM"
az vm open-port --port <port-number> --name <vm-name> --resource-group <rg>
```

### Azure Storage

```bash title="Create a storage account"
az storage account create --name <storage-name> --resource-group <rg> --location <region>
```
```bash title="Create a blob container"
az storage container create --name <container-name> --account-name <storage-name>
```
```bash title="Upload a file to Blob Storage"
az storage blob upload --file <file-path> --container-name <container-name> --account-name <storage-name>
```
```bash title="List blobs in a container"
az storage blob list --container-name <container-name> --account-name <storage-name>
```
```bash title="Delete a storage account"
az storage account delete --name <storage-name> --resource-group <rg>
```

### Azure Kubernetes Service (AKS)

```bash title="Create an AKS cluster"
az aks create --resource-group <rg> --name <aks-name> --node-count <num> --generate-ssh-keys
```
```bash title="Get kubeconfig for AKS cluster"
az aks get-credentials --resource-group <rg> --name <aks-name>
```
```bash title="List AKS cluster nodes"
kubectl get nodes
```
```bash title="List all pods in AKS"
kubectl get pods -A
```
```bash title="Deploy an application in AKS"
kubectl apply -f <file.>
```
```bash title="Remove an application from AKS"
kubectl delete -f <file.>
```
```bash title="Delete an AKS cluster"
az aks delete --name <aks-name> --resource-group <rg>
```

### Azure Functions

```bash title="Create an Azure Function App"
az functionapp create --resource-group <rg> --name <app-name> --consumption-plan-location <region> --runtime <runtime>
```
```bash title="List all Function Apps"
az functionapp list -o table
```
```bash title="Delete a Function App"
az functionapp delete --name <app-name> --resource-group <rg>
```
```bash title="Initialize a local Azure Functions project"
func init <app-name>
```
```bash title="Create a new function"
func new
```
```bash title="Run functions locally"
func start
```
```bash title="Deploy function to Azure"
func azure functionapp publish <app-name>
```

### Azure Networking

#### Virtual Network (VNet)

```bash title="List VNets"
az network vnet list --output table
```
```bash title="Create a VNet"
az network vnet create --name myVNet --resource-group myResourceGroup --subnet-name mySubnet
```

#### Network Security Group (NSG)

```bash title="List NSGs"
az network nsg list --output table
```
```bash title="Create an NSG"
az network nsg create --resource-group myResourceGroup --name myNSG
```

#### Load Balancer

```bash title="List load balancers"
az network lb list --output table
```
```bash title="Create a Load Balancer"
az network lb create --resource-group myResourceGroup --name myLB --sku Standard --frontend-ip-name myFrontend --backend-pool-name myBackendPool
```

### Azure DevOps(CI/CD)

#### Azure DevOps Projects

```bash title="List all DevOps projects"
az devops project list --output table
```
```bash title="Create a DevOps project"
az devops project create --name myDevOpsProject
```
```bash title="Delete a DevOps project"
az devops project delete --id PROJECT_ID --yes
```

#### Azure Repos (Git)

```bash title="List all repositories"
az repos list --output table
```
```bash title="Create a new repo"
az repos create --name myRepo
```
```bash title="Delete a repo"
az repos delete --name myRepo --yes
```

#### Azure Pipelines

```bash title="List all pipelines"
az pipelines list --output table
```
```bash title="Create a pipeline"
az pipelines create --name myPipeline --repository myRepo --branch main --repository-type gitHub
```
```bash title="Run a pipeline"
az pipelines run --name myPipeline
```

### Azure Monitor & Logging

#### Azure Monitor

```bash title="List metrics for a resource"
az monitor metrics list --resource myResourceID
```
```bash title="List metric alerts"
az monitor metrics alert list --resource-group myResourceGroup
```

#### Azure Log Analytics

```bash title="List log analytics workspaces"
az monitor log-analytics workspace list --output table
```
```bash title="Create a Log Analytics workspace"
az monitor log-analytics workspace create --resource-group myResourceGroup --workspace-name myWorkspace
```

### Security & Compliance

#### Azure Security Center

```bash title="List security assessments"
az security assessment list --output table
```
```bash title="Enable auto-provisioning for Security Center"
az security setting update --name AutoProvisioning --value On
```

### Azure Key Vault

```bash title="List Key Vaults"
az keyvault list --output table
```
```bash title="Create a Key Vault"
az keyvault create --name myKeyVault --resource-group myResourceGroup --location eastus
```
```bash title="Store a secret in Key Vault"
az keyvault secret set --vault-name myKeyVault --name mySecret --value "MySecretValue"
```
```bash title="Retrieve a secret"
az keyvault secret show --vault-name myKeyVault --name mySecret
```

### Azure Policies & Governance

```bash title="List all policy assignments"
az policy assignment list --output table
```
```bash title="Assign a policy"
az policy assignment create --name myPolicyAssignment --policy myPolicyDefinition
```
```bash title="Delete a policy assignment"
az policy assignment delete --name myPolicyAssignment
```

### Azure Active Directory (AAD)

```bash title="List all users"
az ad user list --output table
```
```bash title="List all groups"
az ad group list --output table
```
```bash title="List all applications"
az ad app list --output table
```
```bash title="List all service principals"
az ad sp list --output table
```

### Azure Backup & Recovery

```bash title="List all backup vaults"
az backup vault list --output table
```
```bash title="Create a backup vault"
az backup vault create --resource-group myResourceGroup --name myBackupVault
```
```bash title="List backup items"
az backup item list --resource-group myResourceGroup --vault-name myBackupVault --output table
```

---

## GCP

### Compute Engine (VMs)

```bash title="Create a VM instance"
gcloud compute instances create <vm-name> --zone=<zone> --machine-type=<type> --image=<image>
```
```bash title="List all VM instances"
gcloud compute instances list
```
```bash title="Start a VM instance"
gcloud compute instances start <vm-name> --zone=<zone>
```
```bash title="Stop a VM instance"
gcloud compute instances stop <vm-name> --zone=<zone>
```
```bash title="Delete a VM instance"
gcloud compute instances delete <vm-name> --zone=<zone>
```
```bash title="SSH into a VM instance"
gcloud compute ssh <vm-name> --zone=<zone>
```
```bash title="Open a specific port"
gcloud compute firewall-rules create <rule-name> --allow tcp:<port>
```

### Google Kubernetes Engine (GKE)

```bash title="Create a GKE cluster"
gcloud container clusters create <cluster-name> --num-nodes=<num> --zone=<zone>
```
```bash title="Get credentials for the GKE cluster"
gcloud container clusters get-credentials <cluster-name> --zone=<zone>
```
```bash title="List cluster nodes"
kubectl get nodes
```
```bash title="List all running pods"
kubectl get pods -A
```
```bash title="Deploy an application to GKE"
kubectl apply -f <file.>
```
```bash title="Remove an application from GKE"
kubectl delete -f <file.>
```
```bash title="Delete a GKE cluster"
gcloud container clusters delete <cluster-name> --zone=<zone>
```

### Cloud Run (Serverless Containers)

```bash title="Deploy an application to Cloud Run"
gcloud run deploy <service-name> --image=<gcr.io/project/image> --platform=managed --region=<region> --allow-unauthenticated
```
```bash title="List all deployed Cloud Run services"
gcloud run services list
```
```bash title="Update Cloud Run service to the latest image"
gcloud run services update-traffic <service-name> --to-latest
```
```bash title="Delete a Cloud Run service"
gcloud run services delete <service-name>
```
```bash title="Set environment variables in a Cloud Run service"
gcloud run services update <service-name> --set-env-vars VAR_NAME=value
```

### Google Cloud Storage (GCS)

```bash title="List all storage buckets"
gcloud storage buckets list
```
```bash title="Create a storage bucket"
gcloud storage buckets create my-bucket --location US
```
```bash title="Upload a file"
gcloud storage cp file.txt gs://my-bucket/
```
```bash title="Delete a file"
gcloud storage rm gs://my-bucket/file.txt
```
```bash title="Delete a storage bucket"
gcloud storage buckets delete my-bucket
```

### Google Cloud IAM (Identity & Access Management)

```bash title="List all IAM roles"
gcloud iam roles list
```
```bash title="List all service accounts"
gcloud iam service-accounts list
```
```bash title="Assign a role to a user"
gcloud projects add-iam-policy-binding PROJECT_ID --member=user:EMAIL --role=roles/editor
```
```bash title="Remove a role from a user"
gcloud projects remove-iam-policy-binding PROJECT_ID --member=user:EMAIL --role=roles/editor
```

### Google Cloud SQL (Managed Database Service)

```bash title="List all Cloud SQL instances"
gcloud sql instances list
```
```bash title="Create a Cloud SQL instance"
gcloud sql instances create my-db --tier=db-f1-micro --region=us-central1
```
```bash title="Start a Cloud SQL instance"
gcloud sql instances start my-db
```
```bash title="Stop a Cloud SQL instance"
gcloud sql instances stop my-db
```
```bash title="Delete a Cloud SQL instance"
gcloud sql instances delete my-db
```

### Google Cloud Build (CI/CD)

```bash title="List all Cloud Build runs"
gcloud builds lis
```
```bash title="Build and push an image"
gcloud builds submit --tag gcr.io/PROJECT_ID/my-app
```
```bash title="List Cloud Build triggers"
gcloud builds triggers list
```

### Google Cloud Deploy (CI/CD)

```bash title="List deployment releases"
gcloud deploy releases list --delivery-pipeline=my-pipeline
```
```bash title="List rollouts"
gcloud deploy rollouts list --release=my-release --delivery-pipeline=my-pipeline
```

### Google Cloud Logging & Monitoring

#### Cloud Logging

```bash title="List available logs"
gcloud logging logs list
```
```bash title="Read VM logs"
gcloud logging read "resource.type=gce_instance" --limit 10
```

#### Cloud Monitoring

```bash title="List available monitoring metrics"
gcloud monitoring metrics list
```
```bash title="List all dashboards"
gcloud monitoring dashboards list
```

### Google Cloud Networking

#### VPC (Virtual Private Cloud)

```bash title="List VPC networks"
gcloud compute networks list
```
```bash title="Create a VPC network"
gcloud compute networks create my-vpc --subnet-mode=custom
```

#### Firewall Rules

```bash title="List firewall rules"
gcloud compute firewall-rules list
```
```bash title="Allow SSH access"
gcloud compute firewall-rules create allow-ssh --network=my-vpc --allow=tcp:22
```

#### Load Balancers

```bash title="List all load balancers"
gcloud compute forwarding-rules list
```

### Google Cloud Security

#### Cloud Identity-Aware Proxy (IAP)

```bash title="List IAP-secured applications"
gcloud iap web list
```
```bash title="IAP settings"
gcloud iap settings get --resource-type=app-engine Get
```

#### Cloud Key Management Service (KMS)

```bash title="List key rings"
gcloud kms keyrings list --location global
```
```bash title="List encryption keys"
gcloud kms keys list --keyring my-keyring --location global
```

### Google Artifact Registry

```bash title="List all Artifact Repositories"
gcloud artifacts repositories list
```
```bash title="Create a Docker registry"
gcloud artifacts repositories create my-repo --repository-format=docker --location=us-central1
```

### Google Cloud Pub/Sub (Messaging)

```bash title="List all Pub/Sub topics"
gcloud pubsub topics list
```
```bash title="Create a topic"
gcloud pubsub topics create my-topic
```
```bash title="List all subscriptions"
gcloud pubsub subscriptions list
```
```bash title="Create a subscription"
gcloud pubsub subscriptions create my-sub --topic=my-topic
```

### Google Cloud Backup & Disaster Recovery

```bash title="List all backup vaults"
gcloud backup vaults list
```
```bash title="List backup policies"
gcloud backup policies list
```

### Google Cloud Policies & Governance

```bash title="List policy tags"
gcloud policy-tags list --location=us
```
```bash title="List all organizational policies"
gcloud resource-manager org-policies list
```

### Google Cloud Scheduler (Cron Jobs)

```bash title="List all scheduled jobs"
gcloud scheduler jobs list
```
```bash title="Create a scheduled HTTP job"
gcloud scheduler jobs create http my-job --schedule="0 \* \* \* \*" --uri=https://example.com/cron
```

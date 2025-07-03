# Continuous Integration (CI) & Continuous Deployment (CD)

---

## Jenkins (pipelines, declarative scripts)

### Jenkins Installation (Ubuntu)

- Install Java (Jenkins requires Java)

```bash
sudo apt update && sudo apt install -y openjdk-17-jdk
```

- Add Jenkins repository & install Jenkins

```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null sudo apt update && sudo apt install -y jenkins
```

- Start & Enable Jenkins

```bash
sudo systemctl enable --now jenkins
```

- Check Jenkins status

```bash
sudo systemctl status jenkins
```

- Access Jenkins UI at [http://your-server-ip:8080](http://your-server-ip:8080/)

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### 1. Basic Jenkins Commands

- `systemctl start jenkins` - Start Jenkins service 
- `systemctl stop jenkins` - Stop Jenkins service 
- `systemctl restart jenkins` - Restart Jenkins service 
- `systemctl status jenkins` - Check Jenkins service status 
- `journalctl -u jenkins -f` - View real-time logs

### 2. Jenkins CLI Commands

- `java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) list-jobs` - List all jobs
- `java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) build <job-name>` - Trigger a job
- `java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) delete-job <job-name>` - Delete a job
- `java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) enable-job <job-name>` - Enable a job
- `java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) disable-job <job-name>` - Disable a job
- `java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) who-am-i` - Show current user info

### 3. Jenkins Environment Variables

- `JENKINS_HOME` - Jenkins home directory
- `BUILD_NUMBER` - Current build number
- `JOB_NAME` - Job name
- `WORKSPACE` - Workspace directory
- `GIT_COMMIT` - Git commit hash of the build
- `BUILD_URL` - URL of the build
- `NODE_NAME` - Name of the node the build is running on

### 4. Jenkins Pipeline (Declarative)

```groovy

pipeline {
  agent any
  environment {
    APP_ENV = 'production'
  }
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your-repo.git'
      }
    }
    stage('Build') { 
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Test') { 
      steps {
        sh 'mvn test'
      }
    }
    stage('Deploy') { 
      steps {
        sh 'scp target/*.jar user@server:/deploy/'
      }
    }
  }
}
```

### 5. Jenkins Pipeline (Scripted)

```groovy

node {
  stage('Checkout') {
    git 'https://github.com/your-repo.git'
  }
  stage('Build') {
    sh 'mvn clean package'
  }
  stage('Test') { 
    sh 'mvn test'
  }
  stage('Deploy') {
    sh 'scp target/*.jar user@server:/deploy/'
  }
}
```

### 6. Jenkins Webhook (GitHub Example)

1. Go to GitHub Repo > Settings > Webhooks
2. Add URL: http://<jenkins-url>/github-webhook/
3. Select application/json as content type
4. Choose Just the push event
5. Save and trigger a push event to test

### 7. Manage Plugins via CLI

```bash title="Install a plugin"
java-jarjenkins-cli.jar-shttp://localhost:8080install-plugin <plugin-name>
```
```bash title="List installed plugins"
java -jar jenkins-cli.jar -s http://localhost:8080list-plugins 
```
```bash title="Restart safely after installing plugins"
java -jar jenkins-cli.jar -s http://localhost:8080safe-restart
```

### 8. Manage Jenkins Jobs via CLI

```bash title="Create a job "
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) create-job my-job < job-config.xml
```
```bash title="Export job config"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) get-job my-job > job-config.xml
```
```bash title="Update job config"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) update-job my-job < job-config.xml
```

### 9. Backup & Restore Jenkins

```bash title="Restore Jenkins"
cp -r $JENKINS_HOME /backup/jenkins_$(date +%F) - Backup Jenkins cp -r /backup/jenkins_<date>/\* $JENKINS_HOME
```

### 10. Jenkins Security Commands

```bash title="Reload config"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) reload-configuration
```
```bash title="Safe shutdown"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) safe-shutdown
```
```bash title="Restart Jenkins"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) restart
```

### 11. Jenkins Credentials via CLI

```bash title=""
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) create-credentials-by-xml system::system::jenkins _ < credentials.xml
```

### 12. Jenkins Node Management

```bash title="List all nodes"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) list-nodes
```
```bash title="Create a new node"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) create-node <node-name>
```
```bash title="Delete a node"
java -jar jenkins-cli.jar -s [http://localhost:8080](http://localhost:8080/) delete-node <node-name>
```

### 13. Jenkins Job Trigger Examples

```groovy
trigger {
  cron('H 4 * * *') # Run at 4 AM every day
}
trigger {
  pollSCM('H/5 * * * *') # Check SCM every 5 minutes
}
```

### 14. Jenkins File Parameter Example

```groovy
pipeline { 
  agent any 
  parameters {
     file(name: 'configFile')
  }
  stages {
     stage('Read File') { 
        steps {
           sh 'cat ${configFile}'
        }
     }
  }
}
```

### 15. Jenkins Docker Pipeline Example

```groovy 
pipeline {
  agent {
    docker {
      image 'maven:3.8.7'
    }
  }
  stages {
    stage('Build') { 
      steps {
        sh 'mvn clean package'
      }
    }
  }
}
```

---

## Jenkins Pipeline Scripts with DevOps Tools

### 1. Basic CI/CD Pipeline

```groovy
pipeline { 
  agent any 
  stages {
    stage('Clone Repository') { 
      steps {
        git 'https://github.com/user/repo.git'
      }
    }
    stage('Build') { 
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Test') { 
      steps {
        sh 'mvn test'
      }
    }
    stage('Deploy') { 
      steps {
        sh 'scp target/app.jar user@server:/deploy/path'
      }
    }
  }
}
```

### 2. Docker Build & Push

```groovy
pipeline { 
  agent any
  environment {
    DOCKER_HUB_USER = 'your-dockerhub-username'
  }
  stages {
    stage('Build Docker Image') { 
      steps {
        sh 'docker build -t my-app:latest .'
      }
    }
    stage('Push to Docker Hub') { 
      steps {
        withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) { 
          sh 'docker tag my-app:latest $DOCKER_HUB_USER/my-app:latest' 
          sh 'docker push $DOCKER_HUB_USER/my-app:latest'
        }
      }
    }
  }
}
```

### 3. Kubernetes Deployment

```groovy
pipeline { 
  agent any 
  stages {
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s/deployment.'
      }
    }
  }
}
```

### 4. Terraform Deployment

```groovy
pipeline { 
  agent any 
  stages {
    stage('Terraform Init') { 
      steps {
        sh 'terraform init'
      }
    }
    stage('Terraform Apply') { 
      steps {
        sh 'terraform apply -auto-approve'
      }
    }
  }
}
```

### 5. Security Scanning with Trivy

```groovy
pipeline { 
  agent any 
  stages {
    stage('Scan with Trivy') { 
      steps {
        sh 'trivy image my-app:latest'
      }
    }
  }
}
```

### 6. SonarQube Code Analysis

```groovy
pipeline { 
  agent any
  environment {
    SONAR_TOKEN = credentials('sonar-token')
  }
  stages {
    stage('SonarQube Analysis') { 
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
        }
      }
    }
  }
}
```

---

## GitHub Actions (workflows, syntax)

GitHub Actions allows automation for CI/CD pipelines directly within GitHub repositories.

- Workflows (.github/workflows/\*.yml): Defines automation steps.
- Jobs: Runs tasks inside a workflow.
- Steps: Individual commands executed in jobs.
- Actions: Predefined or custom reusable commands.

### Commands

#### 1. Initialize a GitHub Actions workflow

```bash
mkdir -p .github/workflows && touch .github/workflows/main.yml
```

#### 2. Validate GitHub Actions workflow syntax

```bash title="List available workflows"
act -l
```

```bash title="Run a specific job locally"
act -j <job-name>
```

```bash title="Run the workflow locally"
act -w .github/workflows/main.yml
```

#### 3. Set up GitHub Actions runner

- `gh workflow list` - List workflows in the repo
- `gh workflow run <workflow-name>` - Manually trigger a workflow
- `gh workflow enable <workflow-name>` - Enable a workflow
- `gh workflow disable <workflow-name>` - Disable a workflow

#### 4. Manage workflow runs

- `gh run list` - List recent workflow runs
- `gh run view <run-id>` - View details of a specific workflow run
- `gh run rerun <run-id>` - Rerun a failed workflow
- `gh run cancel <run-id>` - Cancel a running workflow
- `gh run delete <run-id>` - Delete a workflow run

#### 5. Manage workflow artifacts

- `gh run download -n <artifact-name>` - Download artifacts from a workflow run
- `gh run view --log` - View logs of the latest workflow run

#### 6. Manage secrets for GitHub Actions

- `gh secret list` - List repository secrets
- `gh secret set <SECRET_NAME> --body <value>` - Add or update a secret
- `gh secret remove <SECRET_NAME>` - Remove a secret

#### 7. Using GitHub Actions Cache

- `actions/cache@v3` - GitHub Actions cache
- `actions/upload-artifact@v3` - Upload artifacts
- `actions/download-artifact@v3` - Download artifacts

#### 8. Run a workflow manually via API

```bash
curl -X POST -H "Authorization: token <YOUR_GITHUB_TOKEN>" \
-H "Accept: application/vnd.github.v3+json" \
https://api.github.com/repos/<owner>/<repo>/actions/workflows/<workflow_file>/ dispatches \
-d '{"ref":"main"}'
```

---

## Github Actions Workflow:

### Basic GitHub Actions Workflow

:file_folder: File: .github/workflows/ci-cd.yml

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
     - main
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4

    - name: Set Up Java
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '17'

    - name: Build with Maven
      run: mvn clean package

    - name: Upload Build Artifact
      uses: actions/upload-artifact@v4
      with:
        name: application
        path: target/\*.jar
```

### Docker Build & Push to Docker Hub

:file_folder: File: .github/workflows/docker.yml

```yaml
name: Docker Build & Push

on:
  push:
    branches:
    - main

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Login to Docker Hub 
      uses: docker/login-action@v3 
      with:
        username: ${{ secrets.DOCKER_USERNAME }} 
        password: ${{ secrets.DOCKER_PASSWORD }}
    - name: Build and Push Docker Image 
      run: |
        docker build -t my-app:latest .
        docker tag my-app:latest ${{ secrets.DOCKER_USERNAME }}/my-app:latest
        docker push ${{ secrets.DOCKER_USERNAME }}/my-app:latest
```

### Kubernetes Deployment

:file_folder: File: .github/workflows/k8s.yml

```yaml
name: Deploy to Kubernetes

on:
  push:
    branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Set Up Kubectl
      uses: azure/setup-kubectl@v3
      with:
        version: 'latest'
    - name: Apply Kubernetes Manifest
      run: kubectl apply -f k8s/deployment.
```

### Terraform Deployment

:file_folder: File: .github/workflows/terraform.yml

```yaml
name: Terraform Deployment

on:
  push:
    branches:
    - main

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Set Up Terraform
      uses: hashicorp/setup-terraform@v3
    - name: Terraform Init & Apply
      run: | 
        terraform init
        terraform apply -auto-approve
```

### Security Scanning with Trivy

:file_folder: File: .github/workflows/trivy.yml

```yaml
name: Security Scan with Trivy

on:
  push:
    branches:
    - main

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Run Trivy Image Scan
      run: | 
        docker pull ${{ secrets.DOCKER_USERNAME }}/my-app:latest
        trivy image ${{ secrets.DOCKER_USERNAME }}/my-app:latest
```

### SonarQube Code Analysis

:file_folder: File: .github/workflows/sonarqube.yml

```yaml
name: SonarQube Analysis

on:
  push:
    branches:
    - main

jobs:
  sonar:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Set Up Java
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '17'
    - name: Run SonarQube Analysis
      run: mvn sonar:sonar -Dsonar.login=${{ secrets.SONAR_TOKEN }}
```

### Upload & Deploy to AWS S3

:file_folder: File: .github/workflows/s3-upload.yml

```yaml
name: Upload to S3

on:
  push:
    branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Upload Files to S3
      run: |
        aws s3 sync . s3://my-bucket-name --delete
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_KEY }}
```

---

## GitLab CI/CD (stages, jobs, runners)

### GitLab CI/CD Basics

- **.gitlab-ci.yml:** Defines the CI/CD pipeline in the repository root.
- **Stages:** Pipeline execution order (build, test, deploy).
- **Jobs:** Specific tasks in each stage.
- **Runners:** Machines executing jobs (shared or self-hosted).
- **Artifacts:** Files preserved after job execution.

### Basic GitLab CI/CD Pipeline

:file_folder: File: .gitlab-ci.yml

```yaml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
  - echo "Building application..."
  - mvn clean package
  artifacts:
    paths:
    - target/\*.jar

test:
  stage: test
    script:
    - echo "Running tests..."
    - mvn test

deploy:
  stage: deploy 
  script:
  - echo "Deploying application..."
  - scp target/*.jar user@server:/deploy/path 
  only:
  - main
```

### Docker Build & Push to GitLab Container Registry

:file_folder: File: .gitlab-ci.yml

```yaml
variables:
  IMAGE_NAME: registry.gitlab.com/your-namespace/your-repo

stages:
  - build
  - push

build:
  stage: build 
  script:
  - docker build -t $IMAGE_NAME:latest . 
  only:
  - main

push:
  stage: push
  script:
  - echo $CI_REGISTRY_PASSWORD | docker login -u $CI_REGISTRY_USER --password-stdin $CI_REGISTRY
  - docker push $IMAGE_NAME:latest 
  only:
  - main
```

### Kubernetes Deployment

:file_folder: File: .gitlab-ci.yml

```yaml
stages:
- deploy

deploy:
  stage: deploy
  image: bitnami/kubectl
  script:
  - kubectl apply -f k8s/deployment.
  only:
  - main
```

### Terraform Deployment

:file_folder: File: .gitlab-ci.yml

```yaml
image: hashicorp/terraform:latest

stages:
- terraform

terraform:
  stage: terraform
  script:
  - terraform init
  - terraform apply -auto-approve 
  only:
  - main
```

### Security Scanning with Trivy

:file_folder: File: .gitlab-ci.yml

```yaml
stages:
- security_scan

security_scan: stage:security_scan
  script:
  - docker pull registry.gitlab.com/your-namespace/your-repo:latest
  - trivy image registry.gitlab.com/your-namespace/your-repo:latest
  only:
  - main
```

### SonarQube Code Analysis

:file_folder: File: .gitlab-ci.yml

```yaml
image: maven:3.8.7

stages:
- analysis

sonarqube:
  stage: analysis
  script:
  - mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN
  only:
  - main
```

### AWS S3 Upload

:file_folder: File: .gitlab-ci.yml

```yaml
stages:
- deploy

deploy_s3:
  stage: deploy
  script:
  - aws s3 sync . s3://my-bucket-name --delete
  only:
  - main
  environment:
  name: production
```

### Notify on Slack

:file_folder: File: .gitlab-ci.yml

```yaml
notify:
  stage: notify
  script:
  - curl -X POST -H 'Content-type: application/json' --data '{"text":"Deployment completed successfully!"}' $SLACK_WEBHOOK_URL
  only:
  - main
```

---

## Tekton

!!! note "What is Tekton?"

    Tekton is a Kubernetes-native CI/CD framework that allows you to create and run pipelines for automating builds, testing, security scans, and deployments. It provides reusable components such as Tasks, Pipelines, and PipelineRuns, making it ideal for cloud-native DevOps workflows.

### Installation (On Kubernetes Cluster)

```bash title="1. Install Tekton Pipelines"
kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.
```

```bash title="2. Verify Installation"

kubectl get pods -n tekton-pipelines
```

```bash title="3. Install Tekton CLI (tkn)"
curl -LO https://github.com/tektoncd/cli/releases/latest/download/tkn-linux-amd64 chmod +x tkn-linux-amd64 sudo mv tkn-linux-amd64 /usr/local/bin/tkn
```

```bash title="4. Check Tekton CLI Version"
tkn version
```

### Tekton Basics

- **Tasks**: The smallest execution unit in Tekton.
- **Pipelines**: A sequence of tasks forming a CI/CD process.
- **PipelineRuns**: Executes a pipeline.
- **TaskRuns**: Executes a task.
- **Workspaces**: Used for sharing data between tasks.
- **Resources**: Defines input/output artifacts (e.g., Git repositories, images).

### Tekton Pipeline Commands

- `tkn pipeline list` - List all pipelines
- `tkn pipeline describe <pipeline-name>` - Describe a specific pipeline
- `tkn pipeline start <pipeline-name>` --showlog - Start a pipeline and show logs
- `tkn pipeline delete <pipeline-name>` - Delete a pipeline

### Tekton Task Commands

- `tkn task list` - List all tasks
- `tkn task describe <task-name> `- Describe a specific task 
- `tkn task start <task-name> --showlog` - Start a task and show logs 
- `tkn task delete <task-name>` - Delete a task

### Tekton PipelineRun Commands

- `tkn pipelinerun list` - List pipeline runs 
- `tkn pipelinerun describe <pipelinerun-name>` - Describe a pipeline run 
- `tkn pipelinerun logs <pipelinerun-name>` - Show logs of a pipeline run 
- `tkn pipelinerun delete <pipelinerun-name>` - Delete a pipeline run

### Tekton TaskRun Commands

- `tkn taskrun list` - List task runs 
- `tkn taskrun describe <taskrun-name>` - Describe a task run 
- `tkn taskrun logs <taskrun-name>` - Show logs of a task run 
- `tkn taskrun delete <taskrun-name>` - Delete a task run

### Tekton Resources Commands

- `tkn resource list` - List all pipeline resources 
- `tkn resource describe <resource-name>` - Describe a specific resource 
- `tkn resource delete <resource-name>` - Delete a resource

### Tekton Triggers Commands

- `tkn triggerbinding list `- List trigger bindings 
- `tkn triggertemplate list` - List trigger templates 
- `tkn eventlistener list` - List event listeners 
- `tkn eventlistener logs <listener-name>` - Show logs of an event listener

### Tekton Debugging & Monitoring

- `kubectl logs -l app=tekton-pipelines-controller -n tekton-pipelines` - View Tekton controller logs 
- `kubectl get pods -n tekton-pipelines - List running Tekton pods kubectl describe pod <pod-name> -n tekton-pipelines` - Get details of a specific pod

### Delete All Tekton Resources

- `kubectl delete pipelineruns --all -n <namespace>`
- `kubectl delete taskruns --all -n <namespace>`
- `kubectl delete pipelines --all -n <namespace>`
- `kubectl delete tasks --all -n <namespace>`

### Basic Tekton Task

:file_folder: File: task.yml

```yaml title="task.yml"
apiVersion: tekton.dev/v1beta1
kind: Task
  metadata:
  name: echo-task
spec:
  steps:
  - name: echo-message
    image: alpine
    script: |
      #!/bin/sh
      echo "Hello from Tekton Task!"
```

```bash title="Apply the task:"
kubectl apply -f task.
```

```bash title="Run the task:"
tkn task start echo-task
```

### Tekton Pipeline with Tasks

:file_folder: File: pipeline.yml

```yaml title="pipeline.yml"
apiVersion: tekton.dev/v1beta1
kind: Pipeline
  metadata:
  name: sample-pipeline
spec:
  tasks:
  - name: build-task
    taskRef:
      name: build-task
  - name: deploy-task
    taskRef:
      name: deploy-task
    runAfter:
    - build-task
```

```bash title="Apply:"
kubectl apply -f pipeline.
```

### Tekton PipelineRun

:file_folder: File: pipelinerun.yml

```yaml title="pipeline.yml"
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  name:sample-pipelinerun
spec:
  pipelineRef:
    name: sample-pipeline
```

```bash title="Run the pipeline:"
kubectl apply -f pipelinerun.
```

```bash title="Check status:"
tkn pipelinerun describe sample-pipelinerun
```

### Build & Push Docker Image

:file_folder: File: task-build-push.yml

```yaml title="task-build-push.yml:"
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: build-push-task
spec:
  steps:
  - name: build-and-push
    image: gcr.io/kaniko-project/executor:latest
    script: | 
      #!/bin/sh /kaniko/executor --context=/workspace/source --destination=docker.io/myrepo/myapp:latest
```

### Kubernetes Deployment with Tekton

:file_folder: File: task-k8s-deploy.yml

```yaml title="task-k8s-deploy.yml"
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: deploy-task
spec:
  steps:
  - name: apply-manifest
    image: bitnami/kubectl
    script: |
    #!/bin/sh
    kubectl apply -f k8s/deployment
```

### Tekton with Terraform

File: task-terraform.

apiVersion: tekton.dev/v1beta1 kind: Task metadata: name: terraform-task spec: steps: - name: terraform-apply image: hashicorp/terraform:latest script: | #!/bin/sh terraform init terraform apply -auto-approve

## Security Scanning with Trivy

File: task-trivy.

apiVersion: tekton.dev/v1beta1 kind: Task metadata: name: trivy-scan spec: steps:

- name: run-trivy image: aquasec/trivy:latest script: | #!/bin/sh trivy image docker.io/myrepo/myapp:latest

## SonarQube Code Analysis

File: task-sonarqube.

apiVersion: tekton.dev/v1beta1 kind: Task metadata: name:sonarqube-task spec: steps: - name: sonar-scan image: maven:3.8.7 script: | #!/bin/sh mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN

## Notify on Slack

apiVersion: tekton.dev/v1beta1 kind: Task metadata: name:slack-notify spec: steps:

- name: send-slack-message image: curlimages/curl:latest script: | #!/bin/sh

curl -X POST -H 'Content-type: application/json' --data '{"text":"Deployment completed successfully!"}' $SLACK_WEBHOOK_URL

## Circle CI

## Introduction

CircleCI is a cloud-based CI/CD tool that automates software testing and deployment. It provides seamless integration with GitHub, Bitbucket, and other version control systems, enabling automated builds, tests, and deployments.

## Installation

- Sign up at CircleCI
- Connect your repository (GitHub, Bitbucket)
- Configure the .circleci/config.yml file in your project

## CircleCI Commands (Pipeline Example)

circleci checkout - Check out the repository to the current directory circleci sphere list - List all the available workspaces in your account circleci config process - Process the CircleCI config file and output the final configuration

circleci step halt - Halt the current job execution, useful in workflows circleci job follow <job_id> - Stream the logs of a specific job in real-time circleci pipeline trigger <pipeline_id> - Trigger a pipeline by its ID circleci pipeline list - List all the pipelines for your project circleci project status <project_slug> - View the status of the project circleci sphere create <sphere_name> - Create a new workspace circleci sphere remove <sphere_name> - Remove a workspace

circleci sync - Sync CircleCI configuration for a given project circleci orb publish <orb_name> <version> <path_to_orb> - Publish a new version of an orb

### CircleCI Pipeline Script Example

### Basic CircleCI Pipeline Configuration

version: 2.1 - Define the CircleCI version

# Define jobs

jobs:

build:

docker:

- image: circleci/python:3.8 - Use a Python 3.8 Docker image

steps:

```
- checkout - Check out the code
```
- run:

name: Install dependencies command: pip install -r requirements.txt

- run:

name: Run tests command: pytest

deploy:

docker:

```
- image: circleci/python:3.8
```
steps:

```
- checkout
```
- run:

name: Deploy application command: ./deploy.sh - Custom deploy script - Define workflows (Job execution order) workflows: version: 2 build_and_deploy: jobs: - build - deploy: requires: - build - Ensure deployment happens after build succeeds

### Advanced CircleCI Features

### 1. Running Jobs Based on Branches

jobs: deploy: docker: - image: circleci/python:3.8 steps: - checkout - run: name: Deploy to Production command: ./deploy_production.sh workflows: version: 2 deploy_to_production: jobs: - deploy: filters: branches: only: main - Deploy only on the 'main' branch

## 2. Caching Dependencies to Speed Up Builds

jobs:

build: docker: - image: circleci/python:3.8 steps: - checkout - restore_cache: keys: - v1-dependencies-{{ checksum "requirements.txt" }} - run: name: Install dependencies command: pip install -r requirements.txt - save_cache: paths: - ~/.cache/pip - Save pip cache key: v1-dependencies-{{ checksum "requirements.txt" }}

## 3. Using Environment Variables for Sensitive Data

jobs:

deploy:

docker:

```
- image: circleci/python:3.8
```
steps:

- checkout

- run:

name: Deploy using environment variables command: ./deploy.sh

environment:

API_KEY: $API_KEY - Use stored API keys

### 4. Running Jobs Conditionally Based on File Changes

jobs: deploy: docker: - image: circleci/python:3.8 steps: - checkout - run: name: Deploy Application command: ./deploy.sh filters: branches: only: main requires: - build when: changes:

- Dockerfile - Only run deploy if the Dockerfile changes

### 5. Running Tests in Parallel

jobs:

test:

docker:

- image: circleci/python:3.8

parallelism: 4 - Run 4 test jobs in parallel steps:

- checkout - run: name: Run tests command: pytest

## 6. Using Multiple Docker Containers

jobs:

build:

docker:

- image: circleci/python:3.8

-image: circleci/postgres:13 - Additional container for PostgreSQL

environment:

POSTGRES_USER: circleci

steps:

- checkout

- run:

name: Install dependencies

command: pip install -r requirements.txt

- run:

name: Run database migrations

command: python manage.py migrate

- run:

name: Run tests command: pytest

## 7. Running Jobs Manually (Manual Approvals)

jobs: manual_deploy: docker: - image: circleci/python:3.8 steps:

- checkout

- run:

name: Deploy to Production command: ./deploy.sh when: manual - Only run when triggered manually

### 8. Sending Notifications on Job Failure

workflows: version: 2 notify_on_failure: jobs: - build notification: email: - [user@example.com](mailto:user@example.com) - Send email notifications on failures

### 9. Running Multiple Jobs in Parallel

workflows: version: 2 build_and_deploy: jobs: - build - deploy: requires: - build - Deploy after build completes filters: branches: only: main

ArgoCD (GitOps)

### Introduction

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. It enables the deployment of applications from Git repositories to Kubernetes clusters, ensuring that the live state of the cluster matches the desired state defined in Git.

### Installation

1. Install Argo CD CLI 

macOS: brew install argocd

### Linux:

curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.5.4/argocd-linux-amd64 chmod +x /usr/local/bin/argocd

### Install Argo CD on Kubernetes

kubectl create namespace argocd kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.

### Accessing the Argo CD UI

Access the Argo CD API Server (Local Port Forwarding) kubectl port-forward svc/argocd-server -n argocd 8080:443

1. Login to the Argo CD UI

Initial Password (default is admin and the password is the name of the pod running Argo CD): kubectl get pods -n argocd kubectl logs <argocd-server-pod-name> -n argocd | grep "admin"

Argo CD Commands 

Login to Argo CD via CLI argocd login <ARGOCD_SERVER> --username admin --password <password>

### View the current applications

argocd app list

#### 1. Sync an Application

○ Syncs the application with the desired state from the Git repository.

argocd app sync <app-name>

### Get Application Status

argocd app get <app-name>

#### 2. Create an Application

○ Creates an app in Argo CD by specifying the Git repository, target namespace, and project.

argocd app create <app-name> \

--repo <git-repository-url> \

--path <path-to-k8s-manifests> \

--dest-server https://kubernetes.default.svc \

```
--dest-namespace <namespace>
```
### Delete an Application

argocd app delete <app-name>

#### 3. Refresh Application

○ Refreshes the application state from the Git repository.

argocd app refresh <app-name>

### Application Resources and Syncing

### Sync Status Check

argocd app sync <app-name> --prune

#### 1. Compare with Live

○ Compare the live state of an application with the Git repository.

argocd app diff <app-name>

#### 2. Manual Sync

○ Manually sync the application state.

argocd app sync <app-name> --force

### Managing Projects

### Create a Project

argocd proj create <project-name> \

- --description "<description>" \
- --dest-namespace <namespace> \
- --dest-server <server-url>

## List Projects

argocd proj list

## Add a Git Repo to a Project

argocd proj add-repo <project-name> --repo <git-repository-url>

### GitOps and Source Repositories

### Add a Git Repository

argocd repo add <git-repo-url> --username <username> --password <password> --type git

## List Repositories

argocd repo list

### Remove a Git Repository

argocd repo rm <git-repo-url>

### Notifications and Alerts

#### 1. Enable Notifications

○ Install Argo CD Notifications to integrate with Slack, email, etc.

kubectl apply -k github.com/argoproj-labs/argocd-notifications/manifests/install

#### 2. Set Up Notification Settings

○ Configure notification settings in the Argo CD UI.

### Application Health and Troubleshooting

### Check Application Health

argocd app health <app-name>

#### 1. Check Logs

○ View logs for troubleshooting.

kubectl logs <pod-name> -n argocd

#### 2. App Rollback

○ Rollback to a previous revision of an application.

argocd app rollback <app-name> <revision>

### Argo CD in CI/CD Pipelines

#### ● Integrate with CI/CD

○ Add Argo CD commands in Jenkins, GitLab CI, or GitHub Actions pipelines to automatically deploy updates to Kubernetes based on changes in Git repositories.

### Best Practices

- Declarative GitOps: Keep all manifests in Git, and let Argo CD automatically synchronize and deploy them.
- Namespaces and Projects: Use projects to group applications and limit resource access across environments.
- RBAC: Use Role-Based Access Control (RBAC) to secure Argo CD's access and resource usage.

### Flux CD

### Introduction

Flux CD is a GitOps tool for Kubernetes that automates deployment, updates, and rollback of applications using Git as the source of truth.

Installation Install Flux CLI curl -s https://fluxcd.io/install.sh | sudo

### Verify Installation

flux --version

### Bootstrap Flux in a Cluster

```
flux bootstrap github \
 --owner=<GITHUB_USER_OR_ORG> \
 --repository=<REPO_NAME> \
 --branch=main \
 --path=clusters/my-cluster \
 --personal
```
### Key Flux CD Commands

### General Commands

| fluxcheck         | #CheckFluxinstallation                   |
|-----------------------|------------------------------------------------------|
| flux install          | - Install Flux components in a cluster               |
| flux bootstrap github | #SetupFluxinGitHubrepository |
| flux version          | - Show Flux CLI version                              |

### Managing Deployments

| flux           | -              |
|----------------|----------------|
| get            | List           |
| sources        | Git            |
| git            | sources        |
| flux           | -              |
| get            | List           |
| kustomizations | kustomizations |

flux reconcile kustomization <name> - Force sync a kustomization flux suspend kustomization <name> - Pause updates for a kustomization flux resume kustomization <name> - Resume updates for a kustomization

### Git Repository Management

```
flux create source git my-app \
 --url=https://github.com/my-org/my-app \
 --branch=main \
 --interval=1m
```

```
flux create kustomization my-app \
```

```
--source=my-app \
--path="./deploy" \
```
--prune=true \

--interval=5m

## Helm Chart Management

```
flux create source helm my-chart \
 --url=https://charts.bitnami.com/bitnami \
 --interval=1h
```

```
flux create helmrelease my-app \
```

```
--source=my-chart \
--chart=nginx \
--values=./values. \
--interval=5m
```
# Monitoring and Debugging

| fluxlogs                   | #ViewFluxlogs                                               |
|--------------------------------|-------------------------------------------------------------------------|
| fluxgetsourceshelm | #ListHelmsources                                            |
| fluxgethelmreleases    | #ListdeployedHelmreleases                               |
| fluxtracekustomization | <name>#Traceerrorsinakustomization</name> |

flux suspend source git <name> - Suspend Git syncing flux resume source git <name> - Resume Git syncing

### Uninstall Flux

flux uninstall --silent
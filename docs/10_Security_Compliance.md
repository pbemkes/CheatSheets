# Security & Compliance

- [SonarQube](#1-sonarqube-code-analysis)
- [Trivy](#2-trivy-container-vulnerability-scanning)
- [OWASP](#3-owasp-dependency-check-software-dependency-analysis)

---

## 1. SonarQube (Code Analysis)

### 1. Install SonarQube

- Download SonarQube from the official website: [https://www.sonarqube.org/downloads/](http://www.sonarqube.org/downloads/)

#### Extract the downloaded file:

```bash title="For Linux/macOS"
tar -xvzf sonarqube-<version>.zip
```
```bash title="For Windows"
unzip sonarqube-<version>.zip
```

### 2. Start SonarQube server

```bash title="For Linux/macOS"
./bin/linux-x86-64/sonar.sh start
```
```bash title="For Windows"
bin\windows-x86-64\StartSonar.bat
```

### 3. Access SonarQube dashboard

- Open your browser and go to: [http://localhost:9000](http://localhost:9000/) (default credentials: admin/admin)

### 4. Install SonarScanner (if not already installed)

- download and install from: [https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/)

### 5. Configure SonarScanner

- Set the SonarQube server URL in the `sonar-scanner.properties` file

!!! Example
    sonar.host.url=http://localhost:9000

### 6. Run analysis with SonarScanner

```bash
sonar-scanner -Dsonar.projectKey=<project_key> -Dsonar.sources=<source_directory> -Dsonar.host.url=http://localhost:9000
```

### 7. Maven: Run analysis with SonarQube

- Add SonarQube plugin to your Maven `pom.xml`

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.sonarsource.scanner.maven</groupId>
      <artifactId>sonar-maven-plugin</artifactId>
      <version>3.9.0.1100</version>
    </plugin>
  </plugins>
</build>
```

- Run SonarQube analysis with Maven

```bash
mvn clean verify sonar:sonar -Dsonar.host.url=http://localhost:9000
```

### 8. Set up SonarQube in Jenkins

- Install SonarQube Scanner Plugin in Jenkins (Manage Jenkins > Manage Plugins > Available > SonarQube Scanner for Jenkins)
- Configure SonarQube in Jenkins (Manage Jenkins > Configure System > SonarQube Servers)

### 9. Jenkins Pipeline for SonarQube analysis

```json
pipeline {
  agent any
  environment {
    SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
  }
  stages { 
    stage('Checkout') { 
    steps {
      git 'https://github.com/your-repo.git'
    }
  }
    stage('SonarQube Analysis') { 
      steps {
        script { 
          withSonarQubeEnv('SonarQubeServer') { 
          sh 'mvn sonar:sonar'
          }
        }
      }
    }
  }
}
```

### 10. GitLab CI/CD Integration for SonarQube

```yaml
stages:
  - code_analysis

sonarqube_scan:
  stage: code_analysis
  image: maven:3.8.7-openjdk-17
  script:
  - mvn sonar:sonar -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN
  variables:
    SONAR_HOST_URL: ["http://sonarqube-server:9000"](http://sonarqube-server:9000/)
    SONAR_TOKEN: "your-sonarqube-token"
```

### 11. GitHub Actions Integration for SonarQube

```yaml
name: SonarQube Analysis

on:
  push:
    branches:
    - main

jobs:
  sonar_scan:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Set up JDK
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: '17'
    - name: Run SonarQube Scan
      run: mvn sonar:sonar -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_TOKEN
      env:
        SONAR_HOST_URL: "http://sonarqube-server:9000"
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

### 12. ArgoCD Integration (PreSync Hook)

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sonarqube-analysis
annotations:
argocd.argoproj.io/hook: PreSync
spec:
  template:
    spec:
      containers:
      - name: sonar-scanner
        image: maven:3.8.7-openjdk-17
        command: ["mvn", "sonar:sonar"]
        env:
        - name: SONAR_HOST_URL
          value: "http://sonarqube-server:9000"
        - name: SONAR_TOKEN
          valueFrom:
            secretKeyRef:
            name: sonar-secret
            key: sonar-token
      restartPolicy: Never
```

---

## 2. Trivy (Container Vulnerability Scanning)

### Basic Commands

```bash title="Scan a Docker image"
trivy image <image-name>
```
```bash title="Scan a Kubernetes cluster"
trivy k8s cluster
```
```bash title="Generate a JSON report"
trivy image --format json -o report.json <image-name>
```

### Jenkins Integration

```groovy 
pipeline {
  agent any 
  stages {
    stage('Checkout') { 
      steps {
        git 'https://github.com/your-repo.git'
        }
    }
    stage('Trivy Scan') { 
      steps {
        sh 'trivy image your-docker-image:latest'
      }
    }
  }
}
```

### GitLab CI/CD Integration

```yaml
stages:
- security_scan

trivy_scan:
  stage: security_scan
  image: aquasec/trivy
  script:
  - trivy image your-docker-image:latest --format json -o trivy_report.json
  artifacts:
    paths:
    - trivy_report.json
```

### GitHub Actions Integration

```yaml
name: Trivy Scan
on:
  push:
    branches:
    - main

jobs:
  trivy_scan:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Run Trivy Scan
      run: |
        docker pull your-docker-image:latest
        trivy image your-docker-image:latest --format json --output trivy_report.json
    - name: Upload Trivy Report
      uses: actions/upload-artifact@v4
      with:
        name: trivy-report
        path: trivy_report.json
```

### ArgoCD Integration (PreSync Hook)

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: trivy-scan
annotations:
  argocd.argoproj.io/hook: PreSync
spec:
  template:
    spec:
      containers:
      - name: trivy-scanner
        image: aquasec/trivy
        command: ["trivy", "image", "your-docker-image:latest"]
      restartPolicy: Never
```

### Kubernetes Integration (Admission Controller)

```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: trivy-webhook
webhooks:
- name: trivy-scan.k8s
  rules:
  - apiGroups: [""]
    apiVersions: ["v1"]
    operations: ["CREATE"]
    resources: ["pods"]
  clientConfig:
    service:
      name: trivy-webhook-service
      namespace: security
      path: /validate
    admissionReviewVersions: ["v1"]
    sideEffects: None
```

---

## 3. OWASP Dependency-Check (Software Dependency Analysis)

### Basic Commands

```bash title="Run a scan on a project"
./dependency-check/bin/dependency-check.sh --scan /path/to/project
```

```bash title="Run a scan using Maven plugin"
mvn org.owasp:dependency-check-maven:check
```

### Jenkins Integration

```groovy 
pipeline {
  agent any 
  stages {
    stage('Checkout') { 
      steps {
        git 'https://github.com/your-repo.git'
      }
    }
    stage('OWASP Dependency Check') { 
      steps {
        sh 'mvn org.owasp:dependency-check-maven:check'
      }
    }
  }
}
```

### GitLab CI/CD Integration

```yaml
stages:
- security_scan

owasp_dependency_check:
  stage: security_scan
  image: maven:3.8.7-openjdk-17
  script:
  - mvn org.owasp:dependency-check-maven:check
  artifacts:
    paths:
    - target/dependency-check-report.html
```

### GitHub Actions Integration

```yaml
name: OWASP Dependency Check

on:
  push:
    branches:
    - main

jobs:
  owasp_dependency_check:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v4
    - name: Run OWASP Dependency-Check
      run: mvn org.owasp:dependency-check-maven:check
    - name: Upload OWASP Report
      uses: actions/upload-artifact@v4
      with:
        name: owasp-report
        path: target/dependency-check-report.html
```

### ArgoCD Integration (PreSync Hook)

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: owasp-dependency-check
annotations:
  argocd.argoproj.io/hook: PreSync
spec:
  template:
    spec:
      containers:
      - name: owasp-check
        image: maven:3.8.7-openjdk-17
        command: ["mvn", "org.owasp:dependency-check-maven:check"]
      restartPolicy: Never
```

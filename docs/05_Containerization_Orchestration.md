# Containerization & Orchestration

---

## Docker (build, run, volumes, networks, compose)

### Basic Commands

- `docker ps` - List running containers
- `docker ps -a` - List all containers
- `docker info` - Get Docker configuration
- `docker version` - Get Docker version

### Image Commands

- `docker build -t <image>:<tag> .` - Build an image from a Dockerfile
- `docker login <repository>` - Authenticate with a remote repository
- `docker push <image>:<tag>` - Push an image to a repository
- `docker pull <image>:<tag> ` -Pull an image from a repository
- `docker images` - List locally available images
- `docker create <image>:<tag>` - Create a container from an image
- `docker rmi <image>` - Delete an image
- `docker save <image>` - Save an image as a tarball
- `docker search <image>` - Search for an image in a repository

### Container Commands

- `docker inspect <container>` - View container details
- `docker stats <container>` - Display live resource usage
- `docker logs <container>` - View container logs
- `docker run <container>` - Run a container
- `docker kill <container>` - Force stop a running container
- `docker start <container>` - Start a stopped container
- `docker stop <container>` - Gracefully stop a running container
- `docker restart <container>` - Restart a container
- `docker rm <container>` - Remove a container
- `docker port <container>` - Show port mappings
- `docker pause <container>` - Suspend container processes
- `docker unpause <container>` - Resume container processes

### Network Commands

- `docker network ls` - List networks
- `docker network inspect <network>` - View network details
- `docker network create <network>` - Create a network
- `docker network rm <network>` - Delete a network
- `docker network connect <network> <container>` - Connect a container to a network
- `docker network disconnect <network> <container>` - Disconnect a container from a network

### Volume Commands

- `docker volume ls` - List volumes
- `docker volume inspect <volume>` - View volume details
- `docker volume create <volume>` - Create a volume
- `docker volume rm <volume>` - Delete a volume

### Copy & Execution Commands

- `docker cp <container>:<source_path> <dest_path>` - Copy from container to host
- `docker cp <source_path> <container>:<dest_path>` - Copy from host to container
- `docker exec -ti <container> <command>` - Run a command inside a running container

### Dockerfile Commands

- `FROM <image>:<tag>` - Base image for the container
- `COPY <source> <destination>` - Copy files/directories
- `ADD <source> <destination>` - Copy files & extract archives
- `CMD ["command", "arg1"]` - Default command executed in container
- `ENTRYPOINT ["command", "arg1"]` - Container's main command
- `LABEL key=value` - Add metadata
- `ENV key=value` - Set environment variables
- `EXPOSE <port>` - Declare exposed ports
- `RUN <command>` - Run command during image build
- `WORKDIR <path>` - Set working directory

### System & Diagnostics

- `docker system df` - Show Docker disk usage
- `docker system info` - Display system details
- `docker diff <container>` - Show modified files in a container
- `docker top <container>` - Show running processes inside a container

### General Best Practices for Dockerfiles:

1. **Minimize Layers** - Combine RUN, COPY, and ADD commands to reduce layers and image size.
2. **Use Specific Versions** - Always specify versions for base images (e.g., FROM python:3.9-slim).
3. **.dockerignore** - Use .dockerignore to exclude unnecessary files (e.g., .git, node_modules).
4. **Multi-Stage Builds** - Separate the build process and runtime environment to optimize image size.
5. **Non-root User** - Always create and use a non-root user for security.
6. **Leverage Docker Cache** - Copy dependencies first, so Docker can cache them for faster builds.

## Dockerfile Examples with different Programming language

### 1. Python (Flask/Django)

```dockerfile
FROM python:3.9-slim AS base

WORKDIR /app

# Install dependencies

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

# Copy the app files

COPY . .

EXPOSE 5000

# Run as a non-root user

RUN useradd -m appuser

USER appuser

CMD ["python", "app.py"]
```

#### Best Practices:

- --no-cache-dir to prevent caching Python packages.
- Copy requirements.txt first to leverage Docker cache.
- Use a non-root user (appuser).

### 2. Node.js

```dockerfile
FROM node:16-alpine AS build

WORKDIR /app

# Install dependencies

COPY package.json package-lock.json ./

RUN npm install --production

# Copy the app code

COPY . .

EXPOSE 3000

# Run as a non-root user

RUN addgroup --system app && adduser --system --ingroup app app

USER app

CMD ["node", "app.js"]
```

#### Best Practices:

- Use --production to avoid installing devDependencies.
- Multi-stage builds for optimized images.
- Use a non-root user (app).

### 3. Java (Spring Boot)

```dockerfile
FROM openjdk:17-jdk-slim AS build

WORKDIR /app

# Copy the jar file

COPY target/myapp.jar myapp.jar

EXPOSE 8080

# Run as a non-root user

RUN addgroup --system app && adduser --system --ingroup app app

USER app

CMD ["java", "-jar", "myapp.jar"]
```

#### Best Practices:

- Multi-stage builds for separating build and runtime.
- Use -jdk-slim for smaller images.
- Non-root user (app).

### 4. Ruby on Rails

```dockerfile
FROM ruby:3.0-alpine

# Install dependencies

RUN apk add --no-cache build-base

WORKDIR /app

# Install Ruby gems

COPY Gemfile Gemfile.lock ./

RUN bundle install --without development test

# Copy the app code

COPY . .

EXPOSE 3000

# Run as a non-root user

RUN addgroup --system app && adduser --system --ingroup app app

USER app

CMD ["rails", "server", "-b", "0.0.0.0"]
```

#### Best Practices:

- Install dependencies in one RUN statement.
- Avoid devDependencies in production.
- Use non-root user (app).

### 5. Go

```dockerfile
FROM golang:1.16-alpine AS build

WORKDIR /app

# Copy and install dependencies

COPY go.mod go.sum ./

RUN go mod tidy

# Copy the app code and build

COPY . .

RUN go build -o myapp .

# Use a minimal base image for running

FROM alpine:latest

WORKDIR /app

# Copy the binary

COPY --from=build /app/myapp .

EXPOSE 8080

# Run as a non-root user

RUN addgroup --system app && adduser --system --ingroup app app

USER app

CMD ["./myapp"]
```

#### Best Practices:

- Multi-stage build to separate build and runtime.
- alpine for smaller runtime images.
- Non-root user (app).

### 6. Angular (Frontend)

```dockerfile
# Build stage

FROM node:16 AS build

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build --prod

# Production stage using nginx

FROM nginx:alpine

## # Copy build artifacts from the build stage

COPY --from=build /app/dist/ /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

#### Best Practices:

- Multi-stage build: separate build and serving phases.
- Use nginx:alpine for a minimal serving environment.
- Copy only production build files.

### 7. PHP (Laravel)

```dockerfile
FROM php:8.0-fpm

# Install dependencies

RUN apt-get update && apt-get install -y libzip-dev && docker-php-ext-install zip

WORKDIR /var/www/html

# Install Composer

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Install PHP dependencies

COPY composer.json composer.lock ./

RUN composer install --no-dev --no-scripts

# Copy application files

COPY . .

EXPOSE 9000

# Run as a non-root user

RUN useradd -ms /bin/ appuser

USER appuser

CMD ["php-fpm"]
```

#### Best Practices:

- Use Composer for PHP dependency management.
- Avoid dev dependencies in production (--no-dev).
- Run PHP-FPM as a non-root user.

### 8. Best Practices for Security and Optimization:

- **Minimize Image Size** - Use smaller base images like alpine or slim, and multi-stage builds to reduce the final image size.
- **Use a Non-root User** - Always run applications as a non-root user to enhance security.
- **Pin Versions** - Avoid using the latest tag for images. Use specific versions to ensure predictable builds.
- **Leverage Caching** - Place frequently changing files (e.g., source code) after dependencies to take advantage of Docker's build cache.
- **Avoid ADD Unless Necessary** - Use COPY instead of ADD unless you need to fetch files from a URL or extract archives.

## Docker Compose Commands

- `docker-compose up` - Start all services in the background
- `docker-compose up -d` - Start services in detached mode
- `docker-compose up --build` - Rebuild images before starting services
- `docker-compose down` - Stop and remove containers, networks, volumes
- `docker-compose down -v` - Remove volumes along with containers
- `docker-compose stop` - Stop running containers without removing them
- `docker-compose start` - Restart stopped containers
- `docker-compose restart` - Restart all containers
- `docker-compose ps` - List running containers
- `docker-compose logs` - Show logs from containers
- `docker-compose logs -f` - Follow container logs
- `docker-compose exec <service> <cmd>` - Execute a command inside a running container
- `docker-compose run <service> <cmd>` - Run a one-time command inside a service
- `docker-compose config` - Validate and view merged configuration
- `docker-compose version` - Show Docker Compose version

## Docker Compose (docker-compose.yml) Example

```yaml
version: '3.8'
services:
  app:
    image: my-app:latest
    container_name: my_app
    ports:
    - "8080:80"
    environment:
    - NODE_ENV=production
    volumes:
    - ./app:/usr/src/app
    depends_on:
    - db
  db:
    image: postgres:latest
    container_name: my_db
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydatabase
    ports:
    - "5432:5432"
    volumes:
    - pgdata:/var/lib/postgresql/data
    volumes:
      pgdata:
```

### Key Directives

- **version** - Defines the Compose file format version
- **services** - Defines all application services
- **image** - Specifies the container image
- **container_name** - Names the container explicitly
- **build** - Specifies build context for Dockerfile
- **ports** - Maps container ports to host
- **volumes** - Mounts persistent storage
- **environment** - Passes environment variables
- **depends_on** - Specifies dependencies between services
- **restart** - Defines restart policy (always, unless-stopped, on-failure)

### General Docker Compose Structure

```yaml
version: '3'
services:
  service_name:
    image: <image-name> # The image to use
    build: . # Path to the Dockerfile if you need to build the image
    container_name: <name> # Container name (optional)
    ports:
    - "<host-port>:<container-port>" # Exposing ports
    environment:
    - VAR_NAME=value # Set environment variables
    volumes:
    - <host-path>:<container-path> # Mount volumes for persistent data
    depends_on: - other_service # Define service dependencies
    networks: - <network-name> # Assign the service to a network
```

## Docker Compose Example Configurations

### 1. Python (Flask) + Redis Example:

```yaml
version: '3'
services:
  web:
    build: ./app
    ports:
    - "5000:5000"
    environment:
    - FLASK_APP=app.py
    - FLASK_ENV=development
    volumes:
    - ./app:/app
    networks:
    - app_network
  redis:
    image: "redis:alpine"
    networks:
    - app_network
networks:
  app_network:
    driver: bridge
```

### 2. Node.js (Express) + MongoDB Example:

```yaml
version: '3'
services:
  app:
    build: ./node-app
    ports:
    - "3000:3000"
    environment:
    - MONGO_URI=mongodb://mongo:27017/mydb
    depends_on:
    - mongo
    networks:
    - backend
  mongo:
    image: mongo:latest
    volumes:
    - mongo_data:/data/db
    networks:
    - backend
networks:
  backend:
    driver: bridge
  volumes:
    mongo_data:
```

### 3. Nginx + PHP (Laravel) Example:

```yaml
version: '3'
services:
  nginx:
    image: nginx:alpine
    volumes:
    - ./nginx.conf:/etc/nginx/nginx.conf
    - ./html:/usr/share/nginx/html
    ports:
    - "8080:80"
    depends_on:
    - php
    networks:
    - frontend
  php:
    image: php:8.0-fpm
    volumes:
    - ./html:/var/www/html
    networks:
    - frontend
networks:
  frontend:
    driver: bridge
```

#### Best Practices

- **Use Versioning** - Always specify a version for Docker Compose files (e.g., version: '3')
- **Define Volumes** - Use named volumes for persistent data (e.g., database storage)
- **Environment Variables** - Use environment variables for configuration (e.g., database connection strings)
- **Use depends_on** - Ensure proper start order for dependent services
- **Custom Networks** - Use custom networks for better service communication management
- **Avoid latest Tag** - Always use specific version tags for predictable builds

### Advanced Options

#### Build Arguments:

- Pass information during the image build process

```yaml
build:
  context: .
  args:
    NODE_ENV: production
```

#### Health Checks:

- Add health checks to monitor service status

```yaml
services:
  web:
    image: my-web-app
    healthcheck:
      test: ["CMD", "curl", "-f", ["http://localhost/health"](http://localhost/health)]
      interval: 30s
      retries: 3
```

#### Scaling Services:

- Scale services using the command

```bash
docker-compose up --scale web=3
```

## Kubernetes (K8s)

### 1. Kubernetes Basics

```bash title="Display cluster information"
kubectl cluster-info`
```
```bash title="List all nodes in the cluster"
kubectl get nodes`
```
```bash title="List all pods in the current namespace"
kubectl get pods`
```
```bash title="List all services"
kubectl get services`
```
```bash title="List all deployments"
kubectl get deployments`
```

### 2. Managing Pods

```bash title="Create a pod with Nginx"
kubectl run my-pod` --image=nginx
```
```bash title="Delete a pod"
kubectl delete pod my-pod`
```
```bash title="View pod logs"
kubectl logs my-pod`
```
```bash title="Access a pod's shell"
kubectl exec -it my-pod -- /bin/sh`
```

### 3. Managing Deployments

```bash title="Create a deployment"
kubectl create deployment my-deploy --image=nginx`
```
```bash title="Scale deployment to 3 replicas"
kubectl scale deployment my-deploy --replicas=3`
```
```bash title="Check rollout status"
kubectl rollout status deployment my-deploy`
```
```bash title="Rollback to the previous version"
kubectl rollout undo deployment my-deploy`
```

### 4. Managing Services

```bash title="Expose deployment as a service"
kubectl expose deployment my-deploy --type=NodePort --port=80`
```
```bash title="List services"
kubectl get svc`
```
```bash title="Get service details"
kubectl describe svc my-service`
```

### 5. Namespaces

```bash title="List all namespaces"
kubectl get ns`
```
```bash title="Create a new namespace"
kubectl create namespace dev`
```
```bash title="Delete a namespace"
kubectl delete namespace dev`
```

### 6. ConfigMaps & Secrets

```bash title="Create a ConfigMap"
kubectl create configmap my-config --from-literal=key=value`
```
```bash title="List ConfigMaps"
kubectl get configmap`
```
```bash title="Create a secret"
kubectl create secret generic my-secret --from-literal=password=12345`
```
```bash title="List secrets"
kubectl get secrets`
```

### 7. Troubleshooting

```bash title="View cluster events"
kubectl get events
```
```bash title="Get detailed pod information"
kubectl describe pod my-pod
```
```bash title="View logs of a specific pod"
kubectl logs my-pod
```
```bash title="Show resource usage of pods"
kubectl top pod
```

### 8. Helm (Package Manager for Kubernetes)

```bash title="Add a Helm repo"
helm repo add stable https://charts.helm.sh/stable
```
```bash title="Install a Helm chart"
helm install my-release stable/nginx
```
```bash title="List installed releases"
helm list
```
```bash title="Uninstall a release"
helm delete my-release
```

### 9. Persistent Volumes & Storage

```bash title="List persistent volume claims"
kubectl get pvc
```
```bash title="List persistent volumes"
kubectl get pv
```
```bash title="Describe a persistent volume claim"
kubectl describe pvc <pvc>
```
```bash title="Delete a persistent volume claim"
kubectl delete pvc <pvc>
```

### 10. Autoscaling

```bash title="Enable autoscaling"
kubectl autoscale deployment <deployment> --cpu-percent=50 --min=1 --max=10
```
```bash title="View horizontal pod autoscaler"
kubectl get hpa
```

### 11. Kubernetes Debugging

```bash title="Show events"
kubectl get events --sort-by=.metadata.creationTimestamp
```
```bash title="Show pod details"
kubectl describe pod <pod>
```
```bash title="Check logs"
kubectl logs <pod>
```
```bash title="Access pod shell"
kubectl exec -it <pod> -- /bin/sh
```

### Kubernetes YAML Configurations

#### 1. Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx
  image: nginx:latest
  ports:
  - containerPort: 80
```

#### 2. Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
  matchLabels:
    app: my-app
  template:
  metadata:
    labels:
      app: my-app
    spec:
      containers:
      - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

#### 3. ReplicaSet

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
  matchLabels:
    app: my-app
  template:
  metadata:
    labels:
    app: my-app
  spec:
    containers:
      - name: nginx
      image: nginx:latest
```
#### 4. Service (ClusterIP, NodePort, LoadBalancer)

- ClusterIP (default)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
  port: 80
  targetPort: 80
```

- NodePort

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  type: NodePort
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
```

- LoadBalancer

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

#### 5. ConfigMap

```yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key1: value1
  key2: value2
```

#### 6. Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
  data:
    password: cGFzc3dvcmQ= # Base64 encoded value
```

#### 7. Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

#### 8. Persistent Volume Claim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

#### 9. Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: my-service
          port:
            number: 80
```

#### 10. Horizontal Pod Autoscaler (HPA)

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

#### 11. CronJob

```yaml

apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: my-cron
            image: busybox
            command: ["echo", "Hello from CronJob"]
          restartPolicy: OnFailure
```

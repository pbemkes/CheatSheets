# Networking, Ports & Load Balancing

## Networking Basics

- IP Addressing
	- Private IPs: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`
	- Public IPs: Assigned by ISPs
	- CIDR Notation: 192.168.1.0/24 (Subnet Mask: 255.255.255.0)
- Ports
	- HTTP: 80
	- HTTPS: 443
	- SSH: 22
	- DNS: 53
	- FTP: 21
	- MySQL: 3306
	- PostgreSQL: 5432
- Protocols
	- TCP (Reliable, connection-based)
	- UDP (Fast, connectionless)
	- ICMP (Used for ping)
	- HTTP(S), FTP, SSH, DNS

## 2. Network Commands

### Linux Networking Show network interfaces

```bash title="Show IP addresses"
ip a
```
```bash title="Older command"
ifconfig
```

### Check connectivity

```bash title="Check site/host is reachable"
ping google.com
```
```bash title="Trace route"
traceroute google.com
```
```bash title="Query Internet name servers"
nslookup google.com
```
```bash title="DNS lookup"
dig google.com
```
```bash title="Test ports"
telnet google.com 80

nc -zv google.com 443
```

### Firewall Rules (iptables)

```bash title="List firewall rules"
sudo iptables -L -v
```
```bash title="Allow SSH"
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
```bash title="Block an IP"
sudo iptables -A INPUT -s 192.168.1.100 -j DROP
```
```bash title="Netcat (nc) Start a simple TCP listener"
nc -lvp 8080
```
```bash title="Send data to a listening server"
echo "Hello" | nc 192.168.1.100 8080
```

## 3. Kubernetes Networking

```bash title="List services and their endpoints"
kubectl get svc -o wide
```
```bash title="Get pods and their IPs"
kubectl get pods -o wide
```
```bash title="Port forward a service"
kubectl port-forward svc/my-service 8080:80
```
```bash title="Expose a pod"
kubectl expose pod mypod --type=NodePort --port=80
```

## 4. Docker Networking

```bash title="List networks"
docker network ls
```
```bash title="Inspect a network"
docker network inspect bridge
```
```bash title="Create a custom network"
docker network create mynetwork
```
```bash title="Run a container in a custom network"
docker run -d --network=mynetwork nginx
```

## 5. Cloud Networking (AWS, Azure, GCP)

### AWS

```bash title="List VPCs"
aws ec2 describe-vpcs
```
```bash title="List subnets"
aws ec2 describe-subnets
```
```bash title="Open security group port"
aws ec2 authorize-security-group-ingress --group-id sg-12345 --protocol tcp --port 22 --cidr 0.0.0.0/0
```

### Azure

```bash title="List VNets"
az network vnet list -o table
```
```bash title="List NSGs"
az network nsg list -o table
```
```bash title="Open a port in NSG"
az network nsg rule create --resource-group MyGroup --nsg-name MyNSG --name AllowSSH --protocol Tcp --direction Inbound --priority 100 --source-address-prefixes'\*' --source-port-ranges '\*' --destination-port-ranges 22 --access Allow
```

### AWS VPC Basics

- Definition: A logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network.
- CIDR Block: Define the IP range (e.g., 10.0.0.0/16).
- Components:
	- Subnets: Divide your VPC into public (with internet access) and private (without direct internet access) segments.
	- Route Tables: Control the traffic routing for subnets.
	- Internet Gateway (IGW): Allows communication between instances in your VPC and the internet.
	- NAT Gateway/Instance: Enables outbound internet access for instances in private subnets.
	- VPC Peering: Connects multiple VPCs.
	- VPN Connections & Direct Connect: Securely link your on-premises network with your VPC.
	- VPC Endpoints: Privately connect your VPC to supported AWS services.

### Security Groups Essentials

- Definition: Virtual firewalls that control inbound and outbound traffic for your EC2 instances.
- Key Characteristics:
	- Stateful: Return traffic is automatically allowed regardless of inbound/outbound rules.
	- Default Behavior: All outbound traffic is allowed; inbound is denied until explicitly allowed.
- Rule Components:
	- Protocol: (TCP, UDP, ICMP, etc.)
	- Port Range: Specific ports or a range (e.g., port 80 for HTTP).
	- Source/Destination: IP addresses or CIDR blocks (e.g., 0.0.0.0/0 for all).
- Usage:
	- Assign one or more security groups to an instance.
	- Modify rules anytime without stopping or restarting the instance.

### Common AWS CLI Commands

#### VPC Operations

```bash title="Create a VPC:"
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```
```bash title="Create a Subnet:"
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block 10.0.1.0/24
```
```bash title="Create & Attach an Internet Gateway:"
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id <vpc-id> --internet-gateway-id <igw-id>
```
```bash title="Associate a Route Table:"
aws ec2 associate-route-table --subnet-id <subnet-id> --route-table-id <rtb-id>
```

#### Security Group Operations

```bash title="Create a Security Group:"
aws ec2 create-security-group --group-name MySecurityGroup --description "My security group" --vpc-id <vpc-id>
```
```bash title="Authorize Inbound Traffic:"
aws ec2 authorize-security-group-ingress --group-id <sg-id> --protocol tcp --port 80 --cidr 0.0.0.0/0
```
```bash title="Authorize Outbound Traffic (if restricting defaults):"
aws ec2 authorize-security-group-egress --group-id <sg-id> --protocol tcp --port 443 --cidr 0.0.0.0/0
```
```bash title="List Security Groups:"
aws ec2 describe-security-groups
```

## Ports

### DevOps Essential Port

#### Networking & Security

- **SSH** `22` (Secure remote access)
- **FTP** `21` (File Transfer Protocol)
- **SFTP** `22` (Secure File Transfer Protocol)
- **Telnet** `23` (Unsecured remote access)
- **SMTP** `25, 587` (Email sending)
- **DNS** `53` (Domain Name System)
- **DHCP** `67/68` (Dynamic IP assignment)
- **HTTP** `80` (Default web traffic)
- **HTTPS** `443` (Secure web traffic)
- **SMB** `445` (Windows file sharing)
- **LDAP** `389` (Directory services)
- **LDAPS** `636` (Secure LDAP)
- **RDP** `3389` (Remote Desktop Protocol)

#### CI/CD & DevOps Tools

- **Jenkins** `8080` (CI/CD automation server)
- **Git** `9418` (Git repository access)
- **SonarQube** `9000` (Code quality analysis)
- **Nexus Repository** `8081` (Artifact repository)
- **Harbor** `443` (Container registry)
- **GitLab CI/CD** `443, 80, 22` (GitLab services & SSH)
- **Bitbucket** `7990` (Bitbucket web UI)
- **TeamCity** `8111` (CI/CD server)

#### Containerization & Orchestration

- **Docker Registry** `5000` (Private Docker Registry)
- **Kubernetes API Server** `6443` (Cluster API)
- **Kubelet** `10250` (Node agent)
- **ETCD (Kubernetes Storage)** `2379-2380` (Key-value store)
- **Flannel (Kubernetes Networking)** `8285/8286` (Overlay network)
- **Calico (Kubernetes Networking)** `179` (BGP Protocol)
- **Istio Ingress Gateway** `15021, 15090` (Service mesh ingress)

#### Monitoring & Logging

- **Prometheus** `9090` (Metrics monitoring)
- **Grafana** `3000` (Visualization dashboard)
- **Elasticsearch** `9200` (Search & analytics engine)
- **Logstash** `5044` (Log ingestion)
- **Fluentd** `24224` (Log collector)
- **Kibana** `5601` (Log visualization)
- **Loki** `3100` (Log aggregation)
- **Jaeger** `16686` (Tracing UI)

#### Databases

- **PostgreSQL** `5432` (Relational database)
- **MySQL/MariaDB** `3306` (Relational database)
- **MongoDB** `27017` (NoSQL database)
- **Redis** `6379` (In-memory database)
- **Cassandra** `9042` (NoSQL distributed database)
- **CockroachDB** `26257` (Distributed SQL database)
- **Neo4j** `7474` (Graph database UI), 7687 (Bolt protocol)
- **InfluxDB** `8086` (Time-series database)
- **Couchbase** `8091` (Web UI), 11210 (Data access)

#### Message Brokers & Caching

- **Kafka** `9092` (Event streaming)
- **RabbitMQ** `5672` (Message broker)
- **ActiveMQ** `61616` (JMS messaging)
- **NATS** `4222` (High-performance messaging)
- **Memcached** `11211` (In-memory caching)

#### Web Servers & Reverse Proxies

- **Nginx** `80, 443` (Web server & reverse proxy)
- **Apache HTTP** `80, 443` (Web server)
- **HAProxy** `443, 80` (Load balancer)
- **Caddy** `2019` (API endpoint)

#### Cloud Services & Storage

- **AWS S3** `443` (Object storage API)
- **AWS RDS** `3306, 5432, 1433` (Managed databases)
- **Azure Blob Storage** `443` (Storage API)
- **Google Cloud Storage** `443` (Object storage API)
- **MinIO** `9000` (S3-compatible storage)

## Nginx (Reverse Proxy & Load Balancing)

!!! info "What is a Reverse Proxy?"
    A Reverse Proxy is a server that forwards client requests to backend servers. It helps:

- Improve security by hiding backend servers.
- Handle traffic and reduce load on backend servers.
- Improve performance with caching and compression.

!!! info "What is Load Balancing?"
    Load Balancing distributes traffic across multiple servers to:

- Prevent overloading of a single server.
- Ensure high availability (if one server fails, others handle traffic).
- Improve speed and performance.

### 1. Nginx Reverse Proxy & Load Balancing

#### Reverse Proxy

!!! note "Reverse Proxy (Forward Requests to Backend)"
    When a user visits example.com, Nginx forwards the request to the backend server.

- Configuration File (nginx.conf)

```conf
server { 
    listen 80; # Listen for requests on port 80
    server_name example.com; # Your domain name
    location / {
        proxy_pass http://backend_servers; # Forward requests to backend 
        proxy_set_header Host $host; # Keep the original host 
        proxy_set_header X-Real-IP $remote_addr; # Send real client IP 
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

!!! info "What This Does:"
    - Nginx forwards requests from example.com to backend servers.
    - proxy_pass tells Nginx where to send traffic.
    - Keeps client IP and host name intact for logs.

#### Load Balancing

!!! note "Load Balancing (Distribute Traffic Across Multiple Servers)"
    Instead of sending all traffic to one server, Nginx distributes it across multiple servers.

- Configuration File (nginx.conf)

```conf
upstream backend_servers {
  server server1.example.com; # Backend Server 1 
  server server2.example.com; # Backend Server 2
}
server {
  listen 80;
  server_name example.com;

  location / {
    proxy_pass http://backend_servers; # Send traffic to multiple backend servers
  }
}
```

!!! info "What This Does:"
    - upstream defines multiple backend servers.
    - Traffic is balanced between server1 and server2.
    - Default method: Round-robin (each request goes to the next server).

### 2. Apache (reverse proxy, load balancing)

#### Enable Required Modules

!!! warning ""
    Before using Apache as a Reverse Proxy, enable these modules:

```bash
a2enmod proxy
```
```bash
a2enmod proxy_http
```
```bash
a2enmod proxy_balancer
```
```bash
a2enmod lbmethod_byrequests
```
```bash
systemctl restart apache2 # Restart Apache for changes
```

!!! info "What This Does:"
    These modules allow Apache to forward requests and balance traffic.

#### Reverse Proxy (Forward Requests to Backend Servers)

- Configuration File (apache.conf)

```conf
<VirtualHost \*:80>
  ServerName example.com # Domain handled by Apache

  ProxyPass "/" "http://backend_servers/" 
  ProxyPassReverse "/" "http://backend_servers/"
</VirtualHost>
```

!!! info "What This Does:"
    - Apache listens on example.com and forwards requests to backend servers.
    - ProxyPassReverse ensures responses return correctly to the client.

#### Load Balancing (Send Traffic to Multiple Backend Servers)

- Configuration File (apache.conf)

```conf
<Proxy "balancer://mycluster">
  BalancerMember "http://server1.example.com" 
  BalancerMember "http://server2.example.com"
</Proxy>

<VirtualHost \*:80>
  ServerName example.com

  ProxyPass "/" "balancer://mycluster/"
  ProxyPassReverse "/" "balancer://mycluster/"
</VirtualHost>
```

!!! info "What This Does:"
    - BalancerMember defines multiple backend servers.
    - Apache automatically distributes traffic using round-robin.

### 3. HAProxy (Load Balancing)

!!! note ""
    HAProxy is a lightweight and high-performance Load Balancer for web applications.

#### Install HAProxy

```bash title="Ubuntu/Debian"
apt install haproxy
```
```bash title="RHEL/CentOS"
yum install haproxy
```

#### Load Balancing with HAProxy

- Configuration File (haproxy.cfg)

```conf
frontend http_front
  bind \*:80 # Accept traffic on port 80
  default_backend backend_servers # Forward traffic to backend servers

backend backend_servers
  balance roundrobin # Distribute traffic evenly
  server server1 server1.example.com:80 check # First server
  server server2 server2.example.com:80 check # Second server
```

!!! info "What This Does:"
    - frontend handles incoming requests.
    - backend defines multiple backend servers.
    - Round-robin ensures traffic is evenly distributed.
    - check makes sure only healthy servers receive traffic.

```bash title="Restart HAProxy"
systemctl restart haproxy
```
```bash title="Enable on startup"
systemctl enable haproxy
```

### 4. Kubernetes Ingress Controller

- Install Nginx Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

!!! info "What This Does:"
    Installs Nginx Ingress Controller for managing external traffic in Kubernetes.

#### Define an Ingress Resource

- Configuration File (ingress.yaml)

```yaml
apiVersion: networking.k8s.io/v1 
kind: Ingress
metadata:
 name: my-ingress 
 annotations:
  nginx.ingress.kubernetes.io/rewrite-target: / # Optional URL rewrite 
spec:
  rules:
  - host: example.com # Define the domain 
    http:
      paths:
      - path: / 
        pathType: Prefix
      backend:
        service:
          name: my-service # Forward traffic to this Kubernetes service
          port:
            number: 80
```

!!! info "What This Does:"
    - Routes traffic from example.com to my-service inside Kubernetes.
    - Annotations modify behavior (like URL rewriting).

- Verify Ingress is Working

```bash
kubectl get ingress kubectl describe ingress my-ingress
```

!!! info "What This Does:"
    - kubectl get ingress → Checks if Ingress exists.
    - kubectl describe ingress → Shows detailed configuration.

### Which One Should You Use?

- For a simple website/API → Use Nginx Reverse Proxy.
- For balancing multiple servers → Use Nginx, Apache, or HAProxy.
- For Kubernetes applications → Use Ingress Controller.

## Practical Examples: Docker for Nginx, Apache, HAProxy, and Kubernetes Ingress

- Step-by-step practical examples using Docker for Nginx, Apache, HAProxy, and Kubernetes Ingress.

### 1. Nginx Reverse Proxy & Load Balancer (With Docker)

!!! note "Scenario:"
    We have two backend servers running a Python Flask application, and we want Nginx to act as a Reverse Proxy and Load Balancer.

#### Step 1: Create Two Backend Servers (Flask) Create a directory for the project

```bash
mkdir nginx-loadbalancer && cd nginx-loadbalancer
```

- server1.py (Backend Server 1)

```py
from flask import Flask
app = Flask( name )

@app.route('/')
def home():
  return "Hello from Server 1"

if name == ' main ':
  app.run(host='0.0.0.0', port=5000)
```

- server2.py (Backend Server 2)

```py
from flask import Flask
app = Flask( name )

@app.route('/')
def home():
  return "Hello from Server 2"

if name == ' main ':
  app.run(host='0.0.0.0', port=5000)
```

#### Step 2: Create a Dockerfile for Backend Servers Dockerfile

```dockerfile
FROM python:3.9

WORKDIR /app

COPY server1.py /app/

RUN pip install flask

CMD ["python", "server1.py"]
```

!!! info ""
    For server2.py, create another Dockerfile and replace `server1.py` with `server2.py`


#### Step 3: Create an Nginx Configuration File nginx.conf

```conf
events {}
http {
  upstream backend_servers {
    server server1:5000;
    server server2:5000;
  }

  server {
    listen 80;
    server_name localhost;
    location / {
      proxy_pass http://backend_servers;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }
}
```

#### Step 4: Create a Docker Compose File

- docker-compose.yml

```yaml
version: '3'
services:
  server1:
    build: .
    container_name:server1
    ports:
    - "5001:5000"

  server2:
    build: .
    container_name:server2
    ports:
    - "5002:5000"

  nginx:
    image: nginx:latest
    container_name: nginx_proxy
    ports:
    - "80:80"
    volumes:
    - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
    - server1
    - server2
```

#### Step 5: Run the Containers

```bash
docker-compose up --build
```

#### Step 6: Test Load Balancing Run the following command

```bash
curl http://localhost
```

#### Expected Output (requests will alternate)

- Hello from Server 1
- Hello from Server 2
- Hello from Server 1
- Hello from Server 2

### 2. Apache Reverse Proxy & Load Balancer (With Docker)

#### Step 1: Create Apache Configuration File `apache.conf`

```conf
<VirtualHost \*:80>
  ServerName localhost
  <Proxy "balancer://mycluster">
    BalancerMember "http://server1:5000"
    BalancerMember "http://server2:5000"
  </Proxy>

  ProxyPass "/" "balancer://mycluster/"
  ProxyPassReverse "/" "balancer://mycluster/"
</VirtualHost>
```

#### Step 2: Create docker-compose.yml for Apache

```yaml
version: '3'
services:
  server1:
    build: .
    container_name:server1
    ports:
    - "5001:5000"
  server2:
    build: .
    container_name:server2
    ports:
    - "5002:5000"
  apache:
    image: httpd:latest
    container_name: apache_proxy
    ports:
    - "80:80"
    volumes:
    - ./apache.conf:/usr/local/apache2/conf/httpd.conf
    depends_on:
    - server1
    - server2
```

#### Step 3: Run Apache Proxy

```bash
docker-compose up --build
```

### 3. HAProxy Load Balancer (With Docker)

#### Step 1: Create HAProxy Configuration File `haproxy.cfg`

```conf
frontend http_front
  bind \*:80
  default_backend backend_servers
backend backend_servers
  balance roundrobin
  server server1 server1:5000 check
  server server2 server2:5000 check
```

#### Step 2: Create `docker-compose.yml` for HAProxy

```yaml
version: '3'
services:
  server1:
    build: .
    container_name:server1
    ports:
    - "5001:5000"
  server2:
    build: .
    container_name:server2
    ports:
    - "5002:5000"
  haproxy:
    image: haproxy:latest
    container_name: haproxy_loadbalancer
    ports:
    - "80:80"
    volumes:
    - ./haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg
    depends_on:
    - server1
    - server2
```

#### Step 3: Run HAProxy

```bash
docker-compose up --build
```

### 4. Kubernetes Ingress Controller

#### Step 1: Deploy Nginx Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.
```

#### Step 2: Create Ingress Resource

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

#### Step 3: Apply Ingress

```bash
kubectl apply -f ingress.
```

### Comparison Table

- **Nginx**: Reverse Proxy (forwards requests to backend servers), Load Balancing (distributes traffic across servers)
- **Apache**: Reverse Proxy (similar to Nginx), Load Balancing (balances traffic using a balancer)
- **HAProxy**: Load Balancing (efficient traffic handling)
- **Kubernetes Ingress**: Traffic Routing (manages external traffic in Kubernetes)

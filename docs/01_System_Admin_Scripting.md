# System Administration & Scripting

- [Linux commands](#linux-commands)
- [Shell Scripting](#shell-scripting)
- [Pyton](#python)

---

## Linux commands

### File Management

- `ls` - List directory contents.
- `cd` - Change directory.
- `pwd` - Print working directory.
- `cp` - Copy files or directories.
- `mv` - Move or rename files or directories.
- `rm` - Remove files or directories.
- `touch` - Create a new empty file.
- `mkdir` - Create a new directory.
- `rmdir` - Remove an empty directory.
- `cat` - Concatenate and display file contents.
- `head` - Display the first few lines of a file.
- `tail` - Display the last few lines of a file.
- `chmod` - Change file permissions.
- `chown` - Change file ownership.
- `find` - Search for files in a directory hierarchy.
- `locate` - Find files by name.
- `grep` - Search text using patterns.
- `diff` - Compare two files line by line.
- `tar` - Archive files.
- `zip/unzip` - Compress and extract files.
- `scp` - Securely copy files over SSH.

### System Information and Monitoring

- `top` - Display running processes and system usage.
- `htop` - Interactive process viewer.
- `ps` - Display current processes.
- `df` - Show disk space usage.
- `du` - Show directory space usage.
- `free` - Show memory usage.
- `uptime` - Show system uptime.
- `uname` - Show system information.
- `whoami` - Display the current logged-in user.
- `lsof` - List open files and associated processes.
- `vmstat` - Report virtual memory statistics.
- `iostat` - Report I/O statistics.
- `netstat` - Display network connections and routing tables.
- `ifconfig` - Display or configure a network interface.
- `ping` - Check network connectivity.
- `traceroute` - Track the route packets take to a destination.
- `date` - Show or set the system date and time.
- `env` - Display environment variables.
- `source` - Execute commands from a file in the current shell.

### Package Management (Ubuntu/Debian)

- `apt-get` - Install, Remove or update packages.
    - `apt-get update` - Update package lists.
    - `apt-get upgrade` - Upgrade all packages.
    - `apt-get install` - Install packages.
    - `apt-get remove` - Remove packages.
- `dpkg` - Install, remove, and manage individual Debian packages.

```bash title="Update package lists"
sudo apt-get update
```

```bash title="Install NGINX"
sudo apt-get install nginx
```

### User and Permission Management

- `useradd` - Add a new user.
- `userdel` - Delete a user.
- `usermod` - Modify a user.
- `passwd` - Change user password.
- `groupadd` - Create a new group.
- `groupdel` - Delete a group.
- `groups` - Show groups of a user.
- `su` - Switch user.
- `sudo` - Execute a command as another user, usually root.

```bash title="Add a new user"
sudo useradd -m newuser
```

```bash title="Change file permissions"
chmod 755 script.sh
```

```bash title="Change file owner"
sudo chown newuser file.txt
```

### Networking

- `curl` - Transfer data from or to a server.
- `wget` - Download files from the internet.
- `ssh` - Secure shell to a remote server.
- `telnet` - Connect to a remote machine.
- `nslookup` - Query DNS records.
- `dig` - DNS lookup utility.
- `iptables` - Configure firewall rules.
- `firewalld` - Firewall management (CentOS/RHEL).
- `hostname` - Show or set the system hostname.
- `ping` - Check connectivity to a host.

```bash title="send ICMP ECHO_REQUEST network hosts"
ping google.com
```

```bash title="Send HTTP requests"
curl https://example.com
```

```bash title="View network interfaces"
ifconfig
```

### Process Management

- `kill` - Send a signal to a process.
- `killall` - Kill processes by name.
- `pkill` - Kill processes by pattern matching.
- `bg` - Move a job to the background.
- `fg` - Bring a job to the foreground.
- `jobs` - List background jobs.
- `ps` - Show running processes.

```bash title="List processes related to nginx"
ps aux | grep nginx
```

```bash title="Kill process with PID 1234"
kill 1234
```

```bash title="Kill all nginx processes"
pkill nginx
```

### Disk Management

- `fdisk` - Partition a disk.
- `mkfs` - Make a filesystem.
- `mount` - Mount a filesystem.
- `umount` - Unmount a filesystem.
- `lsblk` - List block devices.
- `blkid` - Print block device attributes.
- `fdisk` - Manage disk partitions.
- `mount` - Mount a filesystem.

```bash title="List disk partitions"
sudo fdisk -l
```

```bash title="Mount device sdb1 to /mnt"
sudo mount /dev/sdb1 /mnt
```

### Text Processing

- `awk` - Pattern scanning and processing.
- `sed` - Stream editor for modifying text.
- `sort` - Sort lines of text files.
- `uniq` - Report or omit repeated lines.
- `cut` - Remove sections from each line of files.
- `wc` - Word, line, character count.
- `tr` - Translate or delete characters.
- `nl` - Number lines of files.
- `grep` - Search text.
- `awk` - Process text with patterns.
- `sed` - Edit text in streams.

```bash title="Search for 'error' in syslog"
grep "error" /var/log/syslog
```

```bash title="Print the first column of each line"
awk '{print $1}' file.txt
```

```bash title="Replace 'old' with 'new'"
sed 's/old/new/g' file.txt
```

### Logging and Auditing

- `dmesg` - Print or control kernel ring buffer.
- `journalctl` - Query the systemd journal.
- `logger` - Add entries to the system log.
- `last` - Show listing of last logged-in users.
- `history` - Show command history.
- `tail` - View end of file in real time.
- `tail -f` - Monitor logs in real time.

```bash title="Follow NGINX access log"
tail -f /var/log/nginx/access.log
```

```bash title="Logs for NGINX service"
sudo journalctl -u nginx
```

### Archiving and Backup

- `tar` - Archive files.
- `rsync` - Synchronize files and directories.
- `tar` - Archive files.
- `rsync` - Sync files and directories.

```bash title="Create an archive"
tar -cvf archive.tar /path/to/files 
```

```bash title="Extract an archive"
tar -xvf archive.tar 
```

```bash title="Sync with compression and archive mode"
rsync -avz /source /destination
```

---

### Shell Scripting

- `echo` - Display message or text.
- `read` - Read input from the user.
- `export` - Set environment variables.
- `alias` - Create shortcuts for commands.
- `sh` - Execute shell scripts.
- `sleep` - Execute commands from a file in the current shell

```bash title="Print message"
echo "Hello, DevOps!"
```

```bash title="Add to PATH variable"
export PATH=$PATH:/new/path
```

### System Configuration and Management

- `crontab` - Schedule periodic tasks.
- `systemctl` - Control the systemd system and service manager.
- `service` - Start, stop, or restart services.
- `timedatectl` - Query and change the system clock.
- `reboot` - Restart the system.
- `shutdown` - Power off the system.

```bash title="Edit the crontab file"
crontab -e

0 2 - - - /path/to/backup.sh
```

```bash title="Restart NGINX"
sudo systemctl restart nginx
```

### Containerization & Virtualization

- `docker` - Manage Docker containers.

```bash title="List running containers"
docker ps
```

```bash title="Run NGINX container"
docker run -d -p 8080:80 nginx
```

- `kubectl` - Manage Kubernetes clusters.

```bash title="List all pods"
kubectl get pods
```

```bash title="Deploy configuration"
kubectl apply -f deployment.yaml
```

### Git Version Control

- `git status` - Show the status of changes.
- `git add` - Add files to staging.
- `git commit` - Commit changes.
- `git push` - Push changes to a remote repository.
- `git pull` - Pull changes from a remote repository.
- `git clone` - Clone a repository.
- `git remote` - Manage set of tracked repositories.

```bash title="Commit changes with a descriptive message"
git commit -m "<message>"
```

```bash title="Push changes to a remote repository"
git push <remote> <branch>
```

```bash title="Pull changes from a remote repository"
git pull <remote> <branch>
```

```bash title="Clone a repository"
git clone <repository>
```

```bash title="Show URLs of remote repositories"
git remote -v
```

```bash title="Add a new remote repository"
git remote add <name> <url>
```

```bash title="Remove a remote repository by name"
git remote remove <name>
```

```bash title="Rename a remote repository"
git remote rename <old-name> <new-name>
```

### Network Troubleshooting and Analysis

- `traceroute` - Track packet route.
- `netstat` - View network connections, routing tables, and more.
- `ss` - Display socket statistics (modern alternative to netstat).
- `iptables` - Manage firewall rules.

```bash title="Show hops to google.com"
traceroute google.com
```

```bash title="Show active listening ports with protocol info"
netstat -tuln
```

```bash title="Show active listening ports"
ss -tuln
```

```bash title="List current iptables rules"
sudo iptables -L
```

### Advanced File Management

- `find` - Search files by various criteria.
- `locate` - Quickly find files by name.

```bash title="Find all '-.log' files under '/var'"
find /var -name "-.log"
```

```bash title="Find directories named 'test'"
find /home -type d -name "test"
```

```bash title="Locate apache2 configuration file"
locate apache2.conf
```

### File Content and Manipulation

- `split` - Split files into parts.
- `sort` - Sort lines in files.
- `uniq` - Remove duplicates from sorted files

```bash title="Split file into 500-line chunks"
split -l 500 largefile.txt smallfile.txt
```

```bash title="Sort lines alphabetically"
sort files.sort file.txt
```

```bash title="Sort numerically"
sort -n numbers.txt
```

```bash title="Remove duplicate lines"
sort file.txt | uniq
```

### Advanced Shell Operations

- `xargs` - Build and execute commands from standard input.
- `tee` - Read from standard input and write to standard output and files.

```bash title="Delete all .txt files"
find . -name "-.txt" | xargs rm
```

```bash title="Write output to file and terminal"
echo "new data" | tee file.txt
```

### Performance Analysis

- `iostat` - Display CPU and I/O statistics.
- `vmstat` - Report virtual memory stats.
- `sar` - Collect and report system activity information.

```bash title="Show disk I/O stats every 2 seconds"
iostat -d 2
```

```bash title="Display 5 samples at 1-second intervals"
vmstat 1 5
```
```bash title="Report CPU usage every 5 seconds"
sar -u 5 5
```

### Disk and File System Analysis

- lsblk: List block devices.
- blkid: Display block device attributes.
- ncdu: Disk usage analyzer with a TUI.

```bash title="Show filesystems and partitions"
lsblk -f
```

```bash title="Show UUIDs for devices"
sudo blkid
```

```bash title="Analyze root directory space usage"
ncdu /
```

### File Compression and Decompression

- `gzip` - Compress files.
- `gunzip` - Decompress .gz files.
- `bzip2` - Compress files with higher compression than gzip.

```bash title="Compress file with .gz extension"
gzip largefile.txt
```

```bash title="Decompress file"
gunzip largefile.txt.gz
```

```bash title="Compress file with .bz2 extension"
bzip2 largefile.txt
```

### Environment Variables and Shell Management

- `env` - Display all environment variables.
- `unset` - Remove an environment variable.
- `set` - Set or display shell options and variables.

```bash title="Display envirnomental variables"
env
```

```bash title="Show the PATH variable"
set | grep PATH
```

```bash title="Remove a specific environment variable"
unset VAR_NAME
```

### Networking Utilities

- `arp` - Show or modify the IP-to-MAC address mappings.
- `nc (netcat)` - Network tool for debugging and investigation.
- `nmap` - Network scanner to discover hosts and services.

```bash title="Display all IP-MAC mappings"
arp -a
```

```bash title="Test if a specific port is open"
nc -zv example.com 80
```

```bash title="Scan all hosts on a subnet"
nmap -sP 192.168.1.0/24
```

### System Security and Permissions

- `umask` - Set default permissions for new files.
- `chmod` - Change file or directory permissions.
- `chattr` - Change file attributes.
- `lsattr` - List file attributes.

```bash title="Set default permissions to 755 for new files"
umask 022
```

```bash title="Owner only read, write, execute"
chmod 700 file.txt
```

```bash title="Make file immutable"
sudo chattr +i file.txt
```

```bash title="Show attributes for a file"
lsattr file.txt
```

### Container and Kubernetes Management

- `docker-compose` - Manage multi-container Docker applications.
- `minikube` - Run a local Kubernetes cluster.
- `helm` - Kubernetes package manager.

```bash title="Start containers in detached mode"
docker-compose up -d
```

```bash title="Start minikube cluster"
minikube start
```

```bash title="Install Helm chart for an app"
helm install myapp ./myapp-chart
```

### Advanced Git Operations

- `git stash` - Temporarily save changes.
- `git rebase` - Reapply commits on top of another base commit.
- `git log` - View commit history.

```bash title="Stash current changes"
git stash
```

```bash title="Rebase current branch onto main"
git rebase main
```

```bash title="Compact log with graph view"
git log --oneline --graph
```

### Troubleshooting and Debugging

- `strace` - Trace system calls and signals.
- `lsof` - List open files by processes.
- `dmesg` - Print kernel ring buffer messages.

```bash title="Trace process with PID 1234"
strace -p 1234
```

```bash title="List processes using port 8080"
lsof -i :8080
```

```bash title="View last 10 kernel messages"
dmesg | tail -10
```

### Data Manipulation and Processing

- `paste` - Merge lines of files.
- `join` - Join lines of two files on a common field.
- `column` - Format text output into columns.

```bash title="Combine lines from two files"
paste file1.txt file2.txt
```

```bash title="Join files on matching lines"
join file1.txt file2.txt
```

```bash title="Display data in columns"
cat data.txt | column -t
```

### File Transfer

- `rsync` - Sync files between local and remote systems.
- `scp` - Securely copy files between hosts.
- `ftp` - Transfer files using FTP protocol.

```bash title="Sync file to remote system"
rsync -avz /local/dir user@remote:/remote/dir
```

```bash title="Copy to remote"
scp file.txt user@remote:/path/to/destination
```

```bash title="Connect to FTP server example.com"
ftp example.com
```

### Job Management and Scheduling

- `bg` - Send a job to the background.
- `fg` - Bring a background job to the foreground.
- `at` - Schedule a command to run once at a specified time.

```bash title="Run a script in the background"
./script.sh &
```

```bash title="Bring job 1 to the foreground"
fg %1
```

```bash title="Run in 2 minutes"
echo "echo Hello, DevOps" | at now + 2 minutes
```

## Shell Scripting

### 01. Automating Server Provisioning (AWS EC2 Launch)

```bash title="Provision AWS EC2"
#!/bin/bash

# Variables

INSTANCE_TYPE="t2.micro"
AMI_ID="ami-0abcdef1234567890" # Replace with the correct AMI ID
KEY_NAME="my-key-pair" # Replace with your key pair name
SECURITY_GROUP="sg-0abc1234def567890" # Replace with your security group ID
SUBNET_ID="subnet-0abc1234def567890" # Replace with your subnet ID
REGION="us-east-2" # Replace with your AWS region

# Launch EC2 instance

aws ec2 run-instances --image-id $AMI_ID --count 1 --instance-type $INSTANCE_TYPE \
--key-name $KEY_NAME --security-group-ids $SECURITY_GROUP --subnet-id $SUBNET_ID --region $REGION

echo "EC2 instance launched successfully!"
```

### 02. System Monitoring (CPU Usage Alert)

```bash title="CPU Alert"
#!/bin/bash

# Threshold for CPU usage
CPU_THRESHOLD=80

# Get the current CPU usage
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | sed "s/.-, -\([0-9.]-\)%- id.-/\1/" | awk '{print 100 - $1}')

# Check if CPU usage exceeds threshold
if (( $(echo "$CPU_USAGE > $CPU_THRESHOLD" | bc -l) ));
then
echo "Alert: CPU usage is above $CPU_THRESHOLD%. Current usage is $CPU_USAGE%" | mail -s "CPU Usage Alert" user@example.com
fi
```

### 03. Backup Automation (MySQL Backup)

```bash title="Backup MySQL"
#!/bin/bash

# Variables
DB_USER="root"
DB_PASSWORD="password"
DB_NAME="my_database"
BACKUP_DIR="/backup"
DATE=$(date +%F)

# Create backup directory if it doesn't exist
mkdir -p $BACKUP_DIR

# Backup command
mysqldump -u $DB_USER -p$DB_PASSWORD $DB_NAME > $BACKUP_DIR/backup_$DATE.sql

# Optional: Compress the backup
gzip $BACKUP_DIR/backup_$DB_NAME_$DATE.sql

echo "Backup completed successfully!"
```

### 04. Log Rotation and Cleanup

```bash title="Rotate Logs"
#!/bin/bash

# Variables
LOG_DIR="/var/log/myapp"
ARCHIVE_DIR="/var/log/myapp/archive"
DAYS_TO_KEEP=30

# Create archive directory if it doesn't exist
mkdir -p $ARCHIVE_DIR

# Find and compress logs older than 7 days
find $LOG_DIR -type f -name "-.log" -mtime +7 -exec gzip {} \; -exec mv {}$ARCHIVE_DIR \;

# Delete logs older than 30 days
find $ARCHIVE_DIR -type f -name "-.log.gz" -mtime +$DAYS_TO_KEEP -exec rm {} \;
echo "Log rotation and cleanup completed!"
```

### 05. CI/CD Pipeline Automation (Trigger Jenkins Job)

```bash title="Trigger Jenkins Job"
#!/bin/bash

# Jenkins details
JENKINS_URL="http://jenkins.example.com"
JOB_NAME="my-pipeline-job"
USER="your-username"
API_TOKEN="your-api-token"

# Trigger Jenkins job
curl -X POST "$JENKINS_URL/job/$JOB_NAME/build" --user "$USER:$API_TOKEN"

echo "Jenkins job triggered successfully!"
```

### 06. Deployment Automation (Kubernetes Deployment)

```bash title="Kubernetes Deployment"
#!/bin/bash

# Variables
NAMESPACE="default"
DEPLOYMENT_NAME="my-app"
IMAGE="my-app:v1.0"

# Deploy to Kubernetes
kubectl set image deployment/$DEPLOYMENT_NAME
$DEPLOYMENT_NAME=$IMAGE --namespace=$NAMESPACE
echo "Deployment updated to version $IMAGE!"
```

### 07. Infrastructure as Code (Terraform Apply)

```bash title="Terraform Apply"
#!/bin/bash

# Variables
TF_DIR="/path/to/terraform/config"

# Navigate to Terraform directory
cd $TF_DIR

# Run terraform apply
terraform apply -auto-approve

echo "Terraform apply completed successfully!"
```

### 08. Database Management (PostgreSQL Schema Migration)

```bash title="PostgreSQL Schema Migration"
#!/bin/bash

# Variables
DB_USER="postgres"
DB_PASSWORD="password"
DB_NAME="my_database"
MIGRATION_FILE="/path/to/migration.sql"

# Run schema migration
PGPASSWORD=$DB_PASSWORD psql -U $DB_USER -d $DB_NAME -f $MIGRATION_FILE

echo "Database schema migration completed!"
```

### 09. User Management (Add User to Group)

```bash title="Add User to Group"
#!/bin/bash

# Variables
USER_NAME="newuser"
GROUP_NAME="devops"

# Add user to group
usermod -aG $GROUP_NAME $USER_NAME
echo "User $USER_NAME added to group $GROUP_NAME!"
```

### 10. Security Audits (Check for Open Ports)

```bash title="Check for Open Ports"
#!/bin/bash

# Check for open ports

OPEN_PORTS=$(netstat -tuln)

# Check if any ports are open (excluding localhost)

if [[ $OPEN_PORTS =~ "0.0.0.0" || $OPEN_PORTS =~ "127.0.0.1" ]]; then

echo "Security Alert: Open ports detected!"

echo "$OPEN_PORTS" | mail -s "Open Ports Security Alert" [user@example.com](mailto:user@example.com) else

echo "No open ports detected."

fi
```

### 11. Performance Tuning

- This script clears memory caches and restarts services to free up system resources.

```bash title=""
#!/bin/bash

# Clear memory caches to free up resources**

sync; echo 3 > /proc/sys/vm/drop_caches

# Restart services to free up resources**

systemctl restart nginx

systemctl restart apache2
```

### 12. Automated Testing

- This script runs automated tests using a testing framework like pytest for Python or JUnit for Java.

```bash title=""
#!/bin/bash

# Run unit tests using pytest (Python example)**

pytest tests/

# Or, run JUnit tests (Java example)**

mvn test
```

### 13. Scaling Infrastructure

- This script automatically scales EC2 instances in an Auto Scaling group based on CPU usage.

```bash title="Sacale EC2"
#!/bin/bash

# Check CPU usage and scale EC2 instances**

CPU_USAGE=$(aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --statistics Average --period 300 --start-time $(date -d '5 minutes ago' --utc +%FT%TZ) --end-time $(date --utc +%FT%TZ) --query 'Datapoints[0].Average' --output text)

if (( $(echo "$CPU_USAGE > 80" | bc -l) )); then

aws autoscaling update-auto-scaling-group --auto-scaling-group-name my-auto-scaling-group --desired-capacity 3

fi
```

### 14. Environment Setup

- This script sets environment variables for different environments (development, staging, production).

```bash title="Set Env variables"
#!/bin/bash

# Set environment variables for different stages

if [ "$1" == "production" ]; then

export DB_HOST="prod-db.example.com"

export API_KEY="prod-api-key"

elif [ "$1" == "staging" ]; then

export DB_HOST="staging-db.example.com"

export API_KEY="staging-api-key"

else

export DB_HOST="dev-db.example.com"

export API_KEY="dev-api-key"

fi
```

### 15. Error Handling and Alerts

- This script checks logs for errors and sends a Slack notification if an error is found.

```bash title="Error alerting"
#!/bin/bash

# Check logs for error messages and send Slack notification

if grep -i "error" /var/log/myapp.log; then

curl -X POST -H 'Content-type: application/json' --data '{"text":"Error found in logs!"}' https://hooks.slack.com/services/your/webhook/url

fi
```

### 16. Automated Software Installation and Updates

- This script installs Docker if it's not already installed on the system.

```bash title="Install Docker"
#!/bin/bash

#### **# Install Docker**

if ! command -v docker &> /dev/null; then

curl -fsSL https://get.docker.com -o get-docker.sh

sudo sh get-docker.sh

fi
```

### 17. Configuration Management

- This script updates configuration files (like nginx.conf) across multiple servers.

```bash title="Update nginx configuration across all servers"
#!/bin/bash

# Update nginx configuration across all servers

scp nginx.conf user@server:/etc/nginx/nginx.conf

ssh user@server "systemctl restart nginx"
```

### 18. Health Check Automation

- This script checks the health of multiple web servers by making HTTP requests.

```bash title="Check web servers"
#!/bin/bash

# Check if web servers are running

for server in "server1" "server2" "server3"; do

curl -s --head http://$server | head -n 1 | grep "HTTP/1.1 200 OK" > /dev/null

if [ $? -ne 0 ]; then

echo "$server is down"

else

echo "$server is up"

done
```

### 19. Automated Cleanup of Temporary Files

- This script removes files older than 30 days from the /tmp directory to free up disk space.

```bash title="Remove files older than x days"
#!/bin/bash

# Remove files older than 30 days in /tmp

find /tmp -type f -mtime +30 -exec rm -f {} \;
```

### 20. Environment Variable Management

- This script sets environment variables from a .env file.

```bash title="Set ENV vars from file"
#!/bin/bash

# Set environment variables from a .env file


export $(grep -v '^#' .env | xargs)

fi
```

### 21. Server Reboot Automation

- This script automatically reboots the server during off-hours (between 2 AM and 4 AM).

```bash title="Reboot server"
#!/bin/bash

# Reboot server during off-hours**

if [ $(date +%H) -ge 2 ] && [ $(date +%H) -lt 4 ]; then

sudo reboot

fi
```

### 22. SSL Certificate Renewal

- This script renews SSL certificates using certbot and reloads the web server.

```bash title="Renew SSL cert"
#!/bin/bash

# Renew SSL certificates using certbot**

certbot renew

systemctl reload nginx
```

### 23. Automatic Scaling of Containers

- This script checks the CPU usage of a Docker container and scales it based on usage.

```bash title="Scale Docker Containers"
#!/bin/bash

# Check CPU usage of a Docker container and scale if necessary

CPU_USAGE=$(docker stats --no-stream --format "{{.CPUPerc}}" my-container | sed 's/%//')

if (( $(echo "$CPU_USAGE > 80" | bc -l) )); then

docker-compose scale my-container=3

fi
```

### 24. Backup Verification

- This script verifies the integrity of backup files and reports any corrupted ones.

```bash title="Verify backup files"
#!/bin/bash

# Verify backup files integrity

for backup in /backups/*.tar.gz; do

if ! tar -tzf $backup > /dev/null 2>&1; then

echo "Backup $backup is corrupted"

else

echo "Backup $backup is valid"

fi

done
```

### 25. Automated Server Cleanup

- This script removes unused Docker images, containers, and volumes to save disk space.

```bash title=""
#!/bin/bash

# Remove unused Docker images, containers, and volumes

docker system prune -af
```

### 26. Version Control Operations

- This script pulls the latest changes from a Git repository and creates a release tag.

```bash title=""
#!/bin/bash

# Pull latest changes from Git repository and create a release tag

git pull origin main

git tag -a v$(date +%Y%m%d%H%M%S) -m "Release $(date)"

git push origin --tags
```

### 27. Application Deployment Rollback

- This script reverts to the previous Docker container image if a deployment fails. ```bash title=""

```bash title="Rollback Docker container image"
#!/bin/bash

# Rollback to the previous Docker container image if deployment fails

if [ $? -ne 0 ]; then docker-compose down

docker-compose pull my-app:previous

docker-compose up -d

fi
```

### 28. Automated Log Collection

- This script collects logs from multiple servers and uploads them to an S3 bucket.

```bash title="Collect logs"
#!/bin/bash

# Collect logs and upload them to an S3 bucket

tar -czf /tmp/logs.tar.gz /var/log/*

aws s3 cp /tmp/logs.tar.gz s3://my-log-bucket/logs/$(date +%Y%m%d%H%M%S).tar.gz
```

### 29. Security Patch Management

- This script checks for available security patches and applies them automatically.

```bash title="Apply patches"
#!/bin/bash

# Check and apply security patches sudo apt-get update

sudo apt-get upgrade -y --only-upgrade
```

### 30. Custom Monitoring Scripts

- This script checks if a database service is running and restarts it if necessary.

```bash title="Check Database is running"
#!/bin/bash

# Check if a database service is running and restart it if necessary

if ! systemctl is-active --quiet mysql; then

systemctl restart mysql

echo "MySQL service was down and has been restarted"

else

echo "MySQL service is running"

fi
```

### 31. DNS Configuration Automation (Route 53)

```bash title="Update Route53"
#!/bin/bash

# Variables

ZONE_ID="your-hosted-zone-id"
DOMAIN_NAME="your-domain.com"
NEW_IP="your-new-ip-address"

# Update Route 53 DNS record

aws route53 change-resource-record-sets --hosted-zone-id $ZONE_ID --change-batch '{
  "Changes": [
    {
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "'$DOMAIN_NAME'",
        "Type": "A",
        "TTL": 60,
        "ResourceRecords": [
          {
            "Value": "'$NEW_IP'"
          }
        ]
      }
    }
  ]
}'
```

### 32. Automated Code Linting and Formatting (ESLint and Prettier)

```bash title="Run ESLint"
#!/bin/bash

# Run ESLint

npx eslint . --fix

# Run Prettier

npx prettier --write "**/*.js"
```

### 33. Automated API Testing (Using curl)

```bash title=""
#!/bin/bash

# API URL

API_URL="https://your-api-endpoint.com/endpoint"

# Make GET request and check for 200 OK response

RESPONSE=$(curl --write-out "%{http_code}" --silent --output /dev/null $API_URL)

if [ $RESPONSE -eq 200 ]; then

echo "API is up and running"

else

echo "API is down. Response code: $RESPONSE"

fi
```

### 34. Container Image Scanning (Using Trivy)

```bash title="Scan Container Image"
#!/bin/bash

# Image to scan

IMAGE_NAME="your-docker-image:latest"

# Run Trivy scan

trivy image --exit-code 1 --severity HIGH,CRITICAL $IMAGE_NAME

if [ $? -eq 1 ]; then

echo "Vulnerabilities found in image: $IMAGE_NAME"

exit 1

else

echo "No vulnerabilities found in image: $IMAGE_NAME"

fi
```

### 35. Disk Usage Monitoring and Alerts (Email Notification)

```bash title="Monitor Disk Usage"
#!/bin/bash

# Disk usage threshold

THRESHOLD=80

# Get current disk usage percentage

DISK_USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

# Check if disk usage exceeds threshold

if [ $DISK_USAGE -gt $THRESHOLD ]; then

echo "Disk usage is above threshold: $DISK_USAGE%" | mail -s "Disk Usage Alert" [your-email@example.com](mailto:your-email@example.com)

fi
```

### 36. Automated Load Testing (Using Apache Benchmark)

```bash title=""
#!/bin/bash

# Target URL

URL="https://your-application-url.com"

# Run Apache Benchmark with 1000 requests and 10 concurrent requests

ab -n 1000 -c 10 $URL
```

### 37. Automated Email Reports (Server Health Report)

```bash title=""
#!/bin/bash

# Server Health Report

REPORT=$(top -n 1 | head -n 10)

# Send report via email

echo "$REPORT" | mail -s "Server Health Report" [your-email@example.com](mailto:your-email@example.com)
```

### 38. DNS Configuration Automation (Route 53)

- Introduction: This script automates the process of updating DNS records in AWS Route 53 when the IP address of a server changes. It ensures that DNS records are updated dynamically when new servers are provisioned.

```bash title="Update Route53"
#!/bin/bash

# Variables
ZONE_ID="your-hosted-zone-id"
DOMAIN_NAME="your-domain.com"
NEW_IP="your-new-ip-address"

# Update Route 53 DNS record
aws route53 change-resource-record-sets --hosted-zone-id $ZONE_ID--change-batch '{
  "Changes": [
    {
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "'$DOMAIN_NAME'",
        "Type": "A",
        "TTL": 60,
        "ResourceRecords": [
          {
          "Value": "'$NEW_IP'"
          }
        ]
      }
    }
  ]
}'
```

### 39. Automated Code Linting and Formatting (ESLint and Prettier)

- Introduction: This script runs ESLint and Prettier to check and automatically format JavaScript code before deployment. It ensures code quality and consistency.

```bash title="Run ESLint"
#!/bin/bash

# Run ESLint

npx eslint . --fix

# Run Prettier

npx prettier --write "**/*.js"
```

### 40. Automated API Testing (Using curl)

- Introduction: This script automates the process of testing an API by sending HTTP requests and verifying the response status. It helps ensure that the API is functioning correctly.

```bash title="API Testing"
#!/bin/bash

# API URL

API_URL="https://your-api-endpoint.com/endpoint"

# Make GET request and check for 200 OK response

RESPONSE=$(curl --write-out "%{http_code}" --silent --output /dev/null $API_URL)

if [ $RESPONSE -eq 200 ]; then

echo "API is up and running"

else

echo "API is down. Response code: $RESPONSE"
```

### 41. Container Image Scanning (Using Trivy)

- Introduction: This script scans Docker images for known vulnerabilities using Trivy. It ensures that only secure images are deployed in production.

```bash title="Container Image Scanning"
#!/bin/bash

# Image to scan

IMAGE_NAME="your-docker-image:latest"

# Run Trivy scan

trivy image --exit-code 1 --severity HIGH,CRITICAL $IMAGE_NAME

if [ $? -eq 1 ]; then

echo "Vulnerabilities found in image: $IMAGE_NAME"

exit 1

else

echo "No vulnerabilities found in image: $IMAGE_NAME"

fi
```

### 42. Disk Usage Monitoring and Alerts (Email Notification)

- Introduction: This script monitors disk usage and sends an alert via email if the disk usage exceeds a specified threshold. It helps in proactive monitoring of disk space.

```bash title="Disk Usage Monitoring"
#!/bin/bash

## # Disk usage threshold

THRESHOLD=80

# Get current disk usage percentage

DISK_USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

# Check if disk usage exceeds threshold

if [ $DISK_USAGE -gt $THRESHOLD ]; then

echo "Disk usage is above threshold: $DISK_USAGE%" | mail -s "Disk Usage Alert" [your-email@example.com](mailto:your-email@example.com)

fi
```

### 43. Automated Load Testing (Using Apache Benchmark)

- Introduction: This script runs load tests using Apache Benchmark (ab) to simulate traffic on an application. It helps measure the performance and scalability of the application.

```bash title="Load testing"
#!/bin/bash

# Target URL

URL="https://your-application-url.com"

# Run Apache Benchmark with 1000 requests and 10 concurrent requests

ab -n 1000 -c 10 $URL
```

### 44. Automated Email Reports (Server Health Report)

- Introduction: This script generates a server health report using system commands like top and sends it via email. It helps keep track of server performance and health.

```bash title="Server Health Report"
#!/bin/bash

# Server Health Report

REPORT=$(top -n 1 | head -n 10)

# Send report via email

echo "$REPORT" | mail -s "Server Health Report" [your-email@example.com](mailto:your-email@example.com)
```

### 45. Automating Documentation Generation (Using pdoc for Python)

- Introduction: This script generates HTML documentation from Python code using pdoc. It helps automate the process of creating up-to-date documentation from the source code.

```bash title=""
#!/bin/bash

# Generate documentation using pdoc

pdoc --html your-python-module --output-dir docs/

# Optionally, you can zip the generated docs

zip -r docs.zip docs/
```

### 46. Crontab

```bash title="List all cron jobs"
crontab -l
```

```bash title="Edit cron jobs"
crontab -e
```

```bash title="Remove all cron jobs"
crontab -r
```

```bash title="Use a specific editor (e.g., nano)"
EDITOR=nano crontab -e
```

```bash title="Cron Job Syntax"
* * * * * command_to_execute
│ │ │ │ └─── Day of the week (0-6, Sunday=0)
│ │ │ └───── Month (1-12 or JAN-DEC)
│ │ └─────── Day of the month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

```bash title="Run a script every minute"
* * * * * /path/to/script.sh
```

```bash title="Run a script every 5 minutes"
*/5 * * * * /path/to/script.sh
```

```bash title="Run a script every 10 minutes"
*/10 * * * * /path/to/script.sh
```

```bash title="Run a script at midnight"
0 0 * * * /path/to/script.sh
```

```bash title="Run a script every hour"
0 * * * * /path/to/script.sh
```

```bash title="Run a script every 2 hours"
0 */2 * * * /path/to/script.sh
```

```bash title="Run a script every Sunday at 3 AM"
0 3 * * 0 /path/to/script.sh
```

```bash title="Run a script at 9 AM on the 1st of every month"
0 9 1 * * /path/to/script.sh
```

```bash title="Run a script every Monday to Friday at 6 PM"
0 18 * * 1-5 /path/to/script.sh
```

```bash title="Run a script on the first Monday of every month"
0 9 * * 1 [ "$(date +\%d)" -le 7 ] && /path/to/script.sh
```

```bash title="Run a script on specific dates (e.g., 1st and 15th of the month)"
0 12 1,15 * * /path/to/script.sh
```

```bash title="Run a script between 9 AM and 5 PM, every hour"
0 9-17 * * * /path/to/script.sh
```

```bash title="Run a script every reboot"
@reboot /path/to/script.sh
```

```bash   title="Run a script daily at midnight "
@daily /path/to/script.sh
```

```bash title="Run a script weekly at midnight on Sunday"
@weekly /path/to/script.sh
```

```bash title="Run a script monthly at midnight on the 1st"
@monthly /path/to/script.sh
```

```bash title="Run a script yearly at midnight on January 1st"
@yearly /path/to/script.sh
```

```bash title="Redirect cron job output to a log file"
0 0 * * * /path/to/script.sh >> /var/log/script.log 2>&1
```

```bash title="Run a job only if the previous instance is not running"
0 * * * * flock -n /tmp/job.lock /path/to/script.sh
```

```bash title="Run a script with a random delay (0-59 minutes)"
RANDOM_DELAY=$((RANDOM % 60)) && sleep $RANDOM_DELAY && /path/to/script.sh
```

```bash title="Run a script with environment variables"
SHELL=/bin/bash

PATH=/usr/local/bin:/usr/bin:/bin

0 5 * * * /path/to/script.sh
```

```bash title="Check cron logs (Ubuntu/Debian)"
grep CRON /var/log/syslog
```

```bash title="Check cron logs (Red Hat/CentOS)"
grep CRON /var/log/cron
```

```bash title="Restart cron service (Linux)"
sudo systemctl restart cron
```

```bash title="Check if cron service is running"
sudo systemctl status cron
```

---

## Python

### Python Basics

- Run a script: `python script.py`
- Start interactive mode: `python`
- Check Python version: `python --version`
- Install a package: `pip install package_name`

### Create a virtual environment:

```bash title="On Linux/macOS"
python -m venv venv source venv/bin/activate
```

```bash title="On Windows"
venv\Scripts\activate
```

```bash title="Deactivate virtual environment"
deactivate
```

### 1. File Operations

```py title="Read a file:"
python

with open('file.txt', 'r') as file: content = file.read() print(content)
```

```py title="Write to a file:"
python

with open('output.txt', 'w') as file: file.write('Hello, DevOps!')
```

### 2. Environment Variables

```py title="Get an environment variable:"
python

import os

db_user = os.getenv('DB_USER') print(db_user)
```

```py title="Set an environment variable:"
python

import os

os.environ['NEW_VAR'] = 'value'
```

### 3. Subprocess Management

```py title="Run shell commands:"
python

import subprocess

result = subprocess.run(['ls', '-l'], capture_output=True, text=True) print(result.stdout)
```

### 4. API Requests

```py title="Make a GET request:"
python

import requests

response = requests.get('https://api.example.com/data') print(response.json())
```

### 5. JSON Handling

```py title="Read JSON from a file:"
python

import json

with open('data.json', 'r') as file: data = json.load(file) print(data)
```

```py title="Write JSON to a file:"
python

import json

data = {'name': 'DevOps', 'type': 'Workflow'} with open('output.json', 'w') as file: json.dump(data, file, indent=4)
```

### 6. Logging

```py title="Basic logging setup:"
python

import logging

logging.basicConfig(level=logging.INFO) logging.info('This is an informational message')
```

### 7. Working with Databases

```py title="Connect to a SQLite database:"
python

importsqlite3

conn = sqlite3.connect('example.db') 
cursor = conn.cursor()
cursor.execute('CREATE TABLE IF NOT EXISTS users (id INTEGER 
PRIMARY KEY, name TEXT)')
conn.commit() 
conn.close()
```

### 8. Automation with Libraries

```py title="Using Paramiko for SSH connections:"
python

import paramiko

ssh = paramiko.SSHClient() 
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy()) 
ssh.connect('hostname', username='user', password='password')

stdin, stdout, stderr = ssh.exec_command('ls') 
print(stdout.read().decode())
ssh.close()
```

### 9. Error Handling

```py title="Try-except block:"
python

try: # code that may raise an exception risky_code() except Exception as e: print(f'Error occurred: {e}')
```

### 10. Docker Integration

```py title="Using the docker package to interact with Docker:"
import docker

client = docker.from_env() containers = client.containers.list() for container in containers: print(container.name)
```

### 11. Working with YAML Files

```py title="Read a YAML file:"
import yaml

- with open('config.yaml', 'r') as file: config = yaml.safe_load(file) print(config)
- Write to a YAML file:
```

```py
import yaml

data = {'name': 'DevOps', 'version': '1.0'} with open('output.yaml', 'w') as file: yaml.dump(data, file)
```

### 12. Parsing Command-Line Arguments

```py title="Using argparse:"
import argparse

parser = argparse.ArgumentParser(description='Process some integers.') parser.add_argument('--num', type=int, help='an integer for the accumulator')

args = parser.parse_args() 
print(args.num)
```

### 13. Monitoring System Resources

```py title="Using psutil to monitor system resources:"
import psutil

print(f"CPU Usage: {psutil.cpu_percent()}%") 
print(f"Memory Usage: {psutil.virtual_memory().percent}%")
```

### 14. Handling HTTP Requests with Flask

```py title="Basic Flask API:"
from flask import Flask, jsonify

app = Flask( name )

@app.route('/health', methods=['GET']) def health_check(): return jsonify({'status': 'healthy'})

if name == ' main ': app.run(host='0.0.0.0', port=5000)
```

### 15. Creating Docker Containers

```py title="Using the Docker SDK to create a container:"
import docker

client = docker.from_env()
container = client.containers.run('ubuntu', 'echo Hello World', detach=True) 
print(container.logs())
```

### 16. Scheduling Tasks

```py title="Using schedule for task scheduling:"
importschedule import time

def job(): print("Running scheduled job...")

schedule.every(1).minutes.do(job)

while True: schedule.run_pending() time.sleep(1)
```

### 17. Version Control with Git

```py title="Using GitPython to interact with Git repositories:"
import git

repo = git.Repo('/path/to/repo') 
repo.git.add('file.txt') 
repo.index.commit('Added file.txt')
```

### 18. Email Notifications

```py title="Sending emails using smtplib:"
import smtplib from email.mime.text import MIMEText

msg = MIMEText('This is the body of the email') 
msg['Subject'] = 'Email Subject'
msg['From'] = 'you@example.com'
msg['To'] = 'recipient@example.com'

with smtplib.SMTP('smtp.example.com', 587) as server: server.starttls() server.login('your_username', 'your_password') server.send_message(msg)
```

### 19. Creating Virtual Environments

```py title="Creating and activating a virtual environment:"
import os import subprocess # Create virtual environment
subprocess.run(['python3', '-m', 'venv', 'myenv'])

# Activate virtual environment (Windows)
os.system('myenv\\Scripts\\activate')

# Activate virtual environment (Linux/Mac)
os.system('source myenv/bin/activate')
```

### 20. Integrating with CI/CD Tools

```py title="Using the requests library to trigger a Jenkins job:"
import requests

url = ['http://your-jenkins-url/job/your-job-name/build'](http://your-jenkins-url/job/your-job-name/build%27) response = requests.post(url, auth=('user', 'token')) print(response.status_code)
```

### 21. Database Migration

```bash title="Using Alembic for database migrations:"
alembic revision -m "initial migration" alembic upgrade head
```

### 22. Testing Code

```py title="Using unittest for unit testing:"
import unittest

def add(a, b): 
  return a + b
class TestMathFunctions(unittest.TestCase): 
  def test_add(self):
     self.assertEqual(add(2, 3), 5)
if name == ' main ': 
  unittest.main()
```

### 23. Data Transformation with Pandas

```py title="Using pandas for data manipulation:"
import pandas as pd

df = pd.read_csv('data.csv') df['new_column'] = df['existing_column'] \* 2 df.to_csv('output.csv', index=False)
```

### 24. Using Python for Infrastructure as Code

```py title="Using boto3 for AWS operations:"
import boto3

ec2 = boto3.resource('ec2')
instances = ec2.instances.filter(Filters=[{'Name': 'instance-state-name', 
'Values': ['running']}])
for instance in instances: 
  print(instance.id, instance.state)
```

### 25. Web Scraping

```py title="Using BeautifulSoup to scrape web pages:"
import requests
from bs4 import BeautifulSoup

response = requests.get('http://example.com')
soup = BeautifulSoup(response.content, 'html.parser')
print(soup.title.string)
```

### 26. Using Fabric for Remote Execution

```py title="Running commands on a remote server:"
from fabric import Connection

conn = Connection(host='user@hostname', connect_kwargs={'password': 
'your_password'})
conn.run('uname -s')
```

### 27. Automating AWS S3 Operations

```py title="Upload and download files using boto3:"
import boto3

s3 = boto3.client('s3')

# Upload a file
s3.upload_file('local_file.txt', 'bucket_name', 's3_file.txt')

# Download a file
s3.download_file('bucket_name', 's3_file.txt', 'local_file.txt')
```

### 28. Monitoring Application Logs

```py title="Tail logs using tail -f equivalent in Python:"
import time

def tail_f(file):
  file.seek(0, 2) # Move to the end of the file
  while True:
    line = file.readline()
    if not line:
      time.sleep(0.1) # Sleep briefly
      continue
    print(line)

with open('app.log', 'r') as log_file:
  tail_f(log_file)
```

### 29. Container Health Checks

```py title="Check the health of a running Docker container:"
import docker

client = docker.from_env()
container = client.containers.get('container_id') 
print(container.attrs['State']['Health']['Status'])
```

### 30. Using requests for Rate-Limited APIs

```py title="Handle rate limiting in API requests:"
import requests
import time
url = 'https://api.example.com/data'
while True:
  response = requests.get(url)
  if response.status_code == 200:
    print(response.json())
    break
  elif response.status_code == 429: # Too Many Requests
    time.sleep(60) # Wait a minute before retrying
  else:
    print('Error:', response.status_code)
    break
```

### 31. Docker Compose Integration

```py title="Using docker-compose in Python:"
import os
import subprocess

# Start services defined in docker-compose.yml
subprocess.run(['docker-compose', 'up', '-d'])

# Stop services
subprocess.run(['docker-compose', 'down'])
```

### 32. Terraform Execution

```py title="Executing Terraform commands with subprocess:"
import subprocess

# Initialize Terraform
subprocess.run(['terraform', 'init'])

# Apply configuration
subprocess.run(['terraform', 'apply', '-auto-approve'])
```

### 33. Working with Prometheus Metrics

```py title="Scraping and parsing Prometheus metrics:"
import requests

response = requests.get('http://localhost:9090/metrics')
metrics = response.text.splitlines()

for metric in metrics:
  print(metric)
```

### 34. Using pytest for Testing

```py title="Simple test case with pytest:"
def add(a, b):
  return a + b

def test_add():
  assert add(2, 3) == 5
```

### 35. Creating Webhooks

```py title="Using Flask to create a simple webhook:"
from flask import Flask, request

app = Flask( name )

@app.route('/webhook', methods=['POST'])
def webhook():
  data = request.json
  print('Received data:', data)
  return 'OK', 200

if name == ' main ':
  app.run(port=5000)
```

### 36. Using Jinja2 for Configuration Templates

```py title="Render configuration files with Jinja2:"
from jinja2 import Template

template = Template('Hello, {{ name }}!')
rendered = template.render(name='DevOps')
print(rendered)
```

### 37. Encrypting and Decrypting Data

```py title="Using cryptography to encrypt and decrypt:"
from cryptography.fernet import Fernet

# Generate a key
key = Fernet.generate_key() 
cipher_suite = Fernet(key)

# Encrypt
encrypted_text = cipher_suite.encrypt(b'Secret Data')

# Decrypt
decrypted_text = cipher_suite.decrypt(encrypted_text) 
print(decrypted_text.decode())
```

### 38. Error Monitoring with Sentry

```py title="Sending error reports to Sentry:"
import sentry_sdk

sentry_sdk.init('your_sentry_dsn')

def divide(a, b):
  return a / b

try:
  divide(1, 0)
except ZeroDivisionError as e: 
  sentry_sdk.capture_exception(e)
```

### 39. Setting Up Continuous Integration with GitHub Actions

```yaml title="Sample workflow file (.github/workflows/ci.yml):"
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.8'
      - name: Install dependencies
        run: | 
          pip install -r requirements.txt
      - name: Run tests
        run: |
          pytest
```

### 40. Creating a Simple API with FastAPI

```py title="Using FastAPI for high-performance APIs:"
from fastapi import FastAPI

app = FastAPI()

@app.get('/items/{item_id}')
async def read_item(item_id: int):
  return {'item_id': item_id}

if name == ' main ':
  import uvicorn
  uvicorn.run(app, host='0.0.0.0', port=8000)
```

### 41. Log Aggregation with ELK Stack

```py title="Sending logs to Elasticsearch:"
from elasticsearch import Elasticsearch

es = Elasticsearch([\['http://localhost:9200'\]](http://localhost:9200/))

log = {'level': 'info', 'message': 'This is a log message'}
es.index(index='logs', body=log)
```

### 42. Using pandas for ETL Processes

```py title="Performing ETL with pandas:"
import pandas as pd

# Extract
data = pd.read_csv('source.csv')

# Transform
data['new_column'] = data['existing_column'].apply(lambda x: x \* 2)

# Load
data.to_csv('destination.csv', index=False)
```

### 43. Serverless Applications with AWS Lambda

```py title=" Deploying a simple AWS Lambda function:"
import json
def lambda_handler(event, context): 
  return {
     'statusCode': 200,
     'body': json.dumps('Hello from Lambda!')
  }
```
### 44. Working with Redis

```py title="Basic operations with Redis using redis-py:"
import redis

r = redis.StrictRedis(host='localhost', port=6379, db=0)

# Set a key
r.set('foo', 'bar')

# Get a key
print(r.get('foo'))
```

### 45. Using pyngrok for Tunneling

```py title="Create a tunnel to expose a local server:"
from pyngrok import ngrok

# Start the tunnel
public_url = ngrok.connect(5000)
print('Public URL:', public_url)

# Keep the tunnel open
input('Press Enter to exit...')
```

### 46. Creating a REST API with Flask-RESTful

```py title="Building REST APIs with Flask-RESTful:"
from flask import Flask
from flask_restful import Resource, Api

app = Flask( name ) 
api = Api(app)

class HelloWorld(Resource): 
  def get(self):
     return {'hello': 'world'}

api.add_resource(HelloWorld, '/')

if name == ' main ': 
  app.run(debug=True)
```

### 47. Using asyncio for Asynchronous Tasks

```py title="Running asynchronous tasks in Python:"
import asyncio

async def main():
  print('Hello')
  await asyncio.sleep(1) 
  print('World')

asyncio.run(main())
```

### 48. Network Monitoring with scapy

```py title="Packet sniffing using scapy:"
from scapy.all import sniff

def packet_callback(packet):
  print(packet.summary())

sniff(prn=packet_callback, count=10)
```

### 49. Handling Configuration Files with configparser

```py title="Reading and writing to INI configuration files:"
import configparser

config = configparser.ConfigParser() 
config.read('config.ini')
print(config['DEFAULT']['SomeSetting'])

config['DEFAULT']['NewSetting'] = 'Value' 
with open('config.ini', 'w') as configfile:
  config.write(configfile)
```

### 50. WebSocket Client Example

```py title="Creating a WebSocket client with websocket-client:"
import websocket

def on_message(ws, message): 
  print("Received message:", message)

ws = websocket.WebSocketApp("ws://echo.websocket.org", 
                on_message=on_message)
ws.run_forever()
```

### 51. Creating a Docker Image with Python

```py title="Using docker library to build an image:"
import docker

client = docker.from_env()

# Dockerfile content
dockerfile_content = """
FROM python:3.9-slim
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
"""

# Create a Docker image
image, build_logs =
client.images.build(fileobj=dockerfile_content.encode('utf-8'), tag='my-python-app')

for line in build_logs:
  print(line)
```

### 52. Using psutil for System Monitoring

```py title="Retrieve system metrics such as CPU and memory usage:"
import psutil

print("CPU Usage:", psutil.cpu_percent(interval=1), "%")
print("Memory Usage:", psutil.virtual_memory().percent, "%")
```

### 53. Database Migration with Alembic

```py title="Script to initialize Alembic migrations:"
from alembic import command
from alembic import config

alembic_cfg = config.Config("alembic.ini") 
command.upgrade(alembic_cfg, "head")
```

### 54. Using paramiko for SSH Connections

```py title="Execute commands on a remote server via SSH:"
import paramiko

client = paramiko.SSHClient() 
client.set_missing_host_key_policy(paramiko.AutoAddPolicy()) 
client.connect('hostname', username='user', password='your_password')

stdin, stdout, stderr = client.exec_command('ls -la') 
print(stdout.read().decode())
client.close()
```

### 55. CloudFormation Stack Creation with boto3

```py title="Creating an AWS CloudFormation stack:"
import boto3

cloudformation = boto3.client('cloudformation')

with open('template.yaml', 'r') as template_file: 
  template_body = template_file.read()

response = cloudformation.create_stack( 
  StackName='MyStack', 
  TemplateBody=template_body, 
  Parameters=[
    {
       'ParameterKey': 'InstanceType', 
       'ParameterValue': 't2.micro'
    },
  ],
  TimeoutInMinutes=5, 
  Capabilities=['CAPABILITY_NAMED_IAM'],
)
print(response)
```

### 56. Automating EC2 Instance Management

```py title="Starting and stopping EC2 instances:"
import boto3

ec2 = boto3.resource('ec2')

# Start an instance
instance = ec2.Instance('instance_id')
instance.start()

# Stop an instance
instance.stop()
```

### 57. Automated Backup with shutil

```py title="Backup files to a specific directory:"
import shutil
import os

source_dir = '/path/to/source'
backup_dir = '/path/to/backup'

shutil.copytree(source_dir, backup_dir)
```

### 58. Using watchdog for File System Monitoring

```py title="Monitor changes in a directory:"
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler

class MyHandler(FileSystemEventHandler): 
  def on_modified(self, event):
     print(f'File modified: {event.src_path}')

event_handler = MyHandler() 
observer = Observer()
observer.schedule(event_handler, path='path/to/monitor', recursive=False) 
observer.start()

try:
  while True: 
    time.sleep(1)
except KeyboardInterrupt: 
  observer.stop()
observer.join()
```

### 59. Load Testing with locust

```py title="Basic Locust load testing setup:"
from locust import HttpUser, task, between

class MyUser(HttpUser):
  wait_time = between(1, 3)

  @task
  def load_test(self): 
    self.client.get('/')
```

!!! note ""
    To run, save this as locustfile.py and run: locust

### 60. Integrating with GitHub API

```py title="Fetching repository details using GitHub API:"
import requests

url = 'https://api.github.com/repos/user/repo'
response = requests.get(url, headers={'Authorization': 'token YOUR_GITHUB_TOKEN'})
repo_info = response.json()
print(repo_info)
```

### 61. Managing Kubernetes Resources with kubectl

```py title="Using subprocess to interact with Kubernetes:"
importsubprocess

# Get pods
subprocess.run(['kubectl', 'get', 'pods'])

# Apply a configuration
subprocess.run(['kubectl', 'apply', '-f', 'deployment.yaml'])
```

### 62. Using pytest for CI/CD Testing

```py title="Integrate tests in your CI/CD pipeline:"
# test_example.py
def test_addition():
  assert 1 + 1 == 2

# Run pytest in your CI/CD pipeline
subprocess.run(['pytest'])
```

### 63. Creating a Simple CLI Tool with argparse

```py title="Build a command-line interface:"
import argparse

parser = argparse.ArgumentParser(description='Process some integers.') 
parser.add_argument('integers', metavar='N', type=int, nargs='+', help='an 
integer to be processed')
parser.add_argument('--sum', dest='accumulate', action='store_const', 
const=sum, default=max, help='sum the integers (default: find the max)')

args = parser.parse_args() 
print(args.accumulate(args.integers))
```

### 64. Using dotenv for Environment Variables

```py title="Load environment variables from a .env file:"
from dotenv import load_dotenv
import os

load_dotenv()

database_url = os.getenv('DATABASE_URL') 
print(database_url)
```

### 65. Implementing Web Scraping with BeautifulSoup

```py title="Scraping a web page for data:"
import requests
from bs4 import BeautifulSoup

response = requests.get('http://example.com')
soup = BeautifulSoup(response.text, 'html.parser')

for item in soup.find_all('h1'):
  print(item.text)
```

### 66. Using PyYAML for YAML Configuration Files

```py title="Load and dump YAML files:"
import yaml

# Load YAML file
with open('config.yaml', 'r') as file:
  config = yaml.safe_load(file)
  print(config)

# Dump to YAML file
with open('output.yaml', 'w') as file:
  yaml.dump(config, file)
```

### 67. Creating a Simple Message Queue with RabbitMQ

```py title="Send and receive messages using pika:"
import pika

# Sending messages 
connection =
pika.BlockingConnection(pika.ConnectionParameters('localhost')) 
channel = connection.channel() 
channel.queue_declare(queue='hello')

channel.basic_publish(exchange='', routing_key='hello', body='Hello World!')
connection.close()

# Receiving messages
def callback(ch, method, properties, body): 
  print("Received:", body)

connection = 
pika.BlockingConnection(pika.ConnectionParameters('localhost')) 
channel = connection.channel() 
channel.queue_declare(queue='hello')

channel.basic_consume(queue='hello', on_message_callback=callback, 
auto_ack=True)
channel.start_consuming()
```

### 68. Using sentry_sdk for Monitoring

```py title="Integrate Sentry for error tracking:"
import sentry_sdk

sentry_sdk.init("YOUR_SENTRY_DSN")

try:
  # Your code that may throw an exception
  1 / 0
except Exception as e:
  sentry_sdk.capture_exception(e)
```

### 69. Using openpyxl for Excel File Manipulation

```py title="Read and write Excel files:"
from openpyxl import Workbook, load_workbook

# Create a new workbook
wb = Workbook()
ws = wb.active
ws['A1'] = 'Hello'
wb.save('sample.xlsx')

# Load an existing workbook
wb = load_workbook('sample.xlsx')
ws = wb.active
print(ws['A1'].value)
```

### 70. Using sqlalchemy for Database Interaction

```py title="Define a model and perform CRUD operations:"
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm importsessionmaker

Base = declarative_base()

class User(Base):
  __tablename__= 'users'

  id = Column(Integer, primary_key=True) 
  name = Column(String)

engine = create_engine('sqlite:///example.db')
Base.metadata.create_all(engine)

Session = sessionmaker(bind=engine)
session = Session()

# Create
new_user = User(name='Alice')
session.add(new_user)
```

### 71. Monitoring Docker Containers with docker-py

```py title="Fetch and print the status of running containers:"
import docker

client = docker.from_env()
containers = client.containers.list()

for container in containers:
  print(f'Container Name: {container.name}, Status: {container.status}')
```

### 72. Using flask to Create a Simple API

```py title="Basic API setup with Flask:"
from flask import Flask, jsonify

app = Flask( name )

@app.route('/api/data', methods=['GET'])

def get_data():
  return jsonify({"message": "Hello, World!"})

if __name__ == '__main__':
  app.run(debug=True)
```

### 73. Automating Certificate Renewal with certbot

```py title="Script to renew Let's Encrypt certificates:"
import subprocess

# Renew certificates
subprocess.run(['certbot', 'renew'])
```

### 74. Using numpy for Data Analysis

```py title="Performing basic numerical operations:"
import numpy as np

data = np.array([1, 2, 3, 4, 5])
mean_value = np.mean(data)
print("Mean Value:", mean_value)
```

### 75. Creating and Sending Emails with smtplib

```py title="Send an email using Python:"
import smtplib
from email.mime.text import MIMEText

sender = 'you@example.com'
recipient = 'recipient@example.com'
msg = MIMEText('This is a test email.') 
msg['Subject'] = 'Test Email' 
msg['From'] = sender
msg['To'] = recipient

with smtplib.SMTP('smtp.example.com') as server:
  server.login('username', 'password')
  server.send_message(msg)
```

### 76. Using schedule for Task Scheduling

```py title="Schedule tasks at regular intervals:"
importschedule import time

def job():
  print("Job is running...")

schedule.every(10).minutes.do(job)

while True:
  schedule.run_pending()
  time.sleep(1)
```

### 77. Using matplotlib for Data Visualization

```py title="Plotting a simple graph:"
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [2, 3, 5, 7, 11]

plt.plot(x, y)
plt.xlabel('X-axis')
plt.ylabel('Y-axis')
plt.title('Simple Plot')
plt.show()
```

### 78. Creating a Custom Python Package

```md title="Structure your project as a package:"
my_package/
 ├── init .py
 ├── module1.py
 └── module2.py
```

```py title="setup.py for packaging:"
from setuptools import setup, find_packages


setup(
  name='my_package', 
  version='0.1', 
  packages=find_packages(), 
  install_requires=[
     'requests', 
     'flask'
  ],
)
```

### 79. Using pytest for Unit Testing

```py title="Writing a simple unit test:"
# test_sample.py
def add(a, b):
  return a + b

def test_add():
  assert add(1, 2) == 3
```

### 80. Implementing OAuth with requests-oauthlib

```py title="Authenticate with an API using OAuth:"
from requests_oauthlib import OAuth1Session

oauth = OAuth1Session(client_key='YOUR_CLIENT_KEY', 
client_secret='YOUR_CLIENT_SECRET')
response = oauth.get('https://api.example.com/user') 
print(response.json())
```

### 81. Using pandas for Data Manipulation

```py title="Load and manipulate data in a CSV file:"
import pandas as pd

df = pd.read_csv('data.csv') 
print(df.head())

# Filter data
filtered_df = df[df['column_name'] > 10]
print(filtered_df)
```

### 82. Using requests for HTTP Requests

```py title="Making a GET and POST request:"
import requests

# GET request
response = requests.get('https://api.example.com/data') 
print(response.json())

# POST request
data = {'key': 'value'}
response = requests.post('https://api.example.com/data', json=data) 
print(response.json())
```

### 83. Creating a Basic Web Server with http.server

```py title="Simple HTTP server to serve files:"
from http.server import SimpleHTTPRequestHandler, HTTPServer

PORT = 8000 handler = SimpleHTTPRequestHandler

with HTTPServer(('', PORT), handler) as httpd: 
  print(f'Serving on port {PORT}') 
  httpd.serve_forever()
```

### 84. Using Flask for Webhooks

```py title="Handling incoming webhook requests:"
from flask import Flask, request

app = Flask( name )
@app.route('/webhook', methods=['POST']) 
def webhook():
  data = request.json 
  print(data)
  return '', 200

if name == ' main ': app.run(port=5000)
```

### 85. Creating a Bash Script with subprocess

```py title="Run shell commands from Python:"
import subprocess

subprocess.run(['echo', 'Hello, World!'])
```

### 86. Using docker-compose with Python

```py title="Programmatically run Docker Compose commands:"
import subprocess

subprocess.run(['docker-compose', 'up', '-d'])
```

### 87. Using moto for Mocking AWS Services in Tests

```py title="Mocking AWS S3 for unit testing:"
import boto3 from moto import mock_s3

@mock_s3
def test_s3_upload():
  s3 = boto3.client('s3', region_name='us-east-1') 
  s3.create_bucket(Bucket='my-bucket') 
  s3.upload_file('file.txt', 'my-bucket', 'file.txt')
  # Test logic here
```

### 88. Using asyncio for Asynchronous Tasks

```py title="Run multiple tasks concurrently:"
import asyncio

async defsay_hello(): 
  print("Hello")
  await asyncio.sleep(1) 
  print("World")


async def main():
  await asyncio.gather(say_hello(), say_hello())

asyncio.run(main())
```

### 89. Using flask-cors for Cross-Origin Resource Sharing

```py title="Allow CORS in a Flask app:"
from flask import Flask
from flask_cors import CORS

app = Flask( name ) 
CORS(app)

@app.route('/data', methods=['GET']) 
def data():
  return {"message": "Hello from CORS!"}

if name == ' main ': app.run()
```

### 90. Using pytest Fixtures for Setup and Teardown

```py title="Create a fixture to manage resources:"
import pytest

@pytest.fixture
defsample_data():
  data = {"key": "value"}
  yield data # This is the test data
  # Teardown code here (if necessary)

def test_sample_data(sample_data):
  assert sample_data['key'] == 'value'
```

### 91. Using http.client for Low-Level HTTP Requests

```py title="Make a raw HTTP GET request:"
import http.client

conn = http.client.HTTPSConnection("www.example.com") 
conn.request("GET", "/")
response = conn.getresponse() 
print(response.status, response.reason) 
data = response.read()
conn.close()
```

### 92. Implementing Redis Caching with redis-py

```py title="Basic operations with Redis:"
import redis

r = redis.StrictRedis(host='localhost', port=6379, db=0)

# Set and get value
r.set('key', 'value')
print(r.get('key').decode('utf-8'))
```

### 93. Using json for Data Serialization

```py title="Convert Python objects to JSON:"
import json

data = {"key": "value"}
json_data = json.dumps(data)
print(json_data)
```

### 94. Using xml.etree.ElementTree for XML Processing

```py title="Parse an XML file:"
import xml.etree.ElementTree as ET

tree = ET.parse('data.xml')
root = tree.getroot()

for child in root:
  print(child.tag, child.attrib)
```

### 95. Creating a Virtual Environment with venv

```py title="Programmatically create a virtual environment:"
import venv

venv.create('myenv', with_pip=True)
```

### 96. Using psutil for System Monitoring

```py title="Get system memory usage:"
import psutil

memory = psutil.virtual_memory()
print(f'Total Memory: {memory.total}, Available Memory:
{memory.available}')
```

### 97. Using sqlite3 for Lightweight Database Management

```py title="Basic SQLite operations:"
importsqlite3

conn = sqlite3.connect('example.db') 
c = conn.cursor()

c.execute('''CREATE TABLE IF NOT EXISTS users (id INTEGER 
PRIMARY KEY, name TEXT)''')
c.execute("INSERT INTO users(name) VALUES ('Alice')") 
conn.commit()

for row in c.execute('SELECT * FROM users'): 
  print(row)

conn.close()
```

### 98. Using pytest to Run Tests in Parallel

```bash title="Run tests concurrently:"
pytest -n 4 # Run tests in parallel with 4 workers
```

### 99. Using argparse for Command-Line Arguments

```py title="Parse command-line arguments:"
import argparse

parser = argparse.ArgumentParser(description='Process some integers.') 
parser.add_argument('integers', metavar='N', type=int, nargs='+',
            help='an integer for the accumulator') 
parser.add_argument('--sum', dest='accumulate', action='store_const',
            const=sum, default=max,
            help='sum the integers (default: find the max)')

args = parser.parse_args() 
print(args.accumulate(args.integers))
```

### 100. Using jsonschema for JSON Validation

```py title="Validate JSON against a schema:"
from jsonschema import validate
from jsonschema.exceptions import ValidationError

schema = {
  "type": "object", 
  "properties": {
     "name": {"type": "string"},
     "age": {"type": "integer", "minimum": 0}
  },
  "required": ["name", "age"]
}
data = {"name": "John", "age": 30} 
try:
  validate(instance=data, schema=schema) 
  print("Data is valid")
except ValidationError as e: 
  print(f"Data is invalid: {e.message}")
```
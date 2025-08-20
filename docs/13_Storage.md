# Storage Cheat Sheet

## Storage Types in DevOps

- **Block Storage** – Used for databases, VMs, containers (e.g., EBS, Cinder)
- **File Storage** – Used for shared access & persistence (e.g., NFS, EFS)
- **Object Storage** – Used for backups, logs, and media (e.g., S3, MinIO)

## Disk Management (Linux)

```bash title="List disks and partitions"
lsblk
```
```bash title="List disk partitions"
fdisk -l
```
```bash title="Show disk usage"
df -h
```
```bash title="Check disk space usage"
du -sh /path/to/directory
```
```bash title="Mount a disk"
mount /dev/sdb1 /mnt
```
```bash title="Unmount a disk"
umount /mnt
```
```bash title="Format a disk"
mkfs.ext4 /dev/sdb1
```
```bash title="Check filesystem usage"
df -Th
```
```bash title="Check disk health"
smartctl -a /dev/sdb
```

## AWS S3

```bash title="List all S3 buckets"
aws s3 ls
```
```bash title="Upload a file to S3"
aws s3 cp file.txt s3://mybucket/
```
```bash title="Download a file from S3"
aws s3 cp s3://mybucket/file.txt .
```
```bash title="Sync local directory to S3"
aws s3 sync /local/path s3://mybucket/
```

## Azure Blob Storage

```bash title="List all storage accounts"
az storage account list
```
```bash title="Upload a file to Azure Blob"
az storage blob upload --container-name mycontainer --file file.txt --name file.txt
```
```bash title="Download a file from Azure Blob"
az storage blob download --container-name mycontainer --name file.txt --file file.txt
```

## Google Cloud Storage (GCS)

```bash title="List GCS buckets"
gsutil ls
```
```bash title="Upload a file to GCS"
gsutil cp file.txt gs://mybucket/
```
```bash title="Download a file from GCS"
gsutil cp gs://mybucket/file.txt .
```

## Linux Storage Monitoring

```bash title="Monitor disk usage in real-time"
iotop
```
```bash title="Check disk performance"
iostat -x 1
```

## Linux Backup

```bash title="Backup using rsync"
rsync -av --delete /source/ /backup/
```

## AWS Backup

```bash title="Backup data to AWS S3"
aws s3 sync /data s3://backup-bucket/
```

## Azure Backup

```bash title="Backup data to Azure Blob"
az storage blob upload-batch --destination mycontainer --source /data
```

## YAML Configurations:

### Persistent Volume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: "/mnt/data"
```

### Persistent Volume Claim (PVC)

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
      storage: 5Gi
```

### Pod with Persistent Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: storage-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: storage-volume
  volumes:
  - name: storage-volume
    persistentVolumeClaim:
    claimName: my-pvc
```

## Terraform Configurations:

- AWS S3 Bucket

```tf
provider "aws" { 
  region = "us-east-1"
}
resource "aws_s3_bucket" "devops_bucket" { 
  bucket = "devops-backup-bucket"
  acl = "private"
}
output "bucket_name" {
  value = aws_s3_bucket.devops_bucket.id
}
```

- Azure Storage Account

```tf
provider "azurerm" {
  features {}
}

resource "azurerm_storage_account" "example" { 
  name = "devopsstorageacc" 
  resource_group_name = "devops-rg"
  location = "East US"
  account_tier = "Standard" 
  account_replication_type = "LRS"
}
```

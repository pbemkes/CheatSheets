# Kubernetes

---

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
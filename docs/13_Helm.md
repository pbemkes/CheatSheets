# Helm Chart

!!! note "What is Helm?"
    - Helm helps deploy applications in Kubernetes using pre-configured templates called Helm Charts.
    - A Chart is a collection of files that describe a Kubernetes application.

## Helm Basics

```bash title="Check which version of Helm is installed"
helm version
```

```bash title="Get help with Helm commands"
helm help
```

```bash title="Show all added Helm repositories"
helm repo list
```

## Adding and Updating Repositories

```bash title="Add a repository"
helm repo add <repo-name> <repo-url>
```

```bash title="Get the latest list of available charts"
helm repo update
```

```bash title="Search for a chart"
helm search repo <chart-name>
```

## Installing Applications using Helm

```bash title="Install an application"
helm install <release-name> <chart>
```

```bash title="See running applications in Kubernetes"
kubectl get pods
```

## Listing Installed Applications

```bash title="Show all installed applications"
helm list
```

## Checking Application Details

```bash title="Check the status of your installed application"
helm status <release-name>
```

```bash title="See configuration values used"
helm get values <release-name>
```

## Updating an Installed Application

```bash title="Update an installed application"
helm upgrade <release-name> <chart> --set <parameter>
```

## Uninstalling an Application

```bash title="Remove an application"
helm uninstall <release-name>
```

## Debugging Helm Charts

```bash title="Check for issues in a Helm chart"
helm lint <chart>
```

```bash title="Simulate installation"
helm install --debug --dry-run <release-name> <chart>
```

## Creating Your Own Helm Chart

```bash title="Create a new Helm chart"
helm create <chart-name>
```
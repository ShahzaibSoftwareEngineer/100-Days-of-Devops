# Day 49: Kubernetes Deployments Lab

## Defination

### What is a Kubernetes Deployment?
A **Deployment** is a Kubernetes resource that provides declarative updates for Pods and ReplicaSets. It's the recommended way to manage stateless applications in Kubernetes.

**Core Functions:**
- **Pod Management**: Creates a pod  and manages identical pods
- **Desired State**: Ensures specified number of replicas are always running
- **Rolling Updates**: Updates applications without downtime
- **Rollback**: Reverts to previous versions if issues occur
- **Scaling**: Increases or decreases pod replicas
- **Self-Healing**: Automatically replaces failed or unhealthy pods

**Architecture Hierarchy:**
```
Deployment
    ↓
ReplicaSet (manages pod replicas)
    ↓
Pods (running containers)
```

**Key Features:**
- **Declarative**: Describe desired state, Kubernetes makes it happen
- **Version Control**: Tracks deployment history and revisions
- **Health Monitoring**: Monitors pod health and status
- **Load Distribution**: Distributes pods across nodes
- **Resource Management**: Manages CPU, memory, and other resources

## Lab Objective
Create a deployment named `httpd` using the `httpd:latest` image.

## Prerequisites
- kubectl configured on jump_host
- Access to Kubernetes cluster

## Step-by-Step Solution

### 1. Verify cluster connection
```bash
kubectl cluster-info
```

### 2. Check current state
```bash
kubectl get deployments
kubectl get pods
```

### 3. Create the deployment
```bash
kubectl create deployment httpd --image=httpd:latest
```

### 4. Verify deployment
```bash
kubectl get deployments
kubectl get pods
```

### 5. Check detailed information
```bash
kubectl describe deployment httpd
kubectl get deployments -o wide
```

## Expected Output

**Deployments:**
```
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
httpd   1/1     1            1           27s
```

**Pods:**
```
NAME                     READY   STATUS    RESTARTS   AGE
httpd-69545969bd-h9ssm   1/1     Running   0          42s
```

**Wide view:**
```
NAME    READY   UP-TO-DATE   AVAILABLE   AGE    CONTAINERS   IMAGES         SELECTOR
httpd   1/1     1            1           116s   httpd        httpd:latest   app=httpd
```

## Key Commands Reference
| Command | Purpose |
|---------|---------|
| `kubectl create deployment <name> --image=<image>` | Create deployment |
| `kubectl get deployments` | List deployments |
| `kubectl get pods` | List pods |
| `kubectl describe deployment <name>` | Detailed deployment info |
| `kubectl delete deployment <name>` | Delete deployment |

## Verification Checklist
- ✅ Deployment shows READY 1/1
- ✅ Pod STATUS is Running
- ✅ Image is httpd:latest
- ✅ No restarts

## What Happened?
1. Deployment `httpd` was created
2. Deployment created ReplicaSet `httpd-69545969bd`
3. ReplicaSet created Pod `httpd-69545969bd-h9ssm`
4. Pod pulled and runs `httpd:latest` image
5. Apache HTTP server is now running on port 80

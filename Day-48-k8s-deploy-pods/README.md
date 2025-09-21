# 🚀 Day 48: Deploy Pods in Kubernetes Cluster

## 📝 What I Learned Today

Successfully deployed my first **Pod** in Kubernetes! Understanding the fundamental building blocks of K8s container orchestration.

### 🎯 Lab Objective
Create a pod named `pod-nginx` with specific configurations:
- **Image**: `nginx:latest`
- **Container Name**: `nginx-container`
- **Label**: `app=nginx_app`

## 🧠 Pod  Simplified

### What is a Pod?
A **Pod**  is the smallest deployable and manageable unit in Kubernetes. It represents a group of one or more containers that share storage, network, and a specification for how to run the containers.

**Key Concepts:**
- 🏠 **Single Home**: One or more containers that share the same network and storage
- 🔗 **Shared Resources**: Containers in a pod share IP address and storage volumes
- ⚡ **Atomic Unit**: Pods are created, scheduled, and destroyed as a single unit
- 🎯 **Single Purpose**: Generally runs one main application container

### Why Pods, Not Just Containers?
```
Container → Pod → Node → Cluster
    ↳ Basic unit  ↳ Scheduling unit  ↳ Compute unit  ↳ Full system
```

## 💻 Lab Commands Performed

### Step 1: Verify Cluster Connectivity
```bash
kubectl cluster-info
```

### Step 2: Check Node Status  
```bash
kubectl get nodes
```

### Step 3: Create Pod YAML Manifest
```bash
vi pod-nginx.yaml
```

**Pod YAML Content:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

### Step 4: Deploy the Pod
```bash
kubectl apply -f pod-nginx.yaml
```

### Step 5: Verify Pod Status
```bash
kubectl get pods
```

### Step 6: Get Detailed Pod Information
```bash
kubectl describe pod pod-nginx
```

### Step 7: Verify Pod Labels
```bash
kubectl get pod pod-nginx --show-labels
```

### Step 8: Check Pod with Network Details
```bash
kubectl get pod pod-nginx -o wide
```

## ✅ Lab Completion Verification

| **Requirement** | **Expected** | **Status** |
|-----------------|-------------|------------|
| Pod Name | `pod-nginx` | ✅ |
| Image | `nginx:latest` | ✅ |
| Container Name | `nginx-container` | ✅ |
| Label | `app=nginx_app` | ✅ |
| Pod Status | Running | ✅ |



## 🔍 Key Takeaways

### Pod Lifecycle States
- **Pending**: Pod accepted but not scheduled
- **Running**: Pod bound to node, containers created
- **Succeeded**: All containers terminated successfully
- **Failed**: At least one container failed
- **Unknown**: Pod state cannot be determined

### Essential kubectl Commands for Pods
```bash
kubectl get pods                    # List all pods
kubectl describe pod <pod-name>     # Detailed pod info
kubectl logs <pod-name>            # View pod logs
kubectl delete pod <pod-name>      # Delete pod
kubectl get pod <name> -o yaml     # Pod YAML output
```



*Building strong foundations in container orchestration, one pod at a time.*

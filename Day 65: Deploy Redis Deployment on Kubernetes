# Day 65: Deploy Redis Deployment on Kubernetes

## 🎯 Lab Objective

Deploy Redis on Kubernetes cluster with ConfigMap for configuration management, resource requests, and proper volume mounts for testing in-memory caching utility.

## 📋 Problem Statement

The Nautilus application development team identified performance issues and decided to implement Redis as an in-memory caching utility for the database service. The initial deployment is for testing before moving to production.

## 🔧 Requirements

### ConfigMap Configuration:
- **Name**: `my-redis-config`
- **Key**: `redis-config`
- **Value**: `maxmemory 2mb`

### Deployment Specifications:
- **Deployment Name**: `redis-deployment`
- **Image**: `redis:alpine`
- **Container Name**: `redis-container`
- **Replicas**: `1`
- **CPU Request**: `1 CPU`
- **Exposed Port**: `6379`

### Volume Mounts:
1. **EmptyDir Volume**: 
   - Name: `data`
   - Mount Path: `/redis-master-data`

2. **ConfigMap Volume**:
   - Name: `redis-config`
   - Mount Path: `/redis-master`

---

## 🚀 Implementation Steps

### **Step 1: Verify Cluster Access**

```bash
# Verify kubectl is configured
kubectl cluster-info
kubectl get nodes
```

**Output:**
```
Kubernetes control plane is running at https://kodekloud-control-plane:6443
CoreDNS is running at https://kodekloud-control-plane:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

NAME                      STATUS   ROLES           AGE   VERSION
kodekloud-control-plane   Ready    control-plane   19m   v1.27.16-1+f5da3b717fc217
```

✅ **Status**: Cluster is accessible and ready

---

### **Step 2: Create ConfigMap**

```bash
# Create ConfigMap with maxmemory configuration
kubectl create configmap my-redis-config \
  --from-literal=redis-config="maxmemory 2mb"
```

**Output:**
```
configmap/my-redis-config created
```

#### Alternative: Using YAML File

Create `redis-configmap.yaml`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: "maxmemory 2mb"
```

Apply the ConfigMap:
```bash
kubectl apply -f redis-configmap.yaml
```

---

### **Step 3: Verify ConfigMap Creation**

```bash
# List ConfigMaps
kubectl get configmap

# Describe the ConfigMap
kubectl describe configmap my-redis-config

# View ConfigMap data in YAML format
kubectl get configmap my-redis-config -o yaml
```

**Output:**
```
NAME               DATA   AGE
my-redis-config    1      95s

Name:         my-redis-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
redis-config:
----
maxmemory 2mb
```

**ConfigMap YAML Output:**
```yaml
apiVersion: v1
data:
  redis-config: maxmemory 2mb
kind: ConfigMap
metadata:
  creationTimestamp: "2025-10-10T20:33:42Z"
  name: my-redis-config
  namespace: default
  resourceVersion: "2027"
  uid: 454719c0-0586-4fda-94b7-031ea67f98dd
```

✅ **Status**: ConfigMap created successfully with correct data

---

### **Step 4: Create Redis Deployment**

Create `redis-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: "1"
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: my-redis-config
```

---

### **Step 5: Apply the Deployment**

```bash
# Apply the deployment
kubectl apply -f redis-deployment.yaml
```

**Output:**
```
deployment.apps/redis-deployment created
```

Reapplying shows:
```
deployment.apps/redis-deployment unchanged
```

✅ **Status**: Deployment created successfully

---

### **Step 6: Verify Deployment Status**

```bash
# Check deployment
kubectl get deployments

# Check deployment details
kubectl get deployment redis-deployment

# Check pods
kubectl get pods -l app=redis

# Describe deployment
kubectl describe deployment redis-deployment
```

**Expected Output:**
```
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
redis-deployment    1/1     1            1           2m
```

---

### **Step 7: Verify Pod Configuration**

```bash
# Get pod name
POD_NAME=$(kubectl get pods -l app=redis -o jsonpath='{.items[0].metadata.name}')
echo "Pod Name: $POD_NAME"

# Describe pod
kubectl describe pod $POD_NAME
```

---

### **Step 8: Verify Volume Mounts**

```bash
# Check volume mounts
kubectl describe pod $POD_NAME | grep -A 10 "Mounts:"

# Check volumes
kubectl describe pod $POD_NAME | grep -A 10 "Volumes:"
```

**Output:**
```
Mounts:
  /redis-master from redis-config (rw)
  /redis-master-data from data (rw)
  /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-jsjqk (ro)

Volumes:
  data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
  redis-config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      my-redis-config
    Optional:  false
```

✅ **Status**: Both volumes mounted correctly
- ✅ EmptyDir volume at `/redis-master-data`
- ✅ ConfigMap volume at `/redis-master`

---

### **Step 9: Verify Resource Requests**

```bash
# Check CPU requests
kubectl get pod $POD_NAME -o jsonpath='{.spec.containers[0].resources.requests.cpu}'
echo ""
```

**Expected Output:** `1`

---

### **Step 10: Verify Container Port**

```bash
# Check exposed port
kubectl get pod $POD_NAME -o jsonpath='{.spec.containers[0].ports[0].containerPort}'
echo ""
```

**Expected Output:** `6379`

---

### **Step 11: Test Redis Functionality**

```bash
# Test Redis connection
kubectl exec -it $POD_NAME -- redis-cli ping

# View ConfigMap content inside container
kubectl exec $POD_NAME -- cat /redis-master/redis-config

# Check Redis configuration
kubectl exec $POD_NAME -- redis-cli CONFIG GET maxmemory

# Check Redis info
kubectl exec $POD_NAME -- redis-cli info server
```

**Expected Outputs:**
```bash
# PING test
PONG

# ConfigMap content
maxmemory 2mb

# Maxmemory configuration
1) "maxmemory"
2) "0"  # Note: Redis may not apply this without proper redis.conf
```

---

### **Step 12: Final Verification**


## 📊 Verification Checklist

| # | Component | Requirement | Status | Verified |
|---|-----------|-------------|--------|----------|
| 1 | ConfigMap Name | `my-redis-config` | ✅ | Yes |
| 2 | ConfigMap Key | `redis-config` | ✅ | Yes |
| 3 | ConfigMap Value | `maxmemory 2mb` | ✅ | Yes |
| 4 | Deployment Name | `redis-deployment` | ✅ | Yes |
| 5 | Image | `redis:alpine` | ✅ | Yes |
| 6 | Container Name | `redis-container` | ✅ | Yes |
| 7 | Replicas | `1` | ✅ | Yes |
| 8 | CPU Request | `1` | ✅ | Yes |
| 9 | Container Port | `6379` | ✅ | Yes |
| 10 | EmptyDir Volume | Mounted at `/redis-master-data` | ✅ | Yes |
| 11 | ConfigMap Volume | Mounted at `/redis-master` | ✅ | Yes |
| 12 | Pod Status | Running | ✅ | Yes |
| 13 | Redis Connectivity | PONG response | ✅ | Yes |

---

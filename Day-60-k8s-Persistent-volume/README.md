# Day-60 Kubernetes Persistent Volumes - Complete Learning Guide


## 🎯 Introduction

This repository provides a comprehensive guide to understanding and implementing Persistent Volumes (PV) and Persistent Volume Claims (PVC) in Kubernetes.

### What You'll Learn
- ✅ Kubernetes storage  and fundamentals
- ✅ PersistentVolume (PV) and PersistentVolumeClaim (PVC) concepts
- ✅ Storage Classes a dynamic provisioning
- ✅ Different volume types and access modes
- ✅ Real-world deployment scenarios

---

## 📖 Theory & Concepts

### 1. Why Do We Need Persistent Volumes?

**Problem:** Containers are ephemeral (temporary). When a pod dies, all data inside is lost.

**Solution:** Persistent Volumes provide storage that exists independently of pod lifecycle.

```
Without PV:           With PV:
┌─────────┐          ┌─────────┐
│   Pod   │          │   Pod   │
│  ┌───┐  │          │  ┌───┐  │
│  │App│  │          │  │App│  │───┐
│  └───┘  │          │  └───┘  │   │
└─────────┘          └─────────┘   │
     ↓                              ↓
 Data Lost!              ┌──────────────┐
                         │ Persistent   │
                         │   Volume     │
                         └──────────────┘
                         Data Survives! ✓
```

### 2. Core Concepts

#### **PersistentVolume (PV)**
- Cluster resource provisioned by administrator
- Independent of any pod
- Has a lifecycle independent of pods
- Like a "storage disk" available in the cluster

#### **PersistentVolumeClaim (PVC)**
- Request for storage by a user/pod
- Binds to an available PV
- Like "ordering" storage from available PVs

#### **StorageClass**
- Defines different types/classes of storage
- Enables dynamic provisioning
- Examples: fast-ssd, slow-hdd, cloud-storage

### 3. The PV/PVC Workflow

```
Step 1: Admin creates PV
┌──────────────────────┐
│  PersistentVolume    │
│  - Size: 10Gi        │
│  - Type: NFS         │
│  - Available         │
└──────────────────────┘

Step 2: User creates PVC
┌──────────────────────┐
│PersistentVolumeClaim │
│  - Request: 5Gi      │
│  - Looking for PV... │
└──────────────────────┘

Step 3: Binding
┌──────────────────────┐
│  PV ←──── Bound ────→ PVC │
└──────────────────────┘

Step 4: Pod uses PVC
┌──────────────────────┐
│       Pod            │
│   Uses PVC name      │
│   Mounts storage     │
└──────────────────────┘
```

### 4. Access Modes

| Mode | Abbreviation | Description |
|------|--------------|-------------|
| ReadWriteOnce | RWO | Volume mounted read-write by single node |
| ReadOnlyMany | ROX | Volume mounted read-only by many nodes |
| ReadWriteMany | RWX | Volume mounted read-write by many nodes |
| ReadWriteOncePod | RWOP | Volume mounted read-write by single pod |


### 5. PV Lifecycle States

```
Available → Bound → Released → Failed
    ↓          ↓         ↓         ↓
 No claim   In use   PVC deleted  Error
```

---


---

## 🧪 Lab Exercises

### Lab 1: Basic PV and PVC (hostPath)

#### Step 1: Create PersistentVolume
```yaml
# pv-basic.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-basic
spec:
  storageClassName: manual
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

```bash
kubectl apply -f pv-basic.yaml
kubectl get pv
```

#### Step 2: Create PersistentVolumeClaim
```yaml
# pvc-basic.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-basic
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

```bash
kubectl apply -f pvc-basic.yaml
kubectl get pvc
```

#### Step 3: Create Pod Using PVC
```yaml
# pod-basic.yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-basic
spec:
  containers:
  - name: app
    image: nginx:latest
    volumeMounts:
    - name: storage
      mountPath: /usr/share/nginx/html
  volumes:
  - name: storage
    persistentVolumeClaim:
      claimName: pvc-basic
```

```bash
kubectl apply -f pod-basic.yaml
kubectl get pods
```

#### Step 4: Test Persistence
```bash
# Write data to volume
kubectl exec pod-basic -- sh -c "echo 'Hello Persistent World!' > /usr/share/nginx/html/index.html"

# Verify data
kubectl exec pod-basic -- cat /usr/share/nginx/html/index.html

# Delete and recreate pod
kubectl delete pod pod-basic
kubectl apply -f pod-basic.yaml

# Data still exists!
kubectl exec pod-basic -- cat /usr/share/nginx/html/index.html
```

---

### Lab 2: Web Server with Persistent Storage (Nautilus Lab)

```yaml
# Complete setup for Nautilus lab
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nautilus
spec:
  storageClassName: manual
  capacity:
    storage: 3Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nautilus
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi

---
apiVersion: v1
kind: Pod
metadata:
  name: pod-nautilus
  labels:
    app: web-nautilus
spec:
  containers:
  - name: container-nautilus
    image: httpd:latest
    volumeMounts:
    - name: pv-storage
      mountPath: /usr/local/apache2/htdocs
  volumes:
  - name: pv-storage
    persistentVolumeClaim:
      claimName: pvc-nautilus

---
apiVersion: v1
kind: Service
metadata:
  name: web-nautilus
spec:
  type: NodePort
  selector:
    app: web-nautilus
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30008
    protocol: TCP
```

```bash
# Apply all at once
kubectl apply -f nautilus-complete.yaml

# Verify everything
kubectl get pv,pvc,pods,svc

# Add content
kubectl exec pod-nautilus -- sh -c "echo '<h1>Persistent Storage Works!</h1>' > /usr/local/apache2/htdocs/index.html"

# Access the service
curl http://<node-ip>:30008
```

---

### Lab 3: StatefulSet with PVC Template

```yaml
# statefulset-mysql.yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
  - port: 3306
  clusterIP: None
  selector:
    app: mysql

---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "password123"
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: manual
      resources:
        requests:
          storage: 5Gi
```

  🤝 Contributing
Feel free to submit issues and enhancement requests


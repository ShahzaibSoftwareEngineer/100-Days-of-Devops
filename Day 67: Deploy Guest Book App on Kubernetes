# Day 66: Deploy MySQL on Kubernetes

## 🎯 Lab Objective

Deploy a MySQL server on Kubernetes cluster with persistent storage, secrets management, and proper service exposure.

## 📋 Lab Requirements

### 1. PersistentVolume (PV)
- **Name**: `mysql-pv`
- **Capacity**: `250Mi`

### 2. PersistentVolumeClaim (PVC)
- **Name**: `mysql-pv-claim`
- **Storage Request**: `250Mi`

### 3. Deployment
- **Name**: `mysql-deployment`
- **Image**: `mysql:latest` (or any MySQL image)
- **Mount Path**: `/var/lib/mysql`

### 4. Service
- **Type**: `NodePort`
- **Name**: `mysql`
- **NodePort**: `30007`

### 5. Secrets
- **Secret 1**: `mysql-root-pass`
  - Key: `password`
  - Value: `YUIidhb667`
  
- **Secret 2**: `mysql-user-pass`
  - Key 1: `username` → Value: `kodekloud_aim`
  - Key 2: `password` → Value: `BruCStnMT5`
  
- **Secret 3**: `mysql-db-url`
  - Key: `database`
  - Value: `kodekloud_db3`

### 6. Environment Variables
- `MYSQL_ROOT_PASSWORD` → from `mysql-root-pass.password`
- `MYSQL_DATABASE` → from `mysql-db-url.database`
- `MYSQL_USER` → from `mysql-user-pass.username`
- `MYSQL_PASSWORD` → from `mysql-user-pass.password`

---

## 🚀 Step-by-Step Solution

### **STEP 1: Verify Cluster Access**

```bash
# Connect to jump_host and verify kubectl
kubectl cluster-info
kubectl get nodes
```

---

### **STEP 2: Create Secrets**

#### Create Secret 1: mysql-root-pass

```bash
kubectl create secret generic mysql-root-pass \
  --from-literal=password=YUIidhb667
```

#### Create Secret 2: mysql-user-pass

```bash
kubectl create secret generic mysql-user-pass \
  --from-literal=username=kodekloud_aim \
  --from-literal=password=BruCStnMT5
```

#### Create Secret 3: mysql-db-url

```bash
kubectl create secret generic mysql-db-url \
  --from-literal=database=kodekloud_db3
```

#### Verify Secrets

```bash
# List all secrets
kubectl get secrets

# Describe secrets
kubectl describe secret mysql-root-pass
kubectl describe secret mysql-user-pass
kubectl describe secret mysql-db-url

# View secret data (base64 encoded)
kubectl get secret mysql-root-pass -o yaml
kubectl get secret mysql-user-pass -o yaml
kubectl get secret mysql-db-url -o yaml
```

---

### **STEP 3: Create PersistentVolume**

Create file `mysql-pv.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data/mysql
```

Apply the PersistentVolume:

```bash
kubectl apply -f mysql-pv.yaml
```

Verify:

```bash
kubectl get pv
kubectl describe pv mysql-pv
```

---

### **STEP 4: Create PersistentVolumeClaim**

Create file `mysql-pvc.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
```

Apply the PersistentVolumeClaim:

```bash
kubectl apply -f mysql-pvc.yaml
```

Verify:

```bash
kubectl get pvc
kubectl describe pvc mysql-pv-claim
```

**Expected Status**: `Bound` (PVC should be bound to PV)

---

### **STEP 5: Create MySQL Deployment**

Create file `mysql-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
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
        image: mysql:5.7
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
```

Apply the Deployment:

```bash
kubectl apply -f mysql-deployment.yaml
```

Verify:

```bash
kubectl get deployments
kubectl get deployment mysql-deployment
kubectl describe deployment mysql-deployment
kubectl get pods -l app=mysql
```

---

### **STEP 6: Create MySQL Service**

Create file `mysql-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
  - port: 3306
    targetPort: 3306
    nodePort: 30007
    protocol: TCP
```

Apply the Service:

```bash
kubectl apply -f mysql-service.yaml
```

Verify:

```bash
kubectl get svc
kubectl get svc mysql
kubectl describe svc mysql
```

**Expected Output**: Service should show `3306:30007/TCP`

---

### **STEP 7: Verify Complete Deployment**

```bash
# Check all resources
kubectl get all

# Check PV and PVC status
kubectl get pv
kubectl get pvc

# Check secrets
kubectl get secrets

# Get pod name
POD_NAME=$(kubectl get pods -l app=mysql -o jsonpath='{.items[0].metadata.name}')
echo "Pod Name: $POD_NAME"

# Check pod status
kubectl get pod $POD_NAME

# Wait for pod to be ready
kubectl wait --for=condition=ready pod -l app=mysql --timeout=120s

# Check pod logs
kubectl logs $POD_NAME

# Describe pod to verify volumes and environment variables
kubectl describe pod $POD_NAME
```

---

### **STEP 8: Test MySQL Connection**

```bash
# Get pod name
POD_NAME=$(kubectl get pods -l app=mysql -o jsonpath='{.items[0].metadata.name}')

# Test MySQL connection with root password
kubectl exec -it $POD_NAME -- mysql -uroot -pYUIidhb667 -e "SHOW DATABASES;"

# Test with created user
kubectl exec -it $POD_NAME -- mysql -ukodekloud_aim -pBruCStnMT5 -e "SHOW DATABASES;"

# Access MySQL shell interactively
kubectl exec -it $POD_NAME -- mysql -uroot -pYUIidhb667

# Inside MySQL shell, run:
# SHOW DATABASES;
# USE kodekloud_db3;
# SELECT USER();
# EXIT;
```

---

### **STEP 9: Verify Environment Variables**

```bash
# Check environment variables in the container
kubectl exec $POD_NAME -- env | grep MYSQL

# Or describe pod to see environment variables
kubectl describe pod $POD_NAME | grep -A 20 "Environment:"
```

**Expected Output:**
```
MYSQL_ROOT_PASSWORD=<set to the key 'password' in secret 'mysql-root-pass'>
MYSQL_DATABASE=<set to the key 'database' in secret 'mysql-db-url'>
MYSQL_USER=<set to the key 'username' in secret 'mysql-user-pass'>
MYSQL_PASSWORD=<set to the key 'password' in secret 'mysql-user-pass'>
```

---

### **STEP 10: Test Service via NodePort**

```bash
# Get node IP
NODE_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="InternalIP")].address}')
echo "Node IP: $NODE_IP"

# Test MySQL connection via NodePort (if mysql client is installed on jump_host)
mysql -h $NODE_IP -P 30007 -ukodekloud_aim -pBruCStnMT5 -e "SHOW DATABASES;"

# Alternative: Use telnet to test port connectivity
telnet $NODE_IP 30007

# Or use nc (netcat)
nc -zv $NODE_IP 30007
```

---

## 📝 Complete YAML Files

### 1. Secrets (Using YAML - Alternative Method)

Create file `mysql-secrets.yaml`:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-root-pass
type: Opaque
stringData:
  password: YUIidhb667
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-user-pass
type: Opaque
stringData:
  username: kodekloud_aim
  password: BruCStnMT5
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-db-url
type: Opaque
stringData:
  database: kodekloud_db3
```

### 2. PersistentVolume

Create file `mysql-pv.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data/mysql
```

### 3. PersistentVolumeClaim

Create file `mysql-pvc.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
```

### 4. Deployment

Create file `mysql-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
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
        image: mysql:5.7
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
```

### 5. Service

Create file `mysql-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
  - port: 3306
    targetPort: 3306
    nodePort: 30007
    protocol: TCP
```

---

## 🔄 Complete Automation Script

```bash
#!/bin/bash

echo "=== Day 66: Deploying MySQL on Kubernetes ==="

# Step 1: Create Secrets
echo -e "\n[1/6] Creating Secrets..."
kubectl create secret generic mysql-root-pass \
  --from-literal=password=YUIidhb667 \
  --dry-run=client -o yaml | kubectl apply -f -

kubectl create secret generic mysql-user-pass \
  --from-literal=username=kodekloud_aim \
  --from-literal=password=BruCStnMT5 \
  --dry-run=client -o yaml | kubectl apply -f -

kubectl create secret generic mysql-db-url \
  --from-literal=database=kodekloud_db3 \
  --dry-run=client -o yaml | kubectl apply -f -

echo "Secrets created successfully!"
kubectl get secrets | grep mysql

# Step 2: Create PersistentVolume
echo -e "\n[2/6] Creating PersistentVolume..."
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data/mysql
EOF

echo "PersistentVolume created!"
kubectl get pv

# Step 3: Create PersistentVolumeClaim
echo -e "\n[3/6] Creating PersistentVolumeClaim..."
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
EOF

echo "PersistentVolumeClaim created!"
kubectl get pvc

# Step 4: Create Deployment
echo -e "\n[4/6] Creating MySQL Deployment..."
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
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
        image: mysql:5.7
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
EOF

echo "Deployment created!"
kubectl get deployment mysql-deployment

# Step 5: Create Service
echo -e "\n[5/6] Creating MySQL Service..."
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
  - port: 3306
    targetPort: 3306
    nodePort: 30007
    protocol: TCP
EOF

echo "Service created!"
kubectl get svc mysql


## 📊 Verification Checklist

| # | Component | Requirement | Command to Verify | Status |
|---|-----------|-------------|-------------------|--------|
| 1 | PV Name | `mysql-pv` | `kubectl get pv mysql-pv` | ⬜ |
| 2 | PV Capacity | `250Mi` | `kubectl get pv mysql-pv -o jsonpath='{.spec.capacity.storage}'` | ⬜ |
| 3 | PVC Name | `mysql-pv-claim` | `kubectl get pvc mysql-pv-claim` | ⬜ |
| 4 | PVC Request | `250Mi` | `kubectl get pvc mysql-pv-claim -o jsonpath='{.spec.resources.requests.storage}'` | ⬜ |
| 5 | PVC Status | `Bound` | `kubectl get pvc mysql-pv-claim -o jsonpath='{.status.phase}'` | ⬜ |
| 6 | Deployment Name | `mysql-deployment` | `kubectl get deployment mysql-deployment` | ⬜ |
| 7 | Mount Path | `/var/lib/mysql` | `kubectl describe pod <pod> \| grep /var/lib/mysql` | ⬜ |
| 8 | Service Name | `mysql` | `kubectl get svc mysql` | ⬜ |
| 9 | Service Type | `NodePort` | `kubectl get svc mysql -o jsonpath='{.spec.type}'` | ⬜ |
| 10 | NodePort | `30007` | `kubectl get svc mysql -o jsonpath='{.spec.ports[0].nodePort}'` | ⬜ |
| 11 | Secret 1 | `mysql-root-pass` | `kubectl get secret mysql-root-pass` | ⬜ |
| 12 | Secret 2 | `mysql-user-pass` | `kubectl get secret mysql-user-pass` | ⬜ |
| 13 | Secret 3 | `mysql-db-url` | `kubectl get secret mysql-db-url` | ⬜ |
| 14 | Env: MYSQL_ROOT_PASSWORD | From secret | `kubectl describe pod <pod> \| grep -A 5 MYSQL_ROOT_PASSWORD` | ⬜ |
| 15 | Env: MYSQL_DATABASE | From secret | `kubectl describe pod <pod> \| grep -A 5 MYSQL_DATABASE` | ⬜ |
| 16 | Env: MYSQL_USER | From secret | `kubectl describe pod <pod> \| grep -A 5 MYSQL_USER` | ⬜ |
| 17 | Env: MYSQL_PASSWORD | From secret | `kubectl describe pod <pod> \| grep -A 5 MYSQL_PASSWORD` | ⬜ |
| 18 | MySQL Running | Pod Running | `kubectl get pods -l app=mysql` | ⬜ |
| 19 | MySQL Connectivity | PONG response | `kubectl exec <pod> -- mysql -uroot -p... -e "SELECT 1;"` | ⬜ |

---

## 🎓 Key Concepts

### 1. **Kubernetes Secrets**
- Store sensitive data like passwords, tokens, keys
- Base64 encoded (not encrypted)
- Can be mounted as files or exposed as environment variables
- Use `stringData` for plain text in YAML, `data` for base64

### 2. **PersistentVolume (PV)**
- Cluster-wide storage resource
- Independent lifecycle from pods
- Can be provisioned statically or dynamically
- `hostPath` type stores data on node's filesystem

### 3. **PersistentVolumeClaim (PVC)**
- Request for storage by a user/pod
- Binds to available PV that satisfies requirements
- Abstracts storage details from pods
- Status should be `Bound` when matched with PV

### 4. **Volume Mounts**
- `/var/lib/mysql` is MySQL's default data directory
- All databases, tables, and data stored here
- Must persist across pod restarts
- PVC provides persistent storage

### 5. **Environment Variables from Secrets**
- `valueFrom.secretKeyRef` injects secret values
- More secure than hardcoding in YAML
- Changes to secrets don't auto-update running pods
- Requires pod restart to pick up new values

### 6. **MySQL Configuration**
- `MYSQL_ROOT_PASSWORD`: Root user password
- `MYSQL_DATABASE`: Auto-creates database on first start
- `MYSQL_USER`: Creates non-root user
- `MYSQL_PASSWORD`: Password for the user

### 7. **NodePort Service**
- Exposes service on each node's IP at static port
- Port range: 30000-32767
- Accessible from outside cluster
- Format: `<NodeIP>:<NodePort>`

---

## 🔍 Architecture Diagram

```
┌─────────────────────────────────────────────────┐
│              MySQL Service (NodePort)            │
│              Type: NodePort                      │
│              Port: 3306:30007                    │
└─────────────────┬───────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────┐
│          mysql-deployment (1 replica)            │
│  ┌───────────────────────────────────────────┐  │
│  │         Pod (mysql)                       │  │
│  │  ┌─────────────────────────────────────┐  │  │
│  │  │   Container: mysql:5.7              │  │  │
│  │  │   Port: 3306                        │  │  │
│  │  │                                     │  │  │
│  │  │   Environment Variables:            │  │  │
│  │  │   • MYSQL_ROOT_PASSWORD ────────┐   │  │  │
│  │  │   • MYSQL_DATABASE ─────────────┼┐  │  │  │
│  │  │   • MYSQL_USER ─────────────────┼┼┐ │  │  │
│  │  │   • MYSQL_PASSWORD ─────────────┼┼┼ │  │  │
│  │  │                                 │││ │  │  │
│  │  │   Volume Mount:                 │││ │  │  │
│  │  │   /var/lib/mysql ◄──────────┐   │││ │  │  │
│  │  └─────────────────────────────┼───┼┼┼─┘  │  │
│  └────────────────────────────────┼───┼┼┼────┘  │
└───────────────────────────────────┼───┼┼┼───────┘
                                    │   │││
                       ┌────────────┘   │││
                       │                │││
              ┌────────▼─────────┐  ┌───▼▼▼─────────────┐
              │  mysql-pv-claim  │  │    Secrets        │
              │  (PVC)           │  │                   │
              │  250Mi           │  │ • mysql-root-pass │
              └────────┬─────────┘  │ • mysql-user-pass │
                       │            │ • mysql-db-url    │
              ┌────────▼─────────┐  └───────────────────┘
              │    mysql-pv      │
              │    (PV)          │
              │    250Mi         │
              │    /mnt/data/mysql│
              └──────────────────┘

# Day 67: Deploy Guest Book App on Kubernetes - Lab Solution

## Architecture Overview
- **Backend Tier**: Redis Master (1 replica) + Redis Slave (2 replicas)
- **Frontend Tier**: PHP Application (3 replicas)
- **Services**: redis-master, redis-slave, frontend (NodePort)

---

## Step 1: Verify Kubernetes Cluster

```bash
# Check cluster connectivity
kubectl cluster-info

# Check available nodes
kubectl get nodes

# Verify kubectl is working
kubectl get pods --all-namespaces
```

---

## Step 2: Deploy Redis Master (Backend)

### Create Redis Master Deployment

```bash
cat <<EOF > redis-master-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
  labels:
    app: redis
    role: master
    tier: backend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
      role: master
      tier: backend
  template:
    metadata:
      labels:
        app: redis
        role: master
        tier: backend
    spec:
      containers:
      - name: master-redis-devops
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379
EOF
```

### Apply Redis Master Deployment

```bash
kubectl apply -f redis-master-deployment.yaml
```

### Verify Redis Master Deployment

```bash
kubectl get deployments
kubectl get pods -l app=redis,role=master
```

---

## Step 3: Create Redis Master Service

```bash
cat <<EOF > redis-master-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-master
  labels:
    app: redis
    role: master
    tier: backend
spec:
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    app: redis
    role: master
    tier: backend
EOF
```

### Apply Redis Master Service

```bash
kubectl apply -f redis-master-service.yaml
```

### Verify Service

```bash
kubectl get service redis-master
```

---

## Step 4: Deploy Redis Slave

### Create Redis Slave Deployment

```bash
cat <<EOF > redis-slave-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
  labels:
    app: redis
    role: slave
    tier: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
      role: slave
      tier: backend
  template:
    metadata:
      labels:
        app: redis
        role: slave
        tier: backend
    spec:
      containers:
      - name: slave-redis-devops
        image: gcr.io/google_samples/gb-redisslave:v3
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
        ports:
        - containerPort: 6379
EOF
```

### Apply Redis Slave Deployment

```bash
kubectl apply -f redis-slave-deployment.yaml
```

### Verify Redis Slave Deployment

```bash
kubectl get deployments
kubectl get pods -l app=redis,role=slave
```

---

## Step 5: Create Redis Slave Service

```bash
cat <<EOF > redis-slave-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
  labels:
    app: redis
    role: slave
    tier: backend
spec:
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    app: redis
    role: slave
    tier: backend
EOF
```

### Apply Redis Slave Service

```bash
kubectl apply -f redis-slave-service.yaml
```

### Verify Service

```bash
kubectl get service redis-slave
```

---

## Step 6: Deploy Frontend Application

### Create Frontend Deployment

```bash
cat <<EOF > frontend-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
      tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis-devops
        image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
        ports:
        - containerPort: 80
EOF
```

### Apply Frontend Deployment

```bash
kubectl apply -f frontend-deployment.yaml
```

### Verify Frontend Deployment

```bash
kubectl get deployments
kubectl get pods -l app=guestbook,tier=frontend
```

---

## Step 7: Create Frontend Service (NodePort)

```bash
cat <<EOF > frontend-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30009
  selector:
    app: guestbook
    tier: frontend
EOF
```

### Apply Frontend Service

```bash
kubectl apply -f frontend-service.yaml
```

### Verify Service

```bash
kubectl get service frontend
```

---

## Step 8: Verify Complete Deployment

### Check All Resources

```bash
# Check all deployments
kubectl get deployments

# Check all services
kubectl get services

# Check all pods
kubectl get pods

# Check pods with labels
kubectl get pods --show-labels
```

### Verify Pod Status

```bash
# Should show 1 redis-master pod
kubectl get pods -l app=redis,role=master

# Should show 2 redis-slave pods
kubectl get pods -l app=redis,role=slave

# Should show 3 frontend pods
kubectl get pods -l app=guestbook,tier=frontend
```

### Check Service Details

```bash
# Get detailed service information
kubectl describe service frontend
kubectl describe service redis-master
kubectl describe service redis-slave
```

---

## Step 9: Access the Guestbook Application

### Get Node IP (for Minikube)

```bash
# For Minikube
minikube ip

# Access the application at:
# http://<minikube-ip>:30009
```

### For Other Clusters

```bash
# Get node IP
kubectl get nodes -o wide

# Access the application at:
# http://<node-ip>:30009
```

### Test the Application

1. Open browser to `http://<node-ip>:30009`
2. Add some guest entries
3. Verify entries are saved and displayed

---

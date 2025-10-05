# Day 63 :Deploy Iron Gallery App on Kubernetes

## Overview
Deploy Iron Gallery application with MariaDB database on Kubernetes cluster using custom configurations.

## Architecture
- **Frontend**: Iron Gallery (Nginx-based)
- **Database**: MariaDB
- **Namespace**: iron-namespace-xfusion
- **Access**: NodePort service on port 32678

## Prerequisites
- Kubernetes cluster configured
- kubectl CLI access

## Deployment Steps

### 1. Create YAML Configuration

Create `iron-gallery-deployment.yaml`:

```yaml
---
# Namespace
apiVersion: v1
kind: Namespace
metadata:
  name: iron-namespace-xfusion

---
# Iron Gallery Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-xfusion
  namespace: iron-namespace-xfusion
  labels:
    run: iron-gallery
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
      - name: iron-gallery-container-xfusion
        image: kodekloud/irongallery:2.0
        resources:
          limits:
            memory: "100Mi"
            cpu: "50m"
        volumeMounts:
        - name: config
          mountPath: /usr/share/nginx/html/data
        - name: images
          mountPath: /usr/share/nginx/html/uploads
      volumes:
      - name: config
        emptyDir: {}
      - name: images
        emptyDir: {}

---
# Iron DB Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-xfusion
  namespace: iron-namespace-xfusion
  labels:
    db: mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
      - name: iron-db-container-xfusion
        image: kodekloud/irondb:2.0
        env:
        - name: MYSQL_DATABASE
          value: database_host
        - name: MYSQL_ROOT_PASSWORD
          value: Pr00t@Pass123
        - name: MYSQL_PASSWORD
          value: Puser@Pass123
        - name: MYSQL_USER
          value: ironuser
        volumeMounts:
        - name: db
          mountPath: /var/lib/mysql
      volumes:
      - name: db
        emptyDir: {}

---
# Iron DB Service
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  selector:
    db: mariadb
  type: ClusterIP
  ports:
  - protocol: TCP
    port: 3306
    targetPort: 3306

---
# Iron Gallery Service
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-xfusion
  namespace: iron-namespace-xfusion
spec:
  selector:
    run: iron-gallery
  type: NodePort
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 32678
```

### 2. Deploy to Kubernetes

```bash
kubectl apply -f iron-gallery-deployment.yaml
```

### 3. Verify Deployment

```bash
kubectl get all -n iron-namespace-xfusion
```

### 4. Access Application

```bash
http://<NODE_IP>:32678
```

## Components

| Component | Type | Name | Image |
|-----------|------|------|-------|
| Namespace | Namespace | iron-namespace-xfusion | - |
| Frontend | Deployment | iron-gallery-deployment-xfusion | kodekloud/irongallery:2.0 |
| Database | Deployment | iron-db-deployment-xfusion | kodekloud/irondb:2.0 |
| DB Service | ClusterIP | iron-db-service-xfusion | Port 3306 |
| Gallery Service | NodePort | iron-gallery-service-xfusion | Port 32678 |

## Configuration Details

### Iron Gallery
- **Resources**: 100Mi memory, 50m CPU
- **Volumes**: config, images (emptyDir)
- **Labels**: run=iron-gallery

### Iron DB
- **Database**: database_host
- **User**: ironuser
- **Volume**: db (emptyDir)
- **Labels**: db=mariadb

## Success Criteria
✅ All pods running  
✅ Services accessible  
✅ Installation page loads on NodePort 32678

## Notes
- Database and frontend connection not required initially
- Installation page confirms successful deployment

---
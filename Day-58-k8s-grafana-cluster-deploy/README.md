# Day 58: Deploy Grafana on Kubernetes Cluster 📊

## Project Overview
Today's challenge involved deploying Grafana, a powerful analytics and monitoring platform, on a Kubernetes cluster. The goal was to set up a production-ready Grafana instance accessible via NodePort service.


## 🎯 Objectives 
- Create a Kubernetes Deployment for Grafana
- Create a Kubernetes Deployment for Grafana
- Expose Grafana using a NodePort service on port 32000
- Ensure accessibility to the Grafana login page

## 🛠️ Technologies Used
- **Kubernetes** - Container orchestration platform
- **Grafana** - Analytics and monitoring solution
- **kubectl** - Kubernetes command-line tool
- **YAML** - Configuration files

## 📋 Requirements
1. Create a deployment named `grafana-deployment-datacenter`
2. Use official Grafana Docker image
3. Create NodePort service with nodePort `32000`
4. Verify access to Grafana login page

## 🚀 Solution

### Step 1: Create Kubernetes Manifests

Created a comprehensive YAML file containing both Deployment and Service configurations:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-datacenter
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:latest
        ports:
        - containerPort: 3000
          protocol: TCP
        env:
        - name: GF_SERVER_HTTP_PORT
          value: "3000"
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"

---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service-datacenter
  labels:
    app: grafana
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 32000
    protocol: TCP
    name: grafana-http
```

### Step 2: Deploy to Kubernetes

```bash
# Apply the configuration
kubectl apply -f grafana-deployment.yaml

# Verify deployment
kubectl get deployments
kubectl get pods
kubectl get services
```

### Step 3: Verify Deployment Status

```bash
# Check deployment details
kubectl describe deployment grafana-deployment-datacenter

# Check pod logs
kubectl logs -l app=grafana

# Check service endpoints
kubectl get svc grafana-service-datacenter
```

### Step 4: Access Grafana

Access Grafana using the NodePort:
```
http://<NODE_IP>:32000
```

**Default Login Credentials:**
- Username: `admin`
- Password: `admin`

## 📸 Screenshots

### Deployment Creation
```bash
$ kubectl apply -f grafana-deployment.yaml
deployment.apps/grafana-deployment-datacenter created
service/grafana-service-datacenter created
```

### Running Resources
```bash
$ kubectl get all
NAME                                               READY   STATUS    RESTARTS   AGE
pod/grafana-deployment-datacenter-xxxxxxxxx-xxxxx   1/1     Running   0          2m

NAME                                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/grafana-service-datacenter   NodePort    10.96.xxx.xxx   <none>        3000:32000/TCP   2m

NAME                                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/grafana-deployment-datacenter   1/1     1            1           2m
```

## 🔑 Key Learnings

1. **Kubernetes Deployments**: Understanding how to create and manage application deployments
2. **Service Types**: Working with NodePort services for external access
3. **Resource Management**: Implementing resource requests and limits for containers
4. **Label Selectors**: Using labels to connect Services with Pods
5. **Port Mapping**: Configuring containerPort, targetPort, port, and nodePort

## 💡 Best Practices Implemented

- ✅ Used specific deployment names as per requirements
- ✅ Implemented resource limits to prevent resource exhaustion
- ✅ Added proper labels for service discovery
- ✅ Used environment variables for configuration
- ✅ Followed Kubernetes naming conventions
- ✅ Created declarative YAML manifests for reproducibility
# Day 56: Deploy Nginx Web Server on Kubernetes Cluster

## 📋 Challenge Overview
Today's task involved deploying a highly available and scalable Nginx web server on a Kubernetes cluster for the Nautilus development team's static website.

## 🎯 Requirements Met

✅ **Deployment**: Created nginx-deployment with nginx:latest image  
✅ **Replicas**: Configured 3 replicas for high availability  
✅ **Container**: Named container as nginx-container  
✅ **Service**: Created NodePort service nginx-service on port 30011  
✅ **Scalability**: Ready for horizontal scaling  

## Step-by-Step Implementation

### Step 1: Verify Cluster Access
First, ensure you can connect to the Kubernetes cluster:

```bash
kubectl cluster-info
kubectl get nodes
```

### Step 2: Create the Nginx Deployment

Create a deployment manifest file:

```bash
cat > nginx-deployment.yaml << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
EOF
```

### Step 3: Apply the Deployment

Deploy the Nginx deployment:

```bash
kubectl apply -f nginx-deployment.yaml
```

### Step 4: Verify Deployment Status

Check if the deployment is successful:

```bash
# Check deployment status
kubectl get deployments

# Check pods status
kubectl get pods -l app=nginx

# Get detailed deployment information
kubectl describe deployment nginx-deployment
```

### Step 5: Create the NodePort Service

Create a service manifest file:

```bash
cat > nginx-service.yaml << EOF
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
    protocol: TCP
EOF
```

### Step 6: Apply the Service

Create the NodePort service:

```bash
kubectl apply -f nginx-service.yaml
```

### Step 7: Verify Service Creation

Check if the service is created successfully:

```bash
# Check service status
kubectl get services

# Get detailed service information
kubectl describe service nginx-service
```

## 📊 Results Achieved 

- **High Availability**: 3 running replicas across cluster nodes
- **External Access**: Service accessible via NodePort 30011
- **Load Balancing**: Traffic distributed automatically across pods
- **Scalability**: Ready for horizontal pod autoscaling

## 🌟 Key Learnings

- **Kubernetes Deployments** provide declarative application management
- **NodePort Services** enable external cluster access
- **Multi-replica deployments** ensure high availability and fault tolerance
- **Label selectors** create loose coupling between deployments and services
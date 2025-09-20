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
A **Pod** is the smallest deployable unit in Kubernetes. Think of it as a "wrapper" around your containers.

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

## 💻 Step-by-Step Implementation

### Step 1: Verify Cluster Connectivity
```bash
thor@jumphost ~$ kubectl cluster-info
Kubernetes control plane is running at https://kodekloud-control-plane:6443
CoreDNS is running at https://kodekloud-control-plane:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
✅ **Status**: Cluster is accessible and control plane is running

### Step 2: Check Node Status
```bash
thor@jumphost ~$ kubectl get nodes
NAME                     STATUS   ROLES           AGE     VERSION
kodekloud-control-plane  Ready    control-plane   8m17s   v1.27.16-1+f5da3b717fc217
```
✅ **Status**: Node is Ready and running Kubernetes v1.27.16

### Step 3: Create Pod YAML Manifest
```bash
thor@jumphost ~$ vi pod-nginx.yaml
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
thor@jumphost ~$ kubectl apply -f pod-nginx.yaml
pod/pod-nginx created
```
✅ **Status**: Pod creation initiated successfully

### Step 5: Verify Pod Status
```bash
thor@jumphost ~$ kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
pod-nginx   1/1     Running   0          14s
```
✅ **Status**: Pod is running and ready (1/1)

### Step 6: Detailed Pod Information
```bash
thor@jumphost ~$ kubectl describe pod pod-nginx
Name:             pod-nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Sat, 20 Sep 2025 08:03:34 +0000
Labels:           app=nginx_app
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  nginx-container:
    Container ID:   containerd://a943af5fe7e9be15d79fee324a66f053b30d90de76e6b837ca76f6bf8198b1c0
    Image:          nginx:latest
    Image ID:       docker.io/library/nginx@sha256:d5f28ef21aabddd098f3dbc21fe5b7a7d7a184720bc07da0b6c9b9820e97f25e
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sat, 20 Sep 2025 08:03:41 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-6x7cl (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  41s   default-scheduler  Successfully assigned default/pod-nginx to kodekloud-control-plane
  Normal  Pulling    41s   kubelet            Pulling image "nginx:latest"
  Normal  Pulled     35s   kubelet            Successfully pulled image "nginx:latest" in 6.042322606s
  Normal  Created    35s   kubelet            Created container nginx-container
  Normal  Started    34s   kubelet            Started container nginx-container
```
✅ **Status**: All conditions are True, container is running successfully

### Step 7: Verify Labels
```bash
thor@jumphost ~$ kubectl get pod pod-nginx --show-labels
NAME        READY   STATUS    RESTARTS   AGE     LABELS
pod-nginx   1/1     Running   0          3m22s   app=nginx_app
```
✅ **Status**: Label `app=nginx_app` applied correctly

### Step 8: Check Pod with Network Details
```bash
thor@jumphost ~$ kubectl get pod pod-nginx -o wide
NAME        READY   STATUS    RESTARTS   AGE     IP           NODE                     NOMINATED NODE   READINESS GATES
pod-nginx   1/1     Running   0          4m15s   10.244.0.5   kodekloud-control-plane   <none>           <none>
```
✅ **Status**: Pod assigned IP `10.244.0.5` and scheduled on control-plane node

## ✅ Lab Completion Verification

| **Requirement** | **Expected** | **Actual** | **Status** |
|-----------------|-------------|------------|------------|
| Pod Name | `pod-nginx` | `pod-nginx` | ✅ |
| Image | `nginx:latest` | `nginx:latest` | ✅ |
| Container Name | `nginx-container` | `nginx-container` | ✅ |
| Label | `app=nginx_app` | `app=nginx_app` | ✅ |
| Pod Status | Running | Running (1/1) | ✅ |
| Container Ready | True | True | ✅ |



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

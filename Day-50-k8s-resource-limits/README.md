# Day 50: Set Resource Limits in Kubernetes Pods 🚀

## 📋 Lab Overview
Learn how to set resource limits and requests in Kubernetes pods to prevent resource starvation and ensure optimal cluster performance.

## 🎯 What are Kubernetes Resources?

Kubernetes resources are specifications that define how much computational power (CPU) and memory a container needs and can consume. They are crucial for proper cluster management, ensuring fair resource distribution among applications, and preventing any single container from monopolizing system resources.

### 🌟 Why Resource Management Matters
- **Multi-tenancy**: Multiple applications share the same cluster nodes
- **Performance isolation**: Prevents noisy neighbors from affecting other workloads  
- **Predictable behavior**: Applications get consistent performance
- **Cost optimization**: Efficient resource utilization reduces infrastructure costs
- **Autoscaling foundation**: Enables horizontal and vertical pod autoscaling

### 🔹 Resource Requests
- **Definition**: Minimum guaranteed resources reserved for a container
- **Scheduler role**: Kubernetes scheduler uses requests to find nodes with sufficient available resources
- **Guarantee**: The container will always have access to at least this amount of resources
- **Node capacity**: Sum of all requests cannot exceed node capacity
- **Billing impact**: In cloud environments, you're often charged based on requested resources

**Example**: If you request 100m CPU, Kubernetes guarantees your container will get at least 0.1 CPU core, even under heavy load.

### 🔹 Resource Limits  
- **Definition**: Maximum ceiling of resources a container can consume
- **Protection mechanism**: Prevents runaway processes from consuming all node resources
- **Enforcement behavior**:
  - **CPU**: Container is throttled (slowed down) when hitting the limit
  - **Memory**: Container is killed (OOMKilled) when exceeding memory limit
- **Quality of Service**: Affects pod QoS class (Guaranteed, Burstable, BestEffort)

**Example**: With a 200m CPU limit, your container can burst up to 0.2 CPU cores but will be throttled if it tries to use more.

### 📊 Resource Types & Units

#### CPU Resources
- **Measurement**: Cores or millicores (m)
- **Examples**:
  - `1` or `1000m` = 1 full CPU core
  - `500m` = 0.5 CPU core (half a core)
  - `100m` = 0.1 CPU core (10% of a core)
  - `50m` = 0.05 CPU core (5% of a core)
- **Fractional support**: Can specify fractional values like `0.1` (equivalent to `100m`)

#### Memory Resources  
- **Binary units** (base 1024):
  - `Ki` = Kibibytes (1024 bytes)
  - `Mi` = Mebibytes (1024² bytes ≈ 1.05 MB)
  - `Gi` = Gibibytes (1024³ bytes ≈ 1.07 GB)
  - `Ti` = Tebibytes (1024⁴ bytes)
- **Decimal units** (base 1000):
  - `K` = Kilobytes (1000 bytes)  
  - `M` = Megabytes (1000² bytes)
  - `G` = Gigabytes (1000³ bytes)
- **Examples**:
  - `128Mi` = 134.2 MB
  - `1Gi` = 1073.7 MB
  - `512Mi` = 537.0 MB



## 🛠️ Lab Task
Create a pod named `httpd-pod` with specific resource constraints:
- **Container**: `httpd-container`
- **Image**: `httpd:latest`
- **Memory Request**: `15Mi`
- **Memory Limit**: `20Mi`
- **CPU Request**: `100m`
- **CPU Limit**: `100m`

## 📝 Solution

### Step 1: Create Pod Manifest
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
  labels:
    app: httpd
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
    ports:
    - containerPort: 80
      protocol: TCP
```

### Step 2: Verify Deployment
```bash
# Check pod status
kubectl get pods

# Verify resource configuration
kubectl describe pod httpd-pod | grep -A 8 "Limits\|Requests"

# View detailed pod info
kubectl get pod httpd-pod -o yaml
```

## 📈 Expected Output
```bash
$ kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          30s

$ kubectl describe pod httpd-pod | grep -A 8 "Limits\|Requests"
    Limits:
      cpu:     100m
      memory:  20Mi
    Requests:
      cpu:        100m
      memory:     15Mi
```

## ✅ Best Practices

- Always set both requests and limits
- Start with conservative values and adjust based on monitoring
- Set requests based on minimum needs
- Set limits based on maximum acceptable usage
- Avoid setting limits too low (causes throttling/OOM kills)
- Avoid setting requests too high (wastes resources)

## 🔧 Useful Commands

```bash
# Apply the configuration
kubectl apply -f httpd-pod.yaml

# Monitor resource usage (requires metrics-server)
kubectl top pod httpd-pod

# Check pod logs
kubectl logs httpd-pod

# Port forward to test (optional)
kubectl port-forward httpd-pod 8080:80

# Cleanup
kubectl delete pod httpd-pod
```

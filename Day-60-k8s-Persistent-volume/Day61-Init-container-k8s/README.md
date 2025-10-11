# Day 61: Init Containers in Kubernetes

## What are Init Containers?

Init Containers are special containers that run **before** your main application container starts. They must complete successfully before the main container runs.

**Simple Flow:**
```
Pod Created → Init Container Runs → Init Container Completes → Main Container Starts
```

## Why Use Init Containers?

- Wait for services to be ready (database, cache)
- Download configuration files
- Pre-populate data
- Run database migrations
- Perform setup tasks

## Init vs Regular Containers

| Feature | Init Containers | Regular Containers |
|---------|----------------|-------------------|
| When | Before app starts | After init completes |
| Duration | Runs once, exits | Runs continuously |
| Order | Sequential | Parallel |
| Purpose | Setup/preparation | Main application |

---

## Lab: xFusionCorp Industries

### Requirements

1. Create Deployment named `ic-deploy-xfusion`
2. Configure 1 replica with label `app: ic-xfusion`
3. Init container `ic-msg-xfusion` writes message to `/ic/blog`
4. Main container `ic-main-xfusion` reads and displays the message
5. Share data using `emptyDir` volume

---

## Step-by-Step Implementation

### Step 1: Create Deployment File

```bash
cat > ic-deploy-xfusion.yaml << 'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-xfusion
  template:
    metadata:
      labels:
        app: ic-xfusion
    spec:
      initContainers:
      - name: ic-msg-xfusion
        image: fedora:latest
        command: ['/bin/bash', '-c', 'echo Init Done - Welcome to xFusionCorp Industries > /ic/blog']
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic
      
      containers:
      - name: ic-main-xfusion
        image: fedora:latest
        command: ['/bin/bash', '-c', 'while true; do cat /ic/blog; sleep 5; done']
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic
      
      volumes:
      - name: ic-volume-xfusion
        emptyDir: {}
EOF
```

### Step 2: Apply Configuration

```bash
kubectl apply -f ic-deploy-xfusion.yaml
```

### Step 3: Verify Deployment

```bash
kubectl get deployment ic-deploy-xfusion
```

### Step 4: Check Pod Status

```bash
kubectl get pods -l app=ic-xfusion
```

### Step 5: Get Pod Name

```bash
POD_NAME=$(kubectl get pods -l app=ic-xfusion -o jsonpath='{.items[0].metadata.name}')
echo $POD_NAME
```

### Step 6: View Main Container Logs

```bash
kubectl logs $POD_NAME -c ic-main-xfusion
```

**Expected Output:**
```
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
```

### Step 7: View Init Container Logs

```bash
kubectl logs $POD_NAME -c ic-msg-xfusion
```

### Step 8: Describe Pod

```bash
kubectl describe pod $POD_NAME
```

### Step 9: Verify File Contents

```bash
kubectl exec $POD_NAME -- cat /ic/blog
```

### Step 10: List Volume Files

```bash
kubectl exec $POD_NAME -- ls -la /ic/
```

---

## Verification Commands

```bash
# Check all resources
kubectl get deployment,pod -l app=ic-xfusion

# View logs (follow mode)
kubectl logs $POD_NAME -c ic-main-xfusion -f

# Check pod details
kubectl describe pod $POD_NAME | grep -A 10 "Init Containers:"
```

---

## Common Use Cases

### Wait for Database
```yaml
initContainers:
- name: wait-for-db
  image: busybox
  command: ['sh', '-c', 'until nc -z mysql 3306; do sleep 2; done']
```


## Key Points

- Init containers run **before** main containers
- They run **sequentially** (one after another)
- Must complete **successfully** before next init or main container starts
- Perfect for **setup and preparation** tasks
- Share **volumes** with main containers

---


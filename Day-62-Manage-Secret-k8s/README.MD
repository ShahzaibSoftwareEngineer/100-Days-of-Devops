# Day 62: Manage Secrets in Kubernetes

## What are Kubernetes Secrets?

Secrets store sensitive data like passwords, tokens, and license keys in Kubernetes. Data is base64 encoded and kept separate from application code.

**Why Use Secrets?**
- Store sensitive information securely
- Separate secrets from application configuration
- Easy to update without rebuilding images
- Mount as files or use as environment variables

---

## Lab: Nautilus eCommerce License Management

### Objective

Deploy a tool with license information stored securely using Kubernetes secrets.

### Requirements

1. Secret file: `/opt/ecommerce.txt` (contains license key)
2. Create generic secret named `ecommerce`
3. Create pod named `secret-devops`
4. Container: `secret-container-devops`, image: `ubuntu:latest`
5. Mount secret at `/opt/apps` inside container

---

## Step-by-Step Commands

### Step 1: Check Secret File Content

```bash
cat /opt/ecommerce.txt
```

**Output:**
```
5ecur3
```

---

### Step 2: Create Secret from File

```bash
kubectl create secret generic ecommerce --from-file=/opt/ecommerce.txt
```

**Output:**
```
secret/ecommerce created
```

---

### Step 3: Verify Secret Created

```bash
kubectl get secrets
```

**Output:**
```
NAME        TYPE     DATA   AGE
ecommerce   Opaque   1      13s
```

---

### Step 4: View Secret Details (Optional)

```bash
kubectl get secret ecommerce -o yaml
```

**Output:**
```yaml
apiVersion: v1
data:
  ecommerce.txt: NWVjdXIzCg==
kind: Secret
metadata:
  name: ecommerce
  namespace: default
type: Opaque
```

**Note:** `NWVjdXIzCg==` is base64 encoded version of `5ecur3`

---

### Step 5: Check Current Pods

```bash
kubectl get pods
```

**Output:**
```
No resources found in default namespace.
```

---

### Step 6: Create Pod Configuration File

```bash
cat > secret-devops-pod.yaml << 'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: secret-devops
spec:
  containers:
  - name: secret-container-devops
    image: ubuntu:latest
    command: ["/bin/bash", "-c", "sleep 10000"]
    volumeMounts:
    - name: secret-volume
      mountPath: /opt/apps
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: ecommerce
EOF
```

---

### Step 7: Apply Pod Configuration

```bash
kubectl apply -f secret-devops-pod.yaml
```

**Output:**
```
pod/secret-devops created
```

---

### Step 8: Verify Pod is Running

```bash
kubectl get pods
```

**Output:**
```
NAME            READY   STATUS    RESTARTS   AGE
secret-devops   1/1     Running   0          5s
```

---

### Step 9: List Files in Mounted Directory

```bash
kubectl exec secret-devops -- ls -la /opt/apps

**Understanding the output:**
- Kubernetes creates symbolic links for secret files
- `ecommerce.txt` → symlink to actual secret data
- Allows atomic updates when secret changes

---

### Step 10: Read Secret Content from Container

```bash
kubectl exec secret-devops -- cat /opt/apps/ecommerce.txt
```

**Output:**
```
5ecur3
```



## Key Takeaways

1. **Secrets vs ConfigMaps**: Use secrets for sensitive data, ConfigMaps for non-sensitive configuration
2. **Base64 Encoding**: Secrets are base64 encoded, not encrypted by default
3. **Volume Mounts**: Secrets mounted as volumes appear as files
4. **Atomic Updates**: Kubernetes uses symlinks for seamless secret updates
5. **ReadOnly**: Always mount secrets as readOnly for security
6. **Restart Required**: Pods using secret env vars need restart after secret updates

---



# Day 57: Print Environment Variables


### What are Environment Variables?
Environment variables are dynamic values that can affect the behavior of running processes in a container. In Kubernetes, they allow you to:
- Pass configuration data to containers
- Separate configuration from code
- Make containers more portable and reusable
- Avoid hardcoding values in your application

### Why Use Environment Variables in Kubernetes?
- **Flexibility**: Change configuration without rebuilding images
- **Security**: Store sensitive data separately (using Secrets)
- **Portability**: Same container image works in different environments
- **12-Factor App**: Follows cloud-native best practices

### Pod Restart Policies
Kubernetes offers three restart policies:
- **Always** (default): Restart container regardless of exit status
- **OnFailure**: Restart only if container exits with error
- **Never**: Don't restart container (useful for one-time jobs)

---

## 🎯 Lab Objective

Create a Kubernetes pod that:
1. Uses environment variables to store greeting components
2. Prints a custom message using those variables
3. Runs once and completes (no restart)

---

## 🛠️ Lab Implementation

### Step 1: Create Pod YAML File

```bash
vi print-envars-greeting.yaml
```

### Step 2: Add Configuration

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
  - name: print-env-container
    image: bash
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "xFusionCorp"
    - name: GROUP
      value: "Group"
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
  restartPolicy: Never
```

### Step 3: Deploy the Pod

```bash
kubectl apply -f print-envars-greeting.yaml
```

### Step 4: Verify Output

```bash
kubectl logs -f print-envars-greeting
```

**Expected Output:**
```
Welcome to xFusionCorp Group
```

---


## 🔍 Understanding the Command

```bash
["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
```

**Breakdown:**
- `/bin/sh`: Shell interpreter
- `-c`: Execute the following command string
- `echo`: Print to stdout
- `$(VARIABLE)`: Shell syntax to expand environment variables

---

## ✅ Verification Commands

```bash
# Check pod status
kubectl get pods

# View pod details
kubectl describe pod print-envars-greeting

# Check logs
kubectl logs print-envars-greeting

# View environment variables
kubectl exec print-envars-greeting -- env
```

---

## 💡 Key Takeaways

1. **Environment variables** make containers configurable without code changes
2. **restartPolicy: Never** is ideal for one-time tasks or jobs
3. **kubectl logs** shows container output for debugging
4. Environment variables are accessed using `$(VARIABLE_NAME)` in shell commands
5. Pods with `Never` restart policy show **Completed** status after successful execution


## 📌 Best Practices

✅ Use descriptive environment variable names  
✅ Set restart policy based on workload type  
✅ Use ConfigMaps for non-sensitive configuration  
✅ Use Secrets for sensitive data  
✅ Always verify logs after deployment  
✅ Document your environment variables  


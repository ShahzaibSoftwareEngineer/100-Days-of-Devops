# Day 52 of #100DaysOfDevOps – Revert Deployment to Previous Version in Kubernetes

## 📖 Scenario
Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer reported a bug related to this recent release. Consequently, the team aims to **revert the deployment back to the previous working version**.

There exists a deployment named **`nginx-deployment`**. Your task is to initiate a rollback to the previous revision.

**Note:** The kubectl utility on jump_host is configured to interact with the Kubernetes cluster.

---

## 🎯 Learning Objectives
- Understand Kubernetes deployment rollback mechanisms
- Learn to check rollout history and revisions
- Practice safe deployment rollback procedures
- Verify rollback success and application health

---

## 🔹 Step-by-Step Implementation

### 1. Check Current Deployment Status
First, verify the current state of the deployment:

```bash
kubectl get deployment nginx-deployment
```

**Output:**
```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           16m
```

### 2. View Rollout History
Check the deployment's revision history to see available versions:

```bash
kubectl rollout history deployment/nginx-deployment
```

**Output:**
```
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment nginx-deployment nginx-container=nginx:stable --record=true
```

### 3. Get Detailed Revision Information (Optional)
For better understanding, check specific revision details:

```bash
# Check current revision (revision 2)
kubectl rollout history deployment/nginx-deployment --revision=2

# Check previous revision (revision 1)
kubectl rollout history deployment/nginx-deployment --revision=1
```

### 4. Initiate Rollback to Previous Revision
Revert to the previous working version (Revision 1):

```bash
kubectl rollout undo deployment/nginx-deployment
```

**Alternative:** To rollback to a specific revision:
```bash
kubectl rollout undo deployment/nginx-deployment --to-revision=1
```

### 5. Monitor Rollback Progress
Check the rollback status:

```bash
kubectl rollout status deployment/nginx-deployment
```

**Output:**
```
deployment "nginx-deployment" successfully rolled out
```

### 6. Verify Rollback Success
Confirm the deployment is healthy after rollback:

```bash
kubectl get deployment nginx-deployment
```

**Output:**
```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           18m
```

### 7. Check Pod Status
Verify that all pods are running with the reverted version:

```bash
kubectl get pods
```

**Example Output:**
```
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-989f57c54-gw4br   1/1     Running   0          66s
nginx-deployment-989f57c54-lct8h   1/1     Running   0          64s
nginx-deployment-989f57c54-v56cp   1/1     Running   0          65s
```

### 8. Verify Pod Labels (If Needed)
If pods don't appear, check the correct label selector:

```bash
kubectl get deploy nginx-deployment -o yaml | grep -A5 selector:
```

**Output:**
```
selector:
  matchLabels:
    app: nginx-app
```

Then query pods with the correct label:
```bash
kubectl get pods -l app=nginx-app -o wide
```

### 9. Confirm Image Version
Verify the deployment is using the correct (reverted) image:

```bash
kubectl get deploy nginx-deployment -o jsonpath='{.spec.template.spec.containers[0].image}'; echo
```

### 10. Verify Updated Rollout History
Check that a new revision was created for the rollback:

```bash
kubectl rollout history deployment/nginx-deployment
```


## ✅ What I Accomplished

- ✅ Successfully rolled back **nginx-deployment** to its previous revision
- ✅ Verified all pods are **Running** and healthy after rollback
- ✅ Confirmed the deployment is using the correct (previous) container image
- ✅ Validated that the rollback created a new revision in the history
- ✅ Ensured zero downtime during the rollback process

---

## 🛠️ Key Kubernetes Concepts Learned

| Concept | Description |
|---------|-------------|
| **Rollout History** | Kubernetes maintains revision history for deployments |
| **Rollback Strategy** | Uses rolling update by default to ensure zero downtime |
| **Revision Numbers** | Each deployment change creates a new revision |
| **Undo Operations** | Can rollback to previous or specific revisions |

---

## 📝 Best Practices & Tips

### 🔍 **Before Rollback:**
- Always check rollout history first
- Verify which revision you want to rollback to
- Ensure you have proper backup and monitoring

### 🚀 **During Rollback:**
- Monitor the rollback status continuously
- Check pod health and readiness
- Verify application functionality

### ✨ **After Rollback:**
- Test application to ensure bug is resolved
- Document the incident and rollback
- Plan fix for the problematic version



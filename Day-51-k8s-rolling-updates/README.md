# Day 51: Execute Rolling Updates in Kubernetes

## 📘 What is a Rolling Update?

A Rolling Update is the default deployment strategy in Kubernetes. It ensures your application is updated gradually without downtime by replacing old Pods with new ones step by step.

## 🔑 Key Concepts

Zero Downtime: Your application keeps running while the update happens.

Gradual Replacement: New Pods are created one at a time (or in batches) while old Pods are terminated only after the new ones are ready.

Controlled Speed: Parameters like maxUnavailable and maxSurge let you control how many Pods are replaced or added at once.

Safe Rollouts: If something goes wrong, you can roll back to the previous stable version.

## ⚙️ Why Use Rolling Updates?

Ensures high availability during deployments.

Prevents service outages caused by taking down all Pods at once.

Allows for smooth version upgrades (e.g., updating app v1.0 → v1.1).

Provides an easy way to test new releases safely before fully rolling them out.

## Simple Explain

**Problem**: Your app runs on 3 pods with nginx:1.16, but you need nginx:1.17
**Old Way**: Stop all 3 pods → Start 3 new pods (Service down!)
**Rolling Update**: Replace 1 pod at a time (Service stays up!)

## Lab Scenario

- **Current**: nginx-deployment with nginx:1.16
- **Goal**: Update to nginx:1.17
- **Requirement**: Zero downtime

## Step 1: Check Current State

```bash
kubectl get deployment nginx-deployment
```
Output:
```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           6m5s
```

```bash
kubectl describe deployment nginx-deployment
```
Key info:
- Current image: `nginx:1.16`
- Container name: `nginx-container`
- Pods labeled: `app=nginx-app`

## Step 2: Execute Rolling Update

```bash
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17
```

## Step 3: Monitor the Update

```bash
kubectl rollout status deployment/nginx-deployment
```

Watch pods change:
```bash
kubectl get pods -l app=nginx-app --watch
```

```

## Step 4: Verify Success

```bash
# Check all pods are updated
kubectl get pods -l app=nginx-app

# Confirm new image version
kubectl get deployment nginx-deployment -o jsonpath='{.spec.template.spec.containers[0].image}'
```

## Key Rolling Update Settings

From our lab deployment:
- **MaxUnavailable**: 25% (can lose 1 pod during update)
- **MaxSurge**: 25% (can have 1 extra pod during update)
- **Strategy**: RollingUpdate (gradual replacement)

## Common Lab Mistakes

1. **Wrong label selector**:
   ```bash
   # ❌ Wrong
   kubectl get pods -l app=nginx
   
   # ✅ Correct (check your deployment labels)
   kubectl get pods -l app=nginx-app
   ```

2. **Wrong container name**:
   ```bash
   # ❌ Wrong
   kubectl set image deployment/nginx-deployment nginx=nginx:1.17
   
   # ✅ Correct (check describe output)
   kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17
   ```

## Essential Commands for Lab

```bash
# 1. Check current deployment
kubectl get deployment nginx-deployment

# 2. Execute rolling update
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17

# 3. Monitor progress
kubectl rollout status deployment/nginx-deployment

# 4. Verify pods updated
kubectl get pods -l app=nginx-app

# 5. Emergency rollback (if needed)
kubectl rollout undo deployment/nginx-deployment
```

## Why Rolling Updates Matter

- **Zero Downtime**: Users can access your app during updates
- **Safe Updates**: If new version fails, old pods still serve traffic
- **Gradual Process**: Problems affect only some pods, not all
- **Easy Rollback**: Can quickly return to previous version

## Lab Success Criteria

✅ All 3 pods running nginx:1.17  
✅ No service interruption during update  
✅ Deployment shows "3/3" ready status  
✅ Update completed without errors

Rolling updates make deployments safe and reliable your users stay happy while you upgrade your applications!
# Day 54 of #100DaysOfDevOps – Kubernetes Shared Volumes 🚀

## 🎯 What is Kubernetes Shared Volumes?

A **shared volume** in Kubernetes is a storage space that can be **used by multiple containers in the same pod**.  
It allows containers to **read and write the same data** temporarily.  
For example, one container can create a file, and the other container can access it instantly!

### 💡 Why Use Shared Volumes?
- **Sharing temporary data** between containers  
- **Caching files** for faster access  
- **Inter-container communication** without network calls  
- **Log aggregation** from multiple services  

---

## 📋 Lab Requirements

✅ Create pod named `volume-share-nautilus`  
✅ Two Ubuntu containers with shared volume  
✅ Test file sharing between containers  
✅ Verify data persistence across containers  

---

## 🔧 Step 1: Create Pod YAML

We will create a pod with **two containers** and a **shared volume** called `volume-share`.

- **Container 1** mounts it at `/tmp/blog`  
- **Container 2** mounts it at `/tmp/cluster`  

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-nautilus
  labels:
    lab: "day54-shared-volumes"
spec:
  containers:
    - name: volume-container-nautilus-1
      image: ubuntu:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/blog
    - name: volume-container-nautilus-2
      image: ubuntu:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/cluster
  volumes:
    - name: volume-share
      emptyDir: {}
```

**💭 Explanation:**
* `emptyDir: {}` creates a **temporary directory** shared by containers in the pod
* Containers run `sleep 3600` to **stay active** while we test the shared volume
* Both containers can **read/write** to the same storage space

---

## 🚀 Step 2: Deploy the Pod

```bash
# Apply the configuration
kubectl apply -f volume-share-nautilus.yaml

# Check pod status
kubectl get pods
```

**Expected Output:**
```
NAME                   READY   STATUS    RESTARTS   AGE
volume-share-nautilus   2/2     Running   0          30s
```

✅ Ensure the pod is **Running** and both containers are **ready (2/2)**

---

## 📝 Step 3: Create File in First Container

```bash
# Access first container
kubectl exec -it volume-share-nautilus -c volume-container-nautilus-1 -- bash

# Create test file
echo "This is a shared file created on $(date)" > /tmp/blog/blog.txt

# Verify file creation
ls /tmp/blog/
cat /tmp/blog/blog.txt

# Exit container
exit
```

🎉 This file `blog.txt` is now saved in the **shared volume**!

---

## 🔍 Step 4: Verify File in Second Container

```bash
# Access second container
kubectl exec -it volume-share-nautilus -c volume-container-nautilus-2 -- bash

# Check if file exists
ls /tmp/cluster/
cat /tmp/cluster/blog.txt

# Exit container
exit
```

**🎯 Result:** The same file appears here because **both containers share the volume**!

---

---

## 🧹 Step 5: Cleanup (Optional)

```bash
kubectl delete pod volume-share-nautilus
```

---

## 🔑 Key Learning Points

| Concept | Description |
|---------|-------------|
| **emptyDir** | Temporary storage that exists for pod lifetime |
| **volumeMounts** | How containers access shared storage |
| **Inter-container sharing** | Multiple containers can read/write same data |
| **Pod lifecycle** | Volume exists as long as pod exists |

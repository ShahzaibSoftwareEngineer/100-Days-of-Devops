
# Day 38 of 100 Days of DevOps — Pull Docker Image & Re-Tag

Today in the Nautilus project, I worked on **Docker images**.  
The developers wanted to test a containerized environment, and my task as part of the DevOps team was:

**Task:**  
- Pull the `busybox:musl` image on **App Server 1 (stapp01)**.  
- Re-tag (create a new tag) this image as `busybox:media`.  

---

## 🐳 What is a Docker Image?

A **Docker Image** is a lightweight, standalone, and immutable package that contains everything required to run a piece of software, including:
- The **application code**
- **Runtime environment**
- **Libraries and dependencies**
- **Configuration files**

Think of an image as a **blueprint** for a container.  
When you start a container, Docker uses the image as the foundation and creates a running instance from it.

### Key Points about Docker Images:
- **Read-only:** Images cannot be changed once built.  
- **Layered:** Built in layers for efficiency 
- **Tagged:** Images can have tags like `busybox:musl` or `busybox:media`. Tags help identify versions.  
- **Portable:** Can be shared via Docker Hub or private registries, making them easy to distribute across environments.  

In this lab, we are using the `busybox` image, which is one of the smallest and most useful images for testing purposes.  
The `musl` tag is a specific variant of BusyBox built with the `musl` C library.  

---

## 🔧 Steps Performed

### 1️⃣ SSH into App Server 1
```bash
ssh tony@172.16.238.10
# password: Ir0nM@n
````

### 2️⃣ Check Docker Installation

```bash
docker --version
sudo systemctl status docker
```

### 3️⃣ Pull BusyBox (musl) Image

```bash
sudo docker pull busybox:musl
```

### 4️⃣ Verify the Pulled Image

```bash
docker images
```

**Output:**

```
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
busybox      musl      44f1048931f5   11 months ago   1.46MB
```

### 5️⃣ Re-Tag the Image

```bash
sudo docker tag busybox:musl busybox:media
```

### 6️⃣ Confirm Both Tags Exist

```bash
docker images | grep busybox
```

**Output:**

```
busybox      media     44f1048931f5   11 months ago   1.46MB
busybox      musl      44f1048931f5   11 months ago   1.46MB
```

### 7️⃣ Test the New Tag

```bash
sudo docker run --rm busybox:media echo "Task complete - busybox:media works!"
```

**Output:**

```
Task complete - busybox:media works!
```

---

## ✅ Result

* Learned what Docker Images are and why they are important.
* Successfully pulled `busybox:musl`.
* Re-tagged it as `busybox:media`.
* Verified both tags and tested the image.
* **Task completed successfully.**




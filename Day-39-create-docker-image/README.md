
# 📌 Day 39 of 100 Days of DevOps — Create a Docker Image From Container

### 📝 Definition

In Docker, containers are **running instances of images**. Developers often make changes directly inside containers (like installing packages, editing configurations, or testing code).

However, once the container is stopped or deleted, those changes are **lost** unless we save them. To preserve this modified state, we can create a **new image from a container** using the `docker commit` command.

This allows us to:

* Capture the current state of a container.
* Save it as a reusable image.
* Run new containers based on this updated image in the future.

---

### 🔑 Concept

* **Image** = Blueprint (read-only, base layer).
* **Container** = Running instance (writable, can be modified).
* **Problem** = Container changes disappear after stop/delete.
* **Solution** = Use `docker commit` to save changes → new image.

---

### 🛠️ Lab Task

On **Application Server 3 (stapp03)**:

* Create an image `official:xfusion` from the running container `ubuntu_latest`.

---

### ⚙️ Step-by-Step Execution

**1. SSH into the target server**

```bash
ssh banner@172.16.238.12
# password: BigGr33n
```

**2. Check running containers**

```bash
sudo docker ps
```

Output examples:

```
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          NAMES
79229c1a8355   ubuntu    "/bin/bash"  5 minutes ago    Up 5 minutes    ubuntu_latest
```

**3. Create new image from the container**

```bash
sudo docker commit ubuntu_latest official:xfusion
```

This commits the container `ubuntu_latest` into a new image named `official:xfusion`.

**4. Verify the image exists**

```bash
sudo docker images
```

Expected output:

```
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
official     xfusion   23da3c9f8b8a   10 seconds ago   132MB
ubuntu       latest    802541663949   3 weeks ago      78.1MB
```

**5. (Optional) Run a test container from the new image**

```bash
sudo docker run -it official:xfusion bash
```

---

### 📌 Notes

* `docker commit` is useful for **backups, snapshots, and testing**.
* But in the  real DevOps workflows, we prefer **Dockerfiles** for reproducibility and automation.
* Still, this command is handy when you need to preserve ad-hoc changes made inside a container.

---

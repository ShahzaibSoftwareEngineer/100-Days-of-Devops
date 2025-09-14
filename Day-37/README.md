
# **Day 37 of 100 Days of DevOps — Copy File to Docker Container**

Today’s task focused on securely copying a confidential file from a Docker host to a running container while ensuring the file’s integrity remained intact. This exercise provided valuable hands-on practice with Docker’s file management capabilities.

---

### **Concept**

In Docker, the `docker cp` command is used to copy files and directories between the host machine and containers. It works similarly to the standard Linux `cp` command but allows operations across container boundaries. The syntax is:

```bash
docker cp <source_path> <container_name>:<destination_path>
docker cp <container_name>:<source_path> <destination_path>
```

Key points:

* You can copy files both **from host → container** and **from container → host**.
* File ownership and permissions may be adjusted to match the container’s environment.
* The contents of the file are not modified during transfer.

Since this exercise involved an **encrypted file**, ensuring data integrity was critical. To achieve this, I used **SHA-256 checksums** before and after the transfer. Matching checksums confirmed that the file remained unchanged during the process.

---

### **Task Overview**

* Host: **stapp03** (`banner@172.16.238.12`)
* File to transfer: `/tmp/nautilus.txt.gpg` (encrypted file)
* Container: `ubuntu_latest`
* Destination path: `/home/` inside the container
* Requirement: Verify that the file was copied without modification.

---

### **Execution Steps**

1. **SSH into the host server**

```bash
ssh banner@172.16.238.12
# Password: BigGr33n
```

2. **Verify the file exists and calculate its checksum on the host**

```bash
ls -l /tmp/nautilus.txt.gpg
sha256sum /tmp/nautilus.txt.gpg
```

**Output:**

```
1b2cde180f9e7ad23fed89d2331d6ab9afc0bbe111f7a14f687918d8405d05dd  /tmp/nautilus.txt.gpg
```

3. **Check if the container is running**

```bash
docker ps
```

**Output:**

```
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
442915b8ba70   ubuntu    "/bin/bash"   4 minutes ago   Up 4 minutes             ubuntu_latest
```

4. **Copy the file into the container**

```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/
```

**Output:**

```
Successfully copied 2.05kB to ubuntu_latest:/home/
```

5. **Verify the file inside the container**

```bash
docker exec ubuntu_latest ls -l /home/nautilus.txt.gpg
```

**Output:**

```
-rw-r--r-- 1 root root 105 Sep  9 08:08 /home/nautilus.txt.gpg
```

6. **Recalculate the checksum inside the container**

```bash
docker exec ubuntu_latest sha256sum /home/nautilus.txt.gpg
```

**Output:**

```
1b2cde180f9e7ad23fed89d2331d6ab9afc0bbe111f7a14f687918d8405d05dd  /home/nautilus.txt.gpg
```

✔️ The checksum matches the host’s checksum, confirming the file was transferred without any modification.

---

### **Result**

The encrypted file was successfully copied from the Docker host to the `ubuntu_latest` container under `/home/`, and checksum verification confirmed its integrity.




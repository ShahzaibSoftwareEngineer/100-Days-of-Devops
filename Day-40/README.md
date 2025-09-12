
# 🚀 Day 40 of #100DaysOfDevOps — Docker EXEC Operations

Today, I explored one of the most **important Docker commands**: `docker exec`.
It allows us to **run processes inside a running container** without stopping or restarting it. This is extremely useful when debugging, installing packages, or running ad-hoc commands.

---

## 📖 Theory: Understanding `docker exec`

* **Purpose**: The `docker exec` command lets us execute a new command in an already running container.
* **Difference from `docker run`**:

  * `docker run` → Creates and starts a new container from an image.
  * `docker exec` → Runs commands inside an existing container.
* **Interactive Mode (`-it`)**:
  Using `-it` allows us to interact with the container shell, just like logging into a VM.
* **Practical Uses**:

  * Debugging containers (`exec bash` or `exec sh`)
  * Installing missing tools/packages inside a container
  * Checking logs or service status inside a container
  * Running one-time commands without modifying Dockerfile

---

## ⚡ Practical Commands

1. **List Running Containers**

   ```bash
   docker ps
   ```

2. **Open an Interactive Shell in a Container**

   ```bash
   docker exec -it <container_id> /bin/bash
   ```

   or if bash is not available:

   ```bash
   docker exec -it <container_id> sh
   ```

3. **Run a One-Time Command Inside the Container**

   ```bash
   docker exec <container_id> ls -l /var/www
   ```

4. **Check Service Status (example: Apache inside container)**

   ```bash
   docker exec <container_id> service apache2 status
   ```

5. **Install a Package Inside the Container (Debian/Ubuntu-based)**

   ```bash
   docker exec -it <container_id> apt-get update
   docker exec -it <container_id> apt-get install -y curl
   ```

---

## 🔑 Key Notes

* Any changes made with `docker exec` are **temporary**—if the container is deleted, changes are lost (unless committed as a new image).
* Use `docker exec` mainly for debugging or temporary fixes. For permanent changes, update the **Dockerfile** or create a new **image**.

---


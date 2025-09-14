
#  Day 36 of 100 Days of DevOps — Deploying Nginx Container with Docker

Today’s lab focused on **containerizing an application** by deploying an **Nginx container** on **Application Server 3** using Docker. This exercise highlights the core DevOps practice of **container-based deployment**, which ensures portability, consistency, and easy scalability across environments.

---

##  Objective

* Deploy a container named `nginx_3` using the `nginx:alpine` image.
* Ensure the container is in a **running state** and can serve HTTP requests.
* Apply **restart policies** for reliability.

---

##  Theory

* **Docker Containers**: Lightweight, standalone, and executable packages that include everything needed to run an application.
* **Alpine Images**: Minimal Linux distribution images, useful for small footprint containers.
* **Restart Policies**: Ensure containers restart automatically on failure or system reboot (`--restart unless-stopped`).
* **Container Verification**: Always verify container status (`docker ps`) and functionality (`curl`) after deployment.

---

##  Commands Used

### 1. Pull the Nginx image

```bash
docker pull nginx:alpine
```

### 2. Remove any existing container (if exists)

```bash
docker rm -f nginx_3 2>/dev/null || true
```

### 3. Run the Nginx container

```bash
docker run -d --name nginx_3 --restart unless-stopped nginx:alpine
```

### 4. Verify container is running

```bash
docker ps --filter "name=nginx_3"
```

Expected output:

```
CONTAINER ID   IMAGE          COMMAND                  STATUS   PORTS   NAMES
f11944478a09   nginx:alpine   "/docker-entrypoint.…"   Up 5s    80/tcp  nginx_3
```

### 5. Test HTTP response from container

```bash
docker exec -it nginx_3 curl -I http://localhost/
```

Expected output:

```
HTTP/1.1 200 OK
Server: nginx/1.29.1
```

---

##  Outcome

* The container **nginx\_3** is running successfully.
* Nginx serves HTTP requests properly inside the container.
* The `--restart unless-stopped` policy ensures it remains resilient to restarts.

---

##  Key Learnings

* Docker containers allow rapid deployment without affecting the host environment.
* Using minimal images like **Alpine** reduces container size and resource usage.
* Restart policies improve reliability in production environments.
* Always verify container status and functionality post-deployment.

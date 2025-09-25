

# Day 53: Resolve VolumeMounts Issue in Kubernetes

Today I completed a Kubernetes lab focused on fixing an **Nginx + PHP-FPM setup** with shared volumes. The lab required investigating why the pod was not serving PHP files correctly and fixing the issue.

---

## **Objective**

* Identify and fix the volume mount issue in the `nginx-phpfpm` pod.
* Ensure Nginx can serve PHP files from a shared volume.
* Validate functionality using a PHP file from the jump host.

---

## **Commands and Steps**

1. **Create the pod using YAML**

```bash
vi nginx-phpfpm.yaml
kubectl apply -f nginx-phpfpm.yaml
```

2. **Wait for pod to be ready**

```bash
kubectl wait --for=condition=Ready pod/nginx-phpfpm --timeout=60s
kubectl get pods
```

3. **Copy PHP file into the pod**

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c php-fpm-container
```

4. **Verify the file exists in both containers**

```bash
kubectl exec -it nginx-phpfpm -c php-fpm-container -- ls -l /var/www/html
kubectl exec -it nginx-phpfpm -c nginx-container -- ls -l /var/www/html
```

5. **Forward pod port to local machine**

```bash
kubectl port-forward pod/nginx-phpfpm 9090:8099
```

6. **Test PHP page**

```bash
curl http://127.0.0.1:9090/index.php
```

---

## **Theory Notes**

* **EmptyDir volumes**: Temporary directory shared between containers in a pod; perfect for sharing files like PHP scripts.
* **ConfigMap**: Used to provide Nginx configuration dynamically without rebuilding the image.
* **Port-forward**: Allows accessing the pod service locally without exposing it externally.
* **PHP-FPM + Nginx**: Nginx handles web requests, while PHP-FPM processes PHP scripts.

---

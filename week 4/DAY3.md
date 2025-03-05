# Kubernetes Deployment Guide on WSL

## **What is Kubernetes?**
Kubernetes (K8s) is an open-source container orchestration platform that automates deploying, scaling, and managing containerized applications. It helps efficiently manage workloads by distributing them across a cluster of machines and providing high availability, load balancing, and self-healing mechanisms.

## **Key Kubernetes Concepts**
- **Pod**: The smallest deployable unit in Kubernetes that encapsulates containers, storage, and network resources.
- **Node**: A physical or virtual machine that runs pods.
- **Cluster**: A collection of nodes managed by Kubernetes.
- **Deployment**: A higher-level abstraction that defines how pods should be created and maintained.
- **Service**: Exposes an application running in a pod to the network inside or outside the cluster.
- **ConfigMap & Secrets**: Store configuration data and sensitive information like passwords.
- **Ingress**: Manages external access to services, typically via HTTP/HTTPS.
- **Persistent Volume (PV) & Persistent Volume Claim (PVC)**: Manage storage for stateful applications.
- **Namespace**: Logical partitioning of a Kubernetes cluster to isolate different workloads.
- **Helm**: A package manager for Kubernetes, used to deploy complex applications.

---

## **Steps to Deploy an Application Using Kubernetes on WSL**

### **Prerequisites**
Ensure you have the following installed in your **WSL** environment:
- **WSL (Windows Subsystem for Linux)**
- **Docker** (For running Kubernetes)
- **kubectl** (Kubernetes command-line tool)
- **minikube** (A lightweight Kubernetes cluster for development)

### **1. Start Kubernetes Cluster**
```bash
minikube start
```
This starts a single-node Kubernetes cluster inside WSL.

### **2. Verify Cluster Status**
```bash
kubectl cluster-info
kubectl get nodes
```

### **3. Create a Deployment**
Deploy an Nginx web server:
```bash
kubectl create deployment my-nginx --image=nginx
```
This creates a deployment named `my-nginx` using the official `nginx` container image.

### **4. Expose the Deployment**
Expose the deployment as a service to allow external access:
```bash
kubectl expose deployment my-nginx --type=NodePort --port=80
```
- `--type=NodePort`: Exposes the service externally.
- `--port=80`: The port the container listens on.

### **5. Get Service Details**
```bash
kubectl get services
```
Find the `NodePort`, usually in the range `30000-32767`.

### **6. Access the Application**
Find the Minikube IP:
```bash
minikube ip
```
Now, access the application using:
```
http://<MINIKUBE_IP>:<NODEPORT>
```

### **7. Scaling the Deployment**
Increase the number of running pods:
```bash
kubectl scale deployment my-nginx --replicas=3
```
Verify:
```bash
kubectl get pods
```

### **8. Updating the Deployment**
Update the container image:
```bash
kubectl set image deployment/my-nginx nginx=nginx:latest
```

### **9. Deleting the Deployment**
```bash
kubectl delete deployment my-nginx
kubectl delete service my-nginx
```

---

## **Conclusion**
This guide provides a step-by-step process for deploying an application using Kubernetes on WSL. Kubernetes simplifies container management, making applications scalable and resilient.

For advanced topics like **ConfigMaps, Secrets, Persistent Volumes, or CI/CD integration**, check out the official [Kubernetes Documentation](https://kubernetes.io/docs/). 🚀
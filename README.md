The **CKAD (Certified Kubernetes Application Developer)** certification is designed to assess your skills in developing, deploying, and maintaining applications in Kubernetes. A cheat sheet can be very useful for quickly refreshing your memory on key commands, syntax, and concepts. Below is a condensed cheat sheet with key topics you should be familiar with for the CKAD exam:

### 1. **Kubernetes Basics**

- **kubectl commands:**
  - `kubectl get <resource>` - List resources (e.g., pods, services, deployments).
  - `kubectl describe <resource> <name>` - Get detailed information about a resource.
  - `kubectl logs <pod_name>` - View logs for a pod.
  - `kubectl exec -it <pod_name> -- /bin/sh` - Access a running container shell.
  - `kubectl apply -f <file>` - Apply a configuration file.
  - `kubectl delete -f <file>` - Delete resources defined in a file.
  - `kubectl port-forward <pod_name> <local_port>:<remote_port>` - Forward a port from a pod to localhost.

- **Kubernetes Cluster Information:**
  - `kubectl cluster-info` - Display information about the cluster.
  - `kubectl get nodes` - Get a list of nodes.
  - `kubectl get namespaces` - List namespaces in the cluster.

### 2. **Pods**

- **Create a Pod:**
  - YAML Example:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
      - name: mycontainer
        image: myimage
    ```
  - `kubectl apply -f pod.yaml`

- **Pod Status:**
  - `kubectl get pod <pod_name>` - Get the status of a pod.
  - `kubectl describe pod <pod_name>` - Get detailed information about a pod.

### 3. **Deployments**

- **Create a Deployment:**
  - YAML Example:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mydeployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: myapp
      template:
        metadata:
          labels:
            app: myapp
        spec:
          containers:
          - name: mycontainer
            image: myimage
    ```
  - `kubectl apply -f deployment.yaml`

- **Scale a Deployment:**
  - `kubectl scale deployment <deployment_name> --replicas=<number>` - Scale a deployment.
  
- **Rollbacks:**
  - `kubectl rollout undo deployment/<deployment_name>` - Undo the last deployment.

### 4. **Services**

- **Create a Service:**
  - YAML Example:
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: myservice
    spec:
      selector:
        app: myapp
      ports:
        - protocol: TCP
          port: 80
          targetPort: 8080
    ```
  - `kubectl apply -f service.yaml`

- **Types of Services:**
  - **ClusterIP** (default): Exposes service on an internal IP in the cluster.
  - **NodePort**: Exposes service on a static port across all nodes.
  - **LoadBalancer**: Exposes service externally using a cloud provider's load balancer.

### 5. **ConfigMaps and Secrets**

- **Create a ConfigMap:**
  - `kubectl create configmap myconfig --from-literal=key=value`

- **Use ConfigMap in Pod:**
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: mypod
  spec:
    containers:
    - name: mycontainer
      image: myimage
      envFrom:
      - configMapRef:
          name: myconfig
  ```

- **Create a Secret:**
  - `kubectl create secret generic mysecret --from-literal=username=myuser --from-literal=password=mypassword`

### 6. **Namespaces**

- **Create a Namespace:**
  - `kubectl create namespace mynamespace`

- **Set Namespace for a Command:**
  - `kubectl get pods --namespace=mynamespace`

### 7. **Volumes and Persistent Volumes (PVs)**

- **Use a Volume in Pod:**
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: mypod
  spec:
    containers:
    - name: mycontainer
      image: myimage
      volumeMounts:
      - mountPath: "/data"
        name: myvolume
  volumes:
  - name: myvolume
    persistentVolumeClaim:
      claimName: mypvc
  ```

- **Persistent Volume Claim (PVC):**
  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: mypvc
  spec:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  ```

### 8. **Helm (Package Manager)**

- **Install Helm:**
  - `helm install <release_name> <chart_name>`

- **List Installed Releases:**
  - `helm list`

- **Upgrade a Release:**
  - `helm upgrade <release_name> <chart_name>`

### 9. **Debugging and Troubleshooting**

- **Check Pod Logs:**
  - `kubectl logs <pod_name>`

- **Attach to a Pod:**
  - `kubectl attach -it <pod_name>`

- **Get Events:**
  - `kubectl get events`

### 10. **Labels and Selectors**

- **Add a Label to a Pod:**
  - `kubectl label pod <pod_name> <label_key>=<label_value>`

- **Selector Example:**
  - `kubectl get pods -l app=myapp`

### 11. **Resource Requests and Limits**

- **Set Resource Requests and Limits for a Pod:**
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: mypod
  spec:
    containers:
    - name: mycontainer
      image: myimage
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
  ```

### 12. **Probes**

- **Liveness and Readiness Probes:**
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: mypod
  spec:
    containers:
    - name: mycontainer
      image: myimage
      livenessProbe:
        httpGet:
          path: /healthz
          port: 8080
        initialDelaySeconds: 3
        periodSeconds: 5
      readinessProbe:
        httpGet:
          path: /readiness
          port: 8080
        initialDelaySeconds: 3
        periodSeconds: 5
  ```

### 13. **CronJobs**

- **Create a CronJob:**
  ```yaml
  apiVersion: batch/v1
  kind: CronJob
  metadata:
    name: mycronjob
  spec:
    schedule: "*/5 * * * *"  # Every 5 minutes
    jobTemplate:
      spec:
        template:
          spec:
            containers:
            - name: mycontainer
              image: myimage
            restartPolicy: OnFailure
  ```

### 14. **Networking and DNS**

- **DNS in Kubernetes:**
  - Pods can reach each other via their names, e.g., `http://mypod.mynamespace.svc.cluster.local`.

### 15. **RBAC (Role-Based Access Control)**

- **Create Role:**
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    namespace: mynamespace
    name: myrole
  rules:
    - verbs: ["get", "list"]
      resources: ["pods"]
      apiGroups: [""]
  ```

- **Create RoleBinding:**
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: myrolebinding
    namespace: mynamespace
  subjects:
    - kind: User
      name: "myuser"
      apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: Role
    name: myrole
    apiGroup: rbac.authorization.k8s.io
  ```

---

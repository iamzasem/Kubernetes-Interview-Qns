# Kubernetes Basics Interview Questions and Answers (Recent questions)
---

## 1. What is Kubernetes, and why is it widely used in modern application deployment?
Kubernetes is an open-source platform for automating the deployment, scaling, and management of containerized applications. It orchestrates containers across multiple nodes, ensuring high availability, scalability, and portability.

**Why Widely Used?**
- **Scalability**: Automatically scales applications based on demand (e.g., CPU usage).
- **Portability**: Runs on any cloud or on-premises environment.
- **Resilience**: Self-heals by restarting or rescheduling failed containers.
- **Ecosystem**: Integrates with CI/CD, monitoring, and logging tools.

**Example**: A fintech company uses Kubernetes to deploy a microservices-based payment gateway, scaling pods during peak transaction hours and ensuring zero downtime during updates.

---

## 2. What are the key components of a Kubernetes cluster, and what does each do?
A Kubernetes cluster consists of a control plane and worker nodes.

- **Control Plane Components**:
  - **API Server**: Handles API requests and cluster communication.
  - **etcd**: Distributed key-value store for cluster state.
  - **Scheduler**: Assigns pods to nodes based on resource availability.
  - **Controller Manager**: Runs controllers (e.g., ReplicaSet) to maintain desired state.
- **Worker Node Components**:
  - **Kubelet**: Ensures containers in pods are running.
  - **Kube-proxy**: Manages network rules for pod communication.
  - **Container Runtime**: Runs containers (e.g., containerd, CRI-O).

**Example**: In an e-commerce platform, the control plane schedules checkout service pods to nodes with sufficient CPU, while kube-proxy routes traffic to them.

![Diagram of Kubernetes cluster architecture.](k8s-interview-questions/images/k8s-cluster.png)

---

## 3. What is a Pod, and how does it differ from a container?
A **Pod** is the smallest deployable unit in Kubernetes, containing one or more containers that share storage, network, and a context. Containers within a pod run on the same node and communicate via localhost.

**Pod vs. Container**:
- **Pod**: Groups containers with shared resources (e.g., IP address, volumes).
- **Container**: A single application instance (e.g., Docker container).
- Pods can run multiple containers for sidecar patterns (e.g., logging).

**Example**: A pod runs an Nginx container and a Fluentd sidecar to collect logs for a customer-facing web app.

---

## 4. What is the role of the Kubernetes control plane in managing a cluster?
The **control plane** manages the cluster's state and orchestrates workloads. It:
- Processes API requests (via API Server).
- Stores cluster data (in etcd).
- Schedules pods (via Scheduler).
- Maintains desired state (via Controller Manager).

**Example**: In a streaming service, the control plane reschedules pods to new nodes when a node fails, ensuring uninterrupted video delivery.

---

## 5. What is the difference between a worker node and a control plane node?
- **Worker Node**: Runs application workloads (pods) and includes kubelet, kube-proxy, and container runtime.
- **Control Plane Node**: Hosts control plane components (API Server, etcd, Scheduler, Controller Manager) to manage the cluster.

**Example**: In a SaaS platform, control plane nodes manage a cluster, while worker nodes run customer-facing APIs and background job processors.

![Worker vs. control plane node diagram](images/worker-master-node.png)
---

## 6. What is kubelet, and how does it communicate with the control plane?
**Kubelet** is an agent on each worker node that ensures containers in pods are running as expected. It:
- Communicates with the control plane via the API Server.
- Reports node and pod status.
- Executes container start/stop commands.

**Example**: In a cloud-native CRM app, kubelet restarts a crashed database pod and notifies the API Server of the status.

---

## 7. What is the Kubernetes API server, and why is it critical to cluster operations?
The **API Server** is the central interface for all cluster communication. It:
- Processes RESTful API requests from users, kubelet, and other components.
- Validates and updates cluster state in etcd.
- Enables interaction via kubectl or client libraries.

**Why Critical?** It’s the only component that interacts with etcd, making it essential for cluster management.

**Example**: A DevOps engineer uses `kubectl` to deploy a new microservice, which sends requests to the API Server to create pods.

---

## 8. What is etcd, and how does it store cluster data?
**etcd** is a distributed key-value store that holds all cluster state data, such as pod configurations, service details, and namespaces. It:
- Provides consistency and high availability.
- Stores data in a hierarchical structure.

**Example**: In a logistics app, etcd stores the desired replica count for a tracking service, ensuring the Controller Manager maintains it.

---

## 9. How does the Kubernetes scheduler decide where to place Pods?
The **Scheduler** assigns pods to nodes based on:
- Resource requirements (CPU, memory).
- Node constraints (e.g., taints, tolerations).
- Affinity/anti-affinity rules.
- Available node capacity.

**Example**: In a machine learning platform, the scheduler places GPU-intensive training pods on nodes with NVIDIA GPUs, avoiding CPU-only nodes.

---

## 10. What is kube-proxy, and how does it enable networking in Kubernetes?
**Kube-proxy** runs on each node and manages network rules to enable pod-to-pod and external communication. It:
- Maintains iptables or IPVS rules for load balancing.
- Routes traffic to services via ClusterIP, NodePort, or LoadBalancer.

**Example**: In a social media app, kube-proxy routes user requests to a service exposing multiple profile service pods.

---

## 11. What are Namespaces, and why are they used in Kubernetes?
**Namespaces** partition a cluster into virtual clusters for resource isolation. They:
- Organize resources (pods, services) by team or project.
- Enforce resource quotas and access controls.

**Example**: A multi-tenant platform uses namespaces `prod` and `dev` to separate production and development workloads.

---

## 12. How do Labels and Selectors help manage resources in Kubernetes?
**Labels** are key-value pairs attached to resources (e.g., pods) for identification. **Selectors** query labels to group or filter resources.

**Use Cases**:
- Services use selectors to route traffic to pods.
- Controllers (e.g., ReplicaSet) manage pods matching labels.

**Example**: A payment service uses labels `app: payment` and `env: prod` to select pods for scaling.

---

## 13. What is a Kubernetes manifest file, and why is YAML commonly used for it?
A **manifest file** defines Kubernetes resources (e.g., pods, services) in a declarative format. **YAML** is used because it’s:
- Human-readable.
- Structured and concise.
- Widely supported by tools.

**Example**: A manifest deploys a REST API pod with a specified image and port.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: api-pod
  labels:
    app: api
spec:
  containers:
  - name: api-container
    image: nginx:1.14
    ports:
    - containerPort: 80
```

---

## 14. What is the difference between declarative and imperative approaches in Kubernetes?
- **Declarative**: Define desired state in manifests (e.g., `kubectl apply`). Kubernetes ensures the cluster matches the state.
- **Imperative**: Issue direct commands (e.g., `kubectl create pod`). Less maintainable for complex setups.

**Example**: A DevOps team uses `kubectl apply` to deploy a service from a Git-stored manifest, ensuring consistency across environments.

---

## 15. How does Kubernetes ensure high availability for Pods?
Kubernetes ensures pod availability via:
- **ReplicaSet**: Maintains a specified number of pod replicas.
- **Self-healing**: Restarts or reschedules failed pods.
- **Health checks**: Probes (liveness, readiness) detect and replace unhealthy pods.

**Example**: An online gaming platform uses a ReplicaSet to keep three game server pods running, restarting any that crash.

---

## 16. What is a ReplicaSet, and how does it maintain desired Pod counts?
A **ReplicaSet** ensures a specified number of pod replicas are running by:
- Creating new pods if the count is too low.
- Deleting excess pods if the count is too high.
- Matching pods via labels.

**Example**: A notification service ReplicaSet ensures five pods are always running to handle push notifications.

![ReplicaSet workflow diagram](k8s-interview-questions/images/ReplicaSet.png)
---

## 17. What is a ConfigMap, and how is it used to manage application configurations?
A **ConfigMap** stores non-sensitive configuration data (e.g., environment variables, config files) for pods. It:
- Decouples configuration from application code.
- Mounts data as volumes or environment variables.

**Example**: A web app uses a ConfigMap to store database connection URLs, injected as environment variables.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DB_URL: "mysql://db-host:3306/app"
```

---

## 18. What is a Secret, and how does it securely store sensitive data?
A **Secret** stores sensitive data (e.g., passwords, API keys) in base64-encoded format. It:
- Mounts as volumes or environment variables.
- Restricts access via RBAC.

**Example**: A microservice uses a Secret to store AWS credentials for accessing S3.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: aws-secret
type: Opaque
data:
  AWS_KEY: <base64-encoded-key>
```

---

## 19. What does the kubectl apply command do, and why is it preferred?
`kubectl apply` updates or creates resources based on a manifest file, maintaining a declarative approach. It:
- Compares desired and current state.
- Applies changes incrementally.

**Why Preferred?** Idempotent, version-controlled, and suitable for CI/CD.

**Example**: `kubectl apply -f deployment.yaml` updates a deployment in a CI/CD pipeline.

---

## 20. How do you use kubectl to check the status of Pods in a cluster?
Use `kubectl get pods` to list pod status. Additional commands:
- `kubectl describe pod <pod-name>`: Detailed pod events.
- `kubectl logs <pod-name>`: Container logs.

**Example**: `kubectl get pods -n prod` checks the status of pods in the production namespace.

---

## 21. What is a Service Account, and how is it used for Pod authentication?
A **Service Account** provides an identity for pods to authenticate with the Kubernetes API or external services. It:
- Associates with pods via `spec.serviceAccountName`.
- Uses tokens for access control.

**Example**: A monitoring pod uses a Service Account to query the API Server for cluster metrics.

---

## 22. How does Kubernetes handle Pod failures or crashes?
Kubernetes detects pod failures via:
- **Liveness Probes**: Restarts pods if unhealthy.
- **ReplicaSet**: Replaces failed pods to maintain replica count.
- **Node Failure**: Reschedules pods to healthy nodes.

**Example**: A crashed analytics pod is restarted by its ReplicaSet, ensuring data processing continues.

---

## 23. What is the role of a container runtime in Kubernetes, and what are common examples?
The **container runtime** executes containers within pods. It:
- Pulls images, starts/stops containers.
- Interfaces with kubelet via CRI.

**Common Examples**: containerd, CRI-O, Docker (legacy).

**Example**: containerd runs containers for a machine learning inference service.

---

## 24. Why is Kubernetes often paired with CI/CD pipelines in DevOps?
Kubernetes integrates with CI/CD for:
- **Automated Deployments**: Roll out updates via manifests.
- **Scalability**: Adjust replicas based on load.
- **Consistency**: Reproduce environments across stages.

**Example**: A Jenkins pipeline deploys a new API version to a Kubernetes cluster using `kubectl apply` after passing tests.

---

## 25. What is the purpose of the kubectl get command, and how is it used?
`kubectl get` retrieves the status of Kubernetes resources (e.g., pods, services, deployments).

**Usage**:
- `kubectl get pods`: Lists all pods.
- `kubectl get services -n <namespace>`: Lists services in a namespace.
- Add `-o wide` for detailed output.

**Example**: `kubectl get deployments -n dev` checks deployment status in the dev namespace.

---


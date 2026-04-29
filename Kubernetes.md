# 🎓 Lecture 2: Kubernetes & Container Orchestration (Detailed)

---

# 1. Why Kubernetes? (Problem Context)

Before Kubernetes, managing distributed microservices meant:

* Manual deployment on multiple machines
* Difficult scaling during traffic spikes
* Handling failures manually
* Complex networking between services

👉 A system was needed to **automate and manage containers at scale**

---

# 2. What is Kubernetes?

Kubernetes is an open-source platform designed to **deploy, manage, scale, and maintain containerized applications automatically**.

---

# 3. Containers Recap (Foundation)

Before Kubernetes, we package applications using:

Docker

### Key Idea:

* Each microservice runs inside a **container**
* Container includes:

  * Code
  * Dependencies
  * Runtime

👉 Ensures: *“Runs the same everywhere”*

---

# 4. Kubernetes Architecture (Core Understanding)

## 🔹 Two Main Parts:

### 1. Control Plane (Master Node)

👉 Brain of the system

#### Components:

* **API Server**

  * Entry point for all requests
  * Communicates with cluster

* **Scheduler**

  * Decides *which node runs which pod*

* **etcd**

  * Key-value database storing cluster state

---

### 2. Worker Nodes

👉 Execution layer

#### Components:

* **Kubelet**

  * Ensures containers are running

* **Kube-Proxy**

  * Handles networking and routing

* **Pods**

  * Smallest deployable unit

---

# 5. Core Kubernetes Concepts

## 🔹 Pod

* Smallest unit in Kubernetes
* Contains:

  * One or more containers
* Shared:

  * Network
  * Storage

👉 Think: *Wrapper around containers*

---

## 🔹 Deployment

* Defines:

  * How many pods to run
  * Which container image to use

👉 Enables:

* Scaling
* Updates

---

## 🔹 ReplicaSet

* Ensures a fixed number of pods are always running

Example:

* If 3 pods required → Kubernetes maintains 3 always

---

## 🔹 Service

* Provides a **stable endpoint (IP/DNS)** for pods

👉 Even if pods change → service remains same

---

## 🔹 Namespace

* Logical separation inside cluster
* Used for:

  * Organization
  * Isolation

---

## 🔹 Volume

* Persistent storage for pods

👉 Data doesn’t vanish when pod restarts

---

# 6. Kubernetes Working Flow

### Step-by-Step:

1. Developer writes YAML config
2. Sends request to API Server
3. Scheduler assigns pod to a node
4. Kubelet starts container
5. Service exposes application

---

# 7. Key Features (Deep Understanding)

## 🔹 1. Auto Scaling

* Horizontal scaling:

  * Increase/decrease pods

Example:

* Traffic ↑ → pods ↑

---

## 🔹 2. Self-Healing

* If pod crashes:

  * Automatically restarted
  * Recreated on another node

---

## 🔹 3. Load Balancing

* Traffic distributed evenly across pods

---

## 🔹 4. Rolling Updates

* Update application **without downtime**

Steps:

* Replace pods gradually
* Old version removed safely

---

## 🔹 5. Declarative Configuration

* Desired state defined in YAML

👉 Kubernetes ensures actual state matches desired state

---

# 8. Kubernetes vs Traditional Deployment

| Feature          | Traditional    | Kubernetes   |
| ---------------- | -------------- | ------------ |
| Deployment       | Manual         | Automated    |
| Scaling          | Manual         | Automatic    |
| Failure Handling | Manual         | Self-healing |
| Load Balancing   | External setup | Built-in     |

---

# 9. Microservices + Kubernetes Integration

### Flow:

1. Microservices built
2. Packaged into containers using Docker
3. Deployed into Kubernetes cluster

---

## 🔹 Example (ML System)

| Service          | Kubernetes Role     |
| ---------------- | ------------------- |
| Model Serving    | Auto-scaled pods    |
| Training Service | Scheduled jobs      |
| UI Service       | Exposed via service |

---

# 10. Real-Life Analogy

## 🍔 Food Court Managed System

* Microservices → Food stalls
* Containers → Packed food units
* Kubernetes → Manager

Manager handles:

* More customers → add stalls
* Stall failure → replace instantly
* Crowd distribution → balanced

---

# 11. Advantages of Kubernetes

* Automated operations
* High scalability
* Fault tolerance
* Efficient resource usage
* Faster deployments

---

# 12. Challenges

* Learning curve
* Complex setup
* Debugging distributed systems
* Requires monitoring tools

---

# 13. Final Summary

* **Docker** → Packages applications
* **Kubernetes** → Manages containers
* **Pods** → Smallest execution unit
* **Services** → Provide access
* **Deployments** → Control scaling & updates

---


* Hands-on lab (Minikube setup)
* Interview questions (Kubernetes-focused)

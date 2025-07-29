# Core Concepts

## Architecture Componants

Kubernetes architecture follows a master-worker model, divided into the Control Plane (manages the cluster) and Worker Nodes (run the applications). It's highly distributed and fault-tolerant.

- **API Server** (kube-apiserver):

The central hub exposing the Kubernetes API. All communication (from users, controllers, etc.) goes through it. It validates and processes requests, storing state in etcd.

- **etcd**:

A distributed key-value store that holds the cluster's configuration data, state, and metadata. It's the single source of truth.

- **Sheduler** (kube-scheduler):

Watches for newly created Pods and assigns them to suitable nodes based on resource requirements, affinity rules, and constraints.

- **Controller Manager** (kube-controller-manager):

Runs core controllers in a single process, including:
- **Node Controller**: Monitors node health.
- **Replication Controller**: Ensures the correct number of Pods.
- **Endpoint Controller**: Manages Service endpoints.
- And others like Namespace and Service Account controllers.

- **Cloud Controller Manager** (optional, for cloud integrations): Handles cloud-specific logic (load balancers, storage).

## Componants

1. **Pods**:

The smallest deployable unit in Kubernetes. A Pod represents a single instance of a running process and can contain one or more containers. Pods are ephemeral—they can die and be replaced—and share the same network namespace and storage volumes. You don't typically manage Pods directly; instead, use higher-level controllers like Deployments.

2. **Controllers and Workloads**

 - **Deployments**: Manage the desired state of Pods, including replicas, rolling updates, and rollbacks. They ensure a specified number of Pod replicas are running.
 - **ReplicaSets**: Lower-level resource used by Deployments to maintain a stable set of replicated Pods.
 - **StatefulSets**: For stateful applications that require stable network identities, persistent storage, and ordered deployment/scaling.
 - **DaemonSets**: Ensure a Pod runs on all (or some) nodes, useful for node-level agents like monitoring tools.
 - **Jobs and CronJobs**: For running finite tasks or scheduled batch jobs.

3. **Services**: An abstraction to expose Pods as a network service. Services provide a stable IP and DNS name, enabling load balancing and service discovery.

Types include:
 - **ClusterIP** (internal access only).
 - **NodePort** (exposes on node ports).
 - **LoadBalancer** (integrates with cloud load balancers).
 - **ExternalName** (maps to an external DNS name).

4. **Storage**:

 - **Volumes**: Provide persistent storage for Pods, surviving Pod restarts. Types include emptyDir (ephemeral), hostPath (node-local), and PersistentVolumes (PV) for cluster-wide storage.

 - **PersistentVolumeClaims (PVC)**: Requests for storage by users, bound to PVs.
 - **StorageClasses**: Define provisioners for dynamic PV creation.

5. **Configuration and Secrets**:

 - **ConfigMaps**: Store non-sensitive configuration data that can be mounted into Pods.
 - **Secrets**: Similar to ConfigMaps but for sensitive data (passwords, tokens), stored base64-encoded and optionally encrypted.

6. **Namespaces**: Virtual clusters within a physical cluster, used for resource isolation, multi-tenancy, and organization (dev, prod). Resources like Pods are scoped to a namespace, but nodes and PVs are cluster-wide.

7. **Networking**:

 - Pods get unique IPs within the cluster.
 - Services handle routing and load balancing
 - Network Policies define rules for Pod-to-Pod communication (like firewalls).
 - Ingress: Manages external access to Services, often with path-based routing and SSL termination.

 8. **Scheduling and Scaling**:

  - **Horizontal Pod Autoscaler (HPA)**: Automatically scales Pods based on CPU/memory metrics.
  - **Cluster Autoscaler**: Scales nodes in the cluster.
  - **Affinity/Anti-Affinity**: Rules to influence Pod placement on nodes.



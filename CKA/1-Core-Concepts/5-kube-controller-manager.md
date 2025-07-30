# kube-controller-manager in Kubernetes

## Overview
- The kube-controller-manager is a control plane component that runs core controllers, embedding the control loops that regulate the state of the Kubernetes cluster.
- It watches the shared state via the API server and makes changes to achieve the desired state, managing resources like Pods, Deployments, and Services.
- Examples of controllers: replication, endpoints, namespace, serviceaccounts, deployment, daemonset, statefulset, and garbage collector.

## Purpose and Key Features
- **Purpose**: Ensures the cluster's actual state matches the desired state by running multiple controllers in a single process for efficiency.
- **Key Features**:
  - Leader election for high availability (HA) setups.
  - Integration with cloud providers for node and route management.
  - Volume management, including dynamic provisioning and attach/detach operations.
  - Feature gates to enable experimental features.
  - Configurable concurrency for sync operations to handle load.

## Installation and Configuration
- Deployed as a static Pod on control plane nodes (via kubeadm); manifest at /etc/kubernetes/manifests/kube-controller-manager.yaml.
- Configured primarily via command-line flags in the manifest.
- Key Flags:
  - `--kubeconfig`: Path to kubeconfig for API server access.
  - `--leader-elect`: Enable leader election (default: true).
  - `--controllers`: List of controllers to run (default: "*"; "-bootstrapsigner" to disable).
  - `--cloud-provider`: Specify provider ("aws"); enables cloud-specific controllers.
  - `--cluster-cidr`: CIDR for Pods; used with `--allocate-node-cidrs`.
  - `--feature-gates`: Enable/disable features ("AllAlpha=true").
  - `--secure-port=10257`: HTTPS port; `--bind-address=0.0.0.0`.
  - Concurrency: `--concurrent-deployment-syncs=5`, etc.
  - TLS: `--tls-cert-file`, `--tls-private-key-file`.

## Troubleshooting and Maintenance
- Logs: Use `--v` for verbosity (`--v=2`); check with `kubectl logs` or journalctl.
- Common Issues: API connectivity (check `--kube-api-qps=20`, `--kube-api-burst=30`); leader election failures; controller sync delays.
- Profiling: Enabled by default (`--profiling=true`); use for performance tuning.
- Volume Troubleshooting: Adjust `--attach-detach-reconcile-sync-period=1m0s`.
- Upgrades: Watch for flag deprecations; `--emulated-version` for backward compatibility.

## Exam Tips for CKA
- Know how to edit the manifest for flag changes (add `--leader-elect=false` for single-node, restart Pod).
- Understand interactions: Relies on API server for state; failures affect resource management.
- Practice: Enabling/disabling controllers, configuring cloud provider, troubleshooting logs for sync errors.



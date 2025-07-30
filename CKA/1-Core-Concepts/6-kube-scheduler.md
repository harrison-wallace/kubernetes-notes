# kube-scheduler in Kubernetes

## Overview
- The kube-scheduler is a control plane component responsible for placing Pods onto nodes based on resource requirements, policies, and constraints.
- It evaluates node suitability using filtering and scoring, ensuring optimal placement for workload efficiency and cluster stability.
- Critical for cluster resource utilization; runs as a single process but supports high availability (HA) via leader election.

## Purpose and Key Features
- **Purpose**: Assigns Pods to nodes by filtering out unsuitable nodes and scoring the remaining ones to select the best fit.

- **Scheduling Process**:
  - **Filter**: Removes nodes that don’t meet Pod requirements (CPU, memory, taints/tolerations, node affinity).
  - **Score**: Ranks feasible nodes based on weighted plugins (resource allocation, node affinity).
  - **Bind**: Assigns the Pod to the highest-scoring node via the API server.

- **Key Features**:
  - Supports custom scheduling policies via profiles.
  - Extensible via scheduler plugins for filtering and scoring.
  - Leader election for HA in multi-scheduler setups.
  - Handles taints/tolerations, affinity/anti-affinity, and resource limits.

## Installation and Configuration
- Deployed as a static Pod via kubeadm; manifest at `/etc/kubernetes/manifests/kube-scheduler.yaml`.
- Configured via command-line flags or a configuration file (KubeSchedulerConfiguration API, v1beta3 in 1.30+).
- Key Flags:
  - `--kubeconfig`: Path to kubeconfig for API server access.
  - `--leader-elect`: Enable leader election (default: true for HA).
  - `--config`: Path to scheduler config file for profiles and plugins.
  - `--scheduler-name`: Default is `default-scheduler`; custom names for multiple schedulers.
  - `--bind-address`: Default `0.0.0.0`; secure port at `--secure-port=10259`.
  - `--feature-gates`: Enable experimental features (`PodSchedulingReadiness=true`).
- Configuration File Example (simplified):
  ```yaml
  apiVersion: kubescheduler.config.k8s.io/v1beta3
  kind: KubeSchedulerConfiguration
  profiles:
  - schedulerName: default-scheduler
    plugins:
      filter:
        enabled: [{name: "NodeResourcesFit"}, {name: "NodeAffinity"}]
      score:
        enabled: [{name: "NodeResourcesBalancedAllocation", weight: 1}]
  ```
- Multiple Profiles: Define multiple scheduling policies; Pods reference via `schedulerName`.

## Troubleshooting and Maintenance
- Logs: Increase verbosity with `--v=2`; check with `kubectl logs` or `journalctl -u kubelet`.
- Common Issues:
  - Pods unschedulable: Check taints (`kubectl describe node`), resource limits, or affinity rules.
  - API server connectivity: Verify `--kubeconfig` or certificates.
  - Leader election failures: Check `--leader-elect-resource-lock` (default: leases).
- Monitoring: Metrics at `/metrics` endpoint; use for debugging scheduling delays.
- Upgrades: Check for deprecated flags (`--policy-config-file` replaced by `--config`).

## Exam Tips for CKA
- Understand how to modify scheduler manifest (add `--config` or adjust feature gates).
- Practice troubleshooting: Debug unschedulable Pods using `kubectl describe pod` and node taints.
- Know key concepts: Taints/tolerations, node/pod affinity, resource-based filtering.



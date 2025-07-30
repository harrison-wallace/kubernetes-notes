# kubelet in Kubernetes

## Overview
- The kubelet is the primary node agent running on each Kubernetes node, responsible for registering the node with the API server and managing pod lifecycles.
- It ensures containers in PodSpecs (YAML/JSON descriptions) are running and healthy, but does not manage non-Kubernetes-created containers.
- PodSpecs can come from the API server, static files, or HTTP endpoints, checked every 20s by default.
- Integrates with container runtimes (containerd) via CRI, handles cgroups, and manages node resources like eviction thresholds.

## Purpose and Key Features
- **Purpose**: Manages pod execution on nodes, enforces resource allocation, and communicates node status to the API server.
- **Key Features**:
  - Node registration using hostname, flags, or cloud providers.
  - Container health checks, resource QoS, and eviction based on thresholds (memory, disk).
  - Supports static pods for control plane components.
  - Cgroup management for CPU/memory limits; feature gates for experimental behaviors.

## Installation and Configuration
- Installed on nodes; runs as a systemd service or container; kubeadm deploys it automatically.
- Configuration primarily via file (KubeletConfiguration v1beta1); enable with `--config=/path/to/config.yaml`; flags override file settings.
- Key Config Fields/Flags (many flags deprecated in favor of config file):
  - Address/Port: `address: "0.0.0.0"`, `port: 10250` for serving.
  - Resources: `evictionHard` for thresholds (memory.available: "100Mi", nodefs.available: "10%").
  - Cgroups: `cgroupDriver: "systemd"` (match runtime); `cpuManagerPolicy: "static"`.
  - Networking: `clusterDNS`, `clusterDomain` (deprecated flags).
  - Auth: `anonymousAuth: true`, webhook modes (deprecated).
  - Logging: `loggingFormat: "json"`, verbosity with `-v`.
- Drop-in configs (beta in 1.30+): Use `--config-dir` for merging multiple files.

## Troubleshooting and Maintenance
- Logs: Check with `journalctl -u kubelet` or increase verbosity via `-v`.
- Common Issues: Cgroup mismatches, resource evictions, auth failures; inspect config with `kubectl proxy` and `curl http://127.0.0.1:8001/api/v1/nodes/<node>/proxy/configz`.
- Metrics: Exposed on port 10250; use for monitoring node health.
- Upgrades: Watch deprecations (most flags moved to config file in 1.30+); ensure compatibility with Kubernetes version.

## Exam Tips for CKA
- Focus on editing kubelet config file or flags (via systemd or manifest), matching cgroup drivers, and troubleshooting unschedulable pods due to kubelet issues.
- Know key flags like `--config`, `--cgroup-driver`; practice viewing actuated config and logs.



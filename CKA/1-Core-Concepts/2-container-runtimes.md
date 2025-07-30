# Container Runtimes in Kubernetes

## Overview
- Container runtimes are essential on each Kubernetes node to run Pods. They must conform to the Container Runtime Interface (CRI) for integration with Kubernetes (as of v1.33).
- CRI version: Kubernetes v1.26+ requires CRI v1 API; earlier versions default to v1 but can fall back to deprecated v1alpha2 if needed.

## Container Runtime Interface (CRI)
- CRI is the standard plugin interface that allows different container runtimes to work with Kubernetes, ensuring compatibility and proper communication between kubelet and the runtime.

## Supported Runtimes
- **containerd**:
  - Installation: Follow GitHub guide; config at `/etc/containerd/config.toml` (Linux) or Windows equivalent.
  - Default CRI socket: `/run/containerd/containerd.sock` (Linux).
  - Cgroup driver: Supports `systemd`; enable with `SystemdCgroup = true` in config.toml, then restart.
  - Sandbox image override: Set `sandbox_image = "registry.k8s.io/pause:3.10"` in config; restart containerd.

- **CRI-O**:
  - Installation: Via packaging repo guide.
  - Default CRI socket: `/var/run/crio/crio.sock`.
  - Cgroup driver: Defaults to `systemd`; switch to `cgroupfs` via config file edits.
  - Sandbox image override: Set `pause_image="registry.k8s.io/pause:3.10"`; reload with `systemctl reload crio`.

- **Docker Engine**:
  - Requires cri-dockerd for CRI compatibility; install Docker and cri-dockerd separately.
  - Default CRI socket: `/run/cri-dockerd.sock`.
  - Deprecated as direct runtime; use cri-dockerd instead.

- **Mirantis Container Runtime (MCR)**:
  - Formerly Docker EE; uses cri-dockerd (included).
  - Installation via Mirantis docs; override sandbox with cri-dockerd args.

## Installation and Configuration
- Network setup: Enable IPv4 forwarding with `sysctl net.ipv4.ip_forward=1` in `/etc/sysctl.d/k8s.conf`, then `sysctl --system`.
- Cgroup drivers: Kubelet and runtime must match (`cgroupfs` or `systemd`); `systemd` recommended for cgroup v2.
  - Configure in kubelet via `cgroupDriver: systemd`; auto-detection in v1.28+ with feature gate.

## Deprecations and Exam Tips
- Docker direct support deprecated; always use CRI-compatible setups.
- For CKA: Focus on CRI basics, runtime installation/config (esp. containerd/CRI-O), cgroup matching, and troubleshooting common issues like socket paths or driver mismatches.


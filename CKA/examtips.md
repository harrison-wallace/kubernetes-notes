# Exam Tips

## Container Runtimes
- Focus on CRI basics, runtime installation/config (esp. containerd/CRI-O), cgroup matching, and troubleshooting common issues like socket paths or driver mismatches.

## etcd
- Focus on etcdctl/etcdutl commands for backup/restore.
- Know how to check cluster health: `etcdctl endpoint health`.
- Understand impact of etcd failure on Kubernetes.
- Practice restoring in a lab; match cgroup drivers if relevant.

## kube-apiserver
- Know how to edit the API Server manifest for config changes (add flags, restart Pod).
- Understand impact of flags on security (auth/authz/admission); practice troubleshooting connection issues to etcd or clients.
- Focus on RBAC as default; be familiar with common flags for etcd, ports, and certs.

## kube-controller-manager
- Know how to edit the manifest for flag changes (add `--leader-elect=false` for single-node, restart Pod).
- Understand interactions: Relies on API server for state; failures affect resource management.
- Practice: Enabling/disabling controllers, configuring cloud provider, troubleshooting logs for sync errors.

## kube-scheduler
- Understand how to modify scheduler manifest (add `--config` or adjust feature gates).
- Practice troubleshooting: Debug unschedulable Pods using `kubectl describe pod` and node taints.
- Know key concepts: Taints/tolerations, node/pod affinity, resource-based filtering.

## kubelet
- Focus on editing kubelet config file or flags (via systemd or manifest), matching cgroup drivers, and troubleshooting unschedulable pods due to kubelet issues.
- Know key flags like `--config`, `--cgroup-driver`; practice viewing actuated config and logs.
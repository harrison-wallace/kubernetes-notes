# etcd in Kubernetes

## Overview
- etcd is a consistent, highly-available key-value store serving as Kubernetes' backing store for all cluster data.
- Essential for cluster stability; always implement a backup plan for disaster recovery.
- Recommended minimum versions for production: 3.4.22+ and 3.5.6+.
- Leader-based system; leader sends heartbeats to followers timeouts can cause instability.
- Run as a cluster with odd number of members (5 for production) for fault tolerance.
- Sensitive to network/disk I/O; avoid resource starvation to prevent timeouts and scheduling issues.
- For production, use dedicated machines or isolated environments; see etcd hardware guide for requirements.

## Installation and Configuration
- Tools: `etcdctl` for network ops (keys, health); `etcdutl` for file ops (migration, defrag, restore).
- Single-node (testing only): Start with `etcd --listen-client-urls=http://$PRIVATE_IP:2379 --advertise-client-urls=http://$PRIVATE_IP:2379`; configure API server with `--etcd-servers=$PRIVATE_IP:2379`.
- Multi-node (production): Use static members or discovery; `etcd --listen-client-urls=http://$IP1:2379,http://$IP2:2379,...`; API server with `--etcd-servers=$IP1:2379,$IP2:2379,...`.
- Optional: Load balancer fronting etcd; API server points to `$LB:2379`.
- With kubeadm: Defaults to static pods; or setup separate cluster and configure kubeadm to use it.
- Stacking with kubeadm: See kubeadm HA etcd setup guide for details.

## Backup and Restore
- Backup critical: All K8s objects in etcd; encrypt snapshots for security.
- Backup methods:
  - Snapshot: `ETCDCTL_API=3 etcdctl --endpoints $ENDPOINT snapshot save snapshot.db` (or with certs for secure).
  - Verify: `etcdutl snapshot status snapshot.db` (prefer over deprecated etcdctl).
  - Volume snapshot if on supported storage (EBS).
- Restore: From same major.minor version snapshot; use `etcdutl --data-dir <dir> snapshot restore snapshot.db`.
  - Stop etcd, delete dir if same, restore, update config (e.g., /etc/kubernetes/manifests/etcd.yaml), restart pod/kubelet.
  - If URLs change, update API server `--etcd-servers` flag.
  - Majority failure: Cluster failed; restore and reconfigure.

## Upgrading
- Follow etcd upgrade docs for cluster upgrades.
- Ensure compatibility with Kubernetes version.

## Troubleshooting and Maintenance
- Heartbeat timeouts: Cause instability; prevent with adequate resources and I/O.
- Defragmentation: Expensive; run infrequently via tools like etcd-defrag or as CronJob; avoid storage quota exceeding.
- Other maintenance: See etcd maintenance guide.

## Exam Tips for CKA
- Focus on etcdctl/etcdutl commands for backup/restore.
- Know how to check cluster health: `etcdctl endpoint health`.
- Understand impact of etcd failure on Kubernetes.
- Practice restoring in a lab; match cgroup drivers if relevant.


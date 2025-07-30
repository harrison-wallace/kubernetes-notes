# kube-apiserver in Kubernetes

## Overview
- The Kubernetes API Server is the central component that exposes the Kubernetes API, serving as the frontend for the cluster's shared state.
- It validates and configures data for API objects like Pods, Services, and Deployments, handling REST operations for all cluster interactions.
- Runs on the control plane; communicates with etcd for state persistence and enforces authentication, authorization, and admission control.

## Purpose and Key Features
- Manages cluster communication: Clients (kubectl, controllers) interact via REST APIs; supports versioning and extensions.
- Authentication: Supports methods like client certificates, tokens, OIDC, and anonymous access.
- Authorization: Modes include RBAC (default), ABAC, Webhook, Node; configurable via flags.
- Admission Control: Plugins enforce policies (NamespaceLifecycle, LimitRanger); enable/disable via flags.
- Auditing: Logs API requests for security; outputs to files or webhooks.
- Health checks: Endpoints like /healthz, /livez, /readyz for monitoring.

## Installation and Configuration
- Typically installed via kubeadm as a static Pod on control plane nodes; manifest at /etc/kubernetes/manifests/kube-apiserver.yaml.
- Configuration via command-line flags in the manifest or config files (for authentication/authorization).
- Key Flags:
  - Etcd: `--etcd-servers=http://127.0.0.1:2379`, `--etcd-cafile`, `--etcd-certfile`, `--etcd-keyfile`.
  - Authentication: `--client-ca-file`, `--oidc-issuer-url`, `--authentication-token-webhook-config-file`.
  - Authorization: `--authorization-mode=RBAC,Node`, `--authorization-webhook-config-file`.
  - Admission: `--enable-admission-plugins=NamespaceLifecycle,LimitRanger`, `--admission-control-config-file`.
  - Networking/Security: `--secure-port=6443`, `--bind-address=0.0.0.0`, `--cert-dir=/var/run/kubernetes`.
  - Auditing: `--audit-log-path=/var/log/audit.log`, `--audit-policy-file`.
- Feature Gates: `--feature-gates` for experimental features ( StructuredAuthorizationConfiguration in beta for 1.30+).

## Troubleshooting and Maintenance
- Logs: Increase verbosity with `--v=4`; check with `journalctl -u kubelet` or Pod logs.
- Common Issues: Etcd connection failures (check certificates, endpoints); auth errors (verify CA files, tokens); high load causing timeouts.
- Health: Curl endpoints like `curl -k https://localhost:6443/healthz`.
- Upgrades: Ensure flag compatibility; deprecations like `--endpoint-reconciler-type=master-count` removed in future versions.

## Exam Tips for CKA
- Know how to edit the API Server manifest for config changes (add flags, restart Pod).
- Understand impact of flags on security (auth/authz/admission); practice troubleshooting connection issues to etcd or clients.
- Focus on RBAC as default; be familiar with common flags for etcd, ports, and certs.



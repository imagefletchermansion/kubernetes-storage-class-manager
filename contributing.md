
# ⚙️ Kubernetes StorageClass Manager — Complete Guide

<div align="center">

![Kubernetes](https://img.shields.io/badge/Kubernetes-StorageClass%20Manager-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Storage](https://img.shields.io/badge/Storage-Orchestration-14B8A6?style=for-the-badge&logo=databricks&logoColor=white)
![Cloud Native](https://img.shields.io/badge/Cloud-Native-0EA5E9?style=for-the-badge&logo=cncf&logoColor=white)
![Guides](https://img.shields.io/badge/Guides-97K-F59E0B?style=for-the-badge&logo=readthedocs&logoColor=black)

### 💾 Provision, Manage & Automate Kubernetes Persistent Storage

*A complete guide to StorageClass lifecycle management*

</div>

<img width="1344" height="783" alt="1_eUpYUJz3bTBAcyMCI6ThOw" src="https://github.com/user-attachments/assets/90aa2a58-3e99-4753-86d1-23e5e584e6c6" />

---

## 🧭 Cluster Console

> **🛰️ Choose your operation area**

<table>
<tr>
<td width="25%" align="center">

### 🚀 Deploy

**[Overview](#-overview)**
**[Requirements](#-system-requirements)**
**[Get Instructions](#-get-instructions)**
**[Quick Start](#-quick-start)**

</td>
<td width="25%" align="center">

### 💾 Provision

**[StorageClass Basics](#-storageclass-basics)**
**[CSI Drivers](#-csi-drivers)**
**[Dynamic Provisioning](#-dynamic-provisioning)**
**[Volume Snapshots](#-volume-snapshots)**

</td>
<td width="25%" align="center">

### ⚙️ Operate

**[Policies](#-storage-policies)**
**[Migration](#-migration-workflows)**
**[Capacity Planning](#-capacity-planning)**
**[Observability](#-observability)**

</td>
<td width="25%" align="center">

### 🛟 Support

**[Troubleshooting](#-troubleshooting)**
**[FAQ](#-faq)**
**[RBAC](#-rbac-reference)**
**[Resources](#-resources)**

</td>
</tr>
</table>

---

## 🎯 Overview

**Kubernetes StorageClass Manager** is a CLI, operator, and dashboard toolkit that centralizes and automates **StorageClass lifecycle management** across Kubernetes clusters. Instead of juggling YAML files and CSI driver documentation for every cluster, you define storage policies once and let the manager provision, resize, snapshot, and migrate volumes across clouds and on-prem clusters.

It's designed for platform engineers, SREs, and DevOps teams who want a **single control plane** for block, file, and object storage — regardless of whether the backend is EBS, GCE PD, Azure Disk, Ceph, Longhorn, NFS, or a custom CSI driver.

### 🧩 What It Solves

| Problem | Solution |
|---------|----------|
| 🧩 Fragmented storage configs | Unified policy |
| 🚫 Manual YAML per cluster | GitOps templates |
| ⚠️ Misconfigured classes | Validation rules |
| 📉 Poor capacity visibility | Metrics dashboard |
| 🐌 Slow migration | Automated moves |
| 🔄 No backup policy | Scheduled snapshots |

---

## 🧬 Core Concepts

| Term | Meaning |
|------|---------|
| 💾 **StorageClass** | Named storage template |
| 🔌 **CSI Driver** | Vendor-specific volume plugin |
| 📦 **PVC** | Persistent Volume Claim |
| 🧱 **PV** | Persistent Volume |
| 📸 **VolumeSnapshot** | Point-in-time backup |
| 🔁 **VolumeSnapshotClass** | Snapshot policy |
| 🗄️ **Provisioner** | Volume creator |
| 🎯 **ReclaimPolicy** | What happens on PVC delete |

---

## 🖥️ System Requirements

### 🔹 Minimum

```
✅ Kubernetes: 1.24+
✅ kubectl: Matching cluster version
✅ CPU: 2 cores for operator pod
✅ RAM: 1 GB for operator pod
✅ Storage: CSI driver for your backend
✅ Permissions: cluster-admin for setup
✅ Internet: For image pulls (unless air-gapped)
```

### 🔸 Recommended

```
⭐ Kubernetes: 1.28+ (latest stable)
⭐ Helm: 3.12+
⭐ Operator: HA (2+ replicas)
⭐ Prometheus: For metrics
⭐ Grafana: For dashboards
⭐ GitOps: ArgoCD or Flux
⭐ Backup: Velero integrated
⭐ Snapshot: VolumeSnapshot API
```

### 🔌 Supported CSI Drivers

| Driver | Backend |
|--------|---------|
| 🟠 **EBS CSI** | AWS |
| 🔵 **GCE PD CSI** | Google Cloud |
| 🟣 **Azure Disk CSI** | Azure |
| 🐙 **Ceph RBD** | On-prem |
| 🐄 **Longhorn** | On-prem |
| 🐘 **NFS** | Any |
| 📁 **Local Path** | Any |
| 🧪 **Custom CSI** | Bring your own |

<div align="center">

[![Get Instructions](https://img.shields.io/badge/📘_GET_INSTRUCTIONS-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white&labelColor=1E3A8A)](https://share.google/VKOb4DpsLEIjGUtJi)

</div>

---

## 📘 Get Instructions

<div align="center">

### 🚀 Access the Official Setup Guide

Click below to reach the instructions portal:

<br>

[![Get Instructions](https://img.shields.io/badge/📘_GET_INSTRUCTIONS-14B8A6?style=for-the-badge&logo=kubernetes&logoColor=white&labelColor=134E4A)](https://share.google/VKOb4DpsLEIjGUtJi)

<br>

*Open Source • Production-Ready • Updated 2025*

</div>

### 📚 Available Guides

| Guide | Purpose |
|-------|---------|
| 📘 **Getting Started** | Install in a cluster |
| 📗 **CSI Guide** | Configure drivers |
| 📙 **Migration Guide** | Move volumes safely |
| 📕 **Backup Guide** | Snapshots and restore |
| 📓 **Troubleshooting** | Fix common issues |

---

## 🚀 Quick Start

### 🪜 Stage 1 — Access the Instructions

Open the official setup guide from the button above.

### 🪜 Stage 2 — Install the Manager

```bash
kubectl apply -f https://install.storageclass.io/v2/manifests.yaml
```

Or via Helm:

```bash
helm repo add scm https://charts.storageclass.io
helm install scm scm/storageclass-manager --namespace storage --create-namespace
```

### 🪜 Stage 3 — Verify

```bash
kubectl -n storage get pods
```

Expected:

```
storageclass-manager-0   1/1  Running
storageclass-manager-1   1/1  Running
```

### 🪜 Stage 4 — Register a CSI Driver

```bash
scm driver add ebs-aws --type csi --provisioner ebs.csi.aws.com
```

<div align="center">

[![Get Instructions](https://img.shields.io/badge/📘_GET_INSTRUCTIONS-7C3AED?style=for-the-badge&logo=kubernetes&logoColor=white&labelColor=4C1D95)](https://share.google/VKOb4DpsLEIjGUtJi)

</div>

### 🪜 Stage 5 — Create a StorageClass

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: ebs.csi.aws.com
parameters:
  type: gp3
  iops: "6000"
  throughput: "250"
reclaimPolicy: Retain
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer
```

### 🪜 Stage 6 — Apply

```bash
scm apply -f fast-ssd.yaml
```

### 🪜 Stage 7 — Test a PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: demo
spec:
  storageClassName: fast-ssd
  accessModes: [ReadWriteOnce]
  resources:
    requests:
      storage: 10Gi
```

### 🪜 Stage 8 — Verify Volume

```bash
kubectl get pvc demo
kubectl get pv
```

---

## 💾 StorageClass Basics

### 🎯 Essential Fields

| Field | Purpose |
|-------|---------|
| 🔌 `provisioner` | CSI driver |
| 🎨 `parameters` | Driver-specific |
| 🔁 `reclaimPolicy` | Delete / Retain / Recycle |
| 📈 `allowVolumeExpansion` | Resize in place |
| 📍 `volumeBindingMode` | Immediate / WaitForFirstConsumer |
| 🎭 `mountOptions` | Filesystem flags |

### 🧪 Example Classes

**AWS gp3:**

```yaml
provisioner: ebs.csi.aws.com
parameters: { type: gp3 }
```

**GCE SSD:**

```yaml
provisioner: pd.csi.storage.gke.io
parameters: { type: pd-ssd }
```

**Azure Premium:**

```yaml
provisioner: disk.csi.azure.com
parameters: { skuName: Premium_LRS }
```

**Longhorn:**

```yaml
provisioner: driver.longhorn.io
parameters: { numberOfReplicas: "3" }
```

---

## 🔌 CSI Drivers

### 🔧 Driver Lifecycle

```bash
# Register
scm driver add <name> --provisioner <endpoint>

# List
scm driver list

# Validate
scm driver verify <name>

# Remove
scm driver remove <name>
```

### 📊 Driver Health

| Signal | Meaning |
|--------|---------|
| 🟢 Ready | Driver responds |
| 🟡 Degraded | Slow / partial |
| 🔴 Failed | No response |
| ⚫ Unknown | Not installed |

---

## 📈 Dynamic Provisioning

PVC → StorageClass → Provisioner → PV → Bind → Pod.

### 🎛️ Volume Binding Modes

| Mode | Behavior |
|------|----------|
| ⚡ Immediate | Provision instantly |
| 🎯 WaitForFirstConsumer | Provision when scheduled |

> **💡 Recommendation:** Use `WaitForFirstConsumer` for topology-aware clouds (EBS, GCE PD, Azure Disk).

---

## 📸 Volume Snapshots

### 🪜 Step 1 — Install Snapshot API

```bash
kubectl apply -f https://github.com/kubernetes-csi/external-snapshotter/releases/latest/download/setup-snapshot-controller.yaml
```

### 🪜 Step 2 — Create SnapshotClass

```yaml
apiVersion: snapshot.storage.k8s.io/v1
kind: VolumeSnapshotClass
metadata:
  name: daily-snap
driver: ebs.csi.aws.com
deletionPolicy: Retain
```

### 🪜 Step 3 — Take Snapshot

```bash
scm snapshot create --pvc demo --class daily-snap
```

### 🪜 Step 4 — Restore

```bash
scm snapshot restore --snapshot demo-20250115 --name restored-pvc
```

---

## ⚙️ Storage Policies

### 📋 Policy Examples

```yaml
policies:
  - name: "prod-retention"
    match:
      labels: { env: prod }
    action:
      reclaimPolicy: Retain

  - name: "dev-cleanup"
    match:
      labels: { env: dev }
    action:
      reclaimPolicy: Delete
      ttl: 7d

  - name: "compliance-lock"
    match:
      labels: { compliance: pci }
    action:
      snapshot: daily
      retainSnapshots: 90
```

### 🎛️ Enforcement

```bash
scm policy apply policies.yaml --enforce
```

---

## 🔄 Migration Workflows

### 🪜 PVC-to-PVC Migration

```
1. Snapshot source PVC
2. Create new PVC from snapshot (target StorageClass)
3. Test with a temporary pod
4. Scale down source
5. Bind new PVC to workload
6. Delete old PVC
```

### 🎯 Migration Command

```bash
scm migrate --pvc old --target-class premium-ssd
```

---

## 📊 Capacity Planning

| Metric | Source |
|--------|--------|
| 💾 Total capacity | Sum of PVs |
| 📈 Used | From CSI metrics |
| 📉 Growth trend | Prometheus |
| ⚠️ At-risk PVCs | Threshold 85% |

### 🎯 Alert Rule

```yaml
- alert: PVCNearlyFull
  expr: kubelet_volume_stats_used_bytes / kubelet_volume_stats_capacity_bytes > 0.85
  for: 10m
```

---

## 📊 Observability

| Signal | Where |
|--------|-------|
| 📈 Volume usage | Prometheus |
| 🔍 Provisioning latency | Dashboard |
| 🐛 CSI errors | Logs |
| 📸 Snapshot history | StorageClassManager CRD |
| 🎯 Policy violations | Events |

### 📊 Grafana Dashboard

Import **dashboard ID 14400** for storage metrics.

---

## 🧪 Verification

| Check | Command | Expected |
|-------|---------|----------|
| ✅ Install | `scm --version` | Version shown |
| ✅ Pods | `kubectl -n storage get pods` | Running |
| ✅ Driver | `scm driver list` | Listed |
| ✅ Class create | `scm apply` | Created |
| ✅ PVC bind | `kubectl get pvc` | Bound |
| ✅ Snapshot | `scm snapshot create` | Ready |
| ✅ Resize | `kubectl edit pvc` | Expanded |
| ✅ Metrics | Prometheus | Present |

---

## 🩺 Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| 🚫 PVC stuck Pending | No matching class | Fix class name |
| 🚫 Volume not attached | CSI down | Check pods |
| 🚫 Slow provisioning | Wrong mode | WaitForFirstConsumer |
| 🚫 Resize fails | Not expandable | Set allowVolumeExpansion |
| 🚫 Snapshot fails | API not installed | Install snapshotter |
| 🚫 Policy not enforced | Not applied | `scm policy apply` |
| 🚫 High latency | Too many PVCs | Batch or scale |
| 🚫 Driver auth fails | IAM role | Update secrets |
| 🚫 Migration stalls | Pod still bound | Scale down first |
| 🚫 Dashboard empty | No metrics | Enable Prometheus |

### 🔄 Restart Operator

```bash
kubectl -n storage rollout restart statefulset/storageclass-manager
```

---

## 🧰 CLI Reference

| Command | Purpose |
|---------|---------|
| `scm apply -f <file>` | Apply StorageClass |
| `scm driver add` | Register CSI driver |
| `scm driver list` | List drivers |
| `scm driver verify` | Validate driver |
| `scm snapshot create` | Snapshot a PVC |
| `scm snapshot restore` | Restore snapshot |
| `scm policy apply` | Apply policies |
| `scm migrate` | Migrate PVC |
| `scm status` | Cluster overview |
| `scm --help` | Show help |

---

## 🔐 RBAC Reference

| Resource | Verbs |
|----------|-------|
| `storageclasses` | get, list, watch, create |
| `persistentvolumeclaims` | get, list, watch |
| `persistentvolumes` | get, list |
| `volumesnapshots` | get, list, create |
| `volumesnapshotclasses` | get, list |

### 🎯 Sample Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: scm-manager
rules:
  - apiGroups: ["storage.k8s.io"]
    resources: ["storageclasses"]
    verbs: ["get", "list", "watch", "create", "update", "delete"]
```

---

## 📚 Resources

| Resource | Purpose |
|----------|---------|
| 📖 **K8s Docs** | Official reference |
| 🎓 **CNCF Storage** | Standards |
| 💬 **Slack** | Community |
| 🐛 **GitHub Issues** | Bug reports |
| 🎥 **Video Guides** | Walkthroughs |

---

## 💬 FAQ

**Does it support on-prem?**
Yes — via Longhorn, Ceph, NFS, and other CSI drivers.

**Is it compatible with EKS/GKE/AKS?**
Fully — first-class support.

**Can I migrate volumes across clusters?**
Yes — via snapshot and PVC cloning.

**Does it manage resizing?**
Yes — if `allowVolumeExpansion` is enabled.

**Is backup built-in?**
Snapshots are built-in; Velero integration is supported.

**Can I enforce policies?**
Yes — policies can be enforced cluster-wide.

**Does it support multiple clusters?**
Yes — via GitOps or central control plane.

**Is it free?**
Yes — Apache 2.0 license.

**What about air-gapped clusters?**
Supported with private registries.

**Does it integrate with ArgoCD?**
Yes — all resources are GitOps-friendly.

---

## 📅 Version History

| Version | Date | Highlights |
|---------|------|-----------|
| **2.3.0** | Jan 2025 | Policy engine v2 |
| 2.1.0 | Oct 2024 | Migration tools |
| 2.0.0 | Jun 2024 | Multi-cluster |
| 1.5.0 | Feb 2024 | Initial release |

---

<div align="center">

### 🌟 Found This Guide Helpful?

[![Get Instructions](https://img.shields.io/badge/📘_GET_INSTRUCTIONS-F59E0B?style=for-the-badge&logo=kubernetes&logoColor=black&labelColor=92400E)](https://share.google/VKOb4DpsLEIjGUtJi)

**⭐ Star this repository if it helped! ⭐**

*Made with 💜 for the cloud-native community*

</div>

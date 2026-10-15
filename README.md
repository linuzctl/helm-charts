# helm-charts

This repository contains custom and tracked Helm charts used in my cluster or managed personally. It centralizes chart management, provides automated updates, and publishes charts as OCI artifacts for easy deployment.

## Custom Charts

These charts are created and maintained in-house for specific use cases within the cluster. They represent custom deployments and configurations tailored to organizational needs.
Custom charts live in `charts/`.

| Chart | Source | Managed | Package |
|---|---|---|---|
| [stalwart](charts/stalwart) | [GitHub](https://github.com/stalwartlabs/stalwart) | linuzctl | [stalwart](https://github.com/users/linuzctl/packages/container/package/helm-charts/stalwart) |

## Tracked Charts

These charts are maintained upstream and are **mirrored here automatically**. They are pulled from the upstream source and packaged as OCI artifacts for easier deployment in our cluster. The charts are used as-is, without manual changes or custom patches.
Tracked charts live in `tracked-charts/`.

| Chart | Source | Managed | Package |
|---|---|---|---|
| [aistor-objectstore](tracked-charts/aistor-objectstore) | [GitHub](https://github.com/minio/helm) | Renovate | [aistor-objectstore](https://github.com/users/linuzctl/packages/container/package/helm-charts/aistor-objectstore) |
| [aistor-operator](tracked-charts/aistor-operator) | [GitHub](https://github.com/minio/helm) | Renovate | [aistor-operator](https://github.com/users/linuzctl/packages/container/package/helm-charts/aistor-operator) |
| [awx-operator](tracked-charts/awx-operator) | [GitHub](https://github.com/ansible-community/awx-operator-helm) | Renovate | [awx-operator](https://github.com/users/linuzctl/packages/container/package/helm-charts/awx-operator) |
| [ceph-csi-drivers](tracked-charts/ceph-csi-drivers) | [GitHub](https://github.com/ceph/ceph-csi-operator/tree/main/deploy/charts/ceph-csi-drivers) | Renovate | [ceph-csi-drivers](https://github.com/users/linuzctl/packages/container/package/helm-charts/ceph-csi-drivers) |
| [ceph-csi-operator](tracked-charts/ceph-csi-operator) | [GitHub](https://github.com/ceph/ceph-csi-operator/tree/main/deploy/charts/ceph-csi-operator) | Renovate | [ceph-csi-operator](https://github.com/users/linuzctl/packages/container/package/helm-charts/ceph-csi-operator) |
| [csi-driver-nfs](tracked-charts/csi-driver-nfs) | [GitHub](https://github.com/kubernetes-csi/csi-driver-nfs) | Renovate | [csi-driver-nfs](https://github.com/users/linuzctl/packages/container/package/helm-charts/csi-driver-nfs) |
| [descheduler](tracked-charts/descheduler) | [GitHub](https://github.com/kubernetes-sigs/descheduler) | Renovate | [descheduler](https://github.com/users/linuzctl/packages/container/package/helm-charts/descheduler) |
| [gatekeeper](tracked-charts/gatekeeper) | [GitHub](https://github.com/open-policy-agent/gatekeeper/tree/master/charts/gatekeeper) | Renovate | [gatekeeper](https://github.com/users/linuzctl/packages/container/package/helm-charts/gatekeeper) |
| [gitlab-operator](tracked-charts/gitlab-operator) | [GitLab](https://gitlab.com/gitlab-org/cloud-native/gitlab-operator/-/tree/master/deploy/chart) | Renovate | [gitlab-operator](https://github.com/users/linuzctl/packages/container/package/helm-charts/gitlab-operator) |
| [gitlab-runner](tracked-charts/gitlab-runner) | [GitLab](https://gitlab.com/gitlab-org/charts/gitlab) | Renovate | [gitlab-runner](https://github.com/users/linuzctl/packages/container/package/helm-charts/gitlab-runner) |
| [gitlab](tracked-charts/gitlab) | [GitLab](https://gitlab.com/gitlab-org/charts/gitlab) | Renovate | [gitlab](https://github.com/users/linuzctl/packages/container/package/helm-charts/gitlab) |
| [longhorn](tracked-charts/longhorn) | [GitHub](https://github.com/longhorn/longhorn/tree/master/chart) | Renovate | [longhorn](https://github.com/users/linuzctl/packages/container/package/helm-charts/longhorn) |
| [openebs](tracked-charts/openebs) | [GitHub](https://github.com/openebs/openebs/tree/develop/charts) | Renovate | [openebs](https://github.com/users/linuzctl/packages/container/package/helm-charts/openebs) |
| [metrics-server](tracked-charts/metrics-server) | [GitHub](https://github.com/kubernetes-sigs/metrics-server) | Renovate | [metrics-server](https://github.com/users/linuzctl/packages/container/package/helm-charts/metrics-server) |
| [tetragon](tracked-charts/tetragon) | [GitHub](https://github.com/cilium/tetragon) | Renovate | [tetragon](https://github.com/users/linuzctl/packages/container/package/helm-charts/tetragon) |
| [unpoller](tracked-charts/unpoller) | [GitHub](https://github.com/unpoller/helm-chart) | Renovate | [unpoller](https://github.com/users/linuzctl/packages/container/package/helm-charts/unpoller) |
| [vertical-pod-autoscaler](tracked-charts/vertical-pod-autoscaler) | [GitHub](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler/charts/vertical-pod-autoscaler) | Renovate | [vertical-pod-autoscaler](https://github.com/users/linuzctl/packages/container/package/helm-charts/vertical-pod-autoscaler) |

### How Charts Are Tracked

This repository handles chart updates automatically:

1. **Version Storage**
   All tracked chart versions are stored in `state/` as plain YAML files.

2. **Automatic Updates**
   [Renovate](https://docs.renovatebot.com/) detects upstream version bumps and opens pull requests automatically.

3. **Diff Visibility**
   A GitHub Actions workflow runs on each PR, downloads the full chart source into `tracked-charts/`, and makes the diff visible in the PR.

4. **OCI Packaging**
   After a PR is merged, the chart is packaged and pushed as an OCI artifact to [GitHub Container Registry (GHCR)](https://ghcr.io).

5. **Release**
   A GitHub Release is created automatically whenever a new chart version is packaged and pushed.

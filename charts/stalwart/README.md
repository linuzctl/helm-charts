# stalwart

![Chart version: 0.1.3](https://img.shields.io/badge/chart-0.1.3-blue)
![Stalwart version: v0.16.4](https://img.shields.io/badge/stalwart-v0.16.4-blue)

Helm chart for [Stalwart](https://stalw.art) - an all-in-one Mail & Collaboration server. Secure, scalable and fluent in every protocol (IMAP, JMAP, SMTP, CalDAV, CardDAV, WebDAV).

This Helm chart is originally based on the official [Stalwart Kubernetes documentation](https://stalw.art/docs/cluster/orchestration/kubernetes), with additional custom configurations by me.

> [!CAUTION]
> This Helm chart is a work in progress and has not yet reached a stable 1.0.0 release. As such, breaking changes may be introduced at any time, including between minor or patch versions.
> Before performing any upgrade, make sure to carefully review the changelog and validate your configuration to avoid disrupting your deployment. It is strongly recommended to test upgrades in a staging environment prior to applying them in production.


# Example

A working test environment is available here: [linuzctl/k8s-gitops](https://github.com/linuzctl/k8s-gitops/tree/main/apps/stalwart)

It is managed using FluxCD and can be used as a reference example for deployments.

_...The Helm chart documentation is currently a work in progress (WIP) and will be published as soon as it is considered usable..._
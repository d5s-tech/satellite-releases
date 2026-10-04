# d5s Satellite

Connect your Kubernetes cluster to d5s so authorized agents can read cluster resources, logs, and metrics when requested.

This repository contains release downloads and the release approval workflow. Satellite's runtime source is private.

> **Release status:** Version 0.2.0 has passed its build and security scans. Public downloads are not enabled yet. Installation requires access to the release packages until publication is complete.

## Connect your cluster

Kubernetes access must be enabled for your organization. An organization owner or admin completes these steps:

1. Open **Organization settings → Infrastructure access → Kubernetes** in d5s.
2. Register your cluster and save its one-time enrollment token in your secret manager.
3. Choose **Helm**, **Terraform**, **Pulumi**, or **Kubernetes YAML** in the Installation tab.
4. Follow the instructions to create the namespace, enrollment Secret, TLS Secret, and configuration.
5. Install Satellite and provide an HTTPS endpoint reachable from d5s.
6. Use **Connect and verify** to save the endpoint and enrollment token, then verify a live read.
7. Grant the required workspace permissions under **Access**.

Installing Satellite does not grant workspace access automatically.

### Install with Helm

All four installation methods use the same versioned chart. After completing the configuration shown in Settings, run:

```sh
helm upgrade --install d5s-satellite \
  oci://ghcr.io/d5s-tech/charts/d5s-satellite \
  --version 0.2.0 \
  --namespace d5s-satellite \
  --values satellite-values.yaml --atomic --wait
```

The chart pins the scanned container image. It supports Linux **amd64** and **arm64**.

### Network and credentials

- The chart creates an internal Service. You provide the ingress or load balancer and a publicly trusted HTTPS certificate.
- Keep the connection from your ingress or load balancer to Satellite encrypted with HTTPS. Restrict permitted callers and Kubernetes API destinations.
- Keep enrollment tokens and TLS private keys in your secret manager. Do not put them in values files, infrastructure state, source control, or chat.

### Standalone binaries

[Releases](https://github.com/d5s-tech/satellite-releases/releases) contains published downloads when available. Verify each archive against its `SHA256SUMS` file before use.

Standalone installations also require a reviewed kubeconfig, enrollment credentials, TLS, and host network rules. Helm is the recommended installation method.

## For release maintainers

### Publish a release

1. Start **Actions → Satellite release approval** with the reviewed chart version and exact current private monorepo `main` commit SHA.
2. Copy the approval run ID from its summary.
3. Start the private **Satellite public release** workflow with that run ID, version, and SHA.
4. Verify package visibility and anonymous image and chart downloads before publishing the binary release and announcing availability.

The approval workflow records the release request. The private publisher verifies that request, the approval policy, source commit, and chart version. It then builds, scans, and publishes the artifacts.

### Who can publish?

Anyone can read this repository. Starting workflows and publishing releases require repository write access or higher.

The `main` branch is protected. The `satellite-public-release` environment accepts only protected branches and currently allows a single operator without a second approval. If required reviewers are restored, the private verifier requires approval from another named reviewer.

The private release gate pins the approval workflow's exact file hash. Changes to that workflow require a reviewed update to the pin before another release.

Never add private runtime source, enrollment tokens, or cluster credentials to this repository.

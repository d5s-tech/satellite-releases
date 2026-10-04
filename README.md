# d5s Satellite releases

Version **0.2.0** provides a public Helm chart, a container image for Linux amd64 and arm64, and checksummed Linux binaries. Satellite connects your Kubernetes cluster to d5s for authorized reads. The runtime source remains private.

[Installation guide](https://docs.d5s.tech/operate/kubernetes-satellite) · [Release downloads](https://github.com/d5s-tech/satellite-releases/releases/tag/satellite-v0.2.0)

## Release approval

An operator starts **Actions → Satellite release approval** with a reviewed chart version and the exact current commit SHA from private monorepo `main`. The `satellite-public-release` environment permits a single operator without a second approval. Only protected branches can run this job. The workflow then records its run ID in the run summary. It does not build software or publish packages.

The operator passes that run ID, version, and SHA to the private Satellite public-release workflow. That workflow verifies the public request run, environment policy, exact source SHA, and chart version before it builds, scans, and publishes to GitHub Container Registry. A manual first-release visibility change and anonymous-pull test are still required before customer announcement.

The public `main` branch remains protected. The `satellite-public-release` environment accepts only protected branches and currently has no required reviewers. If reviewers are restored, the private verifier requires approval from another named reviewer. The private release gate pins the exact approval workflow file hash: editing this workflow requires a reviewed update to that pin before another release. This repository holds no private runtime source, customer enrollment tokens, or cluster credentials.

Public visibility allows anyone to read this repository. Starting workflows and publishing releases require repository write access or higher.

## Customer installation

Kubernetes access must be enabled for your organization. An organization owner or admin registers a cluster in **Organization settings → Infrastructure access → Kubernetes**. Save the one-time enrollment token in your secret manager.

Settings provides Helm, Terraform, Pulumi and Kubernetes YAML examples. All methods use this chart:

```text
oci://ghcr.io/d5s-tech/charts/d5s-satellite
Version: 0.2.0
```

The chart is public and pins the release image digest. No registry sign-in is needed. Create the namespace, enrollment Secret and TLS Secret, then fill in the configuration from Settings. Keep tokens and TLS private keys out of values files, infrastructure state and chat.

```sh
helm upgrade --install d5s-satellite \
  oci://ghcr.io/d5s-tech/charts/d5s-satellite \
  --version 0.2.0 \
  --namespace d5s-satellite \
  --values satellite-values.yaml --atomic --wait
```

The chart creates an internal Service. Supply an HTTPS ingress or load balancer with a publicly trusted certificate and restrict its callers. Keep TLS on the connection to Satellite. In Settings, use **Connect and verify** to save the public address and enrollment token. A successful live read verifies the connection; grant workspace access separately under **Access**.

Standalone binaries require a reviewed kubeconfig, enrollment credentials, TLS and host network rules. Verify downloads against `SHA256SUMS` before use. The [installation guide](https://docs.d5s.tech/operate/kubernetes-satellite) covers permissions, optional read sources and revocation.

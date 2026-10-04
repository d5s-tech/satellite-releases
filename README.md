# d5s Satellite releases

This public repository records operator release requests for d5s Kubernetes Satellite releases. The runtime source and build stay in the private d5s monorepo. No public Satellite image or Helm chart has been published yet.

## Release approval

An operator starts **Actions → Satellite release approval** with a reviewed chart version and the exact current commit SHA from private monorepo `main`. The `satellite-public-release` environment permits a single operator without a second approval. Only protected branches can run this job. The workflow then records its run ID in the run summary. It does not build software or publish packages.

The operator passes that run ID, version, and SHA to the private Satellite public-release workflow. That workflow verifies the public request run, environment policy, exact source SHA, and chart version before it builds, scans, and publishes to GitHub Container Registry. A manual first-release visibility change and anonymous-pull test are still required before customer announcement.

The public `main` branch remains protected. The `satellite-public-release` environment accepts only protected branches and currently has no required reviewers. If reviewers are restored, the private verifier requires approval from another named reviewer. The private release gate pins the exact approval workflow file hash: editing this workflow requires a reviewed update to that pin before another release. This repository holds no private runtime source, customer enrollment tokens, or cluster credentials.

Public visibility allows anyone to read this repository. Starting workflows and publishing releases require repository write access or higher.

## Customer installation

Customers will register their cluster in d5s **Organization settings → Infrastructure access**. Once a release is published, the app and operator guide will provide the approved chart version and installation steps.

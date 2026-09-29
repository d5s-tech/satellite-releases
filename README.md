# d5s Satellite releases

This public repository records two-person approval for d5s Kubernetes Satellite releases. The runtime source and build stay in the private d5s monorepo. No public Satellite image or Helm chart has been published yet.

## Release approval

An operator starts **Actions → Satellite release approval** with a reviewed chart version and the exact current commit SHA from private monorepo `main`. The job waits at the protected `satellite-public-release` environment. The operator who started it cannot approve it; another named reviewer must approve. The workflow then records its run ID in the run summary. It does not build software or publish packages.

The operator passes that run ID, version, and SHA to the private Satellite public-release workflow. That workflow verifies the public approval run, reviewer, environment policy, exact source SHA, and chart version before it builds, scans, and publishes to GitHub Container Registry. A manual first-release visibility change and anonymous-pull test are still required before customer announcement.

The public `main` branch requires one pull-request approval, dismisses stale reviews, applies protection to administrators, and blocks force pushes and deletions. The `satellite-public-release` environment accepts only protected branches, requires one of two named reviewers, and prevents self-review. The private release gate pins the exact approval workflow file hash: editing this workflow requires a reviewed update to that pin before another release. This repository holds no private runtime source, customer enrollment tokens, or cluster credentials.

## Customer installation

Customers will register their cluster in d5s **Organization settings → Infrastructure access**. Once a release is published, the app and operator guide will provide the approved chart version and installation steps.

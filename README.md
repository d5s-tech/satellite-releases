# d5s Satellite releases

This public repository is the approval and release-information surface for the d5s Kubernetes Satellite. The runtime source remains in the private d5s monorepo. No public Satellite image or Helm chart has been published yet.

A release request names an exact private source commit and chart version. The protected `satellite-public-release` environment requires a second reviewer before the approval run succeeds. The private release workflow verifies that run before building, scanning, and publishing to GitHub Container Registry. This repository does not hold customer enrollment tokens or cluster credentials.

Customers will register their cluster in d5s **Organization settings → Infrastructure access**. Once a release is published, the app and operator guide will provide the approved chart version and installation steps.

---
name: Provision a Compatibility Matrix (CMX) cluster
description: Create an ephemeral Kubernetes cluster for compatibility testing and fetch its kubeconfig via the Replicated Vendor API v3.
api: openapi/replicated-vendor-api-v3-openapi-original.json
operations: [listClusterVersions, createCluster, getCluster, getClusterKubeconfig, updateClusterTTL, deleteCluster]
---

# Provision a Compatibility Matrix (CMX) cluster

Authenticate with `Authorization: <token>` against
`https://api.replicated.com/vendor/v3`. CMX clusters are ephemeral test
infrastructure — always set a TTL and delete when done.

## Steps

1. **Pick a distribution/version.** Call `listClusterVersions`
   (GET `/cluster/versions`) to choose a supported distribution and version.
2. **Create the cluster.** Call `createCluster` (POST `/cluster`) with the
   distribution, version, node groups, and a `ttl`.
3. **Wait until ready.** Poll `getCluster` (GET `/cluster/{clusterId}`) until the
   status is running.
4. **Fetch kubeconfig.** Call `getClusterKubeconfig` to retrieve credentials and run
   your compatibility tests.
5. **Extend or tear down.** Use `updateClusterTTL` to extend, then `deleteCluster`
   (DELETE `/cluster/{clusterId}`) to release the cluster.

## Rules

- Clusters cost credits — always delete or set a short TTL.
- Handle `429` (quota/rate) and `500` on `addClusterCredits`/create paths; retry
  with backoff. See `errors/replicated-problem-types.yml`.

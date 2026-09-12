---
name: gloo-mesh-register-a-workload-cluster
description: >-
  Register a Kubernetes workload cluster with the Gloo Mesh management plane, verify the relay
  connection, and deregister it cleanly.
api: Gloo Mesh (Gloo Platform APIs)
generated: '2026-09-12'
method: generated
source: >-
  cli/gloo-mesh-cli.yml, crd/gloo-mesh-admin-gloo-solo-io-crds.yaml,
  https://docs.solo.io/gloo-mesh-enterprise/latest/setup/install/enterprise_installation/
operations:
  - meshctl cluster register
  - meshctl cluster list
  - meshctl check
  - meshctl cluster deregister
---

# Register a workload cluster

Gloo Mesh is a management plane in one cluster plus agents in the workload clusters. The
agents connect over a **relay** channel secured with mTLS. Registration creates a
`KubernetesCluster` (`admin.gloo.solo.io/v2`) in the management cluster.

## Before you start

```
export MGMT_CLUSTER=<management-cluster-name>
export MGMT_CONTEXT=<management-cluster-context>
export REMOTE_CLUSTER=<workload-cluster-name>
export REMOTE_CONTEXT=<workload-cluster-context>
```

Cluster names **must not contain underscores** — a published known issue, and it applies to
the kubeconfig context name too.

## Steps

1. Confirm the management plane is healthy first:

   ```
   meshctl check --kubecontext $MGMT_CONTEXT
   meshctl check server --kubecontext $MGMT_CONTEXT
   ```

2. Register:

   ```
   meshctl cluster register $REMOTE_CLUSTER \
     --kubecontext $MGMT_CONTEXT \
     --remote-context $REMOTE_CONTEXT
   ```

3. Verify both sides:

   ```
   meshctl cluster list --kubecontext $MGMT_CONTEXT
   kubectl get kubernetescluster -n gloo-mesh --context $MGMT_CONTEXT -o yaml
   ```

   Read `status` on the `KubernetesCluster` the same way as any Gloo resource — check
   `observedGeneration` first, then `approval` (see `gloo-mesh-apply-a-policy`, step 5).

4. If the relay does not come up, it is almost always certificates. The relay secrets are
   `relay-root-tls-secret`, `relay-tls-signing-secret`, `relay-server-tls-secret` and
   `relay-identity-token-secret` in the `gloo-mesh` namespace; the posture options
   (self-signed, BYO server cert, BYO server + client, TLS-only, insecure) are documented
   under `setup/prod/certs/relay/`. `meshctl debug report` collects a support bundle.

## Reversal

```
meshctl cluster deregister $REMOTE_CLUSTER --kubecontext $MGMT_CONTEXT
```

Deregistration is documented as the **first** step of teardown: if you used the Istio
lifecycle manager, Istio must be uninstalled before any Gloo management or agent component is
removed, or you will strand resources. No time window applies. Full order at
`https://docs.solo.io/gloo-mesh-enterprise/latest/setup/uninstall/`.

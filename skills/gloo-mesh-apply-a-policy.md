---
name: gloo-mesh-apply-a-policy
description: >-
  Apply a Gloo Mesh policy custom resource safely — validate it before applying, apply it
  idempotently, confirm it was accepted by reading ApprovalState, and know how to take it back.
api: Gloo Mesh (Gloo Platform APIs)
generated: '2026-09-12'
method: generated
source: >-
  crd/ (gloo-platform-crds 2.14.0), errors/gloo-mesh-problem-types.yml,
  conventions/gloo-mesh-conventions.yml,
  https://docs.solo.io/gloo-mesh-enterprise/latest/concepts/about/validation/
operations:
  - kubectl apply
  - meshctl experimental validate resources
  - kubectl get -o yaml (read status.state)
  - kubectl delete
---

# Apply a Gloo Mesh policy

The Gloo Mesh API is Kubernetes CRDs — 70 kinds across 13 API groups, all `Namespaced`. There
is no HTTP endpoint to call and no API key to hold: you authenticate to the **cluster**, and
Kubernetes RBAC decides what you may write.

## 1. Pick the right kind

Policies live in four groups (full inventory in `crd/gloo-mesh-crd-index.yml`):

| Group | Examples |
|---|---|
| `security.policy.gloo.solo.io/v2` | `AccessPolicy`, `JWTPolicy`, `ExtAuthPolicy`, `CORSPolicy`, `CSRFPolicy`, `WAFPolicy`, `DLPPolicy`, `ClientTLSPolicy` |
| `trafficcontrol.policy.gloo.solo.io/v2` | `RateLimitPolicy`, `TransformationPolicy`, `HeaderManipulationPolicy`, `LoadBalancerPolicy`, `MirrorPolicy` |
| `resilience.policy.gloo.solo.io/v2` | `RetryTimeoutPolicy`, `FailoverPolicy`, `FaultInjectionPolicy`, `OutlierDetectionPolicy`, `ConnectionPolicy` |
| `observability.policy.gloo.solo.io/v2` | `AccessLogPolicy` |

Routing objects are `networking.gloo.solo.io/v2` (`RouteTable`, `VirtualDestination`,
`VirtualGateway`, `ExternalService`, `ExternalEndpoint`).

## 2. Learn the real fields — never guess them

```
kubectl explain AccessPolicy.spec
kubectl explain RouteTable.spec.http
```

`kubectl explain` prints the shipped schema *and* the validation constraints. The rendered
CRDs in `crd/` and the reference at
`https://docs.solo.io/gloo-mesh-enterprise/latest/reference/api/` are the same source.

Policies attach by **selector**, not by ID. The binding fields are `spec.applyToDestinations`,
`spec.applyToRoutes` and `spec.applyToWorkloads`; the policy body sits under `spec.config`.
`data-model/gloo-mesh-data-model.yml` lists every entity's real top-level spec fields.

## 3. Validate before you apply

```
meshctl experimental validate resources -f policy.yaml
kubectl apply -f policy.yaml --dry-run=server
```

Admission validation is on by default for `ExtAuthPolicy`, `FaultInjectionPolicy`,
`LoadBalancerPolicy`, `OutlierDetectionPolicy`, `RetryTimeoutPolicy`, `RouteTable`,
`VirtualDestination` and `VirtualGateway` — schema constraints plus CEL rules across fields.
Invalid config is rejected and never stored.

## 4. Apply

```
kubectl apply -f policy.yaml --context $MGMT_CONTEXT
```

**This is idempotent across the entire API.** The object's name is the idempotency key;
re-running an identical apply yields an identical stored object and no extra effect. There is
no `Idempotency-Key` header because nothing can be replayed into a duplicate. Concurrency is
`metadata.resourceVersion` optimistic concurrency — a conflict means someone else wrote first,
so re-read and retry rather than forcing.

## 5. Confirm it was accepted — applying is not succeeding

```
kubectl get accesspolicy my-policy -o yaml | yq '.status'
```

Check two things, in this order:

1. `status.state.observedGeneration` == `metadata.generation`. If not, Gloo has not processed
   your write yet — wait, do not interpret the rest of the status.
2. `status.state.approval`:

| ApprovalState | Meaning | What to do |
|---|---|---|
| `PENDING` | not yet processed | wait, then re-read |
| `ACCEPTED` | valid and applied | done |
| `INVALID` | bad fields, or conflicts with an earlier-accepted resource | read `status.state.message`, check reference targets |
| `WARNING` | partially applied — it **is** live | read the message before assuming the whole spec took effect |
| `FAILED` | config is fine, the server could not sync | retry is safe; run `meshctl check` |
| `UNLICENSED` | your license does not cover this resource | retrying will not help |

In a multicluster setup `status.clusters` is a per-cluster map — a resource can be `ACCEPTED`
in one cluster and `INVALID` in another. Check every cluster you targeted.

## 6. Reversal

`kubectl delete <Kind> <name>` withdraws the desired state and Gloo re-translates without it.
**No time window bounds this** — but there is also no Gloo-side undo buffer: restoring the
previous behaviour means re-applying the previous manifest, which is why these resources
belong in Git. Installation-level changes reverse with `helm rollback`; see
`conventions/gloo-mesh-conventions.yml#reversibility`.

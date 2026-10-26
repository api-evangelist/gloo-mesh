# Gloo Mesh (gloo-mesh)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Gloo Mesh is an enterprise service mesh management platform from Solo.io built on Istio, providing multi-cluster and multi-mesh traffic management, security policy enforcement, and observability across hybrid cloud environments. It simplifies service mesh operations with a unified control plane and policy management interface, exposing Kubernetes Custom Resource Definitions (CRDs) such as AccessPolicy, JwtPolicy, and RatelimitPolicy as the primary API surface.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/gloo-mesh/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags:

 - Istio, Kubernetes, Multi-Cluster, Open Source, Service Mesh

## Timestamps

- **Created:** 2026-04-28
- **Modified:** 2026-04-28

## APIs

### Gloo Mesh Enterprise
Gloo Mesh Enterprise (also called Gloo Platform) is a service mesh management platform built on Istio that provides intra-mesh and multi-cluster routing, access policies, JWT authentication, rate limiting, mTLS, fault injection, observability, and more. The platform exposes 100+ Kubernetes Custom Resources as its API surface, deployed via Helm and managed with the meshctl CLI.

**Human URL:** [https://www.solo.io/products/gloo-mesh/](https://www.solo.io/products/gloo-mesh/)

#### Tags:

 - Istio, Kubernetes, Multi-Cluster, Service Mesh

#### Properties

- [Documentation](https://docs.solo.io/gloo-mesh-enterprise/latest/)
- [Getting Started](https://docs.solo.io/gloo-mesh-enterprise/latest/getting_started/)
- [Reference](https://docs.solo.io/gloo-mesh-enterprise/latest/reference/)
- [Change Log](https://docs.solo.io/gloo-mesh-enterprise/latest/changelog/)

### Gloo Mesh Core
Gloo Mesh Core extends a single Istio service mesh with insights, operational tooling, and lifecycle management for upstream Istio deployments. It surfaces Istio insights, telemetry, and a curated set of policies to simplify Day 2 operations across a Kubernetes cluster.

**Human URL:** [https://www.solo.io/products/gloo-mesh/](https://www.solo.io/products/gloo-mesh/)

#### Tags:

 - Istio, Kubernetes, Service Mesh

#### Properties

- [Documentation](https://docs.solo.io/gloo-mesh-core/latest/)
- [Getting Started](https://docs.solo.io/gloo-mesh-core/latest/getting_started/)
- [Reference](https://docs.solo.io/gloo-mesh-core/latest/reference/)

## Common Properties

- [Website](https://www.solo.io/)
- [Portal](https://www.solo.io/products/gloo-mesh/)
- [Documentation](https://docs.solo.io/gloo-mesh-enterprise/latest/)
- [Getting Started](https://docs.solo.io/gloo-mesh-enterprise/latest/getting_started/)
- [Blog](https://www.solo.io/blog/)
- [GitHub Organization](https://github.com/solo-io)
- [Change Log](https://docs.solo.io/gloo-mesh-enterprise/latest/changelog/)
- [Community](https://slack.solo.io/)
- [Support](https://www.solo.io/company/contact/)
- [Terms of Service](https://www.solo.io/legal/terms-of-service/)
- [Privacy Policy](https://www.solo.io/legal/privacy-policy/)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com

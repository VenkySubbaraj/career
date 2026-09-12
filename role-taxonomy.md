# 01 — What Platform Engineers actually do, and how the role differs from its neighbours

Covers your **Part A** (real responsibilities) and **Part B** (role comparison + JD classifier).

---

## 1. The one-sentence definition that survives interviews

> A Platform Engineer builds and operates the internal product that other engineering teams
> use to ship and run software, and is measured on whether those teams adopt it and get faster.

Two halves, both load-bearing:

- **"internal product"** — it has users who can refuse it, a roadmap, docs, support, and
  deprecations. CNCF's maturity model treats *Adoption* as its own axis, ranging from
  "Erratic" → "Extrinsic push" → "Intrinsic pull" → "Participatory" `[OFFICIAL, CNCF Platform
  Engineering Maturity Model, © 2025]`. A platform nobody adopts is a failed platform even if
  the YAML is perfect.
- **"and operate"** — you carry the pager for the substrate. Platform engineering is not
  architecture consulting.

The CNCF model's five axes are the cleanest vocabulary you can bring into an interview:
**Investment, Adoption, Interfaces, Operations, Measurement** `[OFFICIAL]`. Interfaces runs
"Custom processes → Standard tooling → Self-service solutions → Integrated services", which is
exactly the ladder an interviewer means when they ask "how would you make this self-service?"

---

## 2. The work, by time horizon

`[INFERENCE]` — basis: the responsibility surface implied by the AWS EKS Best Practices domains
(security, reliability, autoscaling, networking, scalability, **cluster upgrades**, **version
rollback**, cost) `[OFFICIAL, 2026-09-10]` combined with the CNCF Operations/Adoption axes
`[OFFICIAL]`. Ratios vary enormously by company; treat this as a shape, not a schedule.

### Daily

| Reactive (the interrupt half) | Project (the build half) |
|---|---|
| Triage the platform support channel — "my pod is Pending", "ALB won't provision", "Argo says OutOfSync" | Write/refactor a Terraform module |
| Review a product team's PR against a platform repo | Author or extend a Helm chart / Kustomize base |
| Investigate an alert: node pressure, cert expiry, CI queue depth, cost anomaly | Build a reusable CI workflow |
| Unblock a release | Add a policy (Kyverno/OPA) and roll it out in audit mode first |
| Answer "can we get an S3 bucket / RDS / new namespace" | Automate the previous line out of existence |

The single most useful mental model for interviews: **every recurring ticket is a defect in
the paved road.** Senior candidates say that. Mid candidates describe how they solved the
ticket. That difference is worth a level.

### Weekly

- Cluster/add-on version drift check; plan the next upgrade.
- Cost review — the top movers, usually NAT Gateway, cross-AZ traffic, idle EBS, log ingest.
- Adoption/DX metrics: how many services are on the golden path, lead time, CI p50/p95 duration.
- On-call handover + action items from last week's incidents.
- Office hours / platform demo for product teams.

### Quarterly

- Kubernetes minor-version upgrades across the fleet (AWS publishes both **Cluster Upgrades**
  *and* **Cluster Version Rollback** best-practice guides — the existence of the second one
  tells you how this goes in reality) `[OFFICIAL]`.
- A "golden path" expansion: new runtime, new datastore, new region.
- Reliability programme: SLO review, error-budget policy, DR game day.
- Security posture: IAM policy sweep, image-scan backlog, secrets rotation, audit evidence.
- Deprecate something. Mature platforms remove features; CNCF calls this out explicitly as a
  Level 3 characteristic — *"Feature removal is a key part of the conversation"* `[OFFICIAL]`.

---

## 3. Concrete deliverables (what "I built a platform" decomposes into)

Use this list to audit your portfolio. Each item is something you can point at in a repo.

| Deliverable | What good looks like | Where you'll build it |
|---|---|---|
| **Terraform module** | Versioned, tagged, `variables` documented, examples/, tested, no hardcoded account IDs, sane defaults, `moved` blocks on refactor | Project 2 |
| **Helm chart** | `values.schema.json`, `_helpers.tpl`, hooks, tests, NOTES.txt, works on a bare cluster | Already exists: [sample-project/charts/shipyard](../../sample-project/charts/shipyard) |
| **Reusable CI workflow** | Called by N repos, OIDC not static keys, cached, matrix, required-checks enforced | Project 2 |
| **Argo CD ApplicationSet** | Generates apps from a Git directory or cluster list; App-of-Apps for platform add-ons | Project 2 |
| **Backstage / Port template** | `create service` → repo + CI + chart + Argo app + on-call + dashboard, in one form | Project 3 |
| **Cluster blueprint** | One Terraform stack producing a compliant EKS cluster: OIDC, add-ons, Karpenter, ALB controller, ESO, Argo, Prometheus | Project 2 |
| **Policy repo** | Kyverno/OPA policies, audit→enforce rollout, exceptions with expiry dates | Project 2/3 |
| **AWS account vending** | Organizations + Control Tower Account Factory or Terraform, baseline SCPs, tags, budget, log routing | Project 3 (design at minimum) |
| **Observability standard** | ServiceMonitor conventions, dashboard-as-code, alert library, SLO template with burn-rate rules | Project 2 |
| **Secrets integration** | External Secrets Operator → Secrets Manager, IRSA-scoped, rotation, zero secrets in Git | Project 2 |
| **DR automation** | Velero schedule + restore runbook, tested restore into a *new* cluster | Project 2 |
| **Platform CLI / API** | `plat env create`, `plat service scaffold` — wraps the above so a dev never learns them | Project 3 |
| **Runbooks** | Deploy, roll back, scale, rotate, debug at 3 a.m. Written for someone who is not you | All projects |

---

## 4. What Platform Engineers usually do **not** own

This matters because interviewers ask it to test whether you understand boundaries — and
because taking these on is how platform teams die.

| Not owned | Owned by | Nuance |
|---|---|---|
| Application business logic | Product teams | You review Dockerfiles and manifests, not features |
| Application-level SLOs and their error budgets | Product teams | You provide the *mechanism* and the template |
| Feature-flag decisions / release timing | Product teams | You provide the deployment machinery |
| Schema design, query tuning | App teams / DBA / data platform | You provision, back up, and monitor the engine |
| Business KPI dashboards | Analytics | You own platform + infra telemetry |
| Endpoint/corp IT, laptops, VPN for staff | IT | Frequent red flag when a JD mixes this in |
| Security policy *authorship* | Security team | You implement it as code and provide evidence |
| Being a 24×7 ticket queue for app bugs | Nobody, ideally | Archetype-1 warning sign |

**Interview phrasing:** "We own the paved road and the substrate. Teams own what runs on it.
Where a team needs to leave the road we make that possible, but explicit and time-boxed —
they take the operational burden they've opted into." That single answer signals seniority.

---

## 5. Role comparison

`[INFERENCE]` — basis: functional decomposition of the role families plus the CNCF distinction
between a Level-2 "dedicated team" and a Level-3 "platform as product" `[OFFICIAL]`. Titles
are used inconsistently in the market (see §6); this table describes *archetypes*, and you
should classify any specific JD with the classifier in §7 rather than by its title.

**Legend:** ●●● deep / ●● working / ● conceptual / — not expected

| | Platform Eng | DevOps Eng | SRE | Cloud Eng | Infra Eng | Production Eng | DevSecOps | K8s Eng | Cloud Architect | Systems Eng | DevEx Eng |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **Primary customer** | Internal dev teams | Dev teams + delivery | The service + its users | The business/cloud estate | Other infra consumers | A specific product's prod | Security + dev teams | Cluster tenants | Exec + delivery orgs | The fleet | Developers |
| **Main objective** | Adoption & dev velocity | Delivery automation | Reliability within budget | Cloud build & run | Foundations that scale | Keep this service up & fast | Ship securely by default | Cluster health & tenancy | Correct design & governance | Host-level correctness | Reduce friction end-to-end |
| **Builds product?** | **Yes** (internal) | Sometimes | Sometimes (tooling) | Rarely | Sometimes | No | Sometimes | Rarely | No | No | **Yes** |
| **Coding expectation** | ●● → ●●● | ●● | ●●● | ● → ●● | ●● | ●●● | ●● | ●● | ● | ●● | ●●● |
| **Ops / on-call** | ●● | ●● | ●●● | ●● | ●● | ●●● | ● | ●● | — | ●● | ● |
| **Owns infrastructure** | ●●● | ●● | ●● | ●●● | ●●● | ●● | ● | ●● | ● (design) | ●●● | ● |
| **Owns application** | — | — | ●● (shared) | — | — | ●●● | — | — | — | — | — |
| **Kubernetes depth** | ●●● | ●● | ●● | ● → ●● | ●● | ●● | ●● | ●●● | ● | ●● | ●● |
| **AWS depth** | ●●● | ●● | ●● | ●●● | ●●● | ●● | ●● | ●● | ●●● | ●● | ● |
| **Reliability duty** | ●● | ● | ●●● | ● | ●● | ●●● | ● | ●● | ● | ●● | ● |
| **Security duty** | ●● | ● | ● | ●● | ●● | ● | ●●● | ●● | ●● | ●● | ● |
| **Developer experience** | ●●● | ●● | ● | — | ● | ● | ● | ● | — | — | ●●● |
| **Signature interview topic** | Design a self-service platform; K8s + Terraform + IAM debugging | Pipeline design, K8s basics, scripting | SLOs, incident, systems debugging, coding | VPC/IAM/landing zone design | Networking, capacity, storage | Deep debugging of a live service | Supply chain, policy-as-code, IAM | CNI/CSI/RBAC/upgrades/multi-tenancy | Multi-account architecture, cost, DR | Linux internals, perf | Onboarding time, tooling design |

### The three most useful distinctions

- **Platform vs DevOps.** DevOps optimises *the pipeline*. Platform optimises *the whole
  developer path*, and treats it as a product with adoption metrics. A DevOps engineer asks
  "how do we deploy this?"; a platform engineer asks "why did three teams each build their own
  deploy?"
- **Platform vs SRE.** SRE's north star is reliability of a service within an error budget.
  Platform's north star is leverage across teams. SRE goes deep on one critical path; platform
  goes wide across many. Overlap is large and growing — a great many "Platform Engineer" JDs
  are SRE roles `[UNVERIFIED — test as H-archetype in JD-TRACKER]`.
- **Platform vs Cloud/Infra.** Cloud/Infra own the estate and its governance. Platform owns
  the *interface* to it. If the JD never mentions internal users, developers, self-service or
  adoption, it is archetype 1/5, not 6.

---

## 6. Why the titles are inconsistent (say this out loud in an interview and you sound senior)

`[INFERENCE]`, basis: CNCF's own observation that Level-2 teams "may be called DevOps,
Engineering Enablement, Developer Experience (DevEx or DevX), Shared Tools, a
Centre-Of-Excellence, or even Platform" `[OFFICIAL, 2025]`.

1. **Rebranding without re-scoping.** "Platform" is the current high-status label. Renaming
   the ops team costs nothing and improves the applicant pool.
2. **No certification or curriculum defines the role.** Unlike, say, a DBA, there is no shared
   body of knowledge, so every company writes the JD from its own org chart.
3. **The role genuinely spans a wide range of maturity.** A Level-1 org and a Level-4 org
   `[OFFICIAL]` need very different people and both call the role "Platform Engineer".
4. **Recruiter templates propagate.** One company's JD becomes the industry's copy-paste
   source, tools list and all — which is why postings list 25 mandatory tools that no single
   person uses.
5. **Compensation banding.** Some orgs use "Platform" to place infra people on a
   software-engineering ladder, which is a *good* sign when you see it.

---

## 7. JD classifier — read any posting and place it in 90 seconds

Score the posting, then read the archetype. Use this for every row of
[JD-TRACKER.md](JD-TRACKER.md).

### Signals

| Signal | Look for |
|---|---|
| **S1 Internal users named** | "developer experience", "self-service", "golden path", "internal customers", "platform adoption", "reduce cognitive load" |
| **S2 Software engineering** | "design and build", "code review", "write services/controllers/CLIs", Go or Python as a *required* language, "test coverage" |
| **S3 Kubernetes as a system** | CNI, CSI, RBAC, admission, operators, multi-tenancy, upgrades, "cluster fleet" — as opposed to just "experience with Kubernetes" |
| **S4 Pipeline centre of gravity** | Jenkins/GitLab/Actions named first, "build and release", "deployment automation", artifact management |
| **S5 Reliability language** | SLI/SLO, error budget, incident command, postmortem, MTTR, capacity planning, on-call rotation named |
| **S6 Cloud governance** | Organizations, SCPs, Control Tower, landing zone, account vending, tagging, FinOps, audit/compliance |
| **S7 Ticket/ops language** | "support requests", "shift", "L2/L3", "monitor and respond", "SLA", named ITSM tool, no code mentioned |
| **S8 Portal/catalogue** | Backstage, Port, Cortex, OpsLevel, Crossplane, "service catalogue", "software templates" |

### Mapping

| Archetype | Signal pattern | What the job really is | Take it if… |
|---|---|---|---|
| **1 — Ops renamed** | S7 strong; S1, S2 absent | Ticket queue with a new title. Toil is the job, not the enemy. | Only as a stepping stone with a written path out |
| **2 — CI/CD-heavy DevOps** | S4 dominant; S3 shallow; S1 weak | Own the pipelines, some cluster ops | **Yes — this is your most realistic first jump** |
| **3 — Kubernetes infrastructure** | S3 dominant; S2 medium | Run the cluster fleet: upgrades, networking, tenancy | Strong target after Project 2 |
| **4 — SRE with a Platform title** | S5 dominant; on-call explicit; coding round likely | Reliability of specific critical systems | Good, if you're ready for a coding round |
| **5 — Cloud governance platform** | S6 dominant; K8s may be light | Landing zones, multi-account, guardrails, cost | **Strong fit for your AWS background** |
| **6 — True IDP** | S1 + S8 + S2 all present | Build the portal, templates, self-service APIs | Job #2, not job #1 |
| **7 — SWE-heavy platform** | S2 dominant, Go required, DSA round likely | You are a backend engineer whose product is infra | Only after Go + real systems coding |

### Fast heuristics

- **"On-call" absent from the JD does not mean on-call absent from the job.** Ask in the
  recruiter screen: *"What does the on-call rotation look like, and what's the typical page
  volume per shift?"* The answer classifies the company faster than the JD does.
- **Count the mandatory tools.** More than ~12 "must have" tools ⇒ the JD was written by
  aggregation, not by the hiring manager. Interview will be shallower and broader than it reads.
- **Look for who they report to.** Engineering ⇒ archetype 2–7. IT/Support ⇒ archetype 1.
- **Search the JD for the word "adopt".** Its presence is the single highest-signal indicator
  of archetype 6 `[INFERENCE]`, and it maps directly onto CNCF's Adoption axis `[OFFICIAL]`.

---

## 8. Questions that reveal the archetype in a recruiter screen

Ask two or three. They cost you nothing and reframe you as someone evaluating *them*.

1. "Who are the platform's users, and roughly how many engineers depend on it?"
2. "How does a new service get from an empty repo to production today — and how long does it take?"
3. "What fraction of the team's time goes to interrupts vs planned work?"
4. "How do you measure whether the platform is working?" *(Silence here ⇒ archetype 1–2.)*
5. "Who does a team talk to if they want to do something the platform doesn't support?"
6. "What's the Kubernetes footprint — clusters, teams per cluster, who upgrades them?"
7. "Is there an on-call rotation for the platform itself, and what pages it?"

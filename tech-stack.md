# 02 — Technology map: what to learn, how deep, and in what order

Covers your **Part C**.

---

## How to read the frequency column — please read this

You asked for posting frequency from a JD corpus. **I did not collect one** (see
[00-START-HERE.md](00-START-HERE.md)). Rather than invent numbers, the tables below use:

- **Prevalence** — a banded `[INFERENCE]` judgement: `Near-universal` / `Very common` /
  `Common` / `Role-dependent` / `Specialised` / `Emerging` / `Declining`. Basis is stated
  per section, and is generally: (a) the technology is required to operate something in a
  higher band, (b) it appears as a first-class domain in official guidance such as the
  [AWS EKS Best Practices Guide](https://docs.aws.amazon.com/eks/latest/best-practices/introduction.html) `[OFFICIAL]`,
  (c) ecosystem ubiquity from `[SURVEY]` data.
- **SO 2025** — where a hard number exists, it is the % of *all developers* who did extensive
  work with it in the past year, from the [Stack Overflow Developer Survey 2025](https://survey.stackoverflow.co/2025/technology)
  `[SURVEY, retrieved 2026-09-10]`. **This is not hiring demand.** It is a floor on ubiquity.
  Kubernetes at 28.5% of *all* developers is very high for an infrastructure tool.
- **Priority for me** — my recommendation for *your* background, on a 4-point scale:
  `P0` block everything else until done · `P1` core, before first interviews ·
  `P2` after first offer or in parallel · `P3` awareness only.

Replace the Prevalence column with real counts once you've run [JD-TRACKER.md](JD-TRACKER.md).

**Depth scale:** `Conceptual` (explain it) · `Working` (use it under time pressure) ·
`Deep` (debug it when it misbehaves and reason about internals).

---

## 1. Master table — the 30 things that matter

| Technology | Category | Prevalence `[INFERENCE]` | SO 2025 `[SURVEY]` | Mandatory vs preferred | Typical production use | Expected interview depth | Priority for me |
|---|---|---|---|---|---|---|---|
| **Linux** | Foundation | Near-universal | Ubuntu 27.7% prof. use | Mandatory | Every node, every container, every debug session | **Deep** | **P0** |
| **Bash/shell** | Foundation | Near-universal | 48.7% | Mandatory | Glue, CI steps, break-glass | Working→Deep | **P0** |
| **TCP/IP, DNS, TLS, HTTP** | Foundation | Near-universal | — | Mandatory (implicit) | Every outage you will ever debug | **Deep** | **P0** |
| **Git** | Foundation | Near-universal | GitHub 81.1% | Mandatory | Everything is GitOps now | Working | **P0** |
| **Docker / OCI** | Containers | Near-universal | **71.1%** | Mandatory | Build, ship, debug images | **Deep** | **P0** |
| **Kubernetes** | Orchestration | Near-universal for platform titles | 28.5% | Mandatory | The substrate | **Deep** | **P0** |
| **AWS (IAM, VPC, EC2, S3)** | Cloud | Near-universal in AWS shops | AWS 43.3% | Mandatory | Everything | **Deep** | **P0** (you're mostly here) |
| **Amazon EKS** | Cloud+K8s | Very common | — | Mandatory in AWS platform roles | Managed control plane | **Deep** | **P0** |
| **Terraform** | IaC | Near-universal | 17.8% | Mandatory | All infra | **Deep** | **P0** |
| **Python** | Language | Very common | **57.9%** | Mandatory or strongly preferred | Automation, tooling, glue | Working→Deep | **P1** (you have it) |
| **Helm** | Packaging | Very common | — | Usually mandatory w/ K8s | App + add-on packaging | Working→Deep | **P1** |
| **GitHub Actions** | CI/CD | Very common | GitHub 81.1% | One CI system mandatory | Build/test/deploy, OIDC to AWS | Working→Deep | **P1** |
| **Argo CD** | GitOps | Very common | — | Often mandatory | Declarative delivery, drift, self-heal | Working→Deep | **P1** |
| **Prometheus** | Observability | Very common | 11.8% | Usually mandatory | Metrics + alerting | Working→Deep | **P1** |
| **Grafana** | Observability | Very common | — | Usually mandatory | Dashboards, SLO burn-rate | Working | **P1** |
| **IRSA / EKS Pod Identity** | AWS identity | Very common in EKS roles | — | Mandatory | Pod → AWS API auth | **Deep** | **P1** |
| **Karpenter** | Scaling | Very common (EKS) | — | Preferred → mandatory | Node provisioning, Spot, consolidation | Working→Deep | **P1** |
| **AWS Load Balancer Controller / Ingress** | Networking | Very common | — | Mandatory in practice | ALB/NLB from K8s objects | **Deep** | **P1** |
| **External Secrets / Secrets Manager** | Secrets | Common | — | Mandatory-ish | Secrets into pods without Git | Working | **P1** |
| **SLO / error budgets** | Reliability | Common; universal in SRE-shaped roles | — | Mandatory in archetype 4 | Alerting policy, release gating | **Deep** | **P1** |
| **GitLab CI** | CI/CD | Common | GitLab 35.6% | Alternative to Actions | Same as Actions | Conceptual unless the JD names it | P2 |
| **Jenkins** | CI/CD | Common in India, **declining** globally | — | Mandatory where legacy | Legacy pipelines, migrations | Conceptual + migration story | P2 |
| **Kustomize** | Packaging | Common | — | Preferred | Env overlays, Argo-native | Working | P2 |
| **Ansible** | Config mgmt | Common, **slowly declining** | 11.7% | Preferred | VMs, bootstrap, legacy | Conceptual | P2 |
| **Kyverno / OPA-Gatekeeper** | Policy | Common | — | Preferred | Admission control, guardrails | Working | P2 |
| **Datadog** | Observability | Common (commercial shops) | 8.9% | Mandatory where adopted | Full-stack o11y | Conceptual | P2 |
| **OpenTelemetry** | Observability | **Emerging → common** | — | Preferred | Vendor-neutral traces/metrics | Conceptual→Working | P2 |
| **Go** | Language | Role-dependent; near-mandatory archetype 7 | 16.4% | Usually preferred | Controllers, CLIs, exporters | Working (read + write simple) | P2 |
| **Vault** | Secrets | Role-dependent | — | Mandatory where adopted | Dynamic creds, PKI | Conceptual | P2 |
| **Backstage / Port** | IDP | Specialised — **discussed ≫ required** | — | Almost always preferred | Catalogue, templates, docs | Conceptual + one demo | P2/P3 |
| **Crossplane** | IDP | Specialised/Emerging | — | Preferred | K8s-native infra provisioning | Conceptual | P3 |
| **Istio / Linkerd / Cilium** | Mesh/CNI | Role-dependent | — | Preferred | mTLS, traffic shaping, eBPF | Conceptual (Cilium: Working) | P3 |
| **Pulumi / CDK** | IaC | Specialised | — | Preferred | Where teams want real languages | Conceptual | P3 |
| **Spinnaker / Harness / Tekton** | CD | Specialised, **Spinnaker declining** | — | Preferred | Enterprise CD | Awareness | P3 |
| **Kratix / Score / OAM** | IDP | Emerging | — | Rare | Platform abstraction | Awareness | P3 |

---

## 2. Foundations — the layer people skip and then fail on

`[INFERENCE]`, basis: every scenario in [06-question-bank.md](06-question-bank.md) §"Live
debugging" bottoms out here, and neither Kubernetes nor AWS abstracts these away.

### Linux — `Deep`, **P0**

You need, without notes:

- Process model: fork/exec, PID 1 in a container, zombie/orphan reaping, why `sh -c` changes signal delivery
- **Signals**: `SIGTERM` vs `SIGKILL`, `terminationGracePeriodSeconds`, graceful shutdown
- cgroups v2 and namespaces — *this is what a container actually is*; memory limits → OOMKill (exit 137)
- `systemd` + `journalctl` (kubelet, containerd live here)
- Filesystems: inodes, mounts, `df` vs `du`, "disk full but `df` says 40%" (deleted-but-open files, inode exhaustion)
- Memory: RSS vs cache, page cache, swap-off in K8s, OOM killer scoring
- CPU: load average vs utilisation, throttling (`cpu.stat` `nr_throttled`), why CPU limits cause latency
- Toolbox: `ps`, `top`/`htop`, `ss`, `lsof`, `strace`, `dmesg`, `journalctl`, `tcpdump`, `nsenter`, `/proc`

**Drill:** in your kind cluster, `kubectl exec` into a pod, then find the same process from the
host, read its cgroup limits from `/proc/<pid>/cgroup`, and trigger an OOMKill on purpose.

### Networking — `Deep`, **P0**

- CIDR arithmetic in your head. Subnetting a /16 into /20s. **Non-negotiable.**
- Routing tables, default routes, NAT (and why NAT Gateway is one-directional)
- DNS: resolution order, `/etc/resolv.conf`, `ndots:5` in Kubernetes (a classic latency bug),
  search domains, TTL and negative caching, `dig +trace`, `A`/`CNAME`/`SRV`
- TLS: handshake, SNI, chain of trust, expiry, where termination happens, mTLS
- HTTP: status semantics (**502 vs 503 vs 504 — a standard interview question**), keep-alive,
  connection draining, idle timeouts, `Host` header routing
- L4 vs L7 load balancing, health checks, target groups, connection draining
- Firewalls: stateful (Security Group) vs stateless (NACL) — **the classic AWS trap**

### Git — `Working`, **P0**

Branching, rebase vs merge, PR flow, tags/releases, `.gitignore` hygiene, and the one that
actually bites: **what to do when a secret is committed** (rotate first, rewrite history second).

---

## 3. Programming — how much software engineering is really expected

`[INFERENCE]`, basis: role archetypes in [01-role-taxonomy.md](01-role-taxonomy.md) §7.

| Archetype | Coding bar |
|---|---|
| 1 Ops renamed | Bash. Maybe a Python script. |
| 2 CI/CD DevOps | Bash + Python scripting. YAML fluency. Occasional light coding round. |
| 3 K8s infra | Python/Go for tooling. Read Go well enough to debug an operator. |
| 4 SRE-titled | **Real coding round.** Practical Python/Go, sometimes light DSA. |
| 5 Cloud governance | Python + boto3 + Terraform. Automation, not systems programming. |
| 6 True IDP | Backend-ish: APIs, templates, some frontend contact. |
| 7 SWE-heavy | Full SWE loop, Go, possibly DSA. You are a backend engineer. |

**What is actually asked** (details and exercises in [05-interview-process.md](05-interview-process.md) §Coding):

| Expected | Rarely expected |
|---|---|
| Parse logs/config, compute rates | Dynamic programming |
| Call cloud APIs, paginate, handle throttling | Graph algorithms |
| Retry with exponential backoff **+ jitter** | Advanced tree problems |
| Bounded concurrency (worker pool / semaphore) | Custom data-structure design |
| Stream a large file without loading it | Lock-free programming |
| Build a small CLI with proper exit codes | |
| Validate YAML/JSON against a schema | |
| Graceful shutdown on SIGTERM | |
| Small HTTP service with health/readiness | |

**Python (P1, you have it):** upgrade from "scripts that work" to "code someone else maintains" —
`argparse`/`click`, typing, `pytest`, packaging, `concurrent.futures`, generators for streaming,
`boto3` paginators + `botocore` retry config, structured logging, non-zero exit codes.

**Go (P2):** learn to *read* it before you learn to write it. You will read Go every time you
debug Karpenter, Argo, the ALB controller, or Kyverno. Then write: a CLI with `cobra`, a
worker pool with `errgroup`, an HTTP service with graceful shutdown, one `client-go` list/watch.
**Do not** start Go until Kubernetes + Terraform are solid.

---

## 4. Containers and Kubernetes — the depth ladder

`[OFFICIAL]` anchor: AWS publishes dedicated EKS best-practice guides for **Security,
Reliability, Karpenter, cluster-autoscaler, Auto Mode, Networking, Scalability, Cluster
Upgrades, Cluster Version Rollback, Cost Optimization** `[retrieved 2026-09-10]`. That list is
effectively the syllabus for a senior EKS interview — treat it as one.

### Docker — `Deep`, P0
Layers and cache ordering, multi-stage builds, distroless/scratch, non-root, `USER`, `COPY`
vs `ADD`, `ENTRYPOINT` vs `CMD` (and PID 1 signal handling), `.dockerignore`, image size,
multi-arch (`buildx`, and **arm64/Graviton is now a cost lever, not a curiosity**), registries,
tag immutability, scan-on-push, **why `:latest` is banned**.

### Kubernetes — `Deep`, P0

| Tier | Topics | Bar |
|---|---|---|
| **Must know cold** | Pod lifecycle, Deployment/ReplicaSet, rolling update mechanics, Service types, Endpoints/EndpointSlice, probes (**startup vs readiness vs liveness**), requests vs limits, QoS classes, ConfigMap/Secret, namespaces, labels/selectors, RBAC, ServiceAccount, DaemonSet, StatefulSet, Job/CronJob, PDB, taints/tolerations, affinity, topology spread | Build from memory, no copy-paste |
| **Must be able to debug** | `Pending` (insufficient resources / no matching node / PVC unbound / taint), `CrashLoopBackOff`, `ImagePullBackOff`, `OOMKilled` (137), `Evicted`, `Terminating` forever (finalizers), DNS failures, Service with no endpoints, cert issues, admission rejections | Under 10 min, out loud |
| **Must understand as a system** | Control-plane components, etcd's role, scheduler filtering/scoring, kubelet↔container runtime, CNI (**VPC CNI: one VPC IP per pod, IP exhaustion, prefix delegation**), CSI, CoreDNS, kube-proxy modes, admission chain (mutating → validating), CRDs + controllers/reconciliation, finalizers, owner refs | Whiteboard it |
| **Senior signals** | Multi-tenancy (namespace vs cluster isolation), upgrade strategy incl. **rollback** `[OFFICIAL]`, API deprecations across minors, Gateway API vs Ingress, PSA `restricted`, NetworkPolicy default-deny, resource quotas + LimitRanges, noisy neighbours, scale limits | Have an opinion with reasons |

### EKS specifics — see [03-aws-competency.md](03-aws-competency.md)

---

## 5. Infrastructure as Code — the consumer/author gap

`[INFERENCE]`: this is the single most common place mid-level candidates are exposed. Most
people have *used* Terraform. Interviews test whether you have *owned* it.

| Level | You can… | Enough for |
|---|---|---|
| Consumer | `init/plan/apply`, edit variables, read a module's README | Nothing. Table stakes. |
| **Author** | Design a module (inputs/outputs/versioning/examples), remote backend + **state locking**, workspaces vs directories, `import` legacy resources, `moved` blocks, `for_each` vs `count` (and why `count` breaks on list reorder), provider aliases for multi-account/region, `depends_on` sparingly, lifecycle rules | **Your target** |
| Platform owner | Module registry + release process, policy enforcement (Sentinel/OPA/Checkov), drift detection, `plan` in CI with PR comment + `apply` gated on approval, blast-radius via state segmentation, cross-stack `terraform_remote_state` vs data sources, provider upgrades at scale, `tofu` migration position | Senior/staff |

**Questions you must be able to answer** (full treatment in [06-question-bank.md](06-question-bank.md) §12):

- State is locked by a dead CI job — what now, and what's the risk of `force-unlock`?
- `plan` wants to **destroy and recreate** your production RDS — how do you find out why, and stop it?
- Someone changed a resource in the console. How do you detect it and reconcile it?
- How do you split one 4000-resource state file without an outage?
- How do you test a module?
- Terraform vs OpenTofu — what's your position? *(A short, non-tribal answer scores well.)*

**Ansible (P2):** know what it is for (mutable config, VM fleets, bootstrap) and why it is
less central in a container-first platform. Do not invest heavily.

---

## 6. CI/CD and GitOps

**Learn one CI system deeply, the rest by mapping.** Concepts transfer; syntax does not matter.

Pick **GitHub Actions** `[INFERENCE]` — it pairs with the OIDC-to-AWS pattern you need for
Project 2, has the widest ecosystem, and GitHub itself is used by 81.1% of developers
`[SURVEY, SO 2025]`.

Master: reusable workflows + composite actions, matrix builds, caching, **OIDC federation to
AWS (zero long-lived keys — this is a security answer as much as a CI one)**, environments +
required reviewers, concurrency groups, artifact/attestation handling, self-hosted runners
(and their isolation risks), required status checks.

**GitOps (P1)** — the pull model. Understand and be able to argue:

- Why pull beats push: no cluster credentials in CI, drift detection is free, Git is the audit log
- Argo CD: Application, ApplicationSet, App-of-Apps, sync waves + hooks, `selfHeal` and
  `prune` (and when they are dangerous), health assessment, **why an app shows `OutOfSync`
  when nothing changed** (mutating webhooks, defaulted fields, HPA-managed replicas)
- Repo topology: app code vs config repos, environment promotion by directory/branch/overlay
- Secrets in a GitOps world (ESO / SOPS / Sealed Secrets) — you cannot commit plaintext
- Argo CD vs Flux: know the trade-off (UI/multi-tenancy vs lightweight/composable), don't be tribal
- Progressive delivery: canary and blue/green via Argo Rollouts or Flagger, with **automated
  rollback on SLO breach** — that phrase alone signals maturity

---

## 7. Observability and reliability

**Ship the concepts, not the tool.** Interviewers accept Datadog experience for a Prometheus
shop and vice versa, provided you can explain the model.

| Layer | Learn | Depth |
|---|---|---|
| Metrics | Prometheus data model, labels, **cardinality** (and how it explodes), PromQL: `rate()`, `histogram_quantile()`, `sum by`, recording rules | Working→Deep |
| Logs | Structured JSON, correlation IDs, sampling, retention tiers, **log volume is a top-3 cost driver** | Working |
| Traces | OpenTelemetry model: spans, context propagation, sampling (head vs tail) | Conceptual→Working |
| Dashboards | Dashboard-as-code, RED (Rate/Errors/Duration) and USE (Utilisation/Saturation/Errors) | Working |
| Alerting | **Alert on symptoms, not causes.** Multi-window multi-burn-rate SLO alerts. Every alert must have a runbook or be deleted. | **Deep** |
| SLOs | SLI selection, target setting, error budget, error-budget policy (what actually happens when it's exhausted) | **Deep** |
| Incidents | Roles (IC/comms/ops), sev levels, timeline discipline, **blameless** postmortems, action items with owners | **Deep** |
| Capacity | Headroom, saturation, load testing, scaling limits | Working |
| Chaos | Game days, failure injection, hypothesis-driven | Conceptual |

**The single highest-value thing here:** be able to define an SLI, set an SLO, compute the
error budget, and write a multi-window burn-rate alert — *for a specific service, out loud, in
three minutes*. Very few candidates can. It is the fastest way to look senior.

---

## 8. Security and supply chain

| Area | Learn | Priority |
|---|---|---|
| **AWS IAM** | Policy evaluation logic, trust vs permission vs resource policy, permission boundaries, SCPs, STS, cross-account | **P0** — see [03-aws-competency.md](03-aws-competency.md) |
| **Workload identity** | IRSA (OIDC trust + `sts:AssumeRoleWithWebIdentity`), **EKS Pod Identity** (newer, simpler association model), when each fits | **P1** |
| K8s RBAC | Role/ClusterRole, bindings, aggregation, `kubectl auth can-i`, least privilege for 3 personas | P1 |
| Pod security | PSA (`restricted`), non-root, read-only rootfs, drop ALL caps, seccomp `RuntimeDefault`, no `hostPath`/`hostNetwork`/privileged | P1 |
| Secrets | ESO → Secrets Manager/Parameter Store, KMS envelope encryption, rotation, **etcd encryption at rest**, why K8s Secrets are only base64 | P1 |
| Policy as code | Kyverno (YAML, gentler) vs OPA/Gatekeeper (Rego); **audit → warn → enforce** rollout | P2 |
| Scanning | Trivy (images + IaC), Checkov/tfsec (Terraform), dependency + secret scanning in CI | P2 |
| Supply chain | SBOM, Cosign signing, admission verification of signatures, provenance/SLSA levels | P2/P3 |
| Runtime | Falco, GuardDuty (incl. EKS runtime monitoring), CSPM tools | P3 |
| Network security | NetworkPolicy default-deny, security groups for pods, private clusters, egress control | P1 |

**Interview framing that scores:** "Guardrails, not gates." Preventive controls in the paved
road, detective controls behind it, and an explicit, time-boxed exception process. Then give a
concrete example: Kyverno in audit mode for two weeks, dashboard of violations by team, then
enforce with pre-registered exceptions.

---

## 9. Internal Developer Platforms — the honest assessment

`[INFERENCE]` + `[OFFICIAL]`. CNCF's Interfaces axis runs *Custom processes → Standard tooling
→ **Self-service solutions** → Integrated services*, and only mature orgs reach the right-hand
side `[OFFICIAL, CNCF Maturity Model, 2025]`. Most companies hiring you will be at "Standard
tooling."

**Therefore:** Backstage is over-represented in conference talks and blog posts relative to how
often it is a hiring requirement `[UNVERIFIED — this is exactly hypothesis H4 in
[JD-TRACKER.md](JD-TRACKER.md); go settle it with your own data]`.

**What to actually do:**
- **Do** learn the *vocabulary and the ideas* — golden path, service catalogue, software
  template, self-service, cognitive load, platform-as-product, adoption metrics. These come up
  in *design* rounds even at companies with no portal.
- **Do** build one small thing (Project 3) so you have a concrete story.
- **Don't** spend six weeks on Backstage plugin development before you can debug a CrashLoop.

**Ranking if you must choose one:** Backstage (most mindshare, most work) → Port (SaaS, fast to
demo) → Crossplane (K8s-native infra API; genuinely interesting, genuinely heavy) → Kratix /
Score / OAM (awareness only).

---

## 10. Networking and connectivity — see also [03-aws-competency.md](03-aws-competency.md)

| Topic | Depth | Note |
|---|---|---|
| VPC, subnets, route tables, IGW/NAT | **Deep** | You have partial coverage — convert to whiteboard fluency |
| Security groups vs NACLs | **Deep** | Stateful vs stateless is a guaranteed question |
| VPC endpoints (Interface/PrivateLink vs Gateway) | **Deep** | You already use these with Glue — **big advantage, use it** |
| Transit Gateway, peering | Working | Hub-and-spoke vs mesh, transitive routing rules |
| Route 53 (public/private, failover, health checks) | Working | Split-horizon DNS is a common trap |
| ALB / NLB / target groups | **Deep** | L7 vs L4, target types (`ip` vs `instance`) — matters in EKS |
| CloudFront, Global Accelerator | Conceptual | |
| VPN, Direct Connect | Conceptual | Hybrid patterns |
| VPC CNI internals | **Deep** | IP-per-pod, ENI limits per instance type, IP exhaustion, prefix delegation |
| Service mesh (Istio/Linkerd) | Conceptual | Know *why* (mTLS, traffic shifting, observability) and the cost (complexity, sidecar overhead, ambient mode direction) |
| Cilium / eBPF | Conceptual | Rising. Know what eBPF replaces (iptables) and why. |

---

## 11. Data, messaging, storage — what platform engineers are expected to do

`[INFERENCE]`. The usual division:

| Expectation | Frequency | What it means |
|---|---|---|
| **Provision + operate the managed service** | Most common | Terraform for RDS/ElastiCache/MSK/SQS; parameter groups, backups, failover testing, monitoring, upgrades. You are not the DBA. |
| **Integrate apps with it** | Universal | Connection strings via secrets, IAM auth, network path, connection pooling (PgBouncer), sane pool sizes |
| **Run stateful workloads *in* Kubernetes** | Less common, rising | StatefulSets, CSI, operators (CloudNativePG, Strimzi). **Interviewers love asking whether you should.** Have a real answer. |

Per-technology depth for you:

| Tech | Depth | Why |
|---|---|---|
| S3 | Deep (you have it) | Lifecycle, versioning, replication, policies, endpoints |
| PostgreSQL / RDS | **Working** | Connections, replicas, failover, backup/PITR, parameter groups, IAM auth. SO 2025: Postgres 55.6% `[SURVEY]` — most-used DB by a distance |
| Redis / ElastiCache | Working | Cache patterns, eviction, cluster mode. SO 2025: 28% `[SURVEY]` |
| DynamoDB | Conceptual→Working (you have it) | Capacity modes, partition keys, throttling |
| EBS / EFS | Working | gp3 vs io2, IOPS/throughput, snapshots, RWO vs RWX, CSI drivers |
| SQS / SNS / EventBridge | Working (you have Lambda/S3 events) | DLQs, visibility timeout, `maxReceiveCount`, fan-out |
| Kafka / MSK | Conceptual | Partitions, consumer groups, lag as an SLI, why it's operationally expensive |
| RabbitMQ | Awareness | |

**Your leverage:** you already know how these behave under real load from the data side.
Most DevOps candidates have never watched a pipeline melt because of a partition key. Say so.

---

## 12. Declining / deprioritise

`[INFERENCE]`, basis: direction of official tooling and ecosystem replacement. Be able to
*discuss* these — you will meet them in brownfield estates — but do not invest study hours.

| Technology | Status | Replaced by |
|---|---|---|
| Jenkins (as a *new* choice) | Declining globally, still common in India | GitHub Actions / GitLab CI |
| Cluster Autoscaler on EKS | Being displaced | **Karpenter** (AWS publishes best-practice guides for both, plus Auto Mode `[OFFICIAL]`) |
| `aws-auth` ConfigMap | Superseded | **EKS Access Entries** |
| Docker as the K8s runtime | Removed long ago | containerd |
| PodSecurityPolicy | Removed | Pod Security Admission + Kyverno/OPA |
| Ingress (long-term) | Being succeeded | **Gateway API** — Ingress remains dominant today; know both |
| Spinnaker | Declining | Argo CD / Argo Rollouts |
| Static AWS access keys in CI | Should be extinct | **OIDC federation** |
| Chef / Puppet | Legacy | Immutable images + Terraform |
| Self-managed Prometheus at large scale | Shifting | Managed (AMP) / Thanos / Mimir / vendor |

---

## 13. Your learning order (dependency-correct)

```mermaid
flowchart TD
  A["Linux + Networking<br/>P0 — foundations"] --> B["Docker / OCI<br/>P0"]a
  A --> C["AWS: IAM policy eval + VPC data path<br/>P0 — you are ~60% here"]
  B --> D["Kubernetes core<br/>P0 — free, on kind"]
  C --> E["Terraform: author, not consumer<br/>P0"]
  D --> F["EKS: IRSA, ALB controller, Karpenter, add-ons, upgrades<br/>P0"]
  E --> F
  F --> G["CI/CD: GitHub Actions + OIDC<br/>P1"]
  G --> H["GitOps: Argo CD<br/>P1"]
  F --> I["Observability: Prometheus/Grafana + SLOs<br/>P1"]
  H --> J["Security: policy, secrets, supply chain<br/>P2"]
  I --> J
  J --> K["IDP concepts + one built artefact<br/>P2"]
  K --> L["Platform system design + mock interviews"]
  I --> L
  M["Python hardening<br/>P1 — parallel, you have the base"] --> L
  N["Go: read first, then write<br/>P2 — parallel, after F"] --> L
```

**The rule that matters:** do not start Kubernetes before you can debug a Linux process and a
DNS lookup, and do not start GitOps before you can debug Kubernetes. Every step in that graph
is a prerequisite, not a suggestion — skipping one produces a candidate who can recite YAML
and cannot answer a follow-up.

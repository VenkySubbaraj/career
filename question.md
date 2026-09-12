# 06 — Interview question bank

Covers your **Part G**. Two halves:

- **Part 1 — Topic banks** (§1–§28): questions by difficulty across all your requested categories.
- **Part 2 — Scenario deep-dives** (§S1–§S18): your 18 scenarios, each with what's tested, a
  strong answer, weak answers, follow-ups, and whether hands-on is expected.

**Difficulty:** `F` Foundation · `I` Intermediate · `S` Senior · `St` Staff-oriented.
Work through Part 1 for coverage; rehearse Part 2 out loud, timed to 10 minutes each.

---

# Part 1 — Topic banks

## 1. Linux

**F** — What happens between `fork()` and `exec()`? · Difference between a process and a thread ·
What is a zombie process and who reaps it? · `df` says 40% but writes fail — why? · What does
`chmod 755` mean and when does it matter for a container? · What is a file descriptor?

**I** — What actually *is* a container, in kernel terms? *(namespaces + cgroups + a root
filesystem — if you can't say this, stop and learn it)* · `SIGTERM` vs `SIGKILL`, and what PID 1
does differently · Load average of 12 on an 8-core box — is that bad? · How do you find which
process is eating disk? · What does `strace` show you that logs don't? · Why is swap disabled on
Kubernetes nodes?

**S** — A process is using 4 GB RSS but the box has 32 GB free and it still OOMKilled. Explain.
*(cgroup limit ≠ host memory)* · CPU throttling with utilisation at 40% — how? *(CFS quota;
check `nr_throttled`)* · Walk me from `journalctl -u kubelet` to a root cause · Deleted a 50 GB
log file and disk didn't free — why? *(open FD; `lsof +L1`)* · Explain page cache vs RSS and why
`free -m` confuses people.

**St** — How would you standardise node OS images across a fleet, and how do you roll a kernel
CVE fix to 500 nodes with zero downtime? · When would you build a custom AMI vs use
Bottlerocket/AL2023 as-is?

## 2. Networking

**F** — Split `10.0.0.0/16` into eight equal subnets · Public vs private subnet · What is NAT ·
Difference between a router and a switch, in cloud terms.

**I** — SG vs NACL (stateful/stateless, allow-only vs allow+deny, first-match numbering,
**ephemeral ports on the NACL return path**) · L4 vs L7 load balancing, and when you need each ·
What is MTU and when does it bite? · How does a packet get from a pod on node A to a pod on
node B under the AWS VPC CNI? *(real VPC IPs, secondary ENIs, no overlay)*

**S** — Design VPC addressing for an EKS cluster expected to reach 5,000 pods · You've exhausted
subnet IPs — three fixes, ranked · conntrack table exhaustion: symptoms and fix · Why does
cross-AZ traffic cost money and how would you reduce it without breaking HA?

**St** — Design network segmentation for a regulated multi-account estate · Transit Gateway vs
peering vs PrivateLink — pick one for a hub-and-spoke with 40 accounts and defend it · When is a
service mesh worth its complexity?

## 3. DNS

**F** — A record vs CNAME vs ALIAS · What is TTL · Why can't a CNAME sit at the zone apex?

**I** — Walk a resolution from browser to answer · What is `ndots:5` in Kubernetes and why does
it cause 4 extra lookups for `api.example.com`? · How does a Kubernetes Service name resolve? ·
What's a negative cache and how does it hurt you during a deploy?

**S** — Intermittent DNS failures cluster-wide — ordered hypotheses *(see §S4)* · Split-horizon
DNS: why does the same name resolve differently inside and outside the VPC, and how does that
break EKS? · What are the VPC resolver limits and what happens when you hit them?

**St** — Design DNS for a multi-region active-passive platform including failover and TTL
strategy, and state the effect on your RTO.

## 4. HTTP and TLS

**F** — 4xx vs 5xx · What is a redirect · What does an SSL cert prove?

**I** — **502 vs 503 vs 504** — what does each tell you about *where* the failure is?
*(502 upstream returned garbage/closed; 503 no healthy upstream; 504 upstream too slow)* ·
Walk through a TLS handshake · What is SNI and why do multi-tenant load balancers need it? ·
Keep-alive: why does it matter behind a load balancer?

**S** — Where should TLS terminate and what are the trade-offs of each choice? · Certificate
chain: why does it work in your browser and fail in `curl` inside a container? *(missing CA
bundle / intermediate)* · How do you do zero-downtime cert rotation? · What is mTLS and what
does it actually buy you?

**St** — Design certificate management for 300 internal services with rotation and revocation.

## 5. Git

**F** — Merge vs rebase · What is a detached HEAD · What does `.gitignore` not do? *(doesn't
untrack already-tracked files)*

**I** — Undo a commit already pushed to a shared branch · Cherry-pick vs revert · What is a
shallow clone and why does CI use one?

**S** — **A secret was committed and pushed. What do you do, in order?** *(1. rotate the secret —
it is compromised regardless; 2. remove from history; 3. force-push with coordination;
4. audit access logs; 5. add pre-commit + CI secret scanning. Candidates who start with history
rewriting instead of rotation get this wrong.)* · Trunk-based vs GitFlow for a platform repo ·
How do you structure repos for GitOps: mono vs poly?

## 6. Bash

**F** — What does `set -euo pipefail` do, flag by flag? · `"$var"` vs `$var` · Exit code of a
pipeline?

**I** — Write: find the top 10 IPs in an access log · Write: retry a command up to 5 times with
backoff · What does `trap 'cleanup' EXIT` do? · `[[ ]]` vs `[ ]`

**S** — Read this script and find the three bugs *(usually: unquoted variable, unchecked `cd`,
`-e` not firing inside a pipeline or `if`)* · Write a script that safely deletes all Evicted pods
across namespaces, with `--dry-run` · When should you stop using Bash and switch to Python?
*(when you need data structures, tests, or another person will maintain it)*

## 7. Python

**F** — List vs tuple vs set · What is a dict comprehension · How do you read a large file?

**I** — Generators vs lists for memory · `try/except/else/finally` semantics · Write a function
that pages through a boto3 API · Context managers — what problem do they solve?

**S** — Implement bounded concurrency with `ThreadPoolExecutor`, collecting both results and
errors · Implement retry with exponential backoff **and jitter** — why jitter? *(synchronised
retries create a thundering herd that keeps the service down)* · GIL: when does threading help,
when do you need processes? · How do you make a CLI testable?

**St** — Design a platform CLI: config resolution, auth, plugins, output formats, versioning,
and how you'd ship it to 200 engineers.

## 8. Go

**F** — Why does the ecosystem use Go? · Goroutine vs thread · What is a slice?

**I** — Channels and `select` · Error handling idiom and why no exceptions · `defer` semantics ·
Write a worker pool with `errgroup` and a bounded size.

**S** — `context.Context` — cancellation and deadline propagation, and why every platform API
takes one · Detect a race with `-race`, then explain the fix · Write an HTTP server with
graceful shutdown · Read this controller's `Reconcile` and tell me what it does.

**St** — Design a Kubernetes controller: CRD shape, reconciliation idempotency, status
conditions, finalizers, requeue/backoff, and how you'd test it.

## 9. Docker

**F** — Image vs container · `ENTRYPOINT` vs `CMD` · What is a layer?

**I** — Order a Dockerfile for cache efficiency · Multi-stage build: what and why · Why run as
non-root? · Why is `:latest` banned in production? · How do you get a 900 MB Python image down
to 150 MB?

**S** — Signals: why doesn't your app receive SIGTERM when started via a shell form ENTRYPOINT? ·
Build a multi-arch image and explain why you'd want arm64 · How do you keep secrets out of image
layers? *(build secrets/mounts, never `ARG`)* · Explain image signing and admission verification.

## 10. Kubernetes

**F** — Pod, ReplicaSet, Deployment, Service · Service types · ConfigMap vs Secret · What is a
namespace for?

**I** — Rolling update mechanics: `maxSurge`/`maxUnavailable` · The three probes and what each
failure does · Requests vs limits, and QoS classes · How does a Service route to pods? *(labels
→ Endpoints/EndpointSlice → kube-proxy)* · What is a DaemonSet for? · StatefulSet vs Deployment ·
What does a PodDisruptionBudget actually prevent? *(voluntary disruptions only — not node crashes)*

**S** — Full lifecycle of `kubectl apply` through admission, scheduling, kubelet, CNI, CSI ·
Enumerate every cause of `Pending` · A namespace is stuck `Terminating` — why? *(finalizers)* ·
Explain the admission chain and how a webhook can take down your cluster · How do you make one
cluster safe for 30 teams? · How do you upgrade with zero 5xx? · What's the difference between
Ingress and Gateway API and why is the industry moving?

**St** — Design multi-cluster: when do you add a cluster vs a namespace? · Cluster limits — what
breaks first at scale? *(etcd object count, API server watch load, CNI IPs, CoreDNS)* · Build vs
buy for a Kubernetes platform · How do you run a fleet upgrade across 25 clusters?

## 11. EKS

**F** — What does AWS manage vs you? · What is a managed node group?

**I** — How do nodes join a cluster? · What are EKS add-ons and why use them over Helm? ·
IRSA — what is it, roughly? · Access entries vs the old `aws-auth` ConfigMap.

**S** — Draw the EKS control-plane/data-plane architecture including cross-account ENIs · IRSA
token flow end to end, plus four failure modes · VPC CNI IP exhaustion: cause and three fixes ·
Karpenter vs Cluster Autoscaler — why did Karpenter win? · Sequence a minor-version upgrade
including add-ons and API deprecations · Private cluster: what endpoints do you need and why?

**St** — Design an EKS platform for 30 teams with tenancy, cost attribution and upgrade
strategy · What's your EKS DR plan and what is *not* recoverable from Git? · When would you
choose ECS over EKS? *(small team, simple workloads, no K8s expertise — a good answer says
"probably ECS" more often than candidates expect)* · EKS Auto Mode: what does it take over and
what do you give up?

## 12. Terraform

**F** — What is state and why does it exist? · Variables vs locals vs outputs · What does
`terraform plan` actually do?

**I** — Remote backend + locking · Modules: inputs, outputs, versioning · `count` vs `for_each` ·
Data sources vs resources · How do you manage multiple environments?

**S** — State is locked by a dead job *(see §S2)* · Plan wants to recreate prod RDS *(see §S18)* ·
Import 200 existing resources · Split a giant state file · Test a module · Enforce policy in the
pipeline · Multi-account/multi-region provider aliases · Terraform vs OpenTofu — your position?

**St** — Design the IaC platform for 60 teams: module registry, release process, PR automation,
policy gates, drift detection, and how a team gets unblocked at 2 a.m.

## 13. CI/CD

**F** — What is CI vs CD? · What is an artifact?

**I** — Design a pipeline for a containerised service · Why build once and promote, rather than
rebuild per environment? · How do you cache dependencies safely? · Where do secrets come from?

**S** — How does CI authenticate to AWS with no static keys? *(OIDC federation — say the trust
policy conditions: `repo:org/name:ref:refs/heads/main`)* · Self-hosted runners: what are the
isolation risks? *(a compromised job can reach the runner's IAM role and neighbouring jobs)* ·
How do you keep 40 pipelines consistent? *(reusable workflows as a versioned platform product)* ·
How do you gate on security scans without blocking every build?

**St** — Design the delivery platform: promotion model, approvals, rollback, supply-chain
attestation, and the developer experience of a failed deploy at 6 p.m. on a Friday.

## 14. GitOps

**F** — What is GitOps? · Why is Git the source of truth?

**I** — Argo CD Application and how sync works · `selfHeal` and `prune` — and when each is
dangerous · How do you handle secrets?

**S** — Push vs pull, argued both ways · Persistent `OutOfSync` with no changes *(see §S9)* ·
ApplicationSet: what problem does it solve? · Sync waves and hooks · Argo CD vs Flux ·
Environment promotion strategy.

**St** — Design GitOps for 500 services across 8 clusters and 3 environments, including
tenancy, RBAC, repo topology and blast-radius control.

## 15. AWS architecture

**I** — Design a 3-tier app on AWS · When Lambda vs ECS vs EKS? · How do you achieve HA within
a region?

**S** — Design a landing zone *(see [04-architectures.md](04-architectures.md) §2)* · Multi-AZ
vs multi-region: when is the second justified? · Design for an RTO of 1 hour and an RPO of 5
minutes · How do you migrate from one account to many?

**St** — Design the AWS platform for a company going from 50 to 500 engineers. What breaks
first and in what order?

## 16. AWS IAM

**F** — User vs role · What is a policy?

**I** — Trust policy vs permission policy · How does cross-account access work? · What is STS? ·
Why prefer roles over access keys?

**S** — **Full policy evaluation order** including SCP, boundary, session, resource policy *(see
[03-aws-competency.md](03-aws-competency.md) §3)* · `AccessDenied` despite an Allow *(see §S8)* ·
Permission boundary vs SCP — give a scenario for each · Why are KMS key policies special? ·
IRSA vs Pod Identity.

**St** — Design the identity model for 40 accounts: humans, workloads, CI, third parties,
break-glass, and how you'd prove least privilege to an auditor.

## 17. AWS networking

**I** — VPC components · Public vs private subnet routing · What does a NAT gateway do, and in
which direction?

**S** — Private workload can't reach the internet *(see §S7)* · Gateway vs interface endpoints,
and which are free · SG vs NACL · Design VPC CIDR for EKS at scale · VPC Flow Logs: what can and
can't they tell you? *(they show accept/reject at the ENI — they don't show application errors,
and they don't show intra-node pod traffic)*

**St** — Centralised egress design; TGW route-table strategy; how you'd prevent dev reaching prod
at the network layer and prove it.
a
## 18. Observability

**I** — Metrics vs logs vs traces — when do you reach for each? · What is a histogram and why
not an average? · What is cardinality?

**S** — Write a PromQL query for the 99th-percentile latency of a service · Prometheus is OOMing
*(see §S10)* · Design monitoring for a new service from scratch · What do you alert on for a
queue consumer? *(lag/age of oldest message, not CPU)* · How do you correlate a trace to a log
line?

**St** — Design observability for 100 services with cost control, tenancy and a migration path
off a vendor.

## 19. SRE

**I** — What is an SLI/SLO/error budget? · Availability vs reliability · What is toil?

**S** — Define an SLI and SLO for a specific API and justify the target · Compute an error budget
for 99.9% over 30 days *(≈43.2 minutes)* · Write a multi-window multi-burn-rate alert and explain
each window · What's your error-budget policy when the budget is gone? · Why alert on symptoms
rather than causes?

**St** — Roll out SLOs across an organisation that has never used them. How do you get adoption,
and what do you do when a team's SLO is unachievable with its current architecture?

## 20. Incident response

**I** — What do you do first when paged? · What is an incident commander?

**S** — Walk me through an incident you ran, end to end · Mitigate first or diagnose first?
*(mitigate — restore service, then investigate; a rollback is a valid first move)* · How do you
run a blameless postmortem, and why "blameless"? · How do you decide severity?

**St** — Design the incident-management process for a 200-engineer org: paging, escalation,
comms, review cadence, and how you measure whether it's improving.

## 21. Security

**I** — Least privilege, in practice · Why not run containers as root? · What is defence in depth?

**S** — Threat-model a CI/CD pipeline *(the runner's credentials, dependency confusion, PR
triggers from forks, cache poisoning, artifact tampering)* · Admission control strategy · How do
you handle a CVE in a base image used by 80 services? · What is the OWASP-style risk in a
self-hosted runner?

**St** — Design the security programme for a Kubernetes platform: preventive vs detective
controls, exception process, evidence for auditors, and how you avoid becoming the team that
says no.

## 22. Secrets management

**I** — Why aren't K8s Secrets secure by default? · Secrets Manager vs Parameter Store.

**S** — Secret in a CI log *(see §S15)* · Rotate a DB password with zero downtime · Design ESO
scoping so one compromise doesn't expose everything · Vault vs cloud-native secret stores.

## 23. Platform engineering

**I** — What is a platform team for? · What is a golden path?

**S** — How do you measure platform success? *(adoption, time-to-first-deploy, lead time, change
failure rate, and qualitative feedback — CNCF's Measurement axis goes "Ad hoc → Consistent
collection → Insights → Quantitative and qualitative" `[OFFICIAL]`)* · How do you get adoption
without a mandate? *(see §S13)* · What do you do when a team goes off the golden path?
*(see §S14)* · How do you decide what the platform should and shouldn't own?

**St** — Write the platform's 12-month roadmap for a 200-engineer company and justify the
sequencing · When is a platform team premature? *(a real answer: below ~30 engineers you're
usually building an internal product for three customers)*

## 24. Internal Developer Platforms

**I** — What is a service catalogue? · What is a software template?

**S** — Design a "create a new service" flow end to end · Should the portal be the source of
truth? *(no — Git is; the portal raises PRs)* · How do you keep 40 template-generated repos from
rotting? · Backstage vs Port vs nothing.

## 25. System design

See [04-architectures.md](04-architectures.md) — all eight architectures are design prompts.
Additional: "design a CI system for a monorepo with 2,000 builds/day" · "design a container
registry strategy for 12 accounts" · "design environment-per-PR with cost controls."

## 26. Cost optimisation

**I** — What are the biggest cost drivers in an EKS account? · Spot vs on-demand vs Savings Plans.

**S** — NAT gateway bill tripled *(see §S17)* · CloudWatch costs spiked *(see §S11)* · Reduce EKS
spend by 30% without hurting reliability — what do you do first and how do you prove it? · How do
you attribute shared-cluster cost to teams? *(labels → Kubecost/OpenCost or CUR + tags; and the
honest bit: shared costs need an allocation policy people agree to)*

**St** — Build a FinOps practice: visibility, showback vs chargeback, guardrails, and how you
avoid cost-cutting theatre that damages reliability.

## 27. Disaster recovery

**I** — RTO vs RPO · Backup vs replication vs snapshot.

**S** — DR for an EKS platform — what's in Git and what isn't · Test a restore properly *(into a
new cluster; anything else is a partial test)* · Losing an AZ *(see §S12)* · How do you decide
between warm standby and pilot light?

**St** — Design and *prove* a 1-hour RTO for a regulated workload, including the evidence you'd
show an auditor.

## 28. Behavioural and leadership

**All levels** — Tell me about an incident you caused · Convince a team to adopt something they
didn't want · Disagree with your manager · A project you killed · Something you automated away ·
Learning fast under pressure · **Something you built that nobody used** · How do you say no to a
senior stakeholder? · How do you onboard onto an unfamiliar system? · What's your approach when
you're on-call and the runbook is wrong?

---

# Part 2 — Scenario deep-dives

Format for each: **Tests · Strong answer · Weak answers · Follow-ups · Hands-on?**

---

## S1. A pod cannot reach an AWS service

**Tests:** whether you separate *identity* failures from *network* failures — the single most
diagnostic habit in AWS+K8s work.

**Strong answer.** "First: is it an auth error or a connectivity error? `AccessDenied` means the
network is fine and identity is wrong. A timeout means the packet never arrived. Those are
completely different investigations, so I'd read the actual error before touching anything."

*If AccessDenied (identity path):*
1. What identity is the pod actually using? `kubectl exec` → `aws sts get-caller-identity`.
   If it returns the **node instance role**, IRSA isn't taking effect at all.
2. SA annotation `eks.amazonaws.com/role-arn` present and correct?
3. Role trust policy: OIDC provider ARN correct, `aud` = `sts.amazonaws.com`,
   `sub` = `system:serviceaccount:<ns>:<sa>` — typos here are the most common cause.
4. Is the OIDC provider associated with the cluster in IAM at all?
5. Permission policy actually allows the action *on that resource ARN*.
6. Resource policy / KMS key policy on the target.
7. SCP on the account.
8. SDK version old enough to ignore the web identity token file.

*If timeout (network path):*
1. DNS resolves? (`nslookup` from the pod)
2. Route: private subnet → NAT or → VPC endpoint?
3. Interface endpoint: private DNS enabled? SG on the endpoint allows 443 from the node/pod?
4. Endpoint policy restricting the call?
5. NACL return path (ephemeral ports).
6. NetworkPolicy denying egress from the pod.

**Weak answers.** Jumping straight to "add `AdministratorAccess` and see if it works." Not
checking which identity is in use. Confusing the trust policy with the permission policy.

**Follow-ups.** "It works for one pod and not another in the same namespace" *(different SA)* ·
"It worked yesterday" *(token expiry, policy change — check CloudTrail)* · "How would you prevent
this class of problem?" *(platform-generated IRSA module + a smoke-test job in the golden path)*

**Hands-on?** Yes — commonly a live `kubectl` session. Practise the `sts get-caller-identity`
trick until it's reflex.

---

## S2. Terraform state is locked or corrupted

**Tests:** blast-radius thinking and whether you understand what state *is*.

**Strong answer.** *Locked:* "First I verify the lock holder is genuinely dead — read the lock
info (who, when, which operation), check the CI job. `force-unlock` while an apply is still
running risks two concurrent writers and a corrupted state, which is far worse than waiting.
Once confirmed dead: `terraform force-unlock <ID>`, then immediately `plan` to check whether the
dead run applied partially."

*Corrupted:* "Don't panic-edit. S3 backends should have versioning on — restore the previous
state version. Then `plan` and reconcile: if resources exist in AWS but not in state,
`terraform import` them; if state references resources that no longer exist, `state rm`. Take a
backup copy of the current state before every one of those operations."

**Weak answers.** `force-unlock` immediately. Hand-editing `terraform.tfstate`. Deleting state
and re-applying (which recreates or duplicates production infrastructure).

**Follow-ups.** "How do you prevent this?" *(DynamoDB locking, versioned backend, CI concurrency
groups so two pipelines can't apply the same stack, shorter-running applies via smaller states)*
· "What if state was never versioned?" *(rebuild via `import`, one resource at a time — slow and
a good argument for versioning)*

**Hands-on?** Sometimes — a walkthrough is more common than a live exercise.

---

## S3. An EKS upgrade breaks workloads

**Tests:** change management. Senior candidates answer about *prevention*, not just recovery.

**Strong answer.** "Recovery first: the control plane cannot be downgraded, so my options are
node-level rollback (revert node groups to the previous AMI/version) and workload rollback via
Git. AWS publishes a Cluster Version Rollback guide precisely because this is a real scenario
`[OFFICIAL]`.

Then the actual causes, in likelihood order:
1. **Removed API versions** — the workload used a version deleted in the new minor. Prevention:
   `kubectl convert`, Pluto/kubent scans, and the EKS deprecation insights, run *before* upgrading.
2. **Add-on incompatibility** — VPC CNI / CoreDNS / kube-proxy versions have a supported matrix.
   Upgrade order is control plane → add-ons → nodes.
3. **Deprecated flags or behaviour changes** in controllers.
4. **PDBs blocking drain**, so nodes never actually rolled and you get a half-upgraded fleet.

My process: upgrade a non-prod cluster of the same shape first; scan for deprecated APIs; upgrade
control plane; upgrade add-ons; roll nodes with surge capacity and PDBs that actually permit
disruption; keep the old node group alive until verification passes."

**Weak answers.** "Roll back the control plane" (you can't). No mention of API deprecation. No
staged environment.

**Follow-ups.** "How do you upgrade 25 clusters?" *(waves; canary cluster; automation; a
published calendar)* · "How do you achieve zero 5xx during a node roll?" *(PDBs, surge, graceful
shutdown honouring SIGTERM, `preStop` sleep to let endpoints drain, connection draining on the
target group)*

**Hands-on?** Rarely live. **This repo's Module 11 does it for real** — that experience is worth
more than any answer.

---

## S4. DNS resolution intermittently fails

**Tests:** reasoning about partial failures, which are harder than total failures.

**Strong answer.** "Intermittent means either load-related or one-replica-in-N related. I'd
scope it: which pods, which names, which nodes? If it's a subset of nodes, suspect node-local
resources; if a subset of names, suspect upstream or search-domain behaviour.

Hypotheses:
1. **CoreDNS under-provisioned or CPU-throttled** — check replicas, requests, and
   `nr_throttled`. CoreDNS is the most common single point of cluster-wide flakiness.
2. **`ndots:5`** — every external name generates 4 failed lookups first (`svc.ns.svc.cluster.local`
   etc.), multiplying query volume by ~5. Fix with a `dnsConfig` override or a trailing dot.
3. **conntrack exhaustion on nodes** — UDP conntrack entries fill up; check
   `nf_conntrack_count` vs `max` and `dmesg` for table-full messages.
4. **VPC resolver limits** — there's a hard packets-per-second limit per ENI toward the
   `.2` resolver; NodeLocalDNSCache fixes this by caching on the node.
5. One unhealthy CoreDNS replica still in Endpoints (readiness probe too lenient)."

**Weak answers.** "Restart CoreDNS." Blaming the application. Not knowing what `ndots` is.

**Follow-ups.** "How would you prove it's DNS?" *(dig from the pod in a loop with timing; CoreDNS
metrics: request rate, duration, SERVFAIL count)* · "What's your permanent fix?" *(NodeLocalDNSCache
+ CoreDNS autoscaling + ndots tuning in the platform's default pod spec)*

**Hands-on?** Sometimes reproduced in a live cluster.

---

## S5. Deployment successful, but traffic gets 503

**Tests:** understanding that "Ready" in Kubernetes and "healthy" at the load balancer are two
independent systems.

**Strong answer.** "503 means *no healthy upstream*, so I work backwards from the load balancer.

1. **Target group health** — are targets registered and healthy? If the target group is empty,
   the LB controller didn't register pods.
2. **Readiness probe vs LB health check mismatch** — the pod passes its readiness probe on
   `/healthz` while the target group checks `/` and gets a 404. Extremely common.
3. **Service selector doesn't match pod labels** → Endpoints empty → nothing to route to.
   `kubectl get endpoints <svc>` is the fastest single check.
4. **Target type** — `ip` mode targets pod IPs, so the *pod* security group path matters, not
   the node's.
5. **Timing** — pods came up and old ones terminated before the LB registered the new ones.
   Needs `minReadySeconds`, a `preStop` sleep, and deregistration delay tuning.
6. **Port mismatch** — `containerPort` vs `targetPort` vs the target group port.

I'd check Endpoints first because it discriminates between 'Kubernetes thinks it's fine' and
'the LB thinks it isn't' in one command."

**Weak answers.** Restarting pods. Not knowing what 503 specifically means. Not checking Endpoints.

**Follow-ups.** "Only 10% of requests fail" *(one bad replica, or one AZ's targets unhealthy)* ·
"It happens only during deploys" *(the drain/registration race — this is the good answer)*

**Hands-on?** Frequently — a broken cluster is handed to you.

---

## S6. An ALB target remains unhealthy

Overlaps S5; the difference is the LB is the *stated* symptom.

**Strong answer.** Ordered: health-check path/port/protocol/matcher vs what the container serves
→ does the app respond 200 on that path when curled from inside the pod? → SG chain: ALB SG must
be allowed *by* the target SG on the target port (with `ip` targets, that's the pod ENI's SG) →
target type `ip` vs `instance` and whether the node port is open for `instance` mode → is the
app slow to start (needs a startup probe and a longer health-check grace) → is the app returning
302/401 on the health path (matcher expects 200) → NACLs.

**Follow-ups.** "The target group is completely empty" *(LB controller IRSA permissions, wrong
`ingressClassName`, subnet tags missing: `kubernetes.io/role/elb` on public subnets)* — **subnet
tagging is a classic EKS gotcha, mention it unprompted.**

**Hands-on?** Yes, often.

---

## S7. A private-subnet workload cannot access the internet

**Tests:** the most fundamental AWS networking model. Getting this wrong is usually fatal.

**Strong answer.** "Ordered hypotheses:
1. **Route table** — does the private subnet have `0.0.0.0/0 → nat-xxxx`? Most common cause. And
   is the route table actually *associated* with that subnet?
2. **NAT gateway placement** — the NAT must live in a **public** subnet whose own route table
   points `0.0.0.0/0` at the IGW. A NAT in a private subnet does nothing.
3. **NAT health/AZ** — is the NAT in the same AZ, and is it available?
4. **Security group egress** — default SGs allow all egress, but hardened ones may not.
5. **NACL** — outbound allow *and* inbound ephemeral ports (1024–65535) for the return path.
6. **DNS** — 'no internet' is often actually 'no DNS'; test with an IP to separate them.

And I'd note: NAT is outbound-only. If the requirement is inbound, NAT is the wrong tool
entirely — that needs a load balancer.

If the destination is an AWS service, the better answer is often a VPC endpoint rather than
internet egress at all: cheaper, more secure, and it survives NAT failure."

**Weak answers.** "Open the security group." Suggesting an IGW route for the private subnet
(which breaks the definition of private and won't work without public IPs anyway).

**Follow-ups.** "Curl to an IP works but to a hostname doesn't" *(DNS)* · "It works from one AZ
only" *(per-AZ NAT/route table)* · "How do you avoid NAT costs entirely?" *(endpoints; and for
ECR specifically, interface endpoints for `ecr.api`, `ecr.dkr` plus the S3 gateway endpoint for
layers)*

**Hands-on?** Console or CLI walkthrough is common.

---

## S8. IAM `AccessDenied` despite an allow policy

**Tests:** IAM policy evaluation order. Non-negotiable for AWS platform roles.

**Strong answer.** "An explicit Deny anywhere wins, and there are more places to look than
people expect. In order:
1. **Explicit Deny** in any attached identity policy.
2. **SCP** on the account/OU — a very common cause in enterprises, and it surfaces as a plain
   `AccessDenied` with no hint that an SCP was involved.
3. **Permission boundary** on the principal — allow must be in *both* the policy and the boundary.
4. **Session policy** if the credentials came from `AssumeRole` with one attached.
5. **Resource policy** — an S3 bucket policy denying, or simply not allowing in the
   cross-account case where you need both sides.
6. **KMS key policy** — if the object is SSE-KMS encrypted, `s3:GetObject` is not enough; you
   also need `kms:Decrypt`, and the key policy must permit it.
7. **Condition keys not met** — source IP, VPC endpoint, MFA, tags.
8. **Wrong principal entirely** — am I even the identity I think I am? `aws sts get-caller-identity`.
9. **Resource ARN mismatch** — `arn:aws:s3:::bucket` vs `arn:aws:s3:::bucket/*` are different
   things, and object actions need the second.

Tooling: the error message names the action and resource; CloudTrail shows the denied call and
often the reason; the IAM policy simulator; `--debug` on the CLI."

**Weak answers.** Adding `*` permissions. Not knowing SCPs exist. Missing the KMS layer.

**Follow-ups.** "Works in dev, not prod" *(SCP or boundary difference)* · "Worked an hour ago"
*(policy change — CloudTrail; or expired session)* · "How would you give least privilege
properly?" *(start from CloudTrail-observed actions, use Access Analyzer policy generation)*

**Hands-on?** Whiteboard or CLI walkthrough.

---

## S9. Argo CD reports drift

**Tests:** whether you understand reconciliation, or just click Sync.

**Strong answer.** "First: is this real drift (someone changed the cluster) or phantom drift
(the rendered manifest never matches the live object)? The diff view tells you which.

*Real drift:* someone ran `kubectl edit`. With `selfHeal` on, Argo reverts it — and the fix is
process, not config: remove write access, make the Git path faster than the manual path.

*Phantom drift* — far more common, and the interesting answer:
- **A mutating webhook** adds fields (sidecar injection, defaulted resources) that aren't in Git.
- **API server defaulting** — fields you didn't set come back populated.
- **HPA manages `replicas`** while your manifest also specifies it — permanent fight. Fix:
  remove `replicas` from the manifest or `ignoreDifferences` on that path.
- **Non-determinism in rendering** — a Helm chart using `randAlphaNum` or a timestamp.
- **`resourceVersion`/`status` noise** in a CRD with a badly written diff.

Fix with targeted `ignoreDifferences`, not by turning off sync. Turning off sync is how you end
up with a fleet nobody trusts."

**Weak answers.** "Click Sync." Disabling self-heal permanently. Not knowing webhooks mutate.

**Follow-ups.** "Every app is OutOfSync after a cluster upgrade" *(API defaults changed)* ·
"Should self-heal be on in prod?" *(yes for consistency; be careful combined with `prune`)*

**Hands-on?** Sometimes a live Argo UI walkthrough.

---

## S10. Prometheus cardinality explodes

**Tests:** understanding that observability systems have their own capacity model.

**Strong answer.** "Cardinality = the number of unique label-value combinations, and each unique
series costs memory. It explodes when someone puts an unbounded value in a label: user ID,
request ID, full URL path with IDs in it, pod name on a high-churn deployment, or an error
message string.

Diagnosis: `topk(10, count by (__name__)({__name__=~'.+'}))` for the worst metrics, and the TSDB
status page for label-value counts. `prometheus_tsdb_head_series` trending sharply upward is the
alert I want *before* the OOM.

Immediate mitigation: `metric_relabel_configs` to drop the offending label or the whole metric at
scrape time, then restart. That's a same-hour fix.

Prevention, which is the platform answer: a label allow-list in the platform's scrape config, an
alert on series-count growth per job, metric naming conventions in the golden path, and reviewing
new metrics in PRs. Long term, offload to remote storage (AMP/Mimir/Thanos) so a single
Prometheus isn't the ceiling."

**Weak answers.** "Give it more memory." Not knowing what cardinality is. No prevention story.

**Follow-ups.** "Why not just increase resources?" *(linear cost growth against exponential
cardinality growth — you lose)* · "How do you get per-user metrics then?" *(you don't — that's
what logs and traces are for; this distinction is the point of the question)*

---

## S11. CloudWatch costs suddenly increase

**Tests:** cost reasoning and knowing where CloudWatch charges.

**Strong answer.** "CloudWatch bills separately for log ingest, log storage, custom metrics,
dashboards, alarms and API calls. **Ingest and custom metrics dominate.**

Investigate: Cost Explorer grouped by usage type to see *which* of those spiked. Then, for logs,
`aws logs describe-log-groups` sorted by `storedBytes`, or a Logs Insights query over
`aws/containerinsights` to find the noisiest group.

Common causes:
- Debug logging left on after an incident — the single most common.
- A crash loop generating enormous log volume (the outage *and* the bill).
- A new deployment logging every request body.
- Container Insights enabled at high resolution across a big cluster.
- A high-cardinality custom metric — each unique dimension combination is a separate,
  separately-billed metric.
- No retention set: default is never-expire, so storage accumulates forever.

Fixes: retention on every log group (enforced by policy, not by hope), sampling, filter debug at
Fluent Bit before shipping, move high-volume logs to S3, replace per-request custom metrics with
Prometheus histograms, and a billing anomaly alarm so you find out in a day rather than a month."

**Follow-ups.** "How would you have caught this in an hour?" *(anomaly detection on ingest rate,
budget alert with a daily threshold)*

---

## S12. One availability zone becomes unavailable

**Tests:** whether "multi-AZ" is a checkbox or a design you've reasoned about.

**Strong answer.** "What *should* happen: ~⅓ of nodes go NotReady, pods are rescheduled to the
surviving AZs, the ALB stops sending to targets in the dead AZ, RDS Multi-AZ fails over
automatically, and the service degrades in capacity but stays up.

What actually goes wrong:
- **No spare capacity** — the remaining two AZs can't hold the rescheduled pods, so they sit
  Pending. Mitigation: over-provision, or Karpenter with capacity in the other AZs and
  instance-type flexibility so it can actually get instances during a regional capacity crunch.
- **`topologySpreadConstraints` missing** — all replicas were in the dead AZ.
- **A stateful component was single-AZ** — EBS volumes are AZ-bound, so a StatefulSet pod cannot
  simply move. This is usually the real outage.
- **PDBs too strict** block recovery actions.
- **Cross-AZ dependency** — a service in AZ-a that everything called.

Verification: pod distribution per AZ, target group health per AZ, RDS failover event, error
rate. And the honest bit — the only way to know any of this works is to have tested it, so I'd
want an AZ-failure game day on the calendar."

**Follow-ups.** "How do you test this without an outage?" *(cordon+drain all nodes in one AZ,
or a chaos tool; NACL-block the AZ's subnets in a staging environment)* · "What about a single-AZ
EBS-backed database?" *(that's your RPO problem — Multi-AZ RDS or accept it explicitly)*

---

## S13. The platform has low developer adoption

**Tests:** whether you understand platform-as-product. **This is the archetype-6 signature
question and the one most technical candidates fumble.**

**Strong answer.** "Low adoption is a product failure, not a compliance failure, so I'd start by
finding out *why* rather than by escalating.

Diagnose: talk to five teams that aren't using it and ask what they do instead. Usually it's one
of — it's slower than what they had; it doesn't support something they need; onboarding
documentation assumes knowledge they don't have; they got burned once; or they simply don't know
it exists.

Then: pick the highest-friction step and remove it. Make the paved road *genuinely faster* than
the alternative — CNCF describes the mature state as '**intrinsic pull**' rather than
'extrinsic push' `[OFFICIAL, CNCF Platform Engineering Maturity Model, 2025]`. Migrate one
friendly team with hands-on help, measure before/after time-to-deploy, publish that internally,
and let other teams come to you.

Mandates I'd use sparingly and only for things that are genuinely non-negotiable — usually
security controls, and even then with a migration window and hands-on support.

And I'd instrument adoption from the start: % of services on the golden path, time-to-first-deploy,
support-ticket volume per team. If you can't measure adoption you can't manage it."

**Weak answers.** "Get management to mandate it." "Write better docs" as the whole answer.
Blaming developers.

**Follow-ups.** "What if a team refuses and their reason is good?" *(then the platform has a gap
— that's roadmap input, not a discipline problem)* · "How do you deprecate the old path?"
*(announce, migrate with help, set a date, make the last mile easy, and only then remove)*

---

## S14. A team bypasses the golden path

**Strong answer.** "First, find out why — the answer is almost always informative. There are
three cases:

1. **The platform genuinely can't do it.** That's a gap. Add it to the roadmap, and in the
   meantime support them off-road with an explicit, time-boxed exception.
2. **The platform can do it but they didn't know or it was too hard.** That's a docs/DX failure.
   Fix the friction, help them migrate.
3. **They just prefer their own thing.** Then I'd be clear about the deal: off the paved road,
   you own the operational burden — your own on-call, your own upgrades, your own compliance
   evidence. Most teams choose the paved road once that's explicit and non-punitive.

Non-negotiables — image scanning, no static credentials, audit logging — are enforced by
admission control regardless of the path, because those aren't preferences.

And I'd track exceptions with owners and expiry dates. An exception with no expiry is just a
second, undocumented platform."

**Follow-ups.** "What if it's the highest-revenue team?" *(the deal is the same, the support is
better; escalate on shared risk, not on rules)*

---

## S15. Secrets appear in CI logs

**Tests:** incident instinct. **The order of your first two actions is the whole question.**

**Strong answer.** "**Rotate first.** The moment a secret is in a log it is compromised — I don't
know who read it, and CI logs are widely readable and often retained externally. So: rotate the
credential, then investigate.

Then, in order: determine exposure (public repo? third-party log shipper? how long? who has
access?), delete or redact the logs, check for use of the credential via CloudTrail or the
provider's audit log, then write the postmortem.

Root cause is usually one of: `set -x` in a script, a tool echoing its config, a secret passed
as a command-line argument (visible in process listings *and* logs), an error handler dumping
the environment, or a secret that wasn't registered with the CI's masking mechanism.

Prevention: secrets only via the CI's secret store so masking applies, never as CLI args, OIDC
instead of long-lived credentials wherever possible (nothing to leak), secret scanning in
pre-commit and in CI, and short-lived credentials so the exposure window is bounded even when
this happens again — because it will."

**Weak answers.** Deleting the log and moving on. Investigating before rotating. Not considering
that masking only covers secrets the CI knows about.

**Follow-ups.** "It was in a public repo's Actions log for 3 days" *(assume automated scrapers
found it within minutes; full compromise response)* · "How do you prevent it structurally?"
*(OIDC federation — a token that lasts 15 minutes and is scoped to one repo is a much smaller
prize)*

---

## S16. A shared Kubernetes cluster has noisy-neighbour problems

**Strong answer.** "Identify: which resource is contended — CPU, memory, disk I/O, network,
or the API server itself? Symptoms differ. CPU shows as throttling with normal utilisation;
memory shows as evictions and OOMKills of *innocent* pods; API server shows as slow `kubectl`
for everyone.

Immediate: find the offender (`kubectl top`, throttling metrics, API server request metrics by
user agent — a badly written operator hammering the API is a classic), and isolate it, usually
by moving it to a dedicated node pool with taints.

Structural fixes:
- **Requests and limits mandatory**, enforced by admission policy. Without requests, the
  scheduler is blind and QoS is `BestEffort`, which is exactly what gets killed first.
- **ResourceQuota + LimitRange per namespace** so one team cannot consume the cluster.
- **Node pools by workload class** — batch on Spot with taints, latency-sensitive on dedicated
  nodes, with affinity rules in the platform's base chart.
- **PriorityClasses** so platform components and critical workloads win eviction contests.
- **API server**: client-side rate limiting, fix chatty controllers, watch not poll.

The honest trade-off: full isolation means separate clusters, which costs money and operational
effort. I'd rather push namespace isolation as far as it goes and reserve separate clusters for
genuinely different trust or compliance boundaries."

**Follow-ups.** "One team's Spark job takes every node" *(quota + dedicated pool + Karpenter
limits)* · "How do you charge them for it?" *(labels → OpenCost/Kubecost showback)*

---

## S17. NAT Gateway cost increases unexpectedly

**Strong answer.** "NAT is billed hourly per gateway plus per-GB processed, and the per-GB is
usually the spike. So: what started sending more data through it?

Investigate with **VPC Flow Logs**, filtered to the NAT ENI, aggregated by destination — that
tells you the answer directly, and it's the tool people forget.

Usual causes:
- **S3 or DynamoDB traffic without a gateway endpoint** — the top cause and the easiest fix.
  Gateway endpoints are *free*; there's no reason not to have them.
- **ECR image pulls** — a new deployment with large images pulling on every node scale-up. Fix
  with interface endpoints for `ecr.api`/`ecr.dkr` plus the S3 gateway endpoint for layers.
- Container image pulls from Docker Hub instead of ECR pull-through cache.
- A new data pipeline egressing to the internet.
- A cross-region replication job.
- Something in a retry loop.

Fixes ranked: S3/DynamoDB gateway endpoints (free, immediate) → interface endpoints for
high-volume AWS services (pay for themselves fast) → ECR pull-through cache → single NAT in
non-prod → check nothing is retrying pathologically.

Prevention: gateway endpoints in the standard VPC Terraform module so no VPC is ever built
without them, and a cost anomaly alarm on NAT usage type."

**Follow-ups.** "Cross-AZ costs also rose" *(NAT in one AZ serving all three — put a NAT per AZ,
and now trade off gateway-hours vs cross-AZ transfer)* · "Can you eliminate NAT?" *(for pure
AWS-service workloads, yes — endpoints only; for internet egress, no, though an egress proxy is
an option at scale)*

---

## S18. Terraform wants to recreate a critical resource

**Tests:** whether you can be trusted with production.

**Strong answer.** "I stop. I don't apply. Then I find out *why* the plan says replace — the
plan output names the attribute forcing replacement, marked `# forces replacement`.

Common causes:
1. **A ForceNew attribute changed** — availability zone, subnet, name, engine version on some
   resources. Sometimes because a *default* changed in a new provider version.
2. **Provider version upgrade** changed a default or a schema.
3. **Resource address changed** — someone renamed the resource or moved it between modules, or
   switched `count` → `for_each`. Terraform sees a delete plus a create. **Fix: `moved` blocks,
   or `terraform state mv`.** No infrastructure needs to change at all.
4. **`count` index shift** — an item removed from the middle of a list re-indexes everything
   after it. This is the strongest argument for `for_each` over `count`.
5. **Drift** — someone changed it in the console, and Terraform is 'correcting' it.

Mitigations: `lifecycle { prevent_destroy = true }` on stateful production resources, so this
fails at plan time rather than at 2 a.m.; `ignore_changes` for fields managed elsewhere; require
plan review in the PR with the destroy count called out; and never auto-apply a plan containing
a destroy of a stateful resource without a human approving it."

**Weak answers.** Applying and hoping. Using `-target` as a routine tool. Deleting from state
without understanding why.

**Follow-ups.** "It's the production database" *(prevent_destroy would have stopped this;
verify snapshots exist regardless; if it's a rename, `moved`; if it's genuinely a required
change, plan a migration with a maintenance window — never an in-place apply)* · "How do you
catch this before the PR?" *(plan in CI, comment the plan on the PR, and fail the build if the
destroy count > 0 without a labelled override)*

---

## How to practise this bank

1. **Cover the answer.** Read only the scenario. Talk for 5 minutes into a recorder.
2. **Compare.** Not for wording — for *structure*. Did you order your hypotheses? Did you say
   what would discriminate between them?
3. **Do it in the cluster.** Every scenario marked hands-on can be injected in `kind` or your
   lab EKS cluster. Break it, then fix it. This repo's Module 13 (troubleshooting playbook) and
   [11-day2-operations/node-drain-rebalance](../../11-day2-operations/node-drain-rebalance/README.md)
   exist for exactly this.
4. **Rotate.** Three scenarios per session, mixed topics. Spaced repetition beats cramming, and
   these decay fast.

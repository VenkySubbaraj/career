# Platform Engineer — Role, Stack, and Interview Preparation (2026–2027)

**Prepared:** 2026-09-10
**For:** ~6.5 yrs IT, AWS data engineering background, targeting Platform / DevOps Platform /
Cloud Platform / SRE-Platform roles in India (with global comparison).

---

## READ THIS FIRST — what this document is and is not

You asked for an evidence-based market study built on 90–150 job descriptions and a body of
first-hand interview reports. **That dataset was not collected, and this document does not
pretend otherwise.**

### Why

| Source you asked for | Status | Reason |
|---|---|---|
| LinkedIn Jobs, Naukri, Instahyre, Hirist, Cutshort, Wellfound | ❌ not collected | Login-walled / anti-automation. No verifiable retrieval. |
| Glassdoor JDs + interview reviews | ❌ not collected | Login-walled. |
| Blind, Levels.fyi listings | ❌ not collected | Login-walled. |
| Reddit / LeetCode Discuss / Medium interview reports | ❌ not collected | Retrievable in principle, but a handful of anecdotes cannot support the frequency claims you asked for, and you explicitly forbade presenting anonymous reports as representative. |
| CNCF, AWS, Stack Overflow Survey, official docs | ✅ **collected 2026-09-10** | See below. |

Your own quality constraints said: *"Do not fabricate metrics"* and *"no artificially precise
percentages from a small sample."* Manufacturing a table that says "Terraform: 87% of India
postings" would have been the single most damaging thing I could do to your preparation,
because you would then optimise against fiction. So:

> **Every market-frequency claim in this document is labelled `[INFERENCE]` and states its
> basis. There are no invented citations, no invented posting counts, and no invented
> salary figures anywhere in these files.**

### What *is* evidence-backed

| Label | Source | Retrieved | What it supports |
|---|---|---|---|
| `[OFFICIAL]` | [CNCF Platform Engineering Maturity Model](https://tag-app-delivery.cncf.io/whitepapers/platform-eng-maturity-model/), CNCF TAG App Delivery, © 2025 | 2026-09-10 | Definition of platform engineering; the 5 aspects (Investment, Adoption, Interfaces, Operations, Measurement); why "platform team" ≠ "renamed ops team" |
| `[OFFICIAL]` | [AWS EKS Best Practices Guide](https://docs.aws.amazon.com/eks/latest/best-practices/introduction.html) | 2026-09-10 | The exact domains AWS itself considers day-2 EKS competence: Security, Reliability, Karpenter, cluster-autoscaler, Auto Mode, Networking, Scalability, Cluster Upgrades, **Version Rollback**, Cost Optimization, Windows, Hybrid, AI/ML |
| `[SURVEY]` | [Stack Overflow Developer Survey 2025 — Technology](https://survey.stackoverflow.co/2025/technology) | 2026-09-10 | Tool prevalence **among all developers** (see caveat below) |
| `[SURVEY]` | [CNCF Annual Survey 2024](https://www.cncf.io/reports/cncf-annual-survey-2024/), published 2025-04-01, n=750 | 2026-09-10 | Cloud-native adoption trend; ~25% of respondents report nearly all dev/deploy is cloud native |

**Critical caveat on the Stack Overflow numbers:** they describe *all developers*, not platform
engineering job postings. Kubernetes at 28.5% of all developers is entirely compatible with
Kubernetes appearing in the large majority of Platform Engineer JDs. Do not read them as
hiring demand. They are a *floor* on ecosystem ubiquity, nothing more.

### How to close the gap yourself

[JD-TRACKER.md](JD-TRACKER.md) is a collection protocol + scoring sheet. Roughly 5 hours of
your own copy-paste produces a dataset that is *better* than anything I could have scraped,
because it is filtered to the exact companies you will actually apply to. Do it in week 1.

---

## 1. Executive summary

### What Platform Engineers actually do

A platform team's customer is **other engineers inside the company**, not end users. CNCF's
own framing: platform engineering is *"the practice of planning and providing computing
platforms to developers and users … their people, processes, policies and technologies"*
`[OFFICIAL, CNCF Maturity Model, 2025]`. The work is *product* work aimed at an internal
market that can refuse to adopt what you build.

In practice the job is four things in a ratio that varies by company:

1. **Run a paved road** — an opinionated, self-service path (repo template → CI → registry →
   GitOps → cluster → ingress → observability → alerting) that a product team can walk in a day.
2. **Operate the substrate** — Kubernetes/EKS, cloud accounts, networking, IAM, secrets,
   the CI system itself. Upgrades, capacity, incidents.
3. **Write software** — Terraform modules, Helm charts, controllers, CLIs, GitHub Actions
   reusable workflows, policies. Real code review, real tests.
4. **Sell and support** — docs, office hours, migrations, adoption metrics, saying no.

The CNCF maturity model is blunt that a team doing only #2 is a **Level 2 "dedicated team"** —
"often called DevOps, Engineering Enablement, Shared Tools, a Centre-Of-Excellence, or even
Platform … funded centrally and treated as cost centers; their impact … is not measured"
`[OFFICIAL, 2025]`. **Most jobs advertised as "Platform Engineer" are Level 2.** That is not a
criticism; it is the market you are entering, and it is good news for you, because Level 2 is
reachable from where you stand.

### The most employable stack

Ranked by *return on your learning hours*, not by fashion — `[INFERENCE]`, basis: composition
of the AWS EKS Best Practices domains `[OFFICIAL]`, the CNCF maturity model's "Interfaces" and
"Operations" aspects `[OFFICIAL]`, ecosystem ubiquity `[SURVEY]`, and the fact that every one
of these appears in the tooling required to operate the others.

**Tier 0 — you will be rejected without these:** Linux + Bash, TCP/IP + DNS + TLS + HTTP,
Git, Docker, **Kubernetes**, **Terraform**, **AWS (IAM, VPC, EKS)**, one CI system
(GitHub Actions or GitLab CI), Python.

**Tier 1 — separates offers from "we'll get back to you":** Helm, Argo CD (GitOps),
Prometheus + Grafana, IRSA / EKS Pod Identity, Karpenter, secrets (External Secrets +
Secrets Manager or Vault), SLOs and incident practice, AWS cost mechanics.

**Tier 2 — differentiators, not entry tickets:** Go, Kyverno/OPA, Backstage or Port,
Crossplane, service mesh / Cilium, multi-account Control Tower, supply-chain security
(Cosign/SLSA/SBOM).

**Do not learn yet:** Kubernetes operator authoring, eBPF internals, Istio depth, Spinnaker,
Kratix/Score/OAM. They are real, they are just not what gets you the first platform job.

### Your strongest transition path

**AWS Cloud Platform Engineer / DevOps Platform Engineer**, entering through your AWS depth.
Full reasoning in [07-resume-and-gap.md](07-resume-and-gap.md). The short version:

- You already own the thing most DevOps candidates fake: **production AWS IAM and VPC
  troubleshooting**, plus real Python that runs in production.
- You are missing exactly four things that are teachable in one focused quarter: **Kubernetes,
  Terraform, CI/CD + GitOps, observability.**
- Do **not** aim first at "Internal Developer Platform Engineer" at a platform-mature company.
  Those loops assume you have already operated a platform. Aim there for job #2.
- Do **not** default to "Data Platform Engineer" unless you want to stay in data. It is the
  easiest jump and the one most likely to leave you doing the same work with a new title.

**Seniority realism:** 6.5 years with zero production Kubernetes will be interviewed as a
*mid-level* Platform Engineer at strong product companies and as a *senior* at GCCs/service
companies. Pretending otherwise gets you failed in round 2, not hired at a higher band.

### The five preparation priorities

1. **Kubernetes to debugging depth** — not "I can apply a Deployment." *"A pod gets 403 from
   S3; walk me through it"* and *"deploy succeeded, clients get 503"* are the two questions
   that decide platform interviews. Everything else is negotiable.
2. **Terraform as a module author, not a consumer** — state, backends, locking, `moved`,
   `import`, drift, multi-account provider aliases. "I ran `terraform apply`" is not a skill.
3. **Rebuild your AWS knowledge around IAM policy evaluation and VPC data paths** — you have
   the raw material; convert it into whiteboard-level explanation, including IRSA vs Pod
   Identity and the private-subnet egress question.
4. **One end-to-end platform you built and can destroy/rebuild live** — [08-projects.md](08-projects.md)
   maps this onto the `sample-project/` (Shipyard) work already in this repo.
5. **Language for the platform-product half of the job** — adoption, golden paths, migration
   without mandate, error budgets. This is what makes you sound like a Platform Engineer
   rather than an ops engineer with a Kubernetes certificate.

---

## 2. Document map

| # | File | Covers your Part(s) |
|---|---|---|
| 00 | **00-START-HERE.md** (this file) | Exec summary, methodology, limitations |
| 01 | [01-role-taxonomy.md](01-role-taxonomy.md) | A — real responsibilities & deliverables · B — role comparison + **JD classifier** |
| 02 | [02-tech-stack.md](02-tech-stack.md) | C — technology map, depth, and your priority per item |
| 03 | [03-aws-competency.md](03-aws-competency.md) | D — AWS competency matrix, ranked by interview probability |
| 04 | [04-architectures.md](04-architectures.md) | E — 8 reference architectures with failure modes and trade-offs |
| 05 | [05-interview-process.md](05-interview-process.md) | F — interview loops by company type · H — coding expectations |
| 06 | [06-question-bank.md](06-question-bank.md) | G — question bank, foundation → staff, incl. all 18 of your scenarios |
| 07 | [07-resume-and-gap.md](07-resume-and-gap.md) | I — resume · J — personalised gap analysis & transition path |
| 08 | [08-projects.md](08-projects.md) | K — three portfolio projects, wired to this repo |
| 09 | [09-roadmaps.md](09-roadmaps.md) | L — 12-week, 20-week, 6-month plans |
| 10 | [10-readiness-and-syllabus.md](10-readiness-and-syllabus.md) | Readiness checklist (3 levels) + final ordered syllabus |
| — | [JD-TRACKER.md](JD-TRACKER.md) | The primary-evidence collection you should run yourself |

## 3. Evidence labels used throughout

`[OFFICIAL]` official docs / vendor-neutral foundation output ·
`[SURVEY]` survey with published methodology ·
`[CASE]` production case study ·
`[INFERENCE]` my conclusion, with basis stated ·
`[UNVERIFIED]` commonly repeated but I could not verify it — treat as a hypothesis to test
against your own JD-TRACKER data.

There are no `[JD]`, `[INTERVIEW]`, or `[COMMUNITY]` labels in this document, because I
collected no such sources. When you run [JD-TRACKER.md](JD-TRACKER.md), you will be able to
add them yourself — and you should, because they are the labels that matter most for your
specific market.

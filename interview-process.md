# 05 — Interview loops, by company type — and what coding is actually required

Covers your **Part F** (process and rounds) and **Part H** (coding expectations).

> **Evidence statement.** No first-hand interview reports were collected (see
> [00-START-HERE.md](00-START-HERE.md)). Everything below is `[INFERENCE]`: derived from the
> role archetypes in [01-role-taxonomy.md](01-role-taxonomy.md), the competency surface that
> the job objectively requires, and the structure of technical hiring generally. Likelihood is
> given in bands (`Almost always` / `Common` / `Sometimes` / `Rare`) — **never as a
> percentage**, because I have no sample to derive one from.
>
> Validate and correct this with your own data using the interview-tracker section of
> [JD-TRACKER.md](JD-TRACKER.md). Three reports from the same company type beat any general
> model, including this one.

---

## 1. Round catalogue

| Round | Likelihood (mid) | Likelihood (senior) | Duration (typical) | What it actually tests |
|---|---|---|---|---|
| Recruiter screen | Almost always | Almost always | 20–30 min | Fit, notice period, comp band, whether your CV is honest |
| Hiring-manager discussion | Almost always | Almost always | 30–45 min | Scope of your past work, ownership, communication, "why platform" |
| **Kubernetes** | Almost always (K8s roles) | Almost always | 45–60 min | Debugging depth vs YAML familiarity |
| **AWS / cloud** | Almost always | Almost always | 45–60 min | IAM + VPC reasoning, service selection, cost/DR judgement |
| **Linux + troubleshooting** | Common | Common | 30–60 min | Whether your abstractions have foundations |
| **Terraform / IaC** | Common | Almost always | 30–45 min | Author vs consumer (see §3) |
| **CI/CD + GitOps** | Common | Common | 30–45 min | Pipeline design, promotion, security of the pipeline |
| **Scripting (Bash/Python)** | Common | Common | 45–60 min | Practical automation, not algorithms |
| **System/platform design** | Sometimes | **Almost always** | 60 min | The whole job, compressed |
| Networking deep-dive | Sometimes | Common | 45 min | DNS/TLS/routing/L4-L7 |
| **SRE / incident** | Sometimes | Common (archetype 4) | 45 min | SLOs, on-call judgement, postmortem culture |
| Live debugging (broken cluster/repo) | Sometimes | Sometimes | 60–90 min | Everything at once. **Highest-signal round that exists.** |
| Take-home | Sometimes (startups) | Sometimes | 4–8 h + review | Code quality, docs, judgement about scope |
| DSA coding | Sometimes | Sometimes | 45–60 min | Mostly a filter; heavier at big-tech and archetype 7 |
| Go/systems coding | Rare | Sometimes (archetype 7) | 60 min | Real software engineering |
| Behavioural | Almost always | Almost always | 45 min | Conflict, influence without authority, failure ownership |
| Bar-raiser / cross-functional | Sometimes | Common (large cos) | 45–60 min | Consistency, values, senior judgement |

**Typical shapes** `[INFERENCE]`:

- **Indian service company / consultancy:** screen → 1–2 combined technical → managerial. Broad,
  shallow, tool-name-driven. Certifications carry unusual weight here.
- **Indian product / SaaS / fintech:** screen → HM → K8s+AWS deep-dive → Terraform/CI or coding
  → design → behavioural. Usually 4–5 rounds, often over 2–3 weeks.
- **GCC:** mirrors the global parent's loop, sometimes with an extra local screen. More
  process, more structured rubrics, more likely to include a bar-raiser-style round.
- **Startup (Series A–C):** screen → founder/lead technical → take-home or live build →
  culture. Fast, wide scope, "can you own everything" energy.
- **Global product company:** screen → 4–6 onsite-equivalent rounds including coding and design,
  plus a values/bar-raiser round. Longest loop, most consistent rubric.

---

## 2. Company-type comparison

`[INFERENCE]` — replace with your own observations as you interview.

| Company type | Typical role shape | Main tools | Coding expectation | On-call expectation | Interview focus |
|---|---|---|---|---|---|
| **Indian service / consultancy** | Archetype 1–2 | Jenkins, Ansible, Terraform, K8s (varies by client) | Bash + light Python | Often shift-based / 24×7 support model | Tool breadth, certifications, "have you worked on X" |
| **Indian product / SaaS** | Archetype 2–3 | AWS, EKS, Terraform, GitHub Actions/GitLab, Argo, Prometheus | Python required, Go a plus | Rotation, business-hours-weighted | K8s debugging, AWS depth, pipeline design |
| **Indian fintech** | Archetype 3–5 | Above + heavy IAM, audit, network segmentation, DR | Python required | Rotation, stricter SLAs | Security, compliance, DR, IAM, blast radius |
| **GCC** | Archetype 3–5 | Whatever the parent standardised on | Python; Go where the parent uses it | Follow-the-sun rotation | Structured rubric: design + coding + behavioural |
| **Cloud-native startup** | Archetype 2–3, very wide | Full modern stack, opinionated | Python/Go, you'll write real code | Small team = everyone on-call | Breadth, pragmatism, "ship it" judgement |
| **Infra/devtools startup** | Archetype 6–7 | Kubernetes internals, Go, controllers | **Go, seriously** | Yes | Systems design, controller/API design |
| **Global product co (India office)** | Archetype 4–7 | Internal platforms + open source | Real coding round, sometimes DSA | Formal rotation, comp'd | Coding + design + values, high bar on rigour |

---

## 3. Round-by-round: what "good" looks like

### 3.1 Kubernetes round

**Tests:** whether you have operated a cluster or only applied YAML to one.

**Representative questions**
- Walk me through what happens from `kubectl apply -f deployment.yaml` to a running container.
- A pod is `Pending`. Enumerate every cause, in the order you'd check them.
- Readiness vs liveness vs startup probe — and what happens if you get liveness wrong.
- Requests vs limits, QoS classes, and what actually gets evicted first under memory pressure.
- Deployment shows all pods Ready, users get 503. Debug it. *(See [06-question-bank.md](06-question-bank.md) §S5.)*
- How do you safely upgrade a cluster with traffic running?
- How would you isolate two teams on one cluster? Where does namespace isolation stop being enough?

**Strong answers:** name the *order of investigation* before the answer. Reference actual
commands and what output would discriminate between hypotheses. Say "I'd check X, and if it
showed Y I'd conclude Z" — that's diagnostic reasoning, and it's what's being graded.

**Failure modes:** reciting `kubectl` commands with no hypothesis; not knowing what a
readiness probe failure does to Endpoints; treating Kubernetes as magic above the container
layer; being unable to explain requests vs limits precisely.

**Seniority scaling:** mid = debug it. Senior = debug it, then say how you'd prevent the class
of failure platform-wide, and what you'd add to the paved road.

### 3.2 AWS round

**Tests:** IAM and VPC reasoning above all, then service selection and cost/reliability judgement.

**Representative questions**
- Design a VPC for a 3-tier app across 3 AZs. Now put EKS in it.
- Explain IAM policy evaluation with SCPs and permission boundaries in play.
- A pod gets `AccessDenied` calling S3 despite an Allow policy. *(Guaranteed. §S8 in the bank.)*
- Private subnet workload can't reach the internet. Ordered hypotheses?
- SG vs NACL. When would you ever use a NACL?
- How do you give CI access to AWS without storing keys?
- Our NAT Gateway bill tripled. Find out why.
- Design DR for this system. What's your RTO and how do you know?

**Strong answers:** draw. Always draw. Then reason about the *data path* and the *identity
path* as separate things. Quantify when you can ("a NAT gateway is charged hourly plus per-GB
processed, so any S3 traffic from a private subnet is paying twice unless there's a gateway
endpoint").

**Failure modes:** knowing service names but not the failure modes; not knowing that NAT is
outbound-only; confusing trust policy with permission policy; no opinion on cost.

**Your advantage:** you have production AWS IAM/VPC/endpoint troubleshooting. Most candidates
for platform roles do not. Lead with it.

### 3.3 Terraform / IaC round

**Tests:** the consumer/author gap in [02-tech-stack.md](02-tech-stack.md) §5.

**Representative questions**
- How is state stored, and what breaks without locking?
- The lock is held by a job that died. What do you do?
- `plan` wants to replace the production database. What now?
- You need to split a 4,000-resource state. How, without downtime?
- How do you test a module? How do you release a breaking change to 60 consumers?
- `count` vs `for_each` — when does `count` hurt you?
- How do you deploy to 12 accounts from one pipeline?
- How do you detect and handle drift?

**Strong answers:** talk about *blast radius* and *reversibility*. Mention `moved`, `import`,
`-target` as an emergency-only tool, `prevent_destroy`, and reviewing plan output in PRs.

**Failure modes:** "I just run apply"; not knowing what state is *for*; suggesting
`force-unlock` without checking the job is dead; no answer on module versioning.

### 3.4 CI/CD + GitOps round

**Representative questions**
- Design a pipeline from commit to production for 40 services.
- How do you promote between environments? Why not rebuild per environment?
- How does the pipeline authenticate to AWS and to the cluster?
- Push vs pull deployment — argue both sides.
- How do you do a canary, and what automatically aborts it?
- A secret appeared in a build log. Walk me through the next hour.
- How do you keep 40 pipelines from drifting apart?

**Strong answers:** *build once, promote the digest*. OIDC over static keys. CI has no cluster
credentials. Reusable workflows as a platform product with versioning. Rollback is a first-class
path, not an afterthought.

### 3.5 System / platform design round — the one that decides senior offers

**Prompts you should be able to handle cold:**
- "Design an internal platform for 200 engineers on AWS."
- "Design multi-tenant Kubernetes for 30 teams."
- "Design the deployment system for a company doing 200 deploys/day."
- "Design self-service infrastructure provisioning."
- "We're one account with everything in it. Design the migration to multi-account."
- "Design observability for a 100-service estate."

**Use the 8-step template in [04-architectures.md](04-architectures.md).** Then:

**Strong answers** state the user and the success metric *first* ("the goal is a new service in
production in under a day, and I'd measure time-to-first-deploy"); separate control plane from
data plane explicitly; name what they are deliberately *not* building; give failure modes
unprompted; and end with "at 10× this breaks first, here's why."

**Failure modes:** tool-listing instead of designing; no failure modes; no cost awareness;
gold-plating (multi-region for a 5-person startup); never mentioning the humans who have to
adopt this.

### 3.6 SRE / incident round

- "Define an SLI, SLO and error budget for a checkout API."
- "You're paged: p99 latency is 4s, error rate is normal. What do you do first?"
- "How do you run a postmortem? What makes it blameless and why does that matter?"
- "What's your error-budget policy when the budget is exhausted?"
- "How do you reduce alert fatigue?"

**Strong answers:** stabilise before diagnosing (mitigate, then investigate); communicate early;
one incident commander; timeline as you go, not after; action items with owners and dates. Say
that a postmortem's purpose is to change the *system*, not to find who typed the command.

### 3.7 Behavioural round

Prepare 6 stories in STAR form, each reusable for several questions:

1. A production incident you caused or fixed — **include what you got wrong**
2. Influencing a team to adopt something without authority
3. Disagreeing with a senior person, and the outcome
4. A project you scoped down or killed
5. Something you automated that removed your own toil
6. Learning a technology under deadline pressure

**The question that separates candidates:** "Tell me about something you built that nobody
used." Have a real answer. It demonstrates that you understand platform work is adoption work.

---

## 4. Coding expectations (Part H)

### 4.1 How much, and what kind

`[INFERENCE]`, by archetype (from [01-role-taxonomy.md](01-role-taxonomy.md) §7):

| Archetype | DSA round | Practical coding round | Language |
|---|---|---|---|
| 1 Ops renamed | Rare | Sometimes (Bash) | Bash |
| 2 CI/CD DevOps | Rare | Common (scripting) | Bash + Python |
| 3 K8s infra | Sometimes | Common | Python, read Go |
| 4 SRE-titled | Sometimes–Common | Almost always | Python or Go |
| 5 Cloud governance | Rare | Common (boto3) | Python |
| 6 True IDP | Sometimes | Common | Python/TypeScript/Go |
| 7 SWE-heavy platform | **Common** | Almost always | **Go** |

**Realistic DSA bar by company category:**

| Category | DSA reality |
|---|---|
| Indian service company | Effectively none |
| Indian product / SaaS / fintech | Easy–low-medium: strings, hash maps, sorting, two pointers |
| GCC | Medium, mirroring the parent's bar |
| Startup | Usually replaced by a take-home or live build |
| Global big tech | Medium LeetCode is real, plus design and behavioural |
| Infra/devtools company | Medium, but weighted toward systems reasoning and concurrency |

**Practical implication for you:** budget roughly 3–4 hours/week on practical coding and
*only* enough DSA to not be embarrassed — arrays, strings, hash maps, sorting, two pointers,
binary search, basic recursion. Do not spend three months on dynamic programming for a platform
role. `[INFERENCE]`

### 4.2 The exercises you should actually be able to do

Do each in **Python first**, then repeat the starred ones in **Go** later.

| # | Exercise | Tests | ★ |
|---|---|---|---|
| 1 | Parse a log stream; compute error rate per minute per endpoint; print the top 5 | Streaming, dicts, time bucketing | |
| 2 | Retry with **exponential backoff + full jitter**, capped, only on retryable errors | Understanding of thundering herd | ★ |
| 3 | HTTP health-check service with `/healthz`, `/readyz`, `/metrics` and **graceful SIGTERM shutdown** | The K8s contract, in code | ★ |
| 4 | Query AWS via boto3 (paginated) and report: unencrypted buckets / untagged instances / IAM keys older than 90 days | Real platform work, pagination, throttling | |
| 5 | Detect Terraform drift: run `plan -json`, parse it, exit non-zero if changes, summarise by resource type | Tooling composition, exit codes | |
| 6 | List pods across namespaces via the K8s API and report those without resource limits | API client, filtering | ★ |
| 7 | **Bounded concurrency:** process 10,000 items with exactly N workers, collect errors, don't lose results | The single most common practical question | ★ |
| 8 | Process a 10 GB file counting matches, in constant memory | Generators/streaming; O(1) memory | |
| 9 | CLI with subcommands, flags, `--dry-run`, proper exit codes, `--json` output | Tool design | ★ |
| 10 | Validate YAML against a JSON Schema; report errors with file+line | Config validation, UX of error messages | |
| 11 | Rate limiter (token bucket) | Concurrency + algorithm | ★ |
| 12 | Given code with a race, find and fix it | Concurrency debugging | ★ |
| 13 | Parse `kubectl get events -o json` and cluster them by reason/object | Data shaping | |
| 14 | Walk a directory of Helm charts and assert every one sets `securityContext.runAsNonRoot` | Policy-as-script | |

**What graders look for beyond correctness:** error handling that doesn't swallow the error;
sensible logging; the timeout and the retry; a `--dry-run`; testability; and — genuinely —
whether you asked a clarifying question before writing.

### 4.3 Bash — the round people fail because they didn't prepare it

You will be asked to write or read Bash. Know cold:

- `set -euo pipefail` and **why each flag matters** (and where `-e` doesn't save you)
- Quoting: `"$var"` vs `$var` vs `"${arr[@]}"`; why unquoted variables break on spaces
- `trap` for cleanup on EXIT/ERR
- `[[ ]]` vs `[ ]`, arithmetic `(( ))`
- Streams: `2>&1` vs `>&2`, `/dev/null`, here-docs, process substitution `<(...)`
- Pipelines: `grep`/`awk`/`sed`/`sort`/`uniq -c`/`cut`/`tr`/`xargs -P`
- Exit codes, `$?`, `${PIPESTATUS[@]}`
- `jq` at working depth — `select`, `map`, `-r`, `.[]` — it comes up constantly

**Classic tasks:** "find the top 10 IPs in an access log", "delete Evicted pods across all
namespaces", "retry a curl until it returns 200 with a timeout", "rotate logs older than N days
safely".

---

## 5. Preparing to *perform*, not just to know

The gap between "I know Kubernetes" and "I interview well on Kubernetes" is entirely about
verbalised reasoning under time pressure. Three habits close it:

1. **Narrate every lab.** Talk out loud while you work, as if explaining to a colleague. It
   feels ridiculous and it is the highest-ROI practice technique that exists for these loops.
2. **Predict before you run.** Write the expected output first. Every wrong prediction is a
   real gap you'd otherwise never have found. (This repo's [ROADMAP.md](../../ROADMAP.md)
   already prescribes this — it applies doubly to interview prep.)
3. **Timebox mock scenarios.** 10 minutes per debugging scenario in
   [06-question-bank.md](06-question-bank.md), spoken, no notes. Record yourself once; you will
   immediately hear the difference between reasoning and reciting.

**Two answer templates worth memorising:**

> **Debugging:** "First I'd establish blast radius — one pod, one service, or everything?
> Then I'd form hypotheses in likelihood order: A, B, C. I'd check A first because
> `<command>` distinguishes it in one step. If it shows X, I'd move to B…"

> **Design:** "Before I design — who are the users, what's the scale, and what does success
> look like? … Here's the happy path. Here's the control plane vs the data plane. Here's how
> identity flows. Here's what breaks first, and here's what I'd deliberately not build yet."

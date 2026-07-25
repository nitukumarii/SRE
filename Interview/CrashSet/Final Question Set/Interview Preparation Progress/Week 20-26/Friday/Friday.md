# WirelessCar SRE Technical Round Preparation Report (Rebuilt)

## Date: 25/07/2026
## Category: Service Reliability Engineering
## Interview Stage: Technical Round

---

# Daily Goal

Prepare for the WirelessCar Technical Interview by focusing on the highest priority
topics from the JD, using **real experience where you have it** and a **structured
reasoning framework** everywhere you don't. No invented incidents — everything here
is either true or clearly framed as "here's how I'd approach it."

---

# Priority Based on JD

| Priority | Topic |
|----------|-------|
| ⭐⭐⭐⭐⭐ | Production Support & Incident Management |
| ⭐⭐⭐⭐⭐ | Linux Troubleshooting |
| ⭐⭐⭐⭐⭐ | Kubernetes Troubleshooting |
| ⭐⭐⭐⭐⭐ | RCA / PIR / Problem Management |
| ⭐⭐⭐⭐⭐ | Monitoring & Observability |
| ⭐⭐⭐⭐ | AWS Fundamentals |
| ⭐⭐⭐⭐ | REST API Troubleshooting |
| ⭐⭐⭐⭐ | Automation (Ansible, Bash) |
| ⭐⭐⭐⭐ | Jenkins / CI-CD |
| ⭐⭐⭐ | Kafka |
| ⭐⭐⭐ | Database Troubleshooting |
| ⭐⭐⭐ | Docker / Helm |
| ⭐⭐⭐⭐ | **Real-time System / Reliability Design** (new) |

---

# Answer Framework (use this to fill every "Framework" row below)

## Production/Troubleshooting Scenarios
1. Understand business/customer impact and scope (who/what/how much).
2. Check monitoring dashboards — metrics first, before touching anything.
3. Identify the affected layer (app / infra / network / dependency / DB).
4. Collect logs and evidence, correlate with recent changes.
5. Form hypothesis, test cheapest one first.
6. Apply fix / mitigation (separate from full root cause).
7. Validate recovery against the original metric that showed impact.
8. RCA and prevention — document, assign owner, close the loop.

## Automation Questions
1. What manual/error-prone problem does this solve.
2. Why this tool (Bash vs Ansible vs Terraform — matched to the job).
3. How it's implemented (rough steps, not memorized syntax).
4. How you'd validate it's safe (dry-run, staging, blast radius limits).
5. Benefit (time saved, error reduction, consistency).
6. What you'd improve next.

## Real-Time Design Questions
1. Clarify scope and constraints (scale, SLA, budget, existing stack).
2. State assumptions explicitly.
3. Draw the high-level flow (components, not deep implementation).
4. Call out failure points and how you'd handle each.
5. Address monitoring/observability for what you designed.
6. Address how you'd roll it out safely (phased, canary, rollback plan).

---

# Question Tracker — Production Support & Incident Management

| No | Question | Status | Source |
|----|----------|--------|--------|
| 1 | Tell me about yourself (SRE-focused) | Build tonight | Real background + framing |
| 2 | Walk through your production environment / stack | Build tonight | Real (your actual infra: EC2, networking, Jenkins, monitoring) |
| 3 | Explain your responsibilities in production support | Build tonight | Real |
| 4 | Walk me through a real incident you handled (monitoring-led triage) | ✅ Ready | **Real — your Story 1** |
| 5 | Application outage troubleshooting | Framework | S.S.S.H.T.F.P. |
| 6 | Application latency troubleshooting | Framework | S.S.S.H.T.F.P. |
| 7 | High CPU troubleshooting | Framework | `top`, `ps -ef`, `vmstat` walkthrough |
| 8 | High Memory troubleshooting | Framework | `free -m`, per-process check, leak vs spike |
| 9 | Disk Full troubleshooting | Framework | `df -h`, `du -sh`, log rotation angle |
| 10 | DNS troubleshooting | Framework | `nslookup`/`dig`, resolver config |
| 11 | Database timeout troubleshooting | Framework | connection pool, slow query, lock contention |
| 12 | REST API troubleshooting | Framework | status code triage (4xx vs 5xx), latency vs error split |
| 13 | Load Balancer unhealthy targets | Framework | health check config, target group, app-level check |
| 14 | Failed production deployment | Framework | rollback vs fix-forward decision criteria |
| 15 | Rollback strategy | Framework | automated trigger, criteria, blast radius |
| 16 | RCA process | Build tonight | Real (you already described this well) |
| 17 | Post Incident Review (PIR) | Framework | blameless, action items with owners |
| 18 | Major Incident Management | Framework | incident commander role, comms cadence |
| 19 | Incident Commander responsibilities | Framework | coordinate not fix, single source of truth |
| 20 | Change Management | Framework | risk assessment, approval, rollback plan pre-agreed |
| 21 | Problem Management | Framework | pattern detection across repeated incidents |
| 22 | Reduce MTTR | Framework | better alerting, runbooks, faster escalation |
| 23 | SLI vs SLO vs SLA | Definition | be crisp: SLI=measurement, SLO=internal target, SLA=contractual promise |
| 24 | Error Budget | Definition | allowed unreliability before freezing risky changes |
| 25 | SRE Toil | Definition | manual, repetitive, automatable work with no lasting value |
| 26 | Preventing recurring incidents | Framework | problem management + tracked action items |

---

# Question Tracker — Automation

| No | Question | Status | Source |
|----|----------|--------|--------|
| 27 | Why automation? | Framework | reduce toil, reduce human error, consistency at scale |
| 28 | Automation example from your project | ✅ Ready | **Real — your Jenkins triage story, reframed as "what I'd automate from this"** |
| 29 | What operational activities can you automate? | Framework | patching, log cleanup, disk alerts, restarts, backups |
| 30 | Bash vs Python | Definition | Bash = quick glue/system tasks, Python = complex logic/APIs/readability at scale |
| 31 | Bash automation examples | Framework | disk check script, log cleanup, service restart |
| 32 | Explain Ansible | Definition | agentless config management, declarative, idempotent |
| 33 | Explain Inventory / Playbook / Roles | Definition | inventory=hosts, playbook=tasks, roles=reusable structured playbooks |
| 34 | Automate patching using Ansible | Framework | inventory group, patch module, reboot handler, batch/serial rollout |
| 35 | Restart service only when config changes (Handlers) | Framework | Ansible handler triggered by `notify`, runs once at end |
| 36 | Terraform workflow | Definition | init → plan → apply → state tracks real infra |
| 37 | Terraform state & remote backend | Definition | shared state via S3+lock table, prevents team conflicts |
| 38 | Terraform modules | Definition | reusable infra blocks, DRY principle |
| 39 | Infrastructure drift | Framework | `terraform plan` detects, reconcile via apply or import |
| 40 | CI/CD pipeline end-to-end | Build tonight | Real (Jenkins, git — describe your actual pipeline stages) |
| 41 | Jenkins pipeline | Build tonight | Real |
| 42 | Build succeeds, deploy fails | ✅ Ready | **Real — your Story 2 (build failure triage: stale build, plugin, dependency, code)** |
| 43 | Jenkins agent goes offline | Framework | check agent connectivity, resource exhaustion, restart agent |
| 44 | Blue-Green vs Canary deployment | Definition | Blue-Green = full switch/instant rollback; Canary = gradual %, safer for risk-averse rollout |
| 45 | Rollback after failed deployment | Framework | automated trigger on error-rate/health check failure |

---

# Real-Time System / Reliability Design Questions (New — Your Strongest Ground)

These are scored on reasoning and structure, not memorized facts — good place to build genuine confidence tonight.

| No | Question | Approach |
|----|----------|----------|
| 46 | Design a monitoring & alerting strategy for a new microservice | Define SLIs first (availability, latency, error rate) → set SLOs → alert on customer-facing symptoms, not every internal metric → route alerts by severity |
| 47 | Design a zero-downtime deployment pipeline | Rolling or blue-green → readiness gates → automated post-deploy health checks → auto-rollback on SLO breach |
| 48 | Design an incident escalation process for a small on-call team | Tiered escalation (primary → secondary → manager), clear time-to-escalate thresholds, single incident commander role even under a small team |
| 49 | Design a highly-available deployment on AWS (EC2-based) | Multi-AZ, load balancer with health checks, auto-scaling group, decouple stateful components (DB/cache) from instances |
| 50 | Design a safe automated patching process for 500+ servers | Batch/canary rollout (patch 5% → validate → continue), maintenance windows, automated rollback plan, exclude critical hosts from first wave |
| 51 | Design a CI/CD pipeline with safety gates | Lint/test → build → deploy to staging → automated smoke test → manual/automated gate → canary prod → full rollout, each gate blocking on failure |
| 52 | Design a disaster recovery approach for a production database | Regular automated backups + tested restore process, replication for failover, defined RTO/RPO targets agreed with business |

---

# Commands to Revise

## Linux
```bash
top
ps -ef
free -m
vmstat
iostat
sar
journalctl
df -h
du -sh
ss -tulnp
lsof
systemctl
```

## Kubernetes
```bash
kubectl get pods
kubectl get nodes
kubectl describe pod
kubectl logs
kubectl get events
kubectl top pods
kubectl top nodes
kubectl rollout status
kubectl rollout undo
```

## AWS / Networking (JD-relevant)
```bash
# EC2 / networking checks
dig / nslookup
traceroute
netstat -tulnp
curl -v <endpoint>       # check response code, TLS, latency
aws ec2 describe-instances
aws elb describe-target-health
```

---

# Communication Focus

Today's focus is **communication**, not learning new tools.

Practice:
- Keep answers between **60–90 seconds**.
- Start every answer with a clear approach: *"Let me walk through this systematically..."*
- Avoid filler words: so... / basically... / actually... / like...
- Speak with confidence and pause before answering — don't fill silence.
- End every answer with: **Validation → Prevention → Business impact.**
- When something isn't real hands-on experience, say so in one sentence and pivot
  straight to reasoning: *"I haven't run this at large scale personally, but here's
  how I'd approach it..."* — don't dwell, don't apologize twice.

---

# Practice Status Legend

✅ Ready (grounded in real experience)
Build tonight — real experience exists, needs shaping into a spoken answer
Framework — no direct hands-on experience, use the structured reasoning approach
Definition — concept question, just needs a crisp, confident definition

---

# Confidence Scale

5/10 → Understand concept
7/10 → Can explain
8/10 → Interview ready
9/10 → Senior-level explanation
10/10 → Can handle follow-up questions confidently

---

# End of Tonight's Target

- Turn the 4 "Build tonight" rows into 90-second spoken answers using your real background.
- Practice the two ✅ Ready real stories until they're smooth, since these are your
  strongest, most defensible answers under follow-up.
- Get comfortable with the Real-Time Design section — these reward structured
  thinking and are genuinely your best opportunity to sound senior regardless of
  tool-specific experience.
- Drill the Framework rows using the Answer Framework at the top — you don't need
  to memorize each one separately, just apply the same structure every time.

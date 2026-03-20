# Implementing Secure DevOps (DevSecOps)

**Module 7 — Secure DevOps**

---

## The problem we're solving

Security and speed don't have to be opposites—if we automate and embed security.

- <span class="fragment">❌ How do I add security to our pipeline?</span>
- <span class="fragment">❌ How do I secure containers and cloud workloads?</span>
- <span class="fragment">❌ What do we do when something goes wrong?</span>

**Today:** Security automation, container and cloud security, incident response and monitoring.

---

## Why this matters

- <span class="fragment">Modules 3-6 taught how vulnerabilities happen, how to fix them, and how to test for them.</span>
- <span class="fragment">This module makes those checks routine with automation (default team behavior, not heroics).</span>

---

## Secure DevOps = four things (backbone)
> The goal is fast, reliable security decisions. Not maximum scanning.

- <span class="fragment"><strong>Prevent</strong> bad changes from shipping</span>
- <span class="fragment"><strong>Reduce blast radius</strong> in runtime</span>
- <span class="fragment"><strong>Detect</strong> problems quickly</span>
- <span class="fragment"><strong>Respond</strong> consistently</span>

---

## What "DevSecOps" means in practice

- <span class="fragment">Security checks run **where code changes happen** (local dev, PR, CI/CD).</span>
- <span class="fragment">Developers get feedback early (minutes), not weeks later.</span>
- <span class="fragment">Risk is managed with policy: what blocks, what warns, what gets tracked.</span>
- <span class="fragment">Ops/Sec/Dev share ownership of delivery and response.</span>

---

Rule of thumb: **shift left for detection, shift right for monitoring**.

---

## Pipeline thinking

**Six places to add security:**

- <span class="fragment"><strong>Commit / PR:</strong> SAST, secrets scanning, lint/policy checks</span>
- <span class="fragment"><strong>Build:</strong> Dependency scan (SCA), SBOM generation</span>
- <span class="fragment"><strong>Test:</strong> DAST/API tests, IaC checks</span>
- <span class="fragment"><strong>Package:</strong> Container image scanning, signing/attestation</span>
- <span class="fragment"><strong>Deploy:</strong> Admission/policy checks, config validation</span>
- <span class="fragment"><strong>Run:</strong> Runtime monitoring, alerting, incident response</span>

---

<span class="fragment">Security checks are more effective when they run close to the moment risk is introduced.</span>

<span class="fragment"><strong>Takeaway:</strong> Commit / PR = fastest feedback • Build / package = supply chain + artifact confidence • Deploy / run = policy enforcement + live detection</span>

---

<span class="fragment">You do not need "all at once", start with high-value basics and iterate.</span>

---

## Minimum viable secure pipeline (centrepiece)
if you do only four things first:

- <span class="fragment"><strong>Secrets scanning</strong> on commits + PRs.</span>
- <span class="fragment"><strong>SCA</strong> on every PR/build (dependency CVEs).</span>
- <span class="fragment"><strong>SAST</strong> on changed code paths (or at least nightly full scans).</span>
- <span class="fragment"><strong>A written warn/block policy</strong> (what blocks/warns + who can override).</span>

This gives meaningful coverage without overwhelming teams with noise.

---

## Secure pipeline policy: warn vs block

> “Block when the risk is clear and the action is reversible. Warn when context is needed.”

---

- <span class="fragment"><strong>Leaked secret:</strong> Block immediately</span>
- <span class="fragment"><strong>Critical CVE in runtime dependency:</strong> Block (unless approved exception)</span>
- <span class="fragment"><strong>High SAST with confirmed exploit path:</strong> Block</span>
- <span class="fragment"><strong>Medium findings / uncertain context:</strong> Warn + ticket + SLA</span>
- <span class="fragment"><strong>Informational / low confidence:</strong> Report only</span>

---

<span class="fragment">Policy should be explicit, documented, and visible to the team.</span>

---

## Reducing security noise (so teams decide faster)

- <span class="fragment">Baseline existing issues; fail builds on **new** high-risk findings.</span>
- <span class="fragment">Deduplicate findings across tools (same root cause, multiple alerts).</span>
- <span class="fragment">Track false positives and suppress with justification.</span>
- <span class="fragment">Measure "time to triage" and "time to remediate", not just count of alerts.</span>

The point is fast, reliable security decisions, not maximum alert volume.

---

## Dependency and supply chain security

- <span class="fragment">Dependencies are code you did not write but still ship.</span>
- <span class="fragment">Automate updates (Dependabot/Renovate) and test quickly.</span>
- <span class="fragment">Generate an SBOM (inventory of what is inside your application).</span>
- <span class="fragment">Verify package integrity/signatures where supported</span>

---

Attack surface includes your direct and transitive dependencies.

---

## Container security: build-time controls

- <span class="fragment">Use minimal base images and pin versions; avoid <code>latest</code> tags.</span>
- <span class="fragment">Scan images during build; break on critical findings.</span>
- <span class="fragment">Run as non-root; drop unnecessary Linux capabilities.</span>
- <span class="fragment">Do not bake secrets into images or layers; avoid installing debugging tools into production images.</span>

Secure image + vulnerable runtime config still equals risk.

---

## Container security: runtime controls

- <span class="fragment">Read-only root filesystem where possible.</span>
- <span class="fragment">Restrict network egress and service-to-service access.</span>
- <span class="fragment">Set CPU/memory limits to reduce abuse blast radius.</span>
- <span class="fragment">Monitor for unusual process and network behavior.</span>

Think in terms of **blast radius reduction**, not "perfect prevention."

---

<span class="fragment">Mental model: assume the container may be compromised; what can it still do?</span>

---

## Cloud security: too much access / exposure / little visibility
Most cloud security failures come from three things:

- <span class="fragment"><strong>Too much access</strong> → over-permissive IAM roles ("*:*" grants)</span>
- <span class="fragment"><strong>Too much exposure</strong> → public buckets / public DB; broad secrets exposure</span>
- <span class="fragment"><strong>Too little visibility</strong> → disabled logs / weak auditing</span>

---

Most cloud incidents are configuration and identity mistakes, not zero-days.

---

## Identity and secrets in DevOps workflows
<span class="fragment">These reduce <strong>too much access</strong> and <strong>too much exposure</strong> (especially if CI is compromised).</span>

- <span class="fragment">Prefer short-lived credentials and federated identity (OIDC/workload identity).</span>
- <span class="fragment">Keep secrets in vault/secret store, not in repo or CI vars when avoidable.</span>
- <span class="fragment">Rotate credentials automatically where possible.</span>
- <span class="fragment">Apply least privilege to CI runners and deployment identities.</span>

If CI is compromised, these controls limit impact.

---

## IaC and policy-as-code

- <span class="fragment">Treat infrastructure changes like application code (PRs, review, tests).</span>
- <span class="fragment">Run IaC scanners (Terraform/Kubernetes/Cloud templates) in CI.</span>
- <span class="fragment">Encode guardrails as policy (e.g., no public DB, encryption required) to prevent repeat mistakes.</span>
- <span class="fragment">Use exceptions with expiry dates and owner accountability.</span>

Policies catch recurring mistakes before deployment.

---

## Monitoring: what should make a team suspicious in production?

- <span class="fragment">Repeated login failures or strange auth patterns.</span>
- <span class="fragment">Sudden access to data across accounts or tenants.</span>
- <span class="fragment">Unexpected outbound calls from an app or container.</span>
- <span class="fragment">Security config changes nobody expected.</span>
- <span class="fragment">Crash loops or weird behaviour after a new deployment.</span>

---

Detection complements prevention; you need both.

---

## Incident response for developers (practical)

1. <span class="fragment">Detect & confirm: confirm severity and evidence (is it real?).</span>
2. <span class="fragment">Triage: scope + impact + which systems/accounts to prioritise.</span>
3. <span class="fragment">Contain: disable affected paths/keys/features quickly.</span>
4. <span class="fragment">Eradicate & recover: patch, redeploy, rotate credentials.</span>
5. <span class="fragment">Learn: post-incident review with concrete actions.</span>

---

> “A good incident response process is part of engineering quality, not just a security concern.”

<span class="fragment">Have runbooks before incidents, not during incidents.</span>

---

## Security runbook essentials

- <span class="fragment">Who is on call and who has decision authority?</span>
- <span class="fragment">What logs/metrics do we check first?</span>
- <span class="fragment">How do we quickly disable risky functionality?</span>
- <span class="fragment">How do we communicate status internally and externally?</span>

- <span class="fragment">What decision thresholds trigger escalation?</span>

<span class="fragment">Examples: customer data exposure suspected • credential leak confirmed • production auth bypass observed</span>

Fast response depends on clarity, not heroics.

---

## Metrics that actually help
<span class="fragment">“Measure whether the team is getting faster at making and resolving security decisions.”</span>
- <span class="fragment"><strong>Time to triage:</strong> detection + ownership speed</span>
- <span class="fragment"><strong>Time to remediate:</strong> how quickly risk is reduced</span>
- <span class="fragment"><strong>% repos with baseline controls:</strong> rollout maturity</span>
- <span class="fragment"><strong>Security exceptions older than SLA:</strong> policy debt beyond expiry</span>

<span class="fragment">Avoid vanity metrics like "number of scans run."</span>

---

## Exercise ideas (pick 1-2)

- <span class="fragment"><strong>Best two exercises:</strong></span>
- <span class="fragment">Define a warn/block policy for five sample findings.</span>
- <span class="fragment">Create a minimum runbook for “suspected credential leak”.</span>
- <span class="fragment">Optional (if time): map your current pipeline and place security checks at each stage.</span>

Output should be something teams can use next sprint.

---

## 30-day implementation plan (starter)

Week 1:
- <span class="fragment">Enable SCA + secrets scanning in CI.</span>

Week 2:
- <span class="fragment">Set warn/block policy and baseline existing findings.</span>

Week 3:
- <span class="fragment">Add container scan + minimal hardening checks.</span>

Week 4:
- <span class="fragment">Run one incident tabletop and capture actions.</span>

Small, consistent steps beat a one-time "security sprint."

---

## What we covered

1. <span class="fragment">How to embed security controls through the delivery pipeline.</span>
2. <span class="fragment">Container, cloud, secrets, and identity controls in DevOps workflows.</span>
3. <span class="fragment">Monitoring and incident response as part of normal engineering operations.</span>
4. <span class="fragment">A practical rollout approach with policy and metrics.</span>

---

## Key takeaways (Module 7)

1. <span class="fragment">Prevent bad changes from shipping with a minimum viable secure pipeline + written warn/block policy.</span>
2. <span class="fragment">Reduce blast radius in runtime with container controls and least-privilege/IAM hygiene.</span>
3. <span class="fragment">Detect problems quickly with developer-centred monitoring triggers.</span>
4. <span class="fragment">Respond consistently with incident steps, runbooks, and cycle-time metrics for decisions.</span>

---

## Bridge to Module 8

- <span class="fragment">You now have a delivery pipeline mindset for managing software risk continuously.</span>
- <span class="fragment">Next: Module 8 — **LLM Security**: prompt injection, data leakage, model misuse, and governance for AI-assisted development.</span>
- <span class="fragment">We will apply the same DevSecOps thinking to AI features and AI tooling in your workflow.</span>

---

## Questions?

*Implementing Secure DevOps (DevSecOps) — Module 7*

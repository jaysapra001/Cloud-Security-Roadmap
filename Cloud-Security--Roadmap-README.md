# The Complete Cloud Security Roadmap: From Beginner to Cloud Security Expert

> A structured, hands-on path from cloud fundamentals to cloud security engineering, detection, incident response, DevSecOps, and architecture.

Most people who try to learn cloud security do not fail because there is too little material. They fail because there is too much of it, arriving in the wrong order. You open a browser, search for "how to learn cloud security," and within an hour you are staring at CSPM, CWPP, CIEM, DSPM, KSPM, CNAPP, three cloud providers, a dozen certifications, forty open-source tools and a stack of PDFs from NIST and the Cloud Security Alliance. Nothing tells you what depends on what. So people bounce between Kubernetes tutorials and IAM policies and Terraform scanners, collecting tabs instead of skills, and quietly conclude the field is beyond them.

It usually isn't. The problem is sequence, not capability.

This article is a long-form walkthrough of a public GitHub roadmap — jaysapra001/Cloud-Security-Roadmap — that tries to fix exactly that problem. The repository is a single, densely structured README.md built as an ASCII tree: fifteen learning phases, plus supporting trees for the knowledge-dependency chain, tools, skills, labs, projects, certifications, careers, frameworks and cloud attack-and-defence patterns. It is opinionated about order, honest about time, and it leans toward defensive and engineering work rather than treating cloud security as a purely offensive game.

I have read the whole thing, checked the time-sensitive claims against official sources, and rebuilt it here as a connected narrative rather than a list. Where I add recommendations of my own, I say so. Where the repository is strong, weak or distinctive, I say that too.

<!-- Add a hero/banner image here if available. -->

## Table of Contents

- [What cloud security actually covers](#what-cloud-security-actually-covers)
- [Why the learning order matters](#why-the-learning-order-matters)
- [Phase 0: Understanding the domain](#phase-0-understanding-the-domain)
- [Phase 1: Build the technical foundations](#phase-1-build-the-technical-foundations)
- [Phase 2: Learn one cloud provider properly](#phase-2-learn-one-cloud-provider-properly)
- [Identity and access management: the backbone](#identity-and-access-management-the-backbone)
- [Posture management and cloud misconfigurations](#posture-management-and-cloud-misconfigurations)
- [Logging, monitoring and cloud detection](#logging-monitoring-and-cloud-detection)
- [Cloud network and data security](#cloud-network-and-data-security)
- [Infrastructure as Code and shift-left security](#infrastructure-as-code-and-shift-left-security)
- [Container and Kubernetes security](#container-and-kubernetes-security)
- [Cloud incident response](#cloud-incident-response)
- [Hands-on laboratories](#hands-on-laboratories)
- [Portfolio projects that demonstrate real ability](#portfolio-projects-that-demonstrate-real-ability)
- [Choosing tools instead of collecting them](#choosing-tools-instead-of-collecting-them)
- [Frameworks and standards](#frameworks-and-standards)
- [Certification strategy](#certification-strategy)
- [Career progression](#career-progression)
- [Expert specialisations](#expert-specialisations)
- [A realistic timeline and a 12-month plan](#a-realistic-timeline-and-a-12-month-plan)
- [Common mistakes](#common-mistakes)
- [How to use the repository effectively](#how-to-use-the-repository-effectively)
- [How the pieces relate](#how-the-pieces-relate)
- [Honest assessment of the repository](#honest-assessment-of-the-repository)
- [Conclusion](#conclusion)
- [References](#references)

---

## What cloud security actually covers

Before any roadmap makes sense, it helps to see the shape of the thing you are trying to secure. Cloud security is not one discipline. It is the practice of protecting several connected layers that all live behind the same set of provider APIs:

- **Identities** — human users and, increasingly, non-human identities (service accounts, roles, workload identities, machine credentials).
- **The control plane** — the provider APIs that create, change and delete everything. Whoever controls the API controls the environment.
- **Networks** — virtual private clouds, subnets, firewalls, private endpoints, egress paths.
- **Compute workloads** — virtual machines, containers and serverless functions.
- **Applications and APIs** — the code and interfaces exposed to users and to other services.
- **Storage and databases** — object stores, managed databases, data lakes.
- **Secrets and encryption keys** — the material that protects everything else.
- **Logging and telemetry** — the record of who did what, and the raw signal for detection.
- **Governance, detection and incident response** — the policies, controls and playbooks that hold it together.

The concept that ties this together, and the one the repository treats as foundational, is the shared-responsibility model. The provider secures the infrastructure of the cloud; you secure what you build and configure in it. That line is not fixed. It slides depending on the service model.

### Shared-responsibility model

| Service model | Example | Provider secures | You secure |
|---|---|---|---|
| **IaaS** | Virtual machine (EC2) | Hardware, hypervisor, physical network | OS patching, IAM, firewall rules, data, application |
| **PaaS** | Managed database (RDS) | OS, database engine, patching | Access control, encryption configuration, network exposure, data |
| **SaaS** | Hosted email or office suite | Almost the entire stack | User access, data classification, sharing settings |
| **Serverless** | Functions (Lambda) | Runtime, scaling, host | Function permissions, code, event-source configuration, secrets |

The practical lesson is that "the cloud provider secures everything" is false, and the customer's slice of responsibility grows exactly where beginners tend to look least: identities, configuration and data. This is why the repository opens with a small but genuinely useful exercise — draw the responsibility line yourself for a VM, a managed database, a serverless function and an object store. Once you can do that from memory, most of the field stops feeling arbitrary.

---

## Why the learning order matters

The single most valuable idea in this repository is its knowledge-dependency chain. It argues that cloud-security topics have a natural order, and that skipping steps produces engineers who can run scanners but cannot reason about risk.

```text
Linux + Networking + Scripting (Python/Bash)   [must precede everything]
        │
        ▼
Cloud fundamentals on ONE provider
        │
        ▼
Identity & Access Management            [the gatekeeper topic]
        │
        ├──► Posture management (CSPM)
        ├──► Network & data security
        └──► Logging & monitoring
                │
                ▼
        Detection & incident response
                │
                ▼
        Detection engineering / purple teaming
```

**Parallel tracks that branch off the trunk:**

```text
Containers (Docker) ──► Kubernetes ──► Kubernetes security (KSPM)
IaC (Terraform)     ──► Shift-left / policy-as-code ──► CI/CD security
Frameworks (CSA, CIS, NIST, MITRE) run alongside the whole journey
```

When people skip the base of that chain, predictable things break. Skip networking, and cloud network controls become memorised words with no mental model behind them — you will configure a security group without understanding what a stateful rule actually does. Skip identity, and every later topic is built on sand, because in the cloud almost every attack path eventually runs through a permission. Skip scripting, and you stay stuck at the "click in the console" level while the actual job is automation.

> [!WARNING]
> **Do not skip the dependency chain:** Do not rush into Kubernetes security before you understand containers. KSPM sits at the top of a tall stack — Linux, then networking, then containers, then Kubernetes administration, and only then Kubernetes security. Every step you skip becomes a gap someone will eventually find.

---

## Phase 0: Understanding the domain

The roadmap budgets only three to five days here, and that pacing is right. This phase is orientation, not mastery.

You are trying to internalise a few things. First, that misconfiguration — not exotic zero-day exploits — is the dominant cause of cloud breaches, which is why posture and identity matter more than chasing rare bugs. Second, that cloud security is mostly an engineering-plus-defensive-plus-governance discipline, and only occasionally a purely offensive one. Third, the map of sub-disciplines you will keep meeting: posture (CSPM), workload protection (CWPP), entitlements (CIEM), data (DSPM), Kubernetes (KSPM), application posture (ASPM) and detection-and-response (CDR), all converging into the single platform category called CNAPP.

This is also where the repository draws its ethical line clearly, and it deserves emphasis: test only accounts you own or are explicitly authorised to test, respect each provider's published penetration-testing policy, and never touch shared or provider-managed control planes. That boundary is not bureaucratic caution. It is the difference between a security career and a legal problem.

The most useful part of Phase 0 is its list of beginner misconceptions, because each one maps to a later lesson:

- "The cloud provider secures everything." (No — see the shared-responsibility table.)
- "A green CSPM dashboard means we're secure." (No — identity reachability and runtime behaviour still matter.)
- "Cloud security equals network firewalls." (No — identity is now the primary perimeter.)

> [!TIP]
> **IAM is the cloud perimeter:** Cloud IAM deserves more study time than most beginners initially expect. In traditional security the firewall was the perimeter. In the cloud, the perimeter is identity, and an over-permissioned role is worth more to an attacker than an open port.

---

## Phase 1: Build the technical foundations

The repository allocates four to eight weeks here, with an explicit "skip or skim if experienced" note. That honesty is welcome — a network engineer moving into cloud does not need to relearn subnetting, but a SOC analyst pivoting from alert triage might need to build real Linux fluency.

The point is not to memorise definitions. It is to understand how each foundation reappears in real cloud-security work:

- **Linux and Windows internals** — processes, systemd, logs (journald, auth.log, Windows Event Logs), permissions. You will read these logs during incident response and harden these hosts against a CIS baseline.
- **Networking** — TCP/IP, DNS, HTTP/HTTPS, TLS (including certificates and mutual TLS), routing, NAT, subnets and CIDR. When you later design a VPC with public and private subnets and controlled egress, this is the model you are drawing on.
- **Identity fundamentals** — authentication versus authorisation, RBAC versus ABAC, federation and single sign-on, and the token protocols OAuth 2.0, OpenID Connect (OIDC) and SAML. Understand JWT structure, scopes and claims, and why short-lived credentials beat standing ones.
- **Cryptography basics** — symmetric versus asymmetric encryption, hashing, signing, key lifecycle. This maps directly onto cloud key-management services and envelope encryption later.
- **Scripting and data formats** — Python and Bash fluency, Git, and comfortable reading and writing JSON and YAML, because every cloud policy and manifest lives in one of those two formats.
- **Security principles** — the CIA triad, defence in depth, least privilege, blast-radius thinking, Zero Trust (NIST SP 800-207), and simple threat modelling.

A good capstone for this phase, straight from the repository, is to threat-model a simple three-tier web application on one cloud. If you can reason about where the trust boundaries are, you are ready for Phase 2.

---

## Phase 2: Learn one cloud provider properly

Here is the repository's sharpest piece of advice, and it is worth taking literally: pick one provider and go deep before you touch a second. The roadmap recommends AWS as the primary anchor, largely because its market presence and documentation make it the path of least resistance, but the concepts transfer.

The reason this matters is that multi-cloud learning early on multiplies confusion without adding understanding. The same idea wears three different names across providers — an AWS account is roughly an Azure subscription is roughly a GCP project; AWS Organizations maps to Azure management groups and GCP's resource hierarchy. Learn the pattern once, deeply, and the second provider becomes translation rather than re-learning.

On your chosen provider, get comfortable with the building blocks: compute, object and block storage, virtual networking, managed databases, serverless functions, monitoring, the identity service, the resource hierarchy, and — this one is not optional — billing alerts and budget caps. A forgotten resource running overnight can turn a free-tier experiment into a real bill.

> [!WARNING]
> **Control lab costs:** Set hard billing alerts and a budget cap before you deploy anything. The fastest way to end a cloud-security learning journey is an unexpected invoice from a lab you forgot to tear down.

A realistic first architecture to deploy, drawn from the repository's suggestion, is a small application stack in a free-tier account: a load balancer in a public subnet, an application instance in a private subnet, and a managed database in another private subnet, with a dedicated IAM role for the instance and CloudTrail turned on from day one. Deploy it, break it, secure it, then destroy it. That single exercise teaches more than a week of reading.

---

## Identity and access management: the backbone

If you take one section of this roadmap seriously, take this one. The repository is unambiguous that IAM is where you should spend the most time, and it is right. Identity is the connective tissue of the entire environment; nearly every meaningful cloud attack path passes through a permission that should not have existed.

You need working command of a fairly long list of concepts, but they cluster into a few ideas:

- **The objects. Users, groups, roles and service identities** — and the modern distinction between human and non-human identities. Non-human identities (service accounts, workload identities, machine credentials) now vastly outnumber human ones in most environments, and they are the ones most likely to hold forgotten standing privileges.
- The rules. Identity policies, trust policies (who is allowed to assume a role), resource policies (what a resource permits), permission boundaries (a ceiling on what a policy can grant) and organisation-level guardrails such as service control policies. Internalise the evaluation logic: an explicit deny always wins, no matter how many allows surround it.
- The dynamics. Federation and single sign-on, temporary credentials, role assumption and assume-role chains, cross-account trust, workload identity and just-in-time access. The direction of travel across the whole industry is away from long-lived keys and toward short-lived, federated, on-demand credentials.
- **The failure mode to hunt for is privilege escalation** — the paths where a modestly privileged identity can grant itself more, through a PassRole, a policy attach, or a role-chaining trick. Understanding these paths defensively is what separates someone who can write a policy from someone who can secure an environment.

A concrete exercise the repository recommends, and one I would insist on: take an over-permissioned role, reduce it to genuine least privilege, and keep the evidence. Save the original policy, the access logs showing what the role actually used, your reasoning, the minimised policy, and a re-test proving nothing broke. That before-and-after write-up is one of the strongest early portfolio pieces you can produce, because it demonstrates judgement, not just tool use.

```text
# The shape of a least-privilege refactor (illustrative, not a full policy)
BEFORE:  Action: "s3:*"        Resource: "*"          # everything, everywhere
AFTER:   Action: "s3:GetObject"
         Resource: "arn:aws:s3:::app-bucket/reports/*"
         Condition: source VPC endpoint only
```

---

## Posture management and cloud misconfigurations

CSPM (Cloud Security Posture Management) is continuous scanning for misconfigurations and compliance drift. It is where most learners first feel productive, because a scanner produces hundreds of findings in minutes. That speed is also its trap.

The repository lists sensible tooling: Prowler and ScoutSuite (open-source, multi-cloud, agentless, great for learning), CloudSploit, and the cloud-native services AWS Security Hub, Microsoft Defender for Cloud and Google Security Command Center. Learn an open-source scanner first — you will understand what the checks actually do — then see how the native services present the same idea inside each provider.

The skill worth building is not running the scan. It is the workflow after it:

```text
scan → validate the finding → determine real exposure
     → assess identity reachability → weigh business impact
     → remediate → re-scan → document the evidence
```

A flat list of five hundred findings is close to useless. The valuable output is a short list of the few reachable, exploitable attack paths, prioritised by who can reach what and what it would cost the business. This is the difference between a scanner operator and a security engineer.

> [!CAUTION]
> **A dashboard is not proof of security:** An impressive dashboard is not evidence that an environment is secure. A green CSPM score tells you about known misconfigurations. It says nothing about identity reachability, runtime behaviour or the finding you validated wrongly and closed.

---

## Logging, monitoring and cloud detection

You cannot investigate what you did not record. This phase is about visibility, and it is the bridge from posture into detection and response.

The core log sources are the audit trails of each provider: AWS CloudTrail and AWS Config, Azure Activity Logs with Microsoft Sentinel as the SIEM, and Google Cloud Audit Logs. Beyond turning them on, the real work is designing good logging: centralised collection into a dedicated account, log integrity and retention, and integration with a SIEM (Security Information and Event Management) platform so events become searchable and correlatable.

Once the data exists, you move into detection engineering — and specifically detection-as-code, where detections live in version control, are tested, and map to a known adversary taxonomy. The repository points at Sigma (portable detection rules), KQL (the query language behind Sentinel and other Microsoft tooling) and the MITRE ATT&CK Cloud matrix as the framework for organising what you detect.

Useful cloud detections to build and validate include:

- An access key used from an unfamiliar location or impossible-travel pattern.
- An unexpected AssumeRole or a newly federated identity.
- A change to a public-storage policy or bucket ACL.
- Attachment of a high-privilege policy, or creation of a new access key.
- A new OAuth application consent grant (a common business-email-compromise vector).
- An unusually large data-download volume from object storage.

The discipline that separates good detection engineering from noise is validation: write the detection, trigger the behaviour safely in your own lab (adversary-emulation tooling such as Stratus Red Team exists precisely for this), confirm the alert fires, then tune out false positives. A detection you have never tested is a guess.

---

## Cloud network and data security

Network security in the cloud rewards the foundations you built in Phase 1. The primitives differ slightly by provider but the ideas are shared: VPCs / VNets, public and private subnets, security groups (stateful, instance-level), network ACLs / NSGs (often stateless, subnet-level), route tables, peering, and private endpoints (PrivateLink, Private Endpoints) that keep traffic off the public internet. Layer on egress control, a web application firewall, and DDoS protection (Shield, Front Door, Cloud Armor depending on provider).

Here is a segmented design in text form — the kind you should be able to sketch from memory:

```text
Internet
   │
   ▼
[ WAF + DDoS protection ]
   │
   ▼
Public subnet:  Load balancer only
   │  (controlled inbound; no direct instance exposure)
   ▼
Private subnet: Application instances / containers
   │  (no public IPs; egress via NAT + allow-list)
   ▼
Private subnet: Managed database
   │  (reachable only from the app tier's security group)
   └──► Secrets & keys via private endpoint to KMS / Secrets Manager
```

Data security sits alongside this. Master encryption at rest and in transit, envelope encryption, and the provider key-management service (KMS) — AWS KMS, Azure Key Vault, Google Cloud KMS. Learn a proper secrets-management approach (Secrets Manager, Key Vault, Secret Manager, or HashiCorp Vault) so credentials never live in code. And know the classic breach cold: public object-storage exposure. Finding and remediating a public bucket, then adding an organisation-wide preventive guardrail so it cannot recur, is a beginner project that maps directly to a real-world incident category. Data classification and, at the advanced end, DSPM (Data Security Posture Management) extend this into knowing where your sensitive data actually lives.

---

## Infrastructure as Code and shift-left security

Infrastructure as Code (IaC) means defining your cloud environment in version-controlled files — Terraform (the de facto standard), CloudFormation (AWS-native) or Bicep (Azure). Once infrastructure is code, security can move left: you catch misconfigurations in a pull request, before anything is ever deployed.

The tooling the repository recommends is practical and mostly free: Checkov, Trivy (which now also handles the checks the retired tfsec used to perform) and KICS for scanning IaC; policy-as-code engines such as OPA for enforcing rules; and supply-chain controls including secret detection, SBOM (Software Bill of Materials) generation, and artifact signing with Cosign / Sigstore.

A realistic secure pipeline looks like this:

```text
Terraform code
   → lint
   → IaC misconfiguration scan (Checkov / Trivy)
   → build container image
   → vulnerability + secret scan (Trivy)
   → generate SBOM
   → sign image (Cosign)
   → policy validation (OPA / admission gate)
   → controlled deployment (blocks on critical findings)
```

> [!NOTE]
> **Scanning alone is not DevSecOps:** One caution the repository makes, and I would repeat: scanning alone is not DevSecOps. Bolting a scanner onto a pipeline that never actually blocks a bad build, that nobody reads the output of, and that has no ownership, is theatre. DevSecOps is the culture and the enforced gates, not the tool.

---

## Container and Kubernetes security

This is where sequencing matters most, and where the repository's discipline pays off. Kubernetes security should come after Linux, networking, containers (Docker) and basic Kubernetes administration. Attempting it earlier produces frustration, not capability.

The organising idea is the 4C model: security flows through Cloud → Cluster → Container → Code, and a weakness at any layer undermines the ones inside it.

The tool landscape breaks down cleanly by function:

- **Posture / benchmarking** — kube-bench (CIS Kubernetes Benchmark), Kubescape (which maps to NSA/CISA and MITRE guidance), Trivy for image, config and secret scanning.
- **Runtime detection** — Falco (eBPF-based syscall detection, a CNCF project), Tetragon, KubeArmor.
- **Policy-as-code / admission control** — Kyverno (YAML-native) and OPA Gatekeeper (Rego).
- **Network policy** — Cilium (with Hubble for observability) and Calico.
- **Supply chain** — image signing with Cosign, SBOMs, admission control and registry allow-lists.

The practical starter lab is deliberately local and free: spin up a cluster with kind or Minikube, run kube-bench to find CIS violations and fix them, add a couple of Kyverno admission policies (block privileged pods, require signed images), install Falco and trigger a rule to watch a runtime detection fire. That single lab covers posture, policy and runtime in an afternoon, on your own machine, at zero cloud cost.

---

## Cloud incident response

Cloud incident response inherits the classic lifecycle — detect, contain, eradicate, recover, learn (NIST SP 800-61) — but the mechanics differ sharply from endpoint IR. You are rarely pulling a network cable. You are working through APIs, and your best evidence is the audit log.

The cloud-specific moves the repository highlights are worth committing to memory: revoke or deactivate the compromised credential, invalidate active sessions, rotate keys, isolate the affected workload (a restrictive policy or security group, rather than a physical disconnect), snapshot for forensics before you destroy anything, and reconstruct the timeline from the audit trail (CloudTrail, Activity Log, Audit Logs). Then comes blast-radius analysis — what did that identity actually touch, and across which accounts?

Here is a safe, fictional walkthrough to make it concrete:

> [!IMPORTANT]
> **Scenario: a long-lived access key appears in GuardDuty findings, used from an unfamiliar IP.**
>
> First, deactivate the key — do not delete it yet; you need it for the investigation. Revoke the identity's active sessions. Pull the CloudTrail timeline for that key: what did it call, in which regions, over what window? You notice an AttachUserPolicy granting broad permissions, then a CreateAccessKey — a classic escalation-and-persistence pattern. Remove the escalated grants, disable the second key, and snapshot any resources the identity created. Assess the blast radius across accounts via cross-account trust. Recover by rotating affected credentials and confirming clean state. Finally, write the lessons-learned: the root cause was a standing long-lived key, so the fix is IAM Identity Center and short-lived credentials, plus a detection for anomalous CreateAccessKey.

Working a scenario like this end-to-end — ideally against a deliberately vulnerable lab such as flaws2.cloud's defender path — produces an incident-response runbook that is a genuine portfolio asset.

---

## Hands-on laboratories

Reading about cloud security and doing it are different activities, and only one of them gets you hired. The repository structures labs as a gradual build-out, and the progression is sensible.

- **Stage 1** — single-cloud sandbox. A free-tier account with hard billing alerts and budget caps, CLI and SDK configured with named profiles and no long-lived root keys. First targets are flaws.cloud and flaws2.cloud, which need nothing deployed.
- **Stage 2** — deployable vulnerable targets. Terraform-based scenarios such as CloudGoat, plus IAM Vulnerable (a privilege-escalation playground), AWSGoat and Sadcloud. Run each, then destroy it.
- **Stage 3** — multi-cloud. Add Azure and GCP free tiers; EntraGoat for Entra ID and GCPGoat for Google Cloud; practise cross-cloud posture scanning with Prowler and ScoutSuite.
- **Stage 4** — defender stack. A central logging account, a local Kubernetes cluster with Falco, Trivy and kube-bench, and a detection-as-code repository with tested Sigma rules.
- **Stage 5** — full pipeline. IaC scanned in CI, images signed, a deploy gate that blocks on critical findings, and adversary emulation to verify detections fire.

Two rules run through every stage, and they are non-negotiable:

> [!WARNING]
> **Authorisation:** Deploy vulnerable-by-design environments only in accounts you own or are explicitly authorised to use — never with real data or real credentials. A CloudGoat scenario in a shared or client account is a mistake you cannot take back.

> [!TIP]
> **Teardown discipline:** And on cost: adopt teardown discipline. Tools such as cloud-nuke and aws-nuke remove leftover resources; run them after every session. The habit that protects your wallet also happens to be the habit that keeps a lab environment clean and reproducible.

---

## Portfolio projects that demonstrate real ability

Screenshots of completed courses are weak evidence. They show attendance, not ability. What convinces a hiring manager is a small set of projects that each produced evidence — a before-and-after, a validated finding, a working control.

The repository organises projects into a clear difficulty ramp. A representative selection:

### Beginner

- A public-storage exposure assessment and remediation, with a preventive guardrail added afterward.
- A least-privilege IAM refactor, with the policy diff and a re-test as proof.
- Organisation-wide audit logging with a pack of ten useful security queries.
- A segmented network design, documented as an architecture diagram.
- A CIS-benchmark assessment of a host and a cloud account.

### Intermediate

- CloudGoat scenario write-ups that cover both the attack and the detections that would catch it.
- An open-source "DIY CNAPP" stack (Prowler + Trivy + ScoutSuite + Cloud Custodian) with an honest gap analysis of what a commercial CNAPP would add.
- A CI/CD security gate that fails on critical findings.
- A Kubernetes-hardening project (kube-bench fixes, Kyverno policies, Falco rules).
- A cloud incident-response runbook with a worked tabletop.

### Advanced

- A multi-cloud detection-as-code repository (Sigma + KQL), mapped to ATT&CK Cloud and tested with adversary emulation.
- An attack-path analysis using a graphing tool such as Cartography or PMapper.
- An auto-remediation framework built on Cloud Custodian.
- An OAuth illicit-consent attack-and-defend lab.
- A secure landing-zone reference architecture in IaC.

### Capstone (interview centrepieces)

- An end-to-end secured platform: IaC, scanned pipeline, hardened Kubernetes, detection, IR runbook.
- A full cloud-security assessment of a deliberately vulnerable estate, written as a client-style report.
- A genuine open-source contribution to a cloud-security tool.

For every project, document it the same way. This consistent structure is itself a signal of professionalism:

```text
Objective → Environment → Architecture → Methodology → Findings
→ Evidence → Remediation → Verification → Skills demonstrated → Lessons learned
```

A sanitised portfolio directory that mirrors the repository's own suggestion:

```text
cloud-security-portfolio/
├── README.md                  # who you are, skills index, links to each project
├── projects/
│   ├── 01-cspm-triage/        # Prowler report, before/after, verification
│   ├── 02-iam-least-privilege/# over-privileged role → minimal, with policy diff
│   ├── 03-cloudgoat-writeups/ # attack steps + detections that would catch each
│   ├── 04-k8s-hardening/      # kube-bench fixes, Kyverno policies, Falco rules
│   └── 05-secured-pipeline/   # IaC scan + image sign + deploy gate
├── detections/                # Sigma + KQL rules, ATT&CK-mapped, with tests
├── iac/                       # Terraform landing-zone snippets with guardrails
├── writeups/                  # CTF solutions, technique blog posts
└── scripts/                   # Python/boto3 automation, Cloud Custodian policies
```

> [!WARNING]
> **Sanitise before publishing:** Sanitise everything before you publish. Redact account IDs, IP addresses, hostnames and any real names; use lab data only. A leaked account ID in a portfolio screenshot is a small gift to an attacker and a bad look to a reviewer.

---

## Choosing tools instead of collecting them

The fastest way to look busy and learn little is to install forty tools. The repository's better framing is to understand the category — the security problem each class of tool solves — before memorising any product's dashboard. Here is the discipline map it uses.

### Table 1: Cloud-security discipline comparison

| Discipline | Main purpose | Typical risk addressed | Example tools |
|---|---|---|---|
| **CSPM (Posture)** | Continuously detect misconfigurations and compliance drift | Public buckets, weak defaults, configuration drift | Prowler, ScoutSuite, Security Hub, Defender for Cloud, SCC |
| **CWPP (Workload)** | Protect VMs, containers, and serverless workloads at runtime | Vulnerable or compromised workloads | Trivy, Falco, Defender for Cloud, Sysdig |
| **CIEM (Entitlements)** | Analyse and right-size identity permissions | Over-privileged and escalatable identities | IAM Access Analyzer, PMapper, CNAPP identity graphs |
| **DSPM (Data)** | Discover and classify sensitive data and its exposure | Unknown or exposed sensitive data | Macie, native DSPM, CNAPP data modules |
| **KSPM (Kubernetes)** | Assess Kubernetes cluster and configuration posture | Insecure clusters, weak admission control | kube-bench, Kubescape, Kyverno |
| **CDR (Detection & response)** | Detect and respond to cloud threats | Active attacks, credential abuse | GuardDuty, Defender, SCC, Sigma, Sentinel (KQL) |
| **CNAPP (Unified platform)** | Combine the disciplines into one attack-path view | Fragmented tooling and unprioritised risk | Wiz, Prisma Cloud, Orca, Defender for Cloud |

When you compare specific products, weigh the same criteria the repository lists: agentless (fast, posture-focused) versus agent or eBPF (deep, runtime), multi-cloud coverage, false-positive rate, deployment effort, integration and remediation-workflow fit, and the cost model. You need both agentless and runtime approaches eventually; they answer different questions.

### Table 2: Tool-selection comparison

| Category | Open-source option | Cloud-native option | Commercial option | What to evaluate |
|---|---|---|---|---|
| **Posture (CSPM)** | Prowler, ScoutSuite | Security Hub, Defender for Cloud, SCC | Wiz, Prisma Cloud, Orca | Multi-cloud coverage, noise, remediation fit |
| **IaC scanning** | Checkov, Trivy, KICS | Native pipeline integrations | Snyk IaC, Prisma | Pull-request blocking, framework coverage |
| **Image / SBOM** | Trivy, Grype, Syft | Inspector, artifact scanning | Commercial CWPP | CVE, secret, and SBOM depth |
| **Kubernetes** | kube-bench, Kubescape, Falco | Managed policy add-ons | Sysdig, CNAPP KSPM | Runtime, admission, and network coverage |
| **Detection** | Sigma rules | GuardDuty, Sentinel, SCC | SIEM / CNAPP CDR | ATT&CK mapping and tunability |

A note on the CNAPP market, verified as of July 2026: Google completed its $32 billion acquisition of Wiz on 11 March 2026, and Wiz now sits inside Google Cloud while continuing to support all major clouds. The commercial CNAPP category is expensive; learn the concept through free trials and community editions rather than assuming you must buy anything to become skilled.

---

## Frameworks and standards

Frameworks run in parallel with everything else. You do not need to master all of them, and the repository is careful not to pretend otherwise. What matters is knowing which ones serve which audience.

The minimum to internalise as an engineer: the shared-responsibility model, the CSA Cloud Controls Matrix (CCM), CIS Benchmarks (the configuration gold standard), the MITRE ATT&CK Cloud matrix (for detection), NIST CSF 2.0 and NIST SP 800-207 (Zero Trust), and each provider's Well-Architected security pillar.

Beyond that core, the landscape divides by role. Engineers lean on CIS, ATT&CK, D3FEND, OWASP (the Top 10 and API Security Top 10), and supply-chain standards like SLSA and SBOM formats (SPDX, CycloneDX). Architects add NIST SP 800-53, ISO/IEC 27001, and the cloud-specific ISO/IEC 27017 and 27018. Auditors and governance professionals work with SOC 2, PCI DSS, and regional data-protection law — GDPR, India's DPDP Act, and the EU's DORA and NIS2 where they apply. CCSK study, incidentally, draws on the CSA Security Guidance v5 and the CCM, which is why that certificate doubles as a framework tour.

The honest guidance: engineers should be fluent in the detection and hardening frameworks and merely conversant in the governance ones; auditors and architects invert that. Learn the ones your target role actually uses.

---

## Certification strategy

Certifications help with structured learning and hiring visibility. They do not replace labs, engineering ability or a portfolio. A CloudGoat write-up backed by real evidence often beats a certificate with no practice behind it — and every experienced interviewer knows the difference.

All names and exam versions below were verified against official sources as of July 2026.

### Certification pathway

| Career stage | Certification options | Why they help | Practical work to accompany them |
|---|---|---|---|
| **Foundational** | CompTIA Security+; AWS Cloud Practitioner; Azure AZ-900 / SC-900 | Baseline security and cloud literacy; common HR filters | Deployed sandbox application; shared-responsibility exercise |
| **Cloud fluency** | AWS Solutions Architect Associate or an Azure/GCP associate certification | Builds the service knowledge assumed by security exams | Segmented VPC and a small multi-service architecture |
| **Vendor-neutral core** | CCSK v5 (CSA); CCSP (ISC2) | CCSK provides an accessible cloud-security foundation; CCSP is a senior vendor-neutral standard | CSPM triage report; controls-mapping exercise |
| **Provider-specific** | AWS Security – Specialty (SCS-C03); Azure AZ-500 → SC-100, plus SC-300 for identity; Google Professional Cloud Security Engineer | Builds depth in the cloud you actually operate | IAM least-privilege refactor; detection-and-response lab |
| **Kubernetes** | CKS (Certified Kubernetes Security Specialist) | Validates hands-on cluster security | Hardened cluster with policies and runtime rules |
| **Governance / leadership** | CISSP; CISM / CCAK | Supports architecture, audit, and leadership roles | Landing-zone standard; risk register with exceptions |

A few version notes worth pinning down, verified as of July 2026:

- The AWS Certified Security – Specialty moved from SCS-C02 to SCS-C03 on 2 December 2025; SCS-C02 was retired on 1 December 2025. Study the C03 guide, which added detection/incident-response structure and generative-AI security coverage.
- CCSK v5 replaced v4 on 1 January 2026. It is a 12-domain, open-book exam based on CSA Security Guidance v5, with new AI/GenAI and Zero Trust content.
- CCSP requires five years of IT experience with three in information security and one in a CCSP domain; if you are short on experience you can pass first as an Associate. CKS requires a current CKA as a hard prerequisite.

The repository's suggested arc for an AWS-leaning engineer — CCSK → AWS Security Specialty (SCS-C03) → CKS → CCSP over roughly eighteen months — is a reasonable, ordered path. Do not chase certificates ahead of lab evidence.

> [!NOTE]
> **Certifications are supporting evidence:** A certification is a hiring signal, not a job guarantee. It gets your resume read. What gets you hired is being able to reason through a scenario the interviewer invents on the spot — and that only comes from labs.

---

## Career progression

The roadmap sketches a realistic ladder, and one of its quieter strengths is showing how people already in adjacent roles transfer in.

At entry level, you are a Cloud Security Analyst or Junior Cloud Security Engineer: IAM basics, CSPM triage, logging, one cloud, some scripting. At mid level, you become a Cloud Security Engineer, DevSecOps Engineer or Cloud Pentester, and you specialise — offensive cloud, or detection and IR, or platform and DevSecOps. At senior level, you own landing-zone standards, guardrail strategy and cross-team influence, and you translate technical risk into business terms. Beyond that sit architect, principal and leadership tracks, or independent consulting.

The transfer paths matter because most people do not start from zero:

- SOC analysts already have triage and detection instincts; they add cloud log sources and IAM.
- Network engineers already understand segmentation and routing; they translate it to VPC constructs.
- System administrators already harden hosts; they extend it to cloud configuration and posture.
- Software and DevOps engineers already ship code and pipelines; they add security gates and become DevSecOps engineers, often the fastest transition of all.

### Learning-stage comparison

| Stage | Main topics | Hands-on output | Portfolio evidence | Possible roles |
|---|---|---|---|---|
| **Foundations** | Linux, networking, scripting, identity basics | Hardened VM; threat model | CIS-hardening write-up | Prerequisite stage |
| **Core cloud** | One provider, IAM, CSPM, logging, network, data | Deployed application; least-privilege role | IAM refactor; CSPM triage report | Cloud Security Analyst |
| **Operations** | Incident response, guardrails, detection-as-code | IR runbook; tested detections | Detection repository; IR tabletop | Junior Cloud Security Engineer |
| **Automation & Kubernetes** | IaC, CI/CD gates, Kubernetes security | Secured pipeline; hardened cluster | Pipeline and cluster projects | Cloud Security / DevSecOps Engineer |
| **Advanced** | Detection engineering, attack paths, architecture | Multi-cloud detections; landing zone | Capstone platform; assessment report | Senior Engineer / Architect |

---

## Expert specialisations

From roughly year two onward, the field opens into specialisations. Each has a clear foundation you should not skip:

- **Offensive cloud / red team** — needs deep IAM and provider internals; authorised engagements only.
- **Cloud detection engineering and threat hunting** — needs strong logging and ATT&CK Cloud fluency.
- **Kubernetes and cloud-native security** — needs CKA/CKS-level administration first.
- **DevSecOps / platform security** — needs solid IaC and CI/CD before building secure-by-default golden paths.
- **Cloud security architecture** — needs multi-cloud breadth and Zero Trust.
- **Data Security Posture (DSPM) and privacy** — needs data classification and cryptography.
- **Cloud GRC / compliance and audit** — needs the CCM/ISO/NIST control frameworks.
- **AI and agentic-workload security** — the 2026 frontier: over-privileged AI agents, non-human identities and model/data-pipeline security, built on the same identity-plus-runtime foundations.

The pattern is consistent: every specialisation is a deepening of the trunk, not a shortcut around it.

---

## A realistic timeline and a 12-month plan

The repository is refreshingly honest about time. It offers three pacing tracks over the same content:

- **Fast track** — roughly 15–20 hours per week → about 7–9 months.
- **Standard track** — roughly 8–12 hours per week → about 12–14 months.
- **Part-time track** — roughly 4–6 hours per week → about 18–22 months.

These are ranges, not promises. Actual progress depends on your starting point, how deep you go in labs, your consistency, and the quality of your projects. Nobody becomes an expert on a fixed schedule, and anyone who tells you otherwise is selling something.

Here is a condensed twelve-month plan built from the repository's own learning blocks, suitable for the standard track:

```text
Weeks 1–4    Foundations review + one cloud's fundamentals + IAM basics
             → Deliverable: deployed app stack; first least-privilege role
Weeks 5–9    IAM deep dive + CSPM (Prowler) + logging (CloudTrail/Athena)
             → Deliverable: CSPM triage report; org logging set up
Weeks 10–14  Network + data security + secrets + KMS; flaws.cloud
             → Deliverable: segmented VPC diagram; public-bucket remediation
Weeks 15–20  Tooling (Trivy/Checkov/ScoutSuite) + CloudGoat scenarios
             → Deliverable: CloudGoat write-ups + DIY-CNAPP gap analysis
Weeks 21–26  Incident response + guardrails (SCP/Custodian) + detection-as-code
             → Deliverable: IR runbook + five tested detections
Weeks 27–34  Containers/Kubernetes security (kube-bench, Falco, Kyverno) + CKS prep
             → Deliverable: hardened cluster lab
Weeks 35–44  Automation (Terraform/Python), CI/CD gates, multi-cloud, capstone
             → Deliverable: secured pipeline capstone + full assessment report
Weeks 45–52  Certification push (AWS Security Specialty / CCSP) + interview prep
             → Deliverable: polished, sanitised portfolio
```

Notice that a deliverable — evidence — comes out of every block. That is the point.

<!-- Add a 12-month roadmap graphic here if available. -->

---

## Common mistakes

Most of these are failures of sequence or of discipline, not intelligence:

- Learning all three cloud providers at once instead of going deep on one.
- Collecting tools without understanding the fundamentals beneath them.
- Underinvesting in IAM — the single most common and most costly mistake.
- Skipping networking and then never really understanding cloud network controls.
- Studying only for certifications and neglecting labs.
- Running scanners without validating the findings.
- Building labs but never documenting the evidence.
- Ignoring cloud costs and getting a nasty bill.
- Using real client data in a personal lab.
- Jumping into offensive cloud work without authorisation.
- Treating compliance as proof of security.
- Assuming a green CNAPP dashboard means the environment is safe.

> [!CAUTION]
> **Compliance is not the same as security:** A passed audit is not the same as a secure environment. Compliance proves you met a checklist on a given day. Security is whether an attacker can reach something that matters. They overlap; they are not the same thing.

---

## How to use the repository effectively

The roadmap works best as a loop, not a reading list. A concrete method:

1. Choose a pacing track honestly, based on the hours you can actually commit.
2. Start with one provider and resist the urge to add a second early.
3. Create a secure sandbox with billing alerts and budget caps first.
4. Study one concept at a time.
5. Build a small lab that exercises it.
6. Record the evidence — screenshots, diagrams, before-and-after.
7. Write a short technical report using the project template.
8. Push the sanitised work to GitHub.
9. Check the knowledge-dependency map before moving forward.
10. Revisit identity, logging and automation repeatedly — they deepen every time you return.

### A quick-start checklist

- [ ] Free-tier account created, root secured, MFA on, billing alerts + budget cap set
- [ ] CLI/SDK configured with named profiles; no long-lived root keys
- [ ] Shared-responsibility line drawn from memory for VM / DB / function / storage
- [ ] First app stack deployed, then torn down
- [ ] One over-privileged role reduced to least privilege, with evidence saved
- [ ] Prowler run once; top findings triaged, fixed and re-scanned
- [ ] Org-wide audit logging enabled and centralised
- [ ] flaws.cloud and one CloudGoat scenario completed and written up
- [ ] Local Kubernetes lab hardened with kube-bench + Kyverno + Falco
- [ ] Five detections written, tested and mapped to ATT&CK
- [ ] A CI/CD pipeline that blocks on critical findings
- [ ] Portfolio repository published, fully sanitised

---

## How the pieces relate

Everything in this roadmap connects along a single spine. If you keep this picture in your head, the individual tools stop feeling random:

```text
Human + non-human identities
        │  grant access to
        ▼
Cloud control plane (APIs)
        │  provisions & governs
        ▼
Workloads & applications (VMs, containers, serverless)
        │  run on / read & write
        ▼
Networks & data (VPC/VNet, storage, DB, secrets, KMS)
        │  emit
        ▼
Telemetry (CloudTrail / Activity Log / Audit Logs)
        │  feeds
        ▼
SIEM & detections (Sigma, KQL, ATT&CK-mapped)
        │  triggers
        ▼
Response (automated remediation or manual IR)

Security disciplines layer over the whole stack:
CSPM • CIEM • CWPP • DSPM • KSPM • CDR ── converging into ──► CNAPP
```

<!-- Add an architecture/relationship diagram here if available. -->

---

## Honest assessment of the repository

Since the request was to evaluate as well as explain: the repository's strongest feature is its dependency ordering. Very few free roadmaps take a firm position on what must come before what, and this one does — repeatedly, and correctly. Its treatment of IAM as the centre of gravity, and its insistence on evidence-producing projects over course screenshots, both reflect how the job actually works.

Its most distinctive touch is the attack-and-defence tree, which pairs each cloud technique with the log source, detection, prevention and response — a genuinely useful mental model that most beginner material omits.

Its weakest aspect is presentation: it is a single, very dense ASCII README with no images, no linked sub-pages and, at the time of writing, no stars or forks. That density is a barrier for the exact beginners it is designed to help, which is part of why a narrative walkthrough like this one is worth writing. It is also worth treating the tool and certification lists as a snapshot — the repository itself flags version changes, and as this article shows, several (SCS-C03, CCSK v5, the Google–Wiz close) have moved recently and will keep moving.

---

## Conclusion

Cloud-security expertise does not arrive the day you pass an exam or finish a course. It develops the way most real skills do — by repeatedly connecting the same handful of ideas from new angles until they become instinct. Architecture, identity, telemetry, risk, automation and clear communication: you will return to each of these many times, and each return deepens the others.

The roadmap analysed here, jaysapra001/Cloud-Security-Roadmap, is a solid scaffold for that process — opinionated about order, grounded in labs, honest about time. Treat it as a map, not a script. Fork it, adapt it to the cloud you actually run and the career direction you actually want, and let your portfolio, rather than your tab count, measure your progress. If it saves you from the confusion this article opened with, it has done its job — and a star or a fork is a fair way to say so.

<!-- Add a closing image here if available. -->

---

## References

All time-sensitive facts verified as of July 2026 against official or primary sources.

- **Analysed repository:** [jaysapra001/Cloud-Security-Roadmap](https://github.com/jaysapra001/Cloud-Security-Roadmap)
- **AWS Certified Security – Specialty (SCS-C03):** [Official exam page](https://aws.amazon.com/certification/certified-security-specialty/) · [Official exam guide](https://docs.aws.amazon.com/aws-certification/latest/security-specialty-03/security-specialty-03.html)
- **AWS Training & Certification:** [Security certification updates](https://aws.amazon.com/blogs/training-and-certification/)
- **Cloud Security Alliance:** [CCSK v5](https://cloudsecurityalliance.org/education/ccsk)
- **Google:** [Google completes acquisition of Wiz](https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/wiz-acquisition/)
- **ISC2:** [CCSP](https://www.isc2.org/certifications/ccsp)
- **Linux Foundation:** [CKS and CKA training](https://training.linuxfoundation.org/)
- **NIST:** [Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework) · [SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
- **MITRE:** [ATT&CK Cloud matrices](https://attack.mitre.org/)
- **CIS:** [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks)
- **OWASP:** [OWASP Top 10 and API Security Top 10](https://owasp.org/)
- **Open-source tools:** [Prowler](https://github.com/prowler-cloud/prowler) · [Trivy](https://github.com/aquasecurity/trivy) · [Falco](https://falco.org/) · [Kyverno](https://kyverno.io/)

---

<div align="center">

**Learn in sequence. Build in public. Prove every skill with evidence.**

</div>

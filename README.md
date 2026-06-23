```
CLOUD SECURITY ROADMAP: Beginner -> Expert

├── Phase 0: Domain Orientation (3-5 days)
│   ├── What cloud security actually is
│   │   ├── Securing infra/data/identities/apps across IaaS, PaaS, SaaS, serverless
│   │   ├── Why orgs need it: misconfiguration is the #1 cause of cloud breaches, not 0-days
│   │   └── Where it sits: engineering + defensive + governance (rarely purely offensive)
│   ├── The Shared Responsibility Model (THE foundational concept)
│   │   ├── Concept: provider secures "of the cloud", you secure "in the cloud"
│   │   ├── Line moves by service model: IaaS -> PaaS -> SaaS -> serverless
│   │   └── Task: draw the responsibility line for EC2 vs RDS vs Lambda vs S3
│   ├── Disciplines you'll touch
│   │   ├── CSPM (posture/misconfig), CWPP (workload), CIEM (entitlements)
│   │   ├── DSPM (data), KSPM (Kubernetes), ASPM (app), CDR (detection & response)
│   │   └── These are now converging into CNAPP (one platform) -- know the parts anyway
│   ├── Legal & ethical boundaries
│   │   ├── Test ONLY accounts you own or are explicitly authorised to test
│   │   ├── Provider pentest policies (AWS/Azure/GCP allow most testing without prior approval, with limits)
│   │   └── Never touch shared/provider-managed control planes
│   ├── Common job roles & daily work (analyst -> engineer -> architect)
│   └── Beginner misconceptions
│       ├── "The cloud provider secures everything" (no)
│       ├── "A green CSPM dashboard means we're secure" (no -- identity & runtime matter)
│       └── "Cloud security = network firewalls" (identity is the new perimeter)
│
├── Phase 1: Computing & Security Foundations (4-8 weeks) [skip/skim if experienced]
│   ├── Operating systems
│   │   ├── Linux: processes, systemd, journald/auth.log, permissions, /proc
│   │   ├── Windows: Event Logs, services, processes (relevant for Azure/Entra)
│   │   └── Task: harden a Linux VM to a CIS baseline
│   ├── Networking (cloud-flavoured)
│   │   ├── TCP/IP, OSI, ports/sockets, the 3-way handshake
│   │   ├── DNS, HTTP/HTTPS, TLS (certs, handshake, mTLS)
│   │   ├── Routing, NAT, subnets/CIDR, VLANs vs VPC/VNet constructs
│   │   ├── Tool: Wireshark, tcpdump, dig, openssl s_client
│   │   └── Task: trace a request from client -> load balancer -> private subnet -> DB
│   ├── Identity & access fundamentals
│   │   ├── AuthN vs AuthZ, RBAC vs ABAC, federation, SSO, OIDC/SAML/OAuth2
│   │   ├── Tokens: JWT structure, scopes, claims, short-lived vs long-lived creds
│   │   └── Concept: why standing credentials are the #1 cloud risk
│   ├── Cryptography basics
│   │   ├── Symmetric vs asymmetric, hashing, signing, key lifecycle
│   │   └── Maps later to KMS / Key Vault / Cloud KMS, envelope encryption
│   ├── Scripting & automation prereqs
│   │   ├── Python (must be fluent), Bash (fluent), Git (fluent)
│   │   └── JSON + YAML reading/writing (every cloud policy lives here)
│   ├── Databases (just enough)
│   │   └── Relational vs object vs key-value; where sensitive data lands
│   └── Security principles
│       ├── CIA triad, defence in depth, least privilege, blast-radius thinking
│       ├── Zero Trust (NIST SP 800-207), threat modelling, risk = likelihood x impact
│       └── Task: threat-model a simple 3-tier web app on one cloud
│
├── Phase 2: Core Cloud Security Skills (8-12 weeks) [your real starting line]
│   ├── Cloud fundamentals on ONE provider first (pick AWS)
│   │   ├── Core services: EC2, S3, VPC, IAM, CloudWatch, Lambda, RDS
│   │   ├── Regions/AZs, the resource hierarchy (accounts, orgs, OUs)
│   │   ├── Cert anchor: AWS Cloud Practitioner -> Solutions Architect Associate
│   │   └── Lab: deploy a small app stack from scratch in a free-tier account
│   ├── Identity & Access Management (the backbone -- spend the most time here)
│   │   ├── IAM users/roles/policies/groups; trust policies; assume-role chains
│   │   ├── Permission boundaries, SCPs (org guardrails), session policies
│   │   ├── Federation: IAM Identity Center / Entra ID / Workload Identity
│   │   ├── Least-privilege authoring + policy evaluation logic (explicit deny wins)
│   │   ├── Tool: IAM Access Analyzer, policy simulators
│   │   └── Lab: take an over-privileged role and reduce it to least privilege with evidence
│   ├── Cloud Security Posture Management (CSPM)
│   │   ├── Concept: continuous detection of misconfigurations + compliance drift
│   │   ├── Tool (OSS): Prowler, ScoutSuite, CloudSploit
│   │   ├── Tool (native): AWS Security Hub, Azure Defender for Cloud, GCP Security Command Center
│   │   └── Lab: run Prowler on a real account, triage top 10 findings, fix and re-scan
│   ├── Logging, monitoring & visibility
│   │   ├── AWS CloudTrail + Config; Azure Activity Log + Monitor; GCP Cloud Audit Logs
│   │   ├── What "good" logging coverage looks like; log retention & integrity
│   │   └── Lab: enable org-wide CloudTrail, ship to a central account, query with Athena
│   ├── Network security in the cloud
│   │   ├── Security groups vs NACLs vs NSGs vs firewall rules
│   │   ├── VPC/VNet design, peering, PrivateLink/Private Endpoints, egress control
│   │   ├── WAF, DDoS protection (Shield / Front Door / Cloud Armor)
│   │   └── Lab: build a segmented VPC, public/private subnets, controlled egress
│   ├── Data security
│   │   ├── Encryption at rest/in transit; KMS / Key Vault / Cloud KMS; envelope encryption
│   │   ├── Secrets management (Secrets Manager, Key Vault, Secret Manager, Vault by HashiCorp)
│   │   ├── Storage exposure (public S3/blob/bucket) -- the classic breach
│   │   └── Lab: find and remediate a public bucket; rotate a leaked key
│   └── Evidence you should produce this phase
│       ├── A before/after IAM least-privilege write-up
│       ├── A CSPM triage report (finding -> risk -> fix -> verification)
│       └── A network architecture diagram of a segmented account
│
├── Phase 3: Tools & Platform Mastery (6-10 weeks, ongoing)
│   ├── Posture & compliance scanners
│   │   ├── OSS: Prowler (multi-cloud), ScoutSuite, CloudSploit, OpenSCAP
│   │   └── Native: Security Hub, Defender for Cloud, Security Command Center
│   ├── IaC & shift-left scanning
│   │   ├── Checkov, Trivy (config), KICS, tfsec (now folded into Trivy)
│   │   └── Lab: fail a CI build on a Terraform misconfig, then fix it
│   ├── Container & image scanning
│   │   ├── Trivy (CVEs, secrets, SBOM, IaC -- the OSS workhorse), Grype + Syft
│   │   └── Cosign / Sigstore for image signing & verification
│   ├── CNAPP (the unified commercial category -- learn the concept, demo 1-2)
│   │   ├── Wiz (Security Graph; acquired by Google), Prisma Cloud (Palo Alto)
│   │   ├── Microsoft Defender for Cloud (Azure-first), Orca (agentless SideScanning)
│   │   ├── CrowdStrike Falcon Cloud Security, SentinelOne Singularity CNS, Sysdig Secure
│   │   └── [expensive] don't buy -- use free trials / community editions / vendor labs
│   ├── CIEM / identity risk
│   │   └── Native analyzers + CNAPP identity graphs; map who-can-reach-what
│   ├── Offensive cloud tooling [advanced, authorised labs only]
│   │   ├── Pacu (AWS exploitation framework), CloudGoat targets
│   │   ├── enumerate-iam, PMapper / Cartography (attack-path graphing)
│   │   └── Stratus Red Team (adversary emulation in cloud)
│   ├── Tool-selection criteria
│   │   ├── Agentless (posture, fast) vs agent/eBPF (runtime, deep) -- you need both
│   │   ├── Multi-cloud coverage, noise/false-positive rate, remediation workflow fit
│   │   └── Cost model: OSS / free tier / freemium / enterprise
│   └── Exercise: assemble a free OSS stack (Prowler + Trivy + ScoutSuite + Cloud Custodian)
│       that mimics a CNAPP, and document the gaps a real CNAPP would close
│
├── Phase 4: Hands-On Laboratories (build alongside Phases 2-6)
│   ├── Free-tier accounts on AWS + Azure + GCP (set BILLING ALERTS first)
│   ├── Vulnerable-by-design targets (own account, destroy after)
│   │   ├── flaws.cloud / flaws2.cloud (challenge style, attacker + defender)
│   │   ├── CloudGoat (Rhino) -- terraform-deployed AWS attack scenarios
│   │   ├── AWSGoat, Sadcloud, DVCA, IAM Vulnerable (Bishop Fox, 31 priv-esc paths)
│   │   ├── The Big IAM Challenge, CI/CDon't, EntraGoat (Azure/Entra)
│   │   └── CNAPPGoat (multi-cloud), AWS CIRT incident-response workshops
│   ├── Defender-side lab
│   │   ├── Central logging account, Athena/KQL queries, detection rules
│   │   └── Falco + Trivy + kube-bench on a kind/minikube cluster
│   ├── Cost & hygiene
│   │   ├── nuke leftover resources (aws-nuke / cloud-nuke), tear down nightly
│   │   └── Never put real data or real creds in a lab
│   └── Git repo for every lab: README + steps + findings + remediation
│
├── Phase 5: Intermediate Operations (6-10 weeks)
│   ├── Cloud incident response
│   │   ├── Detect -> contain -> eradicate -> recover -> lessons learned (NIST 800-61)
│   │   ├── Cloud-specific: snapshot for forensics, revoke sessions, rotate keys, quarantine
│   │   ├── Tools: native IR (GuardDuty, Defender, SCC), CloudTrail timeline reconstruction
│   │   └── Lab: work the flaws2.cloud defender path + an AWS CIRT scenario end-to-end
│   ├── Triage & prioritisation
│   │   ├── Move from "1,000 findings" to "the 5 exploitable attack paths"
│   │   ├── Reachability + identity + exposure + sensitivity = real risk
│   │   └── Severity vs exploitability vs business impact
│   ├── Guardrails & SOPs
│   │   ├── Preventive (SCPs, Azure Policy, Org Policy) vs detective (Config rules)
│   │   ├── Auto-remediation (Cloud Custodian, native remediation, EventBridge -> Lambda)
│   │   └── Lab: write an SCP that blocks public S3 org-wide + a detective backstop
│   ├── Reporting & stakeholder comms
│   │   └── Engineer-facing tickets vs risk-owner-facing summaries (same finding, two voices)
│   └── Metrics: mean-time-to-remediate, % critical findings open >30d, coverage gaps
│
├── Phase 6: Advanced Technical Skills (3-6 months, deepening)
│   ├── Identity attacks & defence (the heart of cloud security)
│   │   ├── Privilege escalation paths (IAM misconfig, role chaining, pass-role)
│   │   ├── Cross-account trust abuse, confused-deputy, SالسSRF -> IMDS credential theft
│   │   ├── Entra ID / OAuth consent-grant & illicit-app attacks (you've seen these in BEC IR)
│   │   ├── Detection: anomalous AssumeRole, impossible travel, new federated identity
│   │   └── Defence: IMDSv2, short-lived creds, conditional access, just-in-time access
│   ├── Container & Kubernetes security (KSPM)
│   │   ├── 4C model: Cloud -> Cluster -> Container -> Code
│   │   ├── Posture: kube-bench (CIS), Kubescape (NSA/CISA + MITRE mapping)
│   │   ├── Runtime: Falco (eBPF syscall detection), Tetragon, KubeArmor
│   │   ├── Policy-as-code: OPA/Gatekeeper (Rego), Kyverno (YAML-native)
│   │   ├── Network: Cilium/Calico network policies, mTLS service mesh
│   │   ├── Supply chain: image signing (Cosign), SBOMs, admission control, registry allowlists
│   │   └── Cert anchor: CKA (prereq) -> CKS
│   ├── Serverless & PaaS security
│   │   ├── Lambda/Functions: over-permissioned execution roles, event-source abuse
│   │   └── API security (OWASP API Top 10), API gateways, auth on every edge
│   ├── Cloud-native architecture & Zero Trust
│   │   ├── Identity-centric perimeter, micro-segmentation, workload identity
│   │   └── NIST 800-207 applied to multi-cloud
│   ├── Detection engineering (cloud)
│   │   ├── Detection-as-code: Sigma rules, native analytics, Sentinel KQL, GuardDuty tuning
│   │   ├── Map detections to MITRE ATT&CK Cloud matrix (IaaS / SaaS / containers)
│   │   └── Lab: write 5 cloud detections, test with Stratus Red Team, tune false positives
│   ├── Purple teaming & adversary emulation [authorised labs]
│   │   └── Stratus Red Team / Leonidas -> emulate technique -> verify detection fires
│   └── Multi-cloud & hybrid
│       └── Consistent policy across AWS+Azure+GCP; landing zones; sovereign/air-gapped
│
├── Phase 7: Engineering & Automation (ongoing -- this is what separates senior from junior)
│   ├── Python -- MUST be fluent (boto3, azure-sdk, google-cloud SDKs, automation, parsing)
│   ├── Bash -- fluent (glue, CI steps, quick ops)
│   ├── Go -- read-and-modify is enough (most cloud-native tools are Go; write small tools)
│   ├── SQL / KQL -- fluent for log analysis (Athena, Sentinel KQL, BigQuery)
│   ├── Regex, JSON, YAML -- fluent (policies and findings live here)
│   ├── Git + CI/CD -- fluent (GitHub Actions / GitLab CI; security gates in pipelines)
│   ├── Infrastructure as Code
│   │   ├── Terraform (fluent -- the lingua franca), CloudFormation (read), Bicep (Azure)
│   │   └── Policy-as-code: Sentinel/OPA on IaC, pre-apply scanning (Checkov/Trivy)
│   ├── Containers & orchestration: Docker (fluent), Kubernetes (fluent for K8s roles)
│   ├── Detection-as-code & SOAR: Sigma, native playbooks, EventBridge/Logic Apps automation
│   ├── Cloud Custodian (policy engine for auto-remediation -- learn it well)
│   └── Project: a CI/CD pipeline that scans IaC + images, signs artifacts, and blocks on critical
│
├── Phase 8: Frameworks, Standards & Methodologies (study in parallel, ~ongoing)
│   ├── Pointer -> see the Frameworks Tree below
│   └── Minimum to internalise: Shared Responsibility, CSA CCM, CIS Benchmarks,
│       MITRE ATT&CK Cloud, NIST CSF 2.0, NIST 800-207 (Zero Trust), the
│       Well-Architected Security Pillars (AWS/Azure/GCP)
│
├── Phase 9: Real-World Projects (continuous)
│   └── Pointer -> see the Projects Tree below (5 beginner / 5 intermediate / 5 advanced / 3 capstone)
│
├── Phase 10: Career Roadmap
│   └── Pointer -> see the Career Tree below
│
├── Phase 11: Certification Path
│   └── Pointer -> see the Certification Tree below
│
├── Phase 12: Interview Preparation (2-4 weeks before applying)
│   └── Pointer -> see the Interview section below (20+ questions)
│
├── Phase 13: Professional Reporting & Communication (build throughout)
│   ├── Technical report structure: scope, methodology, findings, evidence, remediation
│   ├── Executive summary: risk in business terms, not CVE counts
│   ├── Severity/priority models (CVSS + exploitability + blast radius)
│   ├── Remediation recommendations engineers can actually action
│   ├── Post-incident reviews / blameless postmortems; lessons learned
│   └── Metrics & KPIs dashboards for leadership
│
├── Phase 14: Expert Specialisations (year 2+)
│   └── Pointer -> see the Specialisations Tree below
│
└── Phase 15: Continuous Learning (forever)
    ├── Official docs & well-architected security pillars (AWS/Azure/GCP) -- primary source
    ├── Standards updates: CSA guidance, CIS Benchmarks, NIST, MITRE ATT&CK releases
    ├── Vendor research: Wiz Research, Datadog Security Labs, Unit 42, Rhino Security Labs
    ├── Communities: Cloud Security Forum/Office Hours, fwd:cloudsec, CSA chapters
    ├── Practice: Wiz Championship CTFs, CloudGoat updates, hackingthe.cloud techniques
    ├── Conferences: fwd:cloudsec, BSides, re:Inforce, KubeCon, Black Hat/DEF CON cloud villages
    ├── Newsletters/blogs: tl;dr sec, Last Week in AWS (security bits), CloudSecList
    └── Watch the frontier: AI/agentic workload security, NHIs (non-human identities), DSPM

Supporting Tree: Knowledge Dependency (learn in this order)

Knowledge Dependency Chain
│
├── Linux + Networking + Scripting (Python/Bash)        [must precede everything]
│   └── enables -> Cloud fundamentals (one provider)
│       └── enables -> IAM & identity                   [the gatekeeper topic]
│           ├── enables -> CSPM / posture management
│           ├── enables -> Network & data security
│           └── enables -> Logging & monitoring
│               └── enables -> Detection & incident response
│                   └── enables -> Detection engineering / purple teaming
│
├── Containers (Docker) ----------> Kubernetes ----------> Kubernetes security (KSPM)
│                                                          (Falco, kube-bench, OPA/Kyverno)
│
├── IaC (Terraform) ---------------> Shift-left / policy-as-code -> CI/CD security
│
└── Frameworks (CSA CCM, CIS, NIST, MITRE) run in parallel the whole way through


Supporting Tree: Tools (grouped by function)

Cloud Security Tooling
│
├── Posture / Misconfiguration (CSPM)
│   ├── OSS: Prowler, ScoutSuite, CloudSploit, OpenSCAP
│   └── Native: AWS Security Hub, Azure Defender for Cloud, GCP Security Command Center
│
├── IaC / Shift-left scanning
│   ├── Checkov, Trivy (config mode), KICS
│   └── tfsec [merged into Trivy]
│
├── Vulnerability / Image / SBOM
│   ├── Trivy (CVEs + secrets + SBOM + IaC), Grype + Syft, Clair
│   └── Cosign / Sigstore (signing & verification)
│
├── Kubernetes / Container
│   ├── Posture & CIS: kube-bench, Kubescape, kubeaudit
│   ├── Runtime (eBPF): Falco, Tetragon, KubeArmor, Sysdig (Falco-based, commercial)
│   ├── Policy-as-code: OPA/Gatekeeper (Rego), Kyverno (YAML)
│   └── Network: Cilium (+ Hubble), Calico
│
├── Identity / Entitlement (CIEM) & attack-path graphing
│   ├── AWS IAM Access Analyzer, native CIEM, conditional access (Azure)
│   └── PMapper, Cartography (asset & relationship graphs)
│
├── Auto-remediation / policy engine
│   └── Cloud Custodian, native EventBridge/Logic Apps -> function remediation
│
├── Detection & response (CDR)
│   ├── GuardDuty, Defender for Cloud, SCC; Sigma rules; Sentinel (KQL)
│   └── Stratus Red Team / Leonidas (adversary emulation -> validate detections)
│
├── Offensive cloud [authorised labs only]
│   ├── Pacu (AWS), enumerate-iam, CloudGoat/AWSGoat targets, IAM Vulnerable
│   └── ROADtools / AzureHound / GraphRunner (Azure/Entra), GCPGoat
│
└── Unified commercial (CNAPP) [expensive -- demo via trials]
    ├── Wiz, Prisma Cloud, Microsoft Defender for Cloud, Orca
    └── CrowdStrike Falcon Cloud Security, SentinelOne Singularity CNS, Sysdig Secure, Aikido


Supporting Tree: Skills

Skills
│
├── Technical
│   ├── IAM / identity & federation (deepest skill in the field)
│   ├── Network & data security in cloud
│   ├── Container & Kubernetes hardening
│   ├── Logging, monitoring, detection engineering
│   └── Offensive cloud (enumeration, priv-esc, attack paths) -- authorised
│
├── Analytical
│   ├── Attack-path & blast-radius thinking (not flat finding lists)
│   ├── Triage & prioritisation by exploitability + impact
│   └── Threat modelling (STRIDE, attack trees) for cloud architectures
│
├── Operational
│   ├── Incident response & forensics in cloud
│   ├── Guardrail design (preventive + detective)
│   └── Vulnerability & posture management lifecycle
│
├── Engineering
│   ├── Python + Bash + Go (read), Terraform, Docker/K8s
│   ├── CI/CD security gates, detection-as-code, policy-as-code
│   └── Automation & auto-remediation
│
├── Governance
│   ├── Mapping controls to CSA CCM / CIS / NIST / ISO 27017/27018
│   ├── Compliance evidence (SOC 2, PCI DSS, HIPAA, DPDP/GDPR)
│   └── Risk registers & exception handling
│
├── Communication
│   ├── Two-audience reporting (engineer ticket vs risk-owner summary)
│   └── Remediation guidance engineers will actually accept
│
└── Leadership (senior+)
    ├── Security strategy & landing-zone standards
    ├── Mentoring, cross-team influence, vendor evaluation
    └── Driving secure-by-default culture


Supporting Tree: Lab (build it up gradually)

Home / Cloud Lab Build-Out
│
├── Stage 1: Single-cloud sandbox
│   ├── AWS free-tier account + HARD billing alerts + budget caps
│   ├── CLI + SDK configured (named profiles, no long-lived root keys)
│   └── First targets: flaws.cloud, flaws2.cloud (nothing to deploy)
│
├── Stage 2: Deployable vulnerable targets
│   ├── CloudGoat (terraform) -- run + destroy each scenario
│   ├── IAM Vulnerable (priv-esc playground), Sadcloud, AWSGoat
│   └── Discipline: cloud-nuke / aws-nuke teardown after every session
│
├── Stage 3: Multi-cloud
│   ├── Add Azure + GCP free tiers; EntraGoat (Entra), GCPGoat
│   └── Practice cross-cloud posture scanning with Prowler/ScoutSuite
│
├── Stage 4: Defender stack
│   ├── Central logging account (org CloudTrail -> S3 -> Athena/KQL)
│   ├── Local Kubernetes (kind/minikube) + Falco + Trivy + kube-bench + Kyverno
│   └── Detection-as-code repo with Sigma rules + tests
│
└── Stage 5: Full pipeline
    ├── IaC repo -> CI scans (Checkov/Trivy) -> image sign (Cosign) -> deploy gate
    └── Stratus Red Team emulation -> verify detections fire


Supporting Tree: Projects

Projects (each must produce EVIDENCE for a portfolio)

├── BEGINNER
│   ├── Public-bucket hunt & fix
│   │   ├── Objective: find and remediate publicly-exposed storage
│   │   ├── Environment: your AWS/Azure/GCP sandbox
│   │   ├── Tasks: scan with Prowler, fix, add a preventive guardrail, re-scan
│   │   ├── Tools: Prowler, native CLI, SCP/Azure Policy
│   │   ├── Deliverables: finding -> fix -> verification report
│   │   ├── Skills: CSPM, data exposure, guardrails
│   │   └── Portfolio: sanitised before/after write-up + screenshots
│   ├── Least-privilege IAM refactor (over-permissioned role -> minimal, with proof)
│   ├── Org-wide CloudTrail + Athena query pack (10 useful security queries)
│   ├── Segmented VPC/VNet with controlled egress (architecture diagram)
│   └── CIS-benchmark a Linux VM + a cloud account (kube-bench / Prowler compliance)
│
├── INTERMEDIATE
│   ├── CloudGoat scenario walkthroughs (attack + the detections that would catch it)
│   ├── OSS "DIY CNAPP" stack (Prowler+Trivy+ScoutSuite+Cloud Custodian) + gap analysis
│   ├── CI/CD security gate (Checkov + Trivy + Cosign signing, fails on critical)
│   ├── Kubernetes hardening lab (kube-bench fixes + Kyverno policies + Falco rules)
│   └── Cloud incident-response runbook + a worked tabletop (key compromise scenario)
│
├── ADVANCED
│   ├── Multi-cloud detection-as-code repo (Sigma + KQL, mapped to ATT&CK Cloud, tested with Stratus)
│   ├── Attack-path analysis tool/report using Cartography or PMapper on a real account
│   ├── Auto-remediation framework with Cloud Custodian (10 policies + dashboards)
│   ├── Entra/OAuth illicit-consent attack-and-defend lab (ties to your BEC IR work)
│   └── Secure landing-zone reference (IaC) with guardrails baked in
│
└── CAPSTONE (interview centrepieces)
    ├── End-to-end secured platform: IaC -> scanned pipeline -> hardened K8s -> detection -> IR runbook
    ├── Full cloud security assessment of a deliberately-vulnerable estate, written as a client report
    └── Open-source contribution to a cloud security tool (Prowler/Trivy/Kubescape) or a published technique


Supporting Tree: Certification Path (verified current as of 2026)

Certification Sequence
│
├── FOUNDATION (optional if experienced)
│   ├── CompTIA Security+        [useful, HR gate] -- general security baseline
│   └── AWS Cloud Practitioner / Azure AZ-900 / SC-900  [useful] -- cloud + security basics
│
├── CLOUD FLUENCY (before security specialisation)
│   └── AWS Solutions Architect Associate (or Azure/GCP associate)  [useful]
│       -- builds the service knowledge the security exams assume
│
├── VENDOR-NEUTRAL CORE
│   ├── CCSK v5 (CSA)            [useful, great first cloud-security cert]
│   │     Open-book; CSA Security Guidance v5 + ENISA; no experience requirement
│   └── CCSP (ISC2)             [essential at senior level; gold-standard, vendor-neutral]
│         6 domains; requires 5 yrs IT / 3 yrs infosec / 1 yr in a CCSP domain
│         (CISSP holders auto-qualify; pass first as "Associate" if short on experience)
│
├── VENDOR-SPECIFIC (pick the cloud you actually run)
│   ├── AWS Certified Security - Specialty  [essential if AWS]
│   │     NOTE: SCS-C03 replaced SCS-C02 in Dec 2025 -- study the C03 guide
│   ├── Azure: AZ-500 (Security Engineer Associate) -> SC-100 (Architect) [+ SC-300 identity]
│   └── Google Professional Cloud Security Engineer  [essential if GCP]
│
├── PRACTICAL / HANDS-ON
│   ├── CKS (Certified Kubernetes Security Specialist)  [essential if K8s; CKA is a hard prereq]
│   └── GIAC GCSA / GCSE (SANS)  [useful but expensive] -- automation / enterprise cloud sec
│
└── LEADERSHIP / GOVERNANCE (year 3+)
    ├── CISSP (ISC2)            [useful for architect/leadership roles]
    └── CISM / CCAK (ISACA)     [optional] -- management / cloud audit

Sequencing advice: CCSK → AWS Security Specialty (SCS-C03) → CKS → CCSP is a strong 18-month arc for an AWS-leaning engineer. Don't chase certs ahead of hands-on lab evidence — a CloudGoat portfolio beats a cert with no practice behind it.


Supporting Tree: Career Progression

Career Progression
│
├── Entry-Level
│   ├── Roles: Cloud Security Analyst, Jr Cloud Security Engineer, SOC Analyst (cloud)
│   ├── Expected: IAM basics, CSPM triage, logging, one cloud, scripting
│   └── Portfolio: CloudGoat write-ups, least-privilege refactor, CSPM reports
│
├── Mid-Level
│   ├── Roles: Cloud Security Engineer, DevSecOps Engineer, Cloud Pentester
│   ├── Depth: automation (Terraform/Python), detection engineering, K8s security
│   └── Specialise: offensive cloud OR detection/IR OR platform/DevSecOps
│
├── Senior
│   ├── Roles: Sr Cloud Security Engineer, Lead, Principal Engineer
│   ├── Owns: landing-zone standards, guardrail strategy, cross-team influence
│   └── Mentoring + architecture + business-risk translation
│
└── Expert & Leadership
    ├── Cloud Security Architect / Principal / Distinguished Engineer
    ├── Head of Cloud Security / Security Director / CISO track (managerial)
    ├── Independent consultant / MSSP practice lead (vs enterprise in-house)
    └── Researcher / vendor security research / conference speaker

Adjacent moves: AppSec, Detection Engineering, Red Team, GRC/Compliance, Platform Engineering.
Transferable in: SOC analysis, network engineering, DevOps, software engineering.


Supporting Tree: Frameworks & Standards (each with its purpose)

Frameworks & Standards
│
├── Cloud-specific governance
│   ├── CSA Cloud Controls Matrix (CCM)  -- control framework for cloud, maps to others
│   ├── CSA Security Guidance v5          -- the CCSK study baseline
│   ├── CSA CAIQ / STAR                   -- vendor assurance questionnaire/registry
│   └── ISO/IEC 27017 & 27018             -- cloud controls + cloud PII protection
│
├── Risk & governance (general)
│   ├── NIST CSF 2.0                      -- govern/identify/protect/detect/respond/recover
│   ├── NIST SP 800-53                    -- control catalogue
│   ├── NIST SP 800-207                   -- Zero Trust Architecture
│   ├── NIST SP 800-204                   -- microservices/service-mesh security
│   └── ISO/IEC 27001                     -- ISMS certification standard
│
├── Attack modelling & detection
│   ├── MITRE ATT&CK (Cloud: IaaS/SaaS, Containers)  -- TTP taxonomy for detections
│   ├── MITRE D3FEND                      -- defensive countermeasure mapping
│   ├── Cyber Kill Chain / Diamond Model  -- intrusion analysis
│   └── Sigma / YARA                      -- portable detection rule formats
│
├── Benchmarks & hardening
│   ├── CIS Benchmarks (AWS/Azure/GCP/Kubernetes/Docker)  -- the config gold standard
│   └── Provider Well-Architected "Security Pillar" (AWS/Azure/GCP)
│
├── Application & supply chain
│   ├── OWASP Top 10 / API Top 10 / Cloud-Native Top 10
│   ├── OWASP ASVS                        -- app verification standard
│   └── SLSA / SBOM (SPDX, CycloneDX)     -- supply-chain integrity & provenance
│
├── Incident response & testing
│   ├── NIST SP 800-61                    -- incident handling
│   └── PTES / OSSTMM                     -- testing methodology
│
└── Compliance (sector/region -- only what applies)
    ├── SOC 2, PCI DSS, HIPAA
    ├── GDPR / India DPDP Act             -- data protection (relevant to your region)
    └── DORA / NIS2 (EU)                  -- if serving EU financial/critical sectors


Supporting Tree: Attack-and-Defence (cloud)

Technique -> Visibility -> Detection -> Investigation -> Prevention -> Response

├── Stolen access key / standing creds
│   ├── Log source: CloudTrail / Activity Log / Audit Logs
│   ├── Detect: GuardDuty creds-from-new-IP, impossible travel
│   ├── Investigate: session timeline, what the key touched
│   ├── Prevent: short-lived creds, IAM Identity Center, no long-lived keys
│   └── Respond: deactivate key, revoke sessions, rotate, review blast radius
│
├── IMDS credential theft via SSRF
│   ├── Log: VPC flow + app logs; Detect: unusual metadata access
│   ├── Prevent: IMDSv2 enforced, egress controls, WAF
│   └── Respond: rotate role creds, patch SSRF, restrict instance role
│
├── IAM privilege escalation (pass-role, policy attach, role chaining)
│   ├── Detect: anomalous AttachPolicy/PassRole/CreateAccessKey
│   ├── Prevent: permission boundaries, deny risky actions via SCP, IAM Access Analyzer
│   └── Respond: revoke escalated grants, snapshot for forensics
│
├── Public storage / data exfiltration
│   ├── Detect: public-ACL/policy changes, large unusual GetObject volume
│   ├── Prevent: block-public-access org-wide, DSPM classification
│   └── Respond: lock bucket, identify exposed data, breach assessment
│
├── OAuth illicit consent / malicious app (Entra)
│   ├── Detect: new high-privilege app consent, unusual Graph access
│   ├── Prevent: admin-consent workflow, app governance, conditional access
│   └── Respond: revoke app, revoke tokens, hunt mailbox/Graph activity
│
└── Container escape / cryptomining
    ├── Log/Detect: Falco runtime rules (shell in container, /etc/shadow read, odd egress)
    ├── Prevent: non-root, read-only FS, Kyverno admission policy, signed images
    └── Respond: kill pod, cordon node, forensic snapshot, rotate cluster secrets


Supporting Tree: Specialisations (year 2+)

Expert Specialisations
│
├── Offensive Cloud / Cloud Red Team
│   └── Foundation: deep IAM + provider internals; Tools: Pacu, attack-path graphs; authorised engagements
├── Cloud Detection Engineering & Threat Hunting
│   └── Foundation: logging + ATT&CK Cloud; detection-as-code at scale; SIEM/UEBA
├── Kubernetes & Cloud-Native Security
│   └── Foundation: CKA/CKS; eBPF runtime, supply chain, service mesh, admission control
├── DevSecOps / Platform Security Engineering
│   └── Foundation: IaC + CI/CD; secure-by-default platforms, golden paths, policy-as-code
├── Cloud Security Architecture
│   └── Foundation: multi-cloud + Zero Trust; landing zones, enterprise standards
├── Data Security Posture (DSPM) & Privacy
│   └── Foundation: data classification + crypto; sensitive-data flow, NHIs, AI-data governance
├── Cloud GRC / Compliance & Audit
│   └── Foundation: CCM/ISO/NIST; control mapping, evidence automation, audit readiness
└── AI / Agentic Workload Security  [emerging, 2026 frontier]
    └── Foundation: identity + runtime; over-privileged AI agents, model/data pipeline security


Supporting Tree: Technology Ecosystem

How the pieces relate
│
Identities (human + non-human/NHIs) ──grant access to──► Cloud control plane (APIs)
        │                                                        │
        ▼                                                        ▼
   Workloads (VMs, containers, serverless) ◄──run on──► Cloud platform (compute/storage/net)
        │                                                        │
        ▼                                                        ▼
   Applications / APIs ──read/write──► Data stores (object, DB, secrets, keys/KMS)
        │                                                        │
        └──────────► Networks (VPC/VNet, egress, WAF) ◄──────────┘
                              │
        Security controls layer over ALL of the above:
        CSPM (posture) • CIEM (entitlements) • CWPP (workload) • DSPM (data)
        KSPM (k8s) • CDR (detection) ── all converging into ──► CNAPP
                              │
                    Telemetry ──► SIEM / detection-as-code ──► SOAR / auto-remediation


Weekly Plan (blocks + 3 pacing tracks)

Content is grouped into blocks; pick your pacing track for calendar length.

Block A (Weeks 1-4):  Foundations review + AWS fundamentals + IAM basics
   - Deliverable: deployed app stack; first least-privilege role
Block B (Weeks 5-9):  IAM deep dive + CSPM (Prowler) + logging (CloudTrail/Athena)
   - Deliverable: CSPM triage report; org logging set up
Block C (Weeks 10-14): Network + data security + secrets + KMS; flaws.cloud
   - Deliverable: segmented VPC diagram; public-bucket remediation
Block D (Weeks 15-20): Tooling mastery (Trivy/Checkov/ScoutSuite) + CloudGoat scenarios
   - Deliverable: CloudGoat write-ups + DIY-CNAPP gap analysis
Block E (Weeks 21-26): IR + guardrails (SCP/Custodian) + detection-as-code (Sigma/KQL)
   - Deliverable: IR runbook + 5 tested detections
Block F (Weeks 27-34): Containers/K8s security (kube-bench, Falco, Kyverno) + CKS prep
   - Deliverable: hardened cluster lab; CKS-ready
Block G (Weeks 35-44): Automation (Terraform/Python), CI/CD gates, multi-cloud, capstone
   - Deliverable: secured pipeline capstone + full assessment report
Block H (Weeks 45-52): Cert push (AWS Security Specialty / CCSP) + interview prep + portfolio

Pacing tracks (how the same content stretches)
├── Fast track     (15-20 hrs/wk) -> ~7-9 months
├── Standard track (8-12 hrs/wk)  -> ~12-14 months
└── Part-time      (4-6 hrs/wk)   -> ~18-22 months


Tool Comparison Tables

CSPM / unified platforms

ToolTypeCostDeploymentStrengthBest forProwlerOSS CSPMFreeCLI/agentlessMulti-cloud checks, fast startLearners, auditsScoutSuiteOSS CSPMFreeCLI/agentlessMulti-cloud, readable reportsQuick posture reviewSecurity Hub / Defender / SCCNativePay-as-you-goNativeDeep provider integrationSingle-cloud shopsWizCNAPPEnterpriseAgentless graphAttack-path Security GraphLarge multi-cloud orgsOrcaCNAPPEnterpriseAgentless SideScanningFast, no agentsAgent-averse teamsPrisma CloudCNAPPEnterpriseAgent + agentlessBroadest feature setFull-stack coverage

Kubernetes security

ToolLayerCostPurposeTrivyScanOSSImages, IaC, K8s configs, secrets, SBOMkube-benchPostureOSSCIS Kubernetes Benchmark auditKubescapePosture+OSSNSA/CISA + MITRE mapping, risk scoreFalcoRuntimeOSS (CNCF)eBPF syscall threat detectionKyverno / OPA GatekeeperPolicyOSSAdmission control / policy-as-codeCiliumNetworkOSS (CNCF)eBPF network policy + observability


Portfolio Guide

Sanitise everything: no real client names, redact IPs/account IDs/hostnames, use lab data only. Show range across attack, defend, and automate.

cloud-security-portfolio/
├── README.md                  (who you are, skills index, links to each project)
├── projects/
│   ├── 01-cspm-triage/         (Prowler report, before/after, verification)
│   ├── 02-iam-least-privilege/ (over-priv role -> minimal, with policy diff)
│   ├── 03-cloudgoat-writeups/  (attack steps + detections that would catch each)
│   ├── 04-k8s-hardening/       (kube-bench fixes, Kyverno policies, Falco rules)
│   └── 05-secured-pipeline/    (IaC scan + image sign + deploy gate)
├── detections/                 (Sigma + KQL rules, ATT&CK-mapped, with tests)
├── iac/                        (Terraform landing-zone snippets with guardrails)
├── writeups/                   (CTF solutions, technique blog posts)
└── scripts/                    (Python/boto3 automation, Cloud Custodian policies)

README format per project: Objective → Environment → What you did → Findings/Result → Evidence (screenshots/diagrams) → Remediation → Skills demonstrated.


Final Summary

1. Ten most important skills


IAM / identity & federation (the single deepest skill)
Attack-path & blast-radius thinking (prioritisation over finding-dumps)
CSPM / misconfiguration management lifecycle
Cloud logging, monitoring & detection engineering
Infrastructure-as-Code + policy-as-code (Terraform, OPA/Kyverno)
Container & Kubernetes hardening
Cloud incident response & forensics
Automation & scripting (Python, Bash)
Network & data security (segmentation, encryption, secrets, KMS)
Framework mapping & risk communication (CSA CCM / CIS / NIST / ATT&CK)


2. Ten most important tools / platforms


Prowler (CSPM) 2. Trivy (scan/SBOM) 3. ScoutSuite (multi-cloud posture)
Cloud Custodian (auto-remediation) 5. Falco (runtime) 6. kube-bench + Kubescape (K8s posture)
OPA/Gatekeeper or Kyverno (policy) 8. Terraform + Checkov (IaC) 9. Pacu + CloudGoat (offensive/labs)
A CNAPP (Wiz/Prisma/Defender/Orca — via trials) + native (CloudTrail/Security Hub/Sentinel)


3. Five best portfolio projects


End-to-end secured pipeline capstone (IaC → scan → sign → hardened K8s → detection → IR)
CloudGoat attack write-ups paired with the detections that catch them
IAM least-privilege refactor with policy diffs and proof
Multi-cloud detection-as-code repo mapped to ATT&CK Cloud (tested with Stratus Red Team)
Full cloud security assessment of a vulnerable estate, written as a client report


4. Recommended certification sequence

CCSK v5 → (cloud associate if needed) → AWS Security Specialty (SCS-C03) or AZ-500/SC-100 → CKS → CCSP → (CISSP/CCAK for leadership). Verify exam codes before booking — they churn (AWS moved C02→C03 in Dec 2025).

5. Most common beginner mistakes


Treating a green CSPM dashboard as "secure" while identity risk is wide open.
Dumping scanner output instead of prioritising the few exploitable paths.
Skipping IAM depth — it's the whole game.
Watching 40 hours of video before ever touching a console (lab first).
Leaving lab resources running (surprise bills) — set budget alerts and nuke nightly.
Memorising one provider's UI instead of the transferable concepts.
Ignoring automation — manual cloud security doesn't scale past one account.


6. Job-readiness checklist


 Can write and reason about a least-privilege IAM policy from scratch
 Can run a CSPM scan and produce a prioritised, business-readable remediation report
 Can stand up org-wide logging and query it (Athena/KQL)
 Can build a segmented network with controlled egress
 Can harden a Kubernetes cluster and write a Falco/Kyverno rule
 Can automate a guardrail (SCP/Azure Policy + Cloud Custodian)
 Can walk through a cloud IR scenario (key compromise) end to end
 Has a public, sanitised portfolio with attack + defend + automate work
 Holds at least one relevant cert (CCSK or a vendor security cert)


7. 30-day starting plan

Week 1: Foundations review + AWS free tier (billing alerts!) + Shared Responsibility.
Week 2: IAM deep dive; do flaws.cloud levels 1–6.
Week 3: Install Prowler, scan a sandbox, triage and fix the top findings.
Week 4: Enable CloudTrail, write 5 Athena queries, document everything in a Git repo.

8. 90-day progress plan

Add ScoutSuite multi-cloud, CloudGoat scenarios with paired detections, a segmented VPC,
secrets/KMS work, and start the DIY-CNAPP OSS stack. Begin CCSK v5 study. Two portfolio projects done.

9. One-year mastery plan

Hands-on across AWS+Azure+GCP; K8s security (Falco/Kyverno/kube-bench) with CKS;
automation in Terraform/Python; detection-as-code mapped to ATT&CK; a secured-pipeline capstone;
AWS Security Specialty (or AZ-500) earned; 4–5 portfolio projects; applying for engineer roles.

10. Recommended next specialisations

Cloud Detection Engineering • Kubernetes/Cloud-Native Security • Offensive Cloud/Red Team •
DevSecOps/Platform Security • Cloud Security Architecture • DSPM/Data & Privacy •
Cloud GRC/Audit • AI/Agentic Workload Security (the 2026 frontier).
...
```

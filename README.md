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

...
```

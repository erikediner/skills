---
name: security-analysis
description: Architectural security review with STRIDE, OWASP Top 10, NSM and GDPR, with a prioritized list of actions.
disable-model-invocation: true
---

# security-analysis

You are a security architect. The goal is to find **obvious architectural weaknesses**
in an existing codebase and deliver a prioritized list of actions. Not a full
audit, and not a line-by-line review.

Create `docs/security/SECURITY-ANALYSIS.md` (template: [REPORT-TEMPLATE.md](REPORT-TEMPLATE.md))
before you start the analysis. Fill in findings **as you go** after each step. Do not
wait until the end.

## Step 1: Gather the basis

Read `/memories/repo/codebase.md` if it exists (from `codebase-overview`). It
is a **starting point**, not the truth. If it is old (see `Last updated`) or
unclear, verify against the actual code. If it does not exist: read `README.md`
and map language, framework, deploy target, auth and data flow yourself.
Identify the **attack surface**: HTTP endpoints, queues, file uploads, external
integrations, secrets, databases.

Call `grilling`. Topic: exposure (internet or internal), the most sensitive data,
whether **personal data** is processed (name, national ID number, health,
location, IP), and whether a threat model or DPIA already exists.

## Step 2: STRIDE per component

Fill findings into the report as you work through the components. For each
clear component (API, frontend, job, integration, database): note findings
under each STRIDE category. Skip what obviously does not apply.

| Letter | Threat | Typical weakness |
|---|---|---|
| **S** | Spoofing | Weak authentication, missing MFA, shared secrets |
| **T** | Tampering | Missing input validation, no integrity check |
| **R** | Repudiation | Missing audit logging of sensitive actions |
| **I** | Information Disclosure | Secrets in code or logs, overly detailed error messages |
| **D** | Denial of Service | No rate limiting, unbounded resources |
| **E** | Elevation of Privilege | Missing authorization check, overly broad roles |

## Step 3: OWASP Top 10 (2021)

Mark each as **finding**, **probably OK** or **not relevant**:
A01 Broken Access Control, A02 Cryptographic Failures, A03 Injection,
A04 Insecure Design, A05 Security Misconfiguration, A06 Vulnerable Components,
A07 Auth Failures, A08 Software/Data Integrity, A09 Logging Failures, A10 SSRF.

## Step 4: NSM secure lifecycle (selection)

Focus on what is observable in the code:

- **Secret handling** (key vault vs. environment variables vs. hardcoded)
- **Logging and monitoring** (sensitive fields masked, centralized)
- **Patching strategy** (CI that flags old packages)
- **Access control** (least privilege, segmentation)
- **Secure configuration** (HTTPS, secure headers, tight CORS)

## Step 5: Privacy (GDPR / personopplysningsloven)

If personal data is processed: go through
[PRIVACY-CHECKLIST.md](PRIVACY-CHECKLIST.md) (legal basis, data minimization,
storage limitation, privacy by design, data subject rights, third-party
transfer, breach notification, DPIA). Clear breaches are weighted as
**Critical** or **High**.

## Step 6: Light dependency check

Find the manifest files (`package.json`, `*.csproj`, `requirements.txt`, `pom.xml`,
`go.mod`). Do not run audit tools. Look for obviously outdated packages
(several major versions behind), abandoned packages (no releases in 2+ years)
and packages with a known poor security record. For a full CVE scan: suggest
the developer runs `npm audit`, `dotnet list package --vulnerable` or `pip-audit`.

## Step 7: Prioritize the findings

Write each finding in **JIRA issue format** (see [REPORT-TEMPLATE.md](REPORT-TEMPLATE.md))
so it can be pasted straight into JIRA. Priority: **Critical** (exploitable
now, high impact) maps to `Highest`, **High** (should be fixed soon) to `High`,
**Medium** (limited exposure) to `Medium`, **Low** (best practice deviation)
to `Low`. Put **easy to fix and high risk** at the top. Each finding must have
a concrete action and acceptance criteria. Not "improve logging", but "log
`userId` and `action` on every PUT/DELETE in `/api/admin/*`".

## Step 8: Finish the report

The report is already filled in along the way. Now: read through for
consistency, sort findings by priority, put today's date in `Last updated`,
and add a line to the change log. If the file already existed: keep the
status (`Open` / `Fixed` / `Accepted risk`) of existing findings.

## Step 9: Confirm

Show the prioritized list and ask:
> Which findings do you want to take further? Should we turn the critical ones into tasks with `split-tasks`?
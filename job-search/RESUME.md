# Bhairi Harish

**Backend Engineer — .NET / C# / Distributed APIs**

Hyderabad, India · `[PHONE]` · `[PERSONAL EMAIL]` · linkedin.com/in/`[HANDLE]` · github.com/`[HANDLE]`

> Replace every `[BRACKETED]` placeholder before sending. See the FILL-IN checklist at the bottom.
> Every non-bracketed claim in this resume is backed by `EVIDENCE-PACK.md`.

---

## Summary

Backend engineer with `[TOTAL]` years' experience building and operating the player-account,
authentication and regulatory-compliance API platform behind Entain's global betting brands
(bwin, Ladbrokes, Coral, BetMGM, Sportingbet, partypoker). Delivered 87 merge requests — 81 merged to
production — across a high-traffic ASP.NET Core platform serving 15+ regulated markets. Owned 10
end-to-end production releases, reviewed ~70 teammates' merge requests, and handled P2 live incidents
on a platform where downtime is a regulatory event. Depth in authentication (passkeys/WebAuthn),
compliance pipelines, SOAP-to-REST modernisation, and concurrency correctness.

---

## Technical Skills

| | |
|---|---|
| **Languages** | C#, SQL (T-SQL), TypeScript, JavaScript |
| **Frameworks** | .NET / ASP.NET Core, WCF & SOAP services, REST, Roslyn |
| **Data & Caching** | MSSQL, distributed caching, in-memory caching, Redis / Valkey, Elasticsearch |
| **Testing** | NUnit, Moq, unit & integration testing, TDD |
| **DevOps & Security** | GitLab CI, Jenkins, Checkmarx (SAST/SCA), Git, feature toggles, API versioning |
| **Cloud & Ops** | AWS, Kubernetes, Cloudflare caching, Kibana, ServiceNow, Jira |
| **Domain** | Regulated iGaming, Responsible Gaming compliance, KYC/AML, authentication & session security, multi-jurisdiction rollout |

---

## Experience

### Entain — Software Development Engineer I
**Entain Software Development Services (India) Pvt Ltd · Hyderabad · `[START MON YYYY]` – `[END MON YYYY]`**

Team: **PosAPI Platform (PPOSv3)** — the ASP.NET Core Web API platform handling player accounts,
login, wallet and regulatory compliance for Entain's brands across Europe, North America and Brazil.

**Authentication & session security**
- Built the platform-side **passkey / WebAuthn** flow end to end — registration start and finish
  endpoints, passkey retrieval, and the passkey-creation login interceptor — enabling passwordless
  login for millions of accounts.
- Designed **versioned session tokens** (version-prefixed `SessionState`) so the platform can rotate
  cryptographic algorithms without invalidating live player sessions — a zero-downtime migration path
  for a security-critical primitive.
- Delivered the **OTP verification API** for UK contact verification at registration, and fixed
  CustomerConnector URL/version resolution that was breaking logins on a new brand integration.

**Regulatory compliance at scale**
- Shipped **10+ login and registration compliance interceptors** against distinct regulatory regimes —
  Pennsylvania and Portugal KYC, Spain document-expiry re-KYC and AML, Italy document upload and
  product-level self-exclusion, UK regulation, Brazil self-exclusion, Alberta and Ontario — on a
  pluggable pipeline where a defect blocks real-money play in an entire jurisdiction.
- Built **Responsible Gaming** features including the end-of-year player statement API, deposit-limit
  cancellation, session- and stake-limit APIs, net-loss limit reporting, and a self-exclusion
  reversal-request flow.
- Code shipped to **15+ markets**: UK, Italy, Spain, Germany, Austria, Denmark, Greece, Portugal,
  Brazil, Ireland, Bulgaria, Netherlands, Canada (Ontario, Alberta), USA (Nevada, Pennsylvania,
  Missouri, Massachusetts).

**Platform modernisation**
- Migrated legacy **SOAP endpoints to REST** (address lookup by postcode, country pre-dials, job-title
  lists, static-data services) and **regenerated the platform's entire WCF contract surface to
  async-only**, removing a class of thread-pool starvation risk.
- Converted core paths from **synchronous to asynchronous** — account publishing, CRM segmentation,
  the SQL invoker abstraction — and removed blocking `Execute`/`ExecuteScalar` from the data layer.
- Replaced bespoke caching and monetary-conversion abstractions (`ICacheFactory`,
  `MonetaryExtensions`) with platform-standard implementations, cutting duplicated infrastructure.

**Reliability & production ownership**
- Cut and shipped **10 production releases** (PPOS v9.1.0 → v10.6.1, PPOS-Auth v9.2.0/v9.3.0,
  StaticDataService v6.1.0) including a **targeted hotfix release** and releases executed under
  change-freeze conditions — owning changelog, versioning, stakeholder comms and merge-back.
- Resolved **7 production incidents up to P2**, including a wallet/betslip/deposit degradation
  affecting eight European markets and a `refresh_token` failure spike on the auth service.
- Fixed concurrency and correctness defects traced from live incidents — a **non-thread-safe MD5
  calculator**, a distributed-cache staleness bug, and null-handling in generic API constraints.
- Named escalation point for other teams: debugged registration failures in Brazil, Ladbrokes
  account-creation errors, Jumio identity-verification breakage, and environment outages across the
  payments, poker, affiliate and risk workstreams.

**Engineering practice**
- Reviewed **~70 merge requests** from teammates across the core platform repository, acting as a
  quality gate on contract changes, interceptor logic and release branches.
- Migrated the CI security pipeline from **Fortify to Checkmarx** across two repositories, adding both
  SAST and software-composition-analysis stages.
- Introduced **unit-test coverage for the interceptor pipeline**, the platform's highest-risk
  compliance surface.
- Delivered **feature-toggle-gated rollouts** (journaling system, tax-province registration field,
  feature-level filtering on batch and configuration endpoints) enabling progressive, reversible releases.

---

## Projects

**C# Type Finder — MCP Server** · .NET 10, Roslyn, Model Context Protocol · `[REPO LINK]`
Open-source MCP server (stdio/JSON-RPC) that resolves C# type definitions from a real solution via
Roslyn's `MSBuildWorkspace`, so first-party source types resolve rather than only compiled metadata.
Designed for zero-configuration use by any MCP client (Claude Desktop, VS Code, Cursor), packaged as a
.NET global tool. Emphasis on a never-throw tool contract: every failure returns a structured result
with workspace diagnostics instead of an exception.

**`[CAPSTONE NAME]`** · Angular, TypeScript, ASP.NET Core · `[REPO LINK]`
`[Full-stack application with typed API client, reactive forms, route guards, lazy-loaded features and
signals-based state. Fill in once built — see 30-day plan, Week 2.]`

---

## Education

**`[DEGREE]`**, `[INSTITUTION]` — `[YEAR]` · `[CGPA / % if strong, else omit]`

---

## FILL-IN CHECKLIST — do this first

- [ ] Phone, personal email (not the work address), LinkedIn URL, GitHub URL
- [ ] `[TOTAL]` years of experience — from your offer letter, not a guess
- [ ] Employment start and end dates
- [ ] Degree, institution, graduation year
- [ ] Any employer earlier than Entain — add it as a second Experience block
- [ ] Certifications, if any (Azure/AWS, etc.)
- [ ] Publish the MCP server and paste the real repo link
- [ ] Build the Angular capstone and paste the real repo link

## Tailoring notes

- **One page** if total experience is under 3 years. Cut the weakest sub-bullet in each group — keep
  Authentication, Regulatory and Reliability; those are your differentiators.
- **Reorder for the role.** Applying to a product company → lead with Platform Modernisation and
  Engineering Practice. Applying to fintech/regtech → lead with Regulatory Compliance. Applying to a
  security team → lead with Authentication.
- **Keep the numbers.** 87 MRs, 81 merged, ~70 reviewed, 10 releases, 15+ markets, 7 incidents. They
  are real and they survive scrutiny.
- **Never paste internal ticket IDs, URLs or code into a resume, portfolio or take-home.** Describe the
  problem and your solution in your own words.
- Export to PDF/DOCX with a plain single-column layout. No tables in the final ATS version — flatten
  the skills table into comma-separated lines when you convert.

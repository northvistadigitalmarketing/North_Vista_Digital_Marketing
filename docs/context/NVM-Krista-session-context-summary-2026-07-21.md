# Session Context Summary — North Vista Marketing (NVM) / Krista Environment

**Date:** 2026-07-21
**Author:** Claude (Cowork mode)
**Purpose:** Shared context/handoff document. Anyone on the team can paste or attach this into their own Claude Desktop (or any LLM) session so they don't start from a clean slate — it captures the connected environment, discovered structures, IDs, governance findings, and open items from this working session.

**How to use:** Attach this file (or paste its contents) at the start of a new session with the North Vista Krista MCP server connected. The assistant will then know the invoker IDs, doc set contents, tool governance quirks, and project classifications without re-discovery.

---

## 1. Environment & Connections

Working inside **Claude Cowork mode**. Three **Krista MCP servers** are connected:

- **North Vista** (a.k.a. NVM / North Vista Marketing) — the primary workspace for this session. SEO/marketing agency tooling: SEMrush, Google Ads, GoHighLevel, GitHub, Outlook, doc sets, skills.
- **MBL Partners** — Outlook, Office 365 Calendar, SharePoint, weekly reports.
- **Nick Grant's Workspace** — Outlook, Jira, PR review, requirements/TAM skills.

"NVM" = North Vista Marketing. The North Vista server exposes both direct MCP tools (callable like normal tools) and a generic execution path (`krista_execute_tool`) for invoker-hosted requests. **Key behavioral difference discovered:** direct MCP tools executed fine, but `krista_execute_tool` enforces Krista role-based governance (see §5).

---

## 2. Doc Sets (Krista Knowledge Bases)

`document_sets_available` on North Vista returns three doc sets:

1. **nv-boost-krista-ai** — not explored this session.
2. **nv-boost-pivot-fitness** — explored (see below).
3. **ws_0bfb7774-940c-4f7d-90db-2675ba74cbb4** — explored (see below).

**Important retrieval caveat:** the `answer_question` tool answers by **relevance / RAG retrieval**, not a true file index. It returns a `Documents referenced` array per query. To enumerate a doc set, run several *varied* queries (different dates, topics, angles) until the referenced-document set **converges** (stops surfacing new filenames). That convergence method is how the inventories below were built.

### 2a. ws_0bfb7774-940c-4f7d-90db-2675ba74cbb4
Contains **3 meeting transcripts**, all "Project Sync – Krista / NVM" calls (late June 2026):

- `meeting_2026-06-23-1900_Project-Sync-Krista-NVM_IN2P4.txt`
- `meeting_2026-06-25-1900_Project-Sync-Krista-NVM_7UUWR.txt`
- `meeting_2026-06-30-1900_Project-Sync-Krista-NVM_RCIMN.txt`

(1900 = 7:00 PM. Contents not yet summarized.)

### 2b. nv-boost-pivot-fitness
Contains **5 files**:

- `NV-2026-21-Pivot-Fitness-MSA.pdf` — Marketing Services Agreement between **North Vista Marketing LLC** and **Pivot Fitness**. Growth partnership, SEO/content strategy, three-phase plan (Discover → Build → Grow), standard legal clauses.
- `pivot-fitness-landing-page.txt` — Landing page copy for Pivot Fitness (Kaysville, Utah).
- `pivot-fitness-blog-strength-training.txt` — Blog/guide on beginner strength training (benefits, safety for older adults/youth; promotes a 2-week trial for $1).
- `test-blog-strength-training-kaysville.txt` — Test/draft version of the strength-training blog. **(test artifact)**
- `Filer-Skill-Test-2026-07-16.txt` — Internal filing-pipeline test doc; marker phrase "THE CANARY LIFTS AT DAWN." **(test artifact)**

Substantive client material = MSA + landing page + strength-training blog. Two files are leftover test artifacts.

---

## 3. Anthropic Compliance / Data-Handling Discussion

Covered how Anthropic handles data across account tiers (grounded in Anthropic's published policies, July 2026). Key points for future reference:

- **Consumer (Free/Pro/Max):** may be used for training if the user allows it; Anthropic is the data **Controller**; retention 30 days (opted out) or up to 5 years (opted in).
- **Commercial (Team, Enterprise, API, Cowork/Code under those):** **not** used for training by default; the **customer is Controller, Anthropic is Processor**; retention configurable.
- **What Anthropic "sees":** by default no personnel read retained conversations; automated trust & safety classifiers scan I/O; human review only via a controlled path when flagged, by a small approved set, with tamper-proof access logs; Clio does privacy-preserving aggregate analysis (not used for enforcement).
- **Certifications:** SOC 2 Type I & II, ISO 27001:2022, ISO/IEC 42001:2023, HIPAA-ready (BAA available), FedRAMP High, UK Cyber Essentials — all in the Anthropic Trust Center.
- **Zero Data Retention (ZDR):** available per-org for Enterprise/API & Claude Code via Sales; **Covered Models** are an exception requiring 30-day retention.
- **Practical note for NVM/Krista setup:** two vendor boundaries exist — Anthropic (processor under commercial terms) and **Krista** (governs the MCP servers, doc sets, and connector data under its own terms). Confirm the account is under Commercial/Enterprise terms when client data is involved.

---

## 4. SEMrush Invoker Investigation

`krista_list_invokers` on North Vista returned **9 connected invokers**:

| Invoker | Extension | Tools | Approved | Invoker ID |
|---|---|---|---|---|
| North Vista GoHighLevel | GoHighLevel | 20 | 9 | INV_9cba517a-16a7-4ab2-aebf-b9a8c109fc13 |
| Google Ads | Google Ads | 5 | 5 | INV_3b2c9d47-cd66-4d08-ba1b-9baeb53145e9 |
| Office 365 Calendar | Office 365 Calendar | 5 | 5 | INV_11d70efb-be5f-4ba8-93a2-573d52e8ab3d |
| Meeting Agent | Meeting Agent | 1 | 0 | INV_b2b5c706-9d14-4e61-b869-72255703a7f1 |
| Outlook | Outlook | 15 | 7 | INV_64cae1f3-ec97-43ac-94c3-b5eb0c9234b3 |
| Github TEST | Github | 58 | 12 | INV_29ec4333-7aa4-4c1b-8e98-8cb801a9532f |
| GitHub NVM | GitHub | 58 | 0 | INV_0fe6c798-8f83-4ba9-96de-9f7ed9bda64a |
| **SEMRush Sandbox** | SEMRush | 13 | 11 | INV_353a856a-867b-4f1f-a004-6ee3ad09c339 |
| AIQA 2.9 on aiqa52 | AIQA 2.9 | 2 | 2 | INV_f3fde30c-e5ff-4cef-885e-c17d73fd3a7d |

### SEMRush Sandbox invoker (INV_353a856a-867b-4f1f-a004-6ee3ad09c339)
13 tools — 11 APPROVED/executable, 2 PENDING_REVIEW (not executable until admin approval):

| Tool | Display | Type | Governance | Key params |
|---|---|---|---|---|
| `get_projects` | Get Projects | QUERY | APPROVED | none |
| `create_project` | Create Project | CHANGE | **PENDING** | Domain URL, Project Name |
| `url_organic_research` | URL Organic Research | QUERY | APPROVED | URL, Database?, Limit? |
| `export_audit_pdf` | Export Audit PDF | QUERY | APPROVED | Project ID, Snapshot ID |
| `deliver_audit_report` | Deliver Audit Report | CHANGE | APPROVED | Project ID, Recipient Email?, Client Name (**sends email**) |
| `keyword_overview_research` | Keyword Overview Research | QUERY | APPROVED | Phrase, Database? |
| `get_audit_status` | Get Audit Status | QUERY | APPROVED | Project ID |
| `domain_organic_research` | Domain Organic Research | QUERY | APPROVED* | Domain, Database?, Limit? |
| `get_audit_report` | Get Audit Report | QUERY | APPROVED | Project ID, Snapshot ID |
| `run_site_audit` | Run Site Audit | CHANGE | APPROVED | Client URL, Client Name, Crawl Limit? |
| `keyword_research` | Keyword Research | QUERY | APPROVED | Phrase, Database?, Limit? |
| `get_project` | Get Project | QUERY | **PENDING** | Project ID |
| `audit_completed` | Audit Completed (sync end-to-end) | QUERY | APPROVED | Client URL, Client Name, Crawl Limit?, Recipient Email? |

`domain_organic_research` catalog_request_id = `localDomainRequest_ke339501-0000-0000-0000-000000000003`.

\* Marked APPROVED in the invoker, but blocked at execution — see §5.

**Suggested end-to-end audit path:** `get_projects` → `run_site_audit` → `get_audit_status` (poll to FINISHED) → `get_audit_report` → `export_audit_pdf` (→ `deliver_audit_report` to email).

---

## 5. SEMrush Test Run (results)

Ran the **research suite** against a neutral public target (**nike.com** / phrase "running shoes"). **4 of 5 passed:**

| Request | Result | Notes |
|---|---|---|
| `get_projects` | ✅ | 38 active projects returned |
| `keyword_overview_research` | ✅ | "running shoes" → 201K volume, $0.97 CPC, comp 1.0, Commercial intent |
| `keyword_research` | ✅ | 10 related keywords (brooks, best running shoes, etc.) |
| `url_organic_research` | ✅ | nike.com → 10 ranking keywords |
| `domain_organic_research` | ❌ | `MCP error -32602: You do not have permission to execute this tool. Required roles: [Marketing Team]` |

**Key finding (governance):** the 4 passing tools are exposed as **direct MCP tools** and ran fine. `domain_organic_research` is **not** a direct tool, so it had to route through `krista_execute_tool`, which enforces Krista **role-based governance** and blocked it (connection lacks the **Marketing Team** role). This is a permissions/role gap, not a broken request. Other execute-path-only tools (e.g., the pending `create_project` / `get_project`) will likely hit the same wall.

---

## 6. SEMrush Projects Inventory & Classification

`get_projects` returned **38 ACTIVE projects**. Classified below.

### Test artifacts — 20 total (cleanup candidates)
- **Krista Integration Test** (8, domain `krista-integration-test.com`): `29561769`, `29561675`, `29247688`, `29188565`, `29188544`, `29188444`, `29188392`, `29170669`
- **"Main Website SEO Audit"** (8 duplicates, domain `blog.example.com`): `29432824`, `29432811`, `29247512`, `29247507`, `29213091`, `29194233`, `29193911`, `29193768`
- **"Test Business Co."** (2): `30240978` (brighthorizonsconsulting.com), `29565837` (sunrisesolutions-test.com)
- **Ad-hoc tests on krista.ai** (2): `29188400` (Test Project1), `29170720` (TEST Project)

### Krista-related — 11 total
- **Genuine:** `27268136` — "Krista" (www.krista.ai) ← keep
- **Tests (overlap with above):** the 8 Krista Integration Test + `29188400` + `29170720`

### Real client projects (~17) — NOT test, NOT Krista
Podiatry/medical: `29183329` apachefoot.com, `29039716` healthierfeet.com, `28306713` Rocky Mountain Eye Institute, `27729360` Ashton Podiatry, `27518416` Foot and Ankle New Mexico, `27518247` Utah Foot Specialists, `27364486` Fall Creek Foot and Ankle, `26623963` rockymtnfootandankle.com, `25280759` barederm.com, `27518228` alleviocareanywhere.com, `25006116` alleviocare.com, `27518221` Matt Mathison. Other: `29214423` Elite Elevation Tumbling, `27777622` Elite Services and Roofing, `25199230` skidsteersdirect.com, `24985475` softminkyblankets.com. Agency: `26847652` **North Vista** (northvistamarketing.com) — the agency's own site (not Krista).

**Note:** classification inferred from names/domains (`example.com`, `krista-integration-test.com`, "TEST"), not an explicit project flag — verify before deleting. **No Pivot Fitness project exists yet** in SEMrush.

---

## 7. Open Items / Recommendations

1. **Role gap:** grant the connection the **Marketing Team** role to unblock `domain_organic_research` and other `krista_execute_tool`-routed requests.
2. **Pending approvals:** approve `create_project` and `get_project` in Krista Studio → MCP Server → Tool Governance if project creation/lookup is needed.
3. **Cleanup:** consider deleting the 20 test projects (IDs in §6).
4. **Pivot Fitness:** no SEMrush project yet — create one to test the audit pipeline against a real client.
5. **Doc set hygiene:** two test files sit in `nv-boost-pivot-fitness` (see §2b).

---

## 8. Quick Reference (IDs)

- SEMRush Sandbox invoker: `INV_353a856a-867b-4f1f-a004-6ee3ad09c339`
- GitHub NVM invoker: `INV_0fe6c798-8f83-4ba9-96de-9f7ed9bda64a` (0 approved)
- Github TEST invoker: `INV_29ec4333-7aa4-4c1b-8e98-8cb801a9532f` (12 approved)
- domain_organic_research catalog_request_id: `localDomainRequest_ke339501-0000-0000-0000-000000000003`
- Genuine Krista SEMrush project: `27268136` | North Vista agency project: `26847652`
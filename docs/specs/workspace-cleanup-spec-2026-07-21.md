# North Vista Krista Workspace — Cleanup & Governance Hardening Spec

Produced by the `system-architect` skill run of 2026-07-21, from validated findings
of the Skill Validator + Workspace Explorer oversight sweep. Owner: Nick Grant.

## 1. Executive Summary

The workspace works, but carries governance debt that will bite as more people and
sessions use it: a dead 58-tool GitHub invoker duplicating one named "TEST" that
carries production traffic, a calling-convention split that role governance breaks,
one hardcoded repo owner, duplicate skills, 125 unreviewed tools, and accumulated
test artifacts in SEMrush, agents, and doc sets. Proposed: a four-phase cleanup —
hygiene, invoker consolidation, skill fixes, catalog thinning — sized for a small
team, with the role-model question explicitly parked as an open decision.
Complexity: Medium. Nothing here changes client-facing behavior.

## 2. Current State (validated 2026-07-21)

- 9 connected invokers, ~183 tools, 125 PENDING_REVIEW.
- Git traffic: flat tools bind to invoker "Github TEST" (12/58 approved).
  "GitHub NVM" invoker: 0/58 approved, prefixed github_nvm_* names, zero usage.
- Skills: 16 in catalog; skill-git-sync v1.0.1 APPROVED (role: Krista Author);
  first full catalog→git sync NOT yet run.
- Known governance behaviors: krista_execute_tool enforces role checks that flat
  tools do not; catalog strips YAML frontmatter (repo copy authoritative).
- OPEN DECISION (on hold): Marketing Team role may be deprecated/merged. No task
  below prescribes granting it.

## 3. Phases & Tasks

### Phase 0 — Zero-risk hygiene (this week)

**T1. First full catalog sync.** Run skill-git-sync Mode B; backfill all 16 skills
to skills/<id>/SKILL.md. Acceptance: sync report table, every skill committed or
accounted for. Effort: Small. Dependencies: none (skill is APPROVED).

**T2. SEMrush test-project purge.** Delete the 20 identified test projects
(8× krista-integration-test.com, 8× blog.example.com "Main Website SEO Audit"
duplicates, 2× Test Business Co., 2× ad-hoc krista.ai tests). No delete tool exists
in the invoker — manual via SEMrush UI. Verify each name/domain before delete
(classification was name-inferred). Keep 27268136 (Krista) and 26847652 (North
Vista). Acceptance: get_projects returns ~18 real projects. Effort: Small-Medium.

**T3. Doc set test-file removal.** Remove test-blog-strength-training-kaysville.txt
and Filer-Skill-Test-2026-07-16.txt from nv-boost-pivot-fitness (Krista Studio; no
MCP delete tool). Risk: none — both are confirmed test artifacts. Effort: Small.

### Phase 1 — Invoker consolidation

**T4. One GitHub invoker.** Recommendation: rename "Github TEST" → "GitHub Prod"
(it holds the approved toolset and the flat-name bindings; renaming is metadata
only) and disconnect/delete "GitHub NVM" (0 approved, 0 usage — dead weight).
Alternative considered and rejected: promote GitHub NVM and migrate bindings —
higher risk, zero benefit, breaks flat-name continuity mid-flight. Rollback:
re-provision the extension. Precaution: export/record both invoker configs before
touching either. Acceptance: one connected GitHub invoker, flat tools still work
(test_connection + a read + a commit smoke test). Effort: Medium.

**T5. Pending-tool triage.** Bulk-review the 125 pending tools with a default-deny
posture: approve only what a named skill or workflow needs (GoHighLevel 11,
Outlook 8, Meeting Agent 1/1, SEMrush create_project/get_project, GitHub NVM's 58
made moot by T4). Everything else stays pending deliberately — pending is a valid
state, not debt, once it's a decision. Acceptance: a one-page approved/denied/hold
ledger committed to docs/. Effort: Medium.

### Phase 2 — Skill fixes (each ships via skill-git-sync publish mode)

**T6. gbp-optimizer repo migration (two steps, ordered).** (a) Move client repos
nv-boost-* from nicholas-grant-krista to the northvistadigitalmarketing org;
(b) then ship gbp-optimizer v0.2.3 changing the Step 1 owner. Do NOT ship (b)
before (a) completes — the idempotency gate makes a mid-migration run halt safely
(HALT-PRECHECK), which is the designed failure. Acceptance: a demo-run spine test
(per the skill's own demo rule) passes against the org-hosted repo. Effort:
Medium-Large. Dependency: org repo permissions for the Krista GitHub connection.

**T7. Calling-convention policy + nv-seo-audit-agent amendment.** Write a one-page
policy: flat tool where exposed; krista_execute_tool only where no flat form
exists, with the required role annotated in the skill's manifest; on a role block,
HALT with a message naming the missing role — never silently degrade. Amend
nv-seo-audit-agent (v0.2.1) to that policy. BLOCKED-BY: the open role-model
decision — draft now, ship after the decision lands. Effort: Medium.

**T8. google-search-campaign-builder v1.0.2.** Mark domain_organic_research as
role-gated/optional; specify graceful degradation to url_organic_research +
keyword tools with a methodology note when it is unavailable. Not blocked — this
is degradation handling, not a role change. Effort: Small.

### Phase 3 — Catalog & agent thinning

**T9. Explorer consolidation.** Keep ONE workspace explorer. Recommendation: keep
the system `workspace_explorer` (MCP Admin) as the oversight instrument; retire or
explicitly re-scope Deepak's `workspace-explorer` (MCP Marketing) — if marketing
users need a tour skill, rename it to reflect the narrower audience. Effort: Small.

**T10. Agent cleanup.** Archive/delete z_Sandbox, z_OLD Conversation Agent,
z_Google Sandbox. Review the six zero-conversation agents (CSM Agent, Content
Creation, Knowledge Agent 2.9.1, Q&A Administration, Reporting Agent, GoHighLevel
Business Automation Agent) with their owners before deleting — zero conversations
may mean new, not dead. Effort: Small-Medium.

**T11. find_meeting_slots_with_attendees.** Pending since May with no roles:
approve it or delete it. A skill pending for 60+ days is a decision avoided.
Effort: Small.

## 4. Risk Notes

- Invoker deletion is the only hard-to-reverse action (T4) — export configs first.
- T6 ordering is load-bearing; the skill's own halt codes are the safety net.
- Role-model hold (T7/T8): interim behavior is explicit HALT/degrade with clear
  messaging — never silent failure, never a prescribed role grant.
- All skill changes flow through skill-git-sync publish mode → every change is
  versioned, verified (E02/E04), and revertible from git.

## 5. Verification

After each phase: re-run Skill Validator (catalog mode) + a Workspace Explorer
snapshot; commit the snapshot beside this spec. Live smoke tests over mocks
throughout — this environment has no staging, so every test is a real call with
read-only or demo-safe inputs (validator, test_connection, demo-rule runs).

## 6. Open Decisions (owner: Nick)

1. Marketing Team role: deprecate, merge, or keep (unblocks T7/T8 final form).
2. GitHub invoker end-state name ("GitHub Prod" proposed).
3. Which explorer survives T9.
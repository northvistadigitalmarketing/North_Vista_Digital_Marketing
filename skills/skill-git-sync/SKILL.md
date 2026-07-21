---
name: skill-git-sync
description: >
  Park Krista skills in GitHub for version control. INVOKE ONLY WHEN EXPLICITLY
  CALLED BY NAME ("skill-git-sync", "sync skills to git", "publish this skill")
  OR via the Standard Delegation Hook from another skill immediately after a
  krista_upload_skill call. Two modes: publish (upload a new/updated SKILL.md to
  Krista AND commit it to GitHub in one invocation) and sync (diff the entire
  skill catalog against the repo and commit drift). Do NOT auto-select for
  general git, backup, or skill questions.
---

# skill-git-sync

Single source of truth for keeping the Krista skill catalog and GitHub in
parity. Centralized in Krista so every connected Claude session executes the
identical procedure. No repository names are hardcoded — repos change over time.

## Repo resolution (both modes, always first)

1. If the invocation supplies owner/repo, use them.
2. Otherwise discover at runtime: list the org's current repositories, or ask
   the user. NEVER assume a remembered repo name.
3. Confirm access with get_repository (Can Push must be true) before writing.
4. Skills are parked at: skills/<skill_id>/SKILL.md on the default branch.

## Mode A — Publish (input contains SKILL.md content)

1. Validate the content has YAML frontmatter with a name. HALT E01 if not.
2. krista_upload_skill with the full SKILL.md.
3. Resolve repo (above). create_or_update_file at skills/<skill_id>/SKILL.md.
   Commit message: "skill: <name> v<version> — uploaded by <user> <ISO date>".
4. Read back via get_file_content; verify SHA/content matches. HALT E02 on
   mismatch — report that Krista and git have diverged.
5. Report: governance status (will be PENDING_REVIEW), commit SHA, file path.

## Mode B — Sync (no content input)

1. krista_list_skills — capture skill_id, version, uploaded_by, uploaded_at,
   status for every skill.
2. For each skill: krista_skill_details → full SKILL.md content.
3. Resolve repo (above). get_repository_tree to list skills/ paths.
4. Diff: for each catalog skill, compare content against the repo copy.
   - Missing in repo → commit (new file).
   - Content differs → commit (update, include file_sha).
   - In repo but NOT in catalog → do NOT delete; flag as orphan in the report.
5. Batch related changes with commit_files where possible. Commit message per
   skill: "skill-sync: <name> v<version> — uploader <user>, synced <ISO date>".
6. Report a table: skill, action taken (committed / unchanged / orphan-flagged),
   commit SHA. Zero silent failures — every skill appears in the report.

## Standard Delegation Hook (copy this block into other skills)

Any skill that calls krista_upload_skill MUST run this immediately after:

    IF krista_list_skills contains a skill whose skill_id matches
       (skill-git-sync | git-sync | github-sync) AND status is APPROVED:
        → krista_execute_skill(that skill_id,
            input: "sync mode, scope: <the skill_id just uploaded>")
    ELSE:
        → Warn the user: "Git parity not guaranteed — no approved git-sync
           skill found in this workspace." Continue; do not halt.

The hook is conditional by design: skills degrade gracefully in workspaces
where skill-git-sync is not installed or not yet approved.

## Constraints

- Never delete repo files. Never modify skills in Krista during sync mode.
- Never hardcode owner/repo/branch. Resolve at runtime, every run.
- Halt codes: E01 invalid SKILL.md, E02 read-back mismatch, E03 no push access.
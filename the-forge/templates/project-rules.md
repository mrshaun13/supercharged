---
trigger: always_on
---

# Project Rules for AI Development

> **Location**: `.windsurf/rules/project-rules.md`  
> **Purpose**: These rules are automatically loaded by Windsurf and guide AI behavior during development.
> **Maintainers**: Keep this file updated as the project evolves. See [CONTRIBUTING.md](../../CONTRIBUTING.md) for guidelines.

## About This File

This file is **automatically loaded** by Windsurf IDE when working in this repository. The AI assistant will follow these rules without you needing to reference them explicitly.

**When to update these rules:**
- New code patterns are established that AI should follow
- Architecture changes require different approaches
- You identify AI behaviors that need correction
- Project-specific requirements are added

---

## Key Documents

Before making changes, read and follow these documents:

- **AI_README.md** - Project architecture, patterns, and context
- **TESTING.md** - Testing methodology and requirements
- **SECURITY.md** - Security guidelines and best practices
- **DEVELOPMENT_CHECKLIST.md** - Validation checklist for changes

---

## FORGE Evaluation Health Check

**Check daily for stale evaluations** - On each interaction, compare today's date with `lastReminderShown` in the global reminder state. If the dates differ (or the file is missing), perform the check below.

### Step 1: Date-Based Rate Limiting Check

Before scanning projects, check global reminder state (`~/.forge/reminder-state.json`):
- If `lastReminderShown` matches today's date → skip reminder entirely (already shown today)
- If `lastReminderShown` is missing or different date → proceed to scan workspace roots

### Step 2: Scan All Workspace Roots

For EACH workspace folder:
1. **Search for FORGE project indicators** using this three-step approach:
   - **Primary search**: Look for `**/AI_README.md` files (FORGE standard)
   - **Secondary search**: Look for `**/AI-README.md` files (legacy FORGE naming)
   - **Tertiary search**: For each project found with AI_README.md or AI-README.md, check for `**/FORGE_EVALUATION_REPORT.md`
2. **Check if `.forge/status.json` exists** for projects found:
   - If exists: Parse `lastEvaluation.date` - is it > 14 days old?
   - If > 14 days old or missing: Add to stale projects list
3. **If `.forge/status.json` missing but AI_README.md or AI-README.md found**: Project is FORGE-enabled but never evaluated → add to stale list
4. **If NO AI_README.md or AI-README.md found**: Check for other FORGE primary indicators:
   - Check for `.forge/` directory
   - Check for `project-rules.md` in IDE rules directories with `trigger: always_on` header
   - If ANY primary indicator found: Add to stale list
   - If NO primary indicators found: Skip this project (not FORGE-enabled)

**Search Strategy**: Use both `**/AI_README.md` and `**/AI-README.md` as the primary search patterns since both are FORGE indicators (underscore is current standard, hyphen is legacy), then cross-reference with `**/FORGE_EVALUATION_REPORT.md` to identify which ones need evaluation.

Collect all stale/unevaluated projects into a list.

### Step 3: Show Consolidated Summary

If 1+ stale projects AND reminder not shown today, display:

> **FORGE Evaluation Reminder**
> 
> The following projects haven't been evaluated against FORGE standards recently:
> 
> | Project | Last Evaluated | Days Ago |
> |---------|---------------|----------|
> | webex-messaging-mcp | 2025-12-28 | 17 days |
> | servicenow-mcp | never | - |
> | ems_mcp | 2025-12-20 | 25 days |
> 
> A FORGE evaluation checks documentation, testing, security practices, and code quality.
> **Each evaluation takes approximately 3-5 minutes.**
> 
> **Would you like me to run an evaluation now?**
> - Reply with a **project name** to evaluate that project
> - Reply **"all"** to evaluate all stale projects sequentially
> - Reply **"skip"** to dismiss until tomorrow

### Step 4: Handle User Response

- **Project name**: Run FORGE evaluation on that specific project
- **"all"**: Run evaluations sequentially on all stale projects (warn: may take a few minutes per project)
- **"skip"** or no response: Update global `lastReminderShown` to today

**After showing reminder**: Update global `lastReminderShown` to today's date (YYYY-MM-DD format) in `~/.forge/reminder-state.json` so the user is not asked again today, regardless of how long the conversation continues.

### FORGE File Indicators

A project is FORGE-enabled if ANY of these **definitive indicators** exist:

**Primary Indicators** (most reliable):
- `.forge/` directory or `.forge/status.json` file - Definitive FORGE marker
- `AI_README.md` - FORGE-specific filename (current standard)
- `AI-README.md` - Legacy FORGE-specific filename (hyphen variant)
- `project-rules.md` in IDE rules directory (`.windsurf/rules/`, `.cursor/rules/`, etc.) with `trigger: always_on` header - look for FORGE-specific configuration in rule file

**Secondary Indicators** (require content verification):
- `TESTING.md`, `SECURITY.md`, or `CONTRIBUTING.md` with FORGE structure/content - These common filenames exist in many non-FORGE projects, so verify they contain FORGE-specific patterns:
  - References to FORGE framework
  - FORGE evaluation mentions
  - Structure matching FORGE templates
  - Integration with `AI_README.md` or `project-rules.md`

**Important**: Do NOT assume a project is FORGE-enabled just because `SECURITY.md` or `TESTING.md` exists - these are common filenames. Always verify FORGE-specific content or check for primary indicators first.

---

## Security Posture Sync Health Check

**Check bi-weekly for security posture drift** - On each interaction, compare today's date with `lastSecuritySync.date` in `.forge/status.json`. If the date is > 14 days old (or missing), perform the security posture sync check below.

### Unified Tracking via `.forge/status.json`

The security sync process uses the same `.forge/status.json` file as FORGE evaluations, extending it with security sync tracking:

```json
{
  "lastEvaluation": {
    "date": "2026-01-14T12:00:00Z",
    "score": 85,
    "grade": "B"
  },
  "lastSecuritySync": {
    "date": "2026-01-14",
    "upstreamVersion": "v1.0.1",
    "upstreamRepo": "project-codeguard/rules",
    "updatesApplied": 5,
    "updatesSkipped": 2
  },
  "forgeVersion": "2.0.0",
  "projectName": "my-project"
}
```

### Step 1: Date-Based Rate Limiting Check

Before checking security posture, verify `.forge/status.json` exists and check `lastSecuritySync.date`:
- If `lastSecuritySync.date` exists and is ≤ 14 days old → skip reminder (already synced recently)
- If `lastSecuritySync.date` is missing or > 14 days old → proceed to security posture sync
- If `.forge/status.json` doesn't exist → create it with initial structure, then proceed to sync

### Step 2: Security Posture Sync Trigger Phrases

When the developer uses these **exact phrases**, execute the security posture sync:

### "Check security posture" or "Sync security rules" or "Security drift check"
**Action**: Compare local SECURITY.md against upstream CodeGuard rules and identify drift

### Step 3: Interactive Sync Workflow with Remediation

When triggered, execute this step-by-step process:

1. **Fetch upstream rules** from `project-codeguard/rules` repository with fallback mechanism:
   
   **Step 1a: Try local temp clone first**
   - Clone repository locally: `git clone git@github.com:project-codeguard/rules.git` to a temporary location (e.g., `/tmp/codeguard-rules-{timestamp}`)
   - If clone succeeds:
     - Use local clone at `{temp_path}/sources/core/` for rule comparison
     - Note: Clean up temporary clone directory after sync completes (see Step 7)
     - Proceed to Step 1d (verification)
   - If clone fails, proceed to Step 1b
   
   **Step 1b: Fallback to online access** (if local clone fails)
   - Attempt to access `https://github.com/project-codeguard/rules/tree/main/sources/core`
   - If successful, proceed to comparison using online source
   - If online access fails, proceed to Step 1c
   
   **Step 1c: Fallback to user-provided path** (if clone and online access both fail)
   - Prompt user: "Unable to clone or access upstream repository online. Please provide the local path to the project-codeguard/rules repository:"
   - Use user-provided path: `{user_path}/sources/core/`
   - Do NOT clean up user-provided paths (only clean temporary clones)
   
   **Step 1d: Verify rules directory**
   - Confirm `sources/core/` directory exists and contains rule files
   - Identify current upstream version/tag (from git tag or release info if available)
   - **CRITICAL**: If rules directory not found or contains no rule files:
     - Report error: "Unable to access CodeGuard rules. Clone failed, online access failed, and no valid local path provided."
     - **Abort sync process** - do not proceed with comparison
     - Do not update `.forge/status.json` (sync did not complete)
     - If temporary clone was created, clean it up before aborting

2. **Compare** local `SECURITY.md` against upstream CodeGuard core rules
   - Parse local SECURITY.md structure and content
   - Compare against upstream rule files in `sources/core/`
   - Identify: new rules (missing locally), removed rules (present locally but removed upstream), changed rules (content differs)

3. **For each identified drift, present ONE update at a time and wait for user response**:

   **Step 3a: Present first update**
   - Present the first security update with issue and remediation
   - Wait for user to choose Accept or Skip
   - Apply the user's choice immediately

   **Step 3b: Continue sequentially**
   - Move to the next update only after receiving user response
   - Repeat until all updates are processed
   - Track progress: maintain count of updates applied vs skipped

Security Update [X] of [Y]

**ISSUE**: [Rule Name]  
**Source**: [codeguard-X-rule-name.md]  
**Status**: [Missing from local / Changed / Removed upstream]

**DESCRIPTION**:  
[Clear explanation of what the security issue or update is, why it matters, and what the upstream rule specifies]

**REMEDIATION** (content to add/update in SECURITY.md):

[Exact markdown content that should be added or updated in SECURITY.md, including proper section headers, code examples, checklists, etc.]

**[Accept] [Skip]**

4. **User chooses for each update**:
   - **Accept**: Apply the remediation content to SECURITY.md at the appropriate location
   - **Skip**: Log as skipped, move to next update without applying
   - **Show Diff**: Display detailed diff if updating existing content (optional)

5. **Repeat step 3** for each identified drift until all updates are processed

6. **Cleanup temporary repository** (if local clone was created):
   - If a temporary clone was created in Step 1a (e.g., `/tmp/codeguard-rules-{timestamp}`):
     - Remove the temporary clone directory: `rm -rf {temp_path}`
     - Confirm cleanup completed
   - If user-provided path was used: Do NOT delete (user's local repository)

### Step 4: Post-Sync Validation

After all updates are processed (accepted or skipped):

1. **Run security posture check** on the updated SECURITY.md:
   - Verify document structure is valid markdown
   - Check that all sections are properly formatted
   - Verify no placeholder content remains
   - Confirm all code examples are properly formatted
   - Check that links and references are valid

2. **Report final status**:
   ```
   Security Posture Sync Complete
   
   Updates Applied: 4
   Updates Skipped: 1
   Upstream Version: v1.0.1
   Upstream Repo: project-codeguard/rules
   
   Validation: PASS - SECURITY.md aligned with upstream
   ```

3. **If validation fails**: Report specific issues and offer to fix them

### Step 5: Update Tracking

After successful sync and validation:

1. **Update `.forge/status.json`** with sync results:
   ```json
   "lastSecuritySync": {
     "date": "2026-01-14",
     "upstreamVersion": "v1.0.1",
     "upstreamRepo": "project-codeguard/rules",
     "updatesApplied": 4,
     "updatesSkipped": 1
   }
   ```

2. **If `.forge/status.json` doesn't exist**: Create it with both `lastEvaluation` (if available) and `lastSecuritySync` structures

3. **Preserve existing data**: Never overwrite `lastEvaluation` or other fields when updating `lastSecuritySync`

### Important Notes

- **Upstream source**: Always reference `project-codeguard/rules` repository, specifically the `sources/core/` directory
- **Remediation quality**: Each remediation must include complete, ready-to-apply markdown content, not just descriptions
- **User control**: User must explicitly Accept or Skip each update - never auto-apply without confirmation
- **Bi-weekly cycle**: Reminder appears every 14 days, but user can trigger sync manually anytime using trigger phrases

---

## FORGE Evaluation Trigger Phrases

When the developer uses these **exact phrases**, execute the FORGE evaluation check and show a summary:

### "FORGE status" or "FORGE project status" or "Show FORGE project"
**Action**: Scan all workspace roots for FORGE-enabled projects and show evaluation status summary

1. Scan all workspace folders for FORGE indicators (using Step 2 logic above)
2. For each FORGE-enabled project found, check `.forge/status.json`
3. Display a summary table showing:
   - Project name
   - Last evaluation date (or "never evaluated")
   - Days since last evaluation
   - Current grade/score (if available)
4. Highlight any projects that need evaluation (>14 days old or never evaluated)

**Example output**:
```
FORGE Project Status Summary

| Project | Last Evaluated | Days Ago | Grade | Status |
|---------|---------------|----------|-------|--------|
| webex-messaging-mcp | 2025-12-28 | 17 days | B (92) | ⚠️ Stale |
| servicenow-mcp | never | - | - | ⚠️ Never evaluated |
| ems_mcp | 2026-01-10 | 4 days | A (95) | ✅ Current |
```

### "Evaluate FORGE projects" or "Run FORGE evaluation"
**Action**: Same as above, but also offer to run evaluations on stale projects

After showing the summary, ask: "Would you like me to evaluate the stale projects now?"

### "Run FORGE evaluation on [project name]" or "Evaluate [project name]"
**Action**: Complete FORGE evaluation for specified project

**CRITICAL REQUIREMENT**: Every FORGE evaluation MUST generate BOTH files:
1. ✅ `FORGE_EVALUATION_REPORT.md` - Detailed evaluation with scores and findings
2. ✅ `FORGE_REMEDIATION_PLAN.md` - Actionable steps to fix identified issues

**Validation Checklist**: Before completing evaluation, verify:
- Both evaluation report AND remediation plan are created
- Remediation plan addresses all issues found in evaluation
- Both files are saved in the project root directory
- File names exactly match the required format

**Failure to generate both files is incomplete work and must be corrected immediately.**

### "FORGE health check" or "Check FORGE health"
**Action**: Comprehensive check including both status summary and any issues found

Show the status summary plus:
- Any missing FORGE files
- Projects with outdated evaluations
- **Missing remediation plans for existing evaluations**
- Recommendations for improvement

---

## Development Guidelines

### Before Writing Code

1. **Understand the context**: Read AI_README.md to understand project architecture
2. **Check existing patterns**: Follow established code patterns in the project
3. **Consider testability**: Think about how the code will be tested

### When Writing Code

1. **Follow project conventions**: Match existing code style, naming, and structure
2. **Keep changes minimal**: Make focused edits, avoid unnecessary refactoring
3. **Maintain existing comments**: Don't add or remove comments unless asked
4. **No hardcoded secrets**: Use environment variables for all sensitive values
5. **Validate inputs**: Never trust external input without validation

### After Writing Code

1. **Suggest tests**: Recommend unit tests for new functions (only if behavior added/changed)
2. **Update documentation**: If architecture changes, note that AI_README.md needs updating
3. **Note security considerations**: Flag any security-relevant changes
4. **Offer validation**: After significant changes, suggest: "Would you like me to validate these changes against DEVELOPMENT_CHECKLIST.md?"

---

## Validation Trigger Phrases

When the developer uses these **exact phrases**, execute the corresponding validation:

### "Validate" or "Validate my changes" or "Run validation"
**Action**: Execute full validation from DEVELOPMENT_CHECKLIST.md

Run all four validation sections:
1. Validate code quality (patterns, naming, conventions)
2. Validate testing (do tests pass? are new tests needed?)
3. Validate security (scan for secrets, check SECURITY.md rules)
4. Validate documentation (AI_README.md current? CHANGELOG needed?)

Provide structured validation report with:
- PASS/FAIL status for each section
- Specific issues with file/line references
- READY TO COMMIT: YES/NO recommendation
- Recommended fixes for each issue

### "Validate code quality" or "Validate code"
**Action**: Code quality validation only
1. Compare code against patterns in AI_README.md
2. Verify naming conventions match project standards
3. Check for unintentional breaking changes
4. Report: PASS/FAIL with specific deviations and suggested fixes

### "Validate tests" or "Validate testing"
**Action**: Testing validation only
1. Confirm all existing tests pass
2. Determine if this change requires new tests
3. If required, verify tests exist with adequate coverage
4. If not required, explain why
5. Report: PASS/FAIL/NOT_APPLICABLE with reasoning and suggested fixes

### "Validate security"
**Action**: Security validation only
1. Scan code for hardcoded secrets, API keys, passwords, tokens
2. Identify which SECURITY.md rules apply to current changes
3. Verify those specific rules are followed
4. Check for common vulnerabilities (SQL injection, XSS, etc.)
5. Report: PASS/FAIL with specific violations, rule references, and suggested fixes

### "Validate documentation" or "Validate docs"
**Action**: Documentation validation only
1. Determine if AI_README.md needs updates (architecture changes?)
2. Check if CHANGELOG.md needs entry (user-facing changes?)
3. Verify code is self-documenting
4. Report: PASS/FAIL with specific gaps and suggested fixes

**Important**: These triggers should produce structured, actionable output with specific file/line references and concrete fix suggestions—not generic responses.

---

## File Management

- **Only create files when necessary** - avoid cluttering the workspace
- **Ask before creating new files** - unless it's clearly part of the task
- **Update AI_README.md** when making significant changes to code, features, or architecture

---

## Testing Expectations

- **New features should have tests** - suggest test cases when adding functionality
- **Bug fixes need regression tests** - write a test that would have caught the bug
- **Don't break existing tests** - if a test fails, understand why before changing it

---

## Security Requirements

- **Never hardcode secrets** - always use environment variables
- **Validate all inputs** - especially from users or external sources
- **Use parameterized queries** - never concatenate SQL
- **Follow SECURITY.md** - for comprehensive security guidelines

---

## Communication Style

- **Be direct and concise** - avoid unnecessary explanations
- **Ask clarifying questions** - when requirements are unclear
- **Explain trade-offs** - when multiple approaches exist
- **Flag concerns proactively** - especially security or testing gaps

---

## When to Ask vs Act

### Act (don't ask)
- Following established patterns in the codebase
- Making requested changes that are clearly defined
- Running tests to verify changes
- Fixing obvious bugs

### Ask first
- Creating new files or directories
- Changing project structure
- Introducing new dependencies
- Making architectural changes
- Deviating from established patterns
